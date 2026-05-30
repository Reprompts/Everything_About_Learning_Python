# Python Processes & Threads: Chapter 8

# Async Functions in Python (`async` / `await`) — Deep Dive

Asynchronous programming in Python relies heavily on **async functions** and the **`await`** keyword. These features allow programs to pause execution without blocking the entire program, enabling other tasks to run while waiting for operations such as network responses or disk I/O.

This article explains:

* How `async` and `await` work
* How the event loop schedules tasks
* Low-level implementation of the event loop
* Data structures used internally
* How task switching works
* Comparison with thread scheduling and the GIL
* How Python switches between async tasks and threads

---

# 1. What Is an Async Function?

An async function is a function defined using the `async` keyword.

## Example

```python
async def my_function():
    print("Hello")
```

Unlike normal functions:

```python
def func():
    ...
```

an async function does **not** run immediately when called.

Instead, it returns a **coroutine object**.

## Example

```python
result = my_function()
```

Here:

```text
result = coroutine object
```

The coroutine must be executed by an event loop.

---

# 2. Coroutine Objects

When Python sees:

```python
async def fetch_data():
    ...
```

calling it creates a coroutine object.

```text
fetch_data() → Coroutine Object
```

Think of it as a paused function waiting to run.

Execution begins only when it is scheduled in the event loop.

## Example

```python
import asyncio

asyncio.run(fetch_data())
```

---

# 3. The `await` Keyword

Inside async functions, Python provides the `await` keyword.

## Example

```python
async def fetch_data():
    await asyncio.sleep(2)
```

## What `await` Does

When Python reaches:

```python
await something
```

the coroutine:

* Pauses execution
* Yields control to the event loop
* Allows the event loop to run another task
* Resumes when the awaited result is ready

---

## Execution Flow

```text
Coroutine A starts
        │
        ▼
await network request
        │
        ▼
Coroutine pauses
        │
        ▼
Event loop runs Coroutine B
        │
        ▼
Network response arrives
        │
        ▼
Coroutine A resumes
```

This enables massive concurrency without threads.

---

# 4. Example Async Function

## Example

```python
import asyncio

async def fetch_data():
    print("Start fetching")

    await asyncio.sleep(2)

    print("Data fetched")

asyncio.run(fetch_data())
```

## Output

```text
Start fetching

(wait 2 seconds)

Data fetched
```

During the waiting period, the event loop can execute other tasks.

---

# 5. Running Multiple Async Tasks

## Example

```python
import asyncio

async def task(name):
    print(name, "started")

    await asyncio.sleep(2)

    print(name, "finished")

async def main():
    await asyncio.gather(
        task("A"),
        task("B"),
        task("C")
    )

asyncio.run(main())
```

## Execution Timeline

### Time 0

```text
A started
B started
C started
```

### Time 2

```text
A finished
B finished
C finished
```

All tasks run concurrently using a single thread.

---

# 6. The Event Loop (Core Engine)

The event loop is responsible for executing asynchronous tasks.

It acts as a scheduler.

## Responsibilities

* Track pending tasks
* Run tasks when ready
* Suspend tasks waiting for I/O
* Resume tasks when results arrive

---

## Event Loop Model

```text
Event Loop
    │
    ├── Task A
    ├── Task B
    ├── Task C
    └── Task D
```

Tasks are executed cooperatively.

---

# 7. Low-Level Event Loop Implementation

The Python `asyncio` event loop is implemented in CPython using a combination of:

* Task queues
* Priority queues
* Selectors
* File descriptor monitoring

---

## Core Data Structures

### 1. Ready Queue (FIFO Queue)

Tasks ready to execute are stored in a queue.

```python
ready_queue = deque()
```

### Example

```text
Task A
Task B
Task C
```

The event loop removes tasks from this queue and executes them.

---

### 2. Scheduled Task Heap

Delayed tasks are stored in a priority queue (min heap).

### Structure

```text
heapq
```

### Example Entries

```text
(time, task)
```

Example:

```text
(10.5, TaskA)
(12.0, TaskB)
```

The event loop always executes the earliest scheduled task first.

---

### 3. Selector for I/O

Async I/O relies on operating system event notification mechanisms.

### Linux

```text
epoll
```

### macOS

```text
kqueue
```

### Windows

```text
IOCP
```

Python abstracts these using the `selectors` module.

### Conceptual Structure

```text
Selector
    │
    ├── Socket 1
    ├── Socket 2
    └── File Descriptor
```

---

# 8. Event Loop Execution Cycle

## Simplified Pseudocode

```python
while True:
    run_ready_tasks()
    check_scheduled_tasks()
    wait_for_io_events()
    move_ready_io_tasks_to_queue()
```

---

## Detailed Flow

```text
1. Take task from ready queue

2. Run until await

3. Task yields control

4. Event loop switches to next task
```

---

# 9. Event Loop Scheduling Strategy

Async tasks use **cooperative multitasking**.

This means:

```text
Tasks voluntarily yield control
```

Switching occurs when:

```text
await is encountered
```

### Example

```text
Task A executes
      │
      ▼
await network request
      │
      ▼
Task yields control
      │
      ▼
Event loop runs Task B
```

Async scheduling is **await-driven**, not time-slice-driven.

---

# 10. Thread Scheduling (Contrast)

Threads use **preemptive multitasking**.

This means:

```text
OS scheduler interrupts threads automatically
```

### Mechanism

```text
Time Slicing
```

### Example

```text
Thread A runs for 5 ms

Thread B runs for 5 ms

Thread C runs for 5 ms
```

Even if a thread never voluntarily yields, the operating system can interrupt it.

---

# 11. Python Threads and the GIL

Python threads operate inside a single interpreter protected by the **Global Interpreter Lock (GIL)**.

The GIL ensures:

```text
Only one thread executes Python bytecode at a time
```

---

## GIL Scheduling

CPython switches threads based on:

* Bytecode execution timing
* Interpreter scheduling intervals

Older Python versions used a check interval.

Modern Python exposes:

```python
sys.getswitchinterval()
```

Default value:

```text
~5 milliseconds
```

---

## Thread Execution Model

```text
Thread A acquires GIL
       │
       ▼
Executes Python bytecode
       │
       ▼
Time slice expires
       │
       ▼
Releases GIL
       │
       ▼
Thread B acquires GIL
```

---

# 12. Where Threads Come From

Threads are managed by the operating system.

Python uses native OS threads.

### Linux / macOS

```text
POSIX Threads (pthreads)
```

### Windows

```text
Win32 Threads
```

Python thread creation:

```python
threading.Thread(...)
```

internally uses operating system thread APIs.

---

# 13. Async Tasks vs Threads

```text
+-------------------+----------------------+----------------------+
| Feature           | Async Tasks          | Threads              |
+-------------------+----------------------+----------------------+
| Scheduling        | Event Loop           | OS Scheduler         |
| Switching         | await Points         | Time Slicing         |
| Memory Overhead   | Low                  | Higher               |
| CPU Parallelism   | No                   | Limited by GIL       |
| Scalability       | Thousands of Tasks   | Hundreds of Threads  |
+-------------------+----------------------+----------------------+
```

---

# 14. Switching Mechanisms Compared

## Async Switching

### Trigger

```text
await
```

### Example

```text
Task A → await
          │
          ▼
Event Loop runs Task B
```

---

## Thread Switching

### Trigger

```text
OS Timer Interrupt
```

### Example

```text
Thread A running
        │
        ▼
Timer interrupt
        │
        ▼
Thread B scheduled
```

---

# 15. Visual Comparison

## Async Event Loop

```text
Single Thread
      │
      ▼
  Event Loop
      │
      ├── Task A
      ├── Task B
      └── Task C
```

Switching mechanism:

```text
await → switch
```

---

## Thread Execution

```text
Process
   │
   ├── Thread A
   ├── Thread B
   └── Thread C
```

Switching mechanism:

```text
time slice → switch
```

---

# 16. Why Async Is Extremely Scalable

Threads consume significant memory.

Typical thread stack:

```text
~1 MB
```

Async tasks usually consume only:

```text
A few KB
```

This means:

```text
100 Threads   → Heavy

10,000 Async Tasks → Possible
```

That is why asynchronous architectures power systems such as:

* FastAPI
* Node.js
* Nginx
* High-performance APIs
* Real-time communication platforms

---

# 17. Summary

Async functions (`async` / `await`) enable efficient concurrency without relying on multiple threads.

## Key Concepts

* `async def` defines a coroutine
* `await` suspends execution
* Event loop schedules tasks
* Coroutines execute cooperatively

---

## Low-Level Event Loop Architecture

The event loop internally uses:

* Ready task queues
* Priority heaps for delayed tasks
* OS selectors (`epoll`, `kqueue`, `IOCP`)
* I/O event monitoring

---

## Scheduling Models

### Async Programming

```text
Cooperative Scheduling
(await-based)
```

### Threads

```text
Preemptive Scheduling
(time-slice-based)
```

Understanding these mechanisms is essential for designing high-performance Python systems capable of handling thousands of concurrent operations efficiently while minimizing memory usage and context-switching overhead.
