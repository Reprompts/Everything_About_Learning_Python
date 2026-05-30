# Python Processes & Threads: Chapter 9

# Event Loop and Task Scheduling in Python (`asyncio`)

Asynchronous programming in Python is powered by a central component called the **event loop**. The event loop acts as the runtime engine that manages asynchronous execution, coordinates tasks, and handles I/O events efficiently.

In traditional programs, execution usually follows a linear path—one instruction runs after another. However, asynchronous programs must manage many tasks concurrently, especially when tasks frequently wait for external operations such as network responses or file reads.

The event loop solves this problem by orchestrating when tasks should run, when they should pause, and when they should resume.

This article explains:

- What the event loop is
- How task scheduling works
- How the event loop manages I/O operations
- Important functions like `asyncio.run()`, `asyncio.create_task()`, and `asyncio.gather()`
- Internal mechanisms used by the event loop

---

# 1. What is an Event Loop?

The event loop is the core scheduler in Python's asynchronous framework.

It continuously monitors and manages:

- Coroutines
- Asynchronous tasks
- I/O events
- Timers
- Callbacks

It ensures that multiple tasks make progress concurrently without blocking the program.

Conceptually, the event loop acts like a traffic controller for tasks.

## Conceptual View

```text
Program
   │
   ▼
Event Loop
   │
   ├── Task A (network request)
   ├── Task B (database query)
   ├── Task C (file read)
   └── Task D (API call)
```

The event loop decides which task runs next.

---

# 2. Continuous Execution of the Event Loop

The event loop runs continuously inside a loop structure.

Simplified model:

```python
while program_running:
    check_ready_tasks()
    execute_task()
    check_io_events()
    resume_waiting_tasks()
```

The loop keeps running until:

- All tasks complete
- Or the program terminates

---

# 3. Core Responsibilities of the Event Loop

The event loop performs three major responsibilities.

---

## 3.1 Scheduling Coroutines

A coroutine is an async function that can pause and resume execution.

Example:

```python
async def task():
    await asyncio.sleep(2)
```

The event loop schedules these coroutines for execution.

```text
Coroutine created
      │
      ▼
Event loop schedules it
      │
      ▼
Coroutine executes until await
      │
      ▼
Coroutine pauses
```

When the awaited operation finishes, the event loop resumes the coroutine.

---

## 3.2 Managing I/O Events

Many asynchronous operations involve I/O tasks, such as:

- Network requests
- Reading files
- Database queries

These operations take time and would normally block execution.

The event loop solves this by using non-blocking I/O mechanisms provided by the operating system.

Common mechanisms include:

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
IOCP (I/O Completion Ports)
```

Python accesses these using the `selectors` module.

### Workflow

```text
Coroutine sends network request
        │
        ▼
Event loop registers socket with OS
        │
        ▼
Coroutine pauses
        │
        ▼
Event loop runs other tasks
        │
        ▼
OS signals response ready
        │
        ▼
Event loop resumes coroutine
```

---

## 3.3 Switching Between Tasks

When a coroutine encounters an `await` expression, it temporarily stops executing.

Example:

```python
await asyncio.sleep(2)
```

At this point:

- Coroutine yields control
- Event loop switches to another task

This mechanism is known as **cooperative multitasking**.

Tasks voluntarily give up control so other tasks can run.

---

# 4. Important `asyncio` Functions

Python provides several important functions for interacting with the event loop.

---

## 4.1 `asyncio.run()`

`asyncio.run()` is the simplest way to start an event loop.

It performs several steps automatically:

1. Creates a new event loop
2. Runs the given coroutine
3. Closes the event loop after completion

Example:

```python
import asyncio

async def main():
    print("Hello async world")

asyncio.run(main())
```

### Execution Flow

```text
asyncio.run()
      │
      ▼
Create event loop
      │
      ▼
Schedule main() coroutine
      │
      ▼
Run until coroutine finishes
      │
      ▼
Close event loop
```

This function is the recommended entry point for async programs.

---

## 4.2 `asyncio.create_task()`

`create_task()` schedules a coroutine to run concurrently.

Example:

```python
async def worker():
    await asyncio.sleep(2)
    print("Task finished")

async def main():
    task = asyncio.create_task(worker())

    print("Task started")

    await task

asyncio.run(main())
```

### Execution

```text
Main coroutine starts
Task scheduled
Event loop runs worker
Worker pauses (sleep)
Event loop continues
Worker resumes later
```

`create_task()` converts a coroutine into a **Task** object that the event loop can manage.

### Task Object

Internally a Task:

- Wraps a coroutine
- Tracks its state
- Schedules execution

Task states:

```text
PENDING
RUNNING
DONE
CANCELLED
```

---

## 4.3 `asyncio.gather()`

`gather()` runs multiple coroutines concurrently and waits for all of them to finish.

Example:

```python
async def fetch_data(id):
    await asyncio.sleep(2)
    return id

async def main():
    results = await asyncio.gather(
        fetch_data(1),
        fetch_data(2),
        fetch_data(3)
    )

    print(results)

asyncio.run(main())
```

### Execution

```text
fetch_data(1)
fetch_data(2)
fetch_data(3)
```

All three start simultaneously.

### Result

```python
[1, 2, 3]
```

---

# 5. Example: Event Loop with Multiple Tasks

Example program:

```python
import asyncio

async def task(name, delay):
    print(f"{name} started")

    await asyncio.sleep(delay)

    print(f"{name} finished")

async def main():
    t1 = asyncio.create_task(task("Task A", 2))
    t2 = asyncio.create_task(task("Task B", 1))

    await t1
    await t2

asyncio.run(main())
```

### Output

```text
Task A started
Task B started
Task B finished
Task A finished
```

### Explanation

- Task A waits 2 seconds
- Task B waits 1 second
- Event loop switches between them

---

# 6. Internal Data Structures of the Event Loop

The `asyncio` event loop internally uses several structures.

---

## Ready Queue

A queue containing tasks ready to run.

Structure:

```python
deque()
```

Example:

```text
Task1
Task2
Task3
```

---

## Scheduled Task Heap

Tasks scheduled for later execution use a priority queue.

Structure:

```python
heapq
```

Example:

```text
(time, task)
```

Earliest task executes first.

---

## Selector for I/O

Tracks file descriptors waiting for events.

Example:

```text
Selector
   │
   ├── socket1
   ├── socket2
   └── file
```

The OS signals when events occur.

---

# 7. Event Loop Execution Cycle

The event loop repeatedly performs this cycle.

```python
while True:
    run_ready_tasks()
    check_scheduled_tasks()
    poll_io_events()
    move_ready_tasks_to_queue()
```

This cycle allows the event loop to manage thousands of asynchronous tasks efficiently.

---

# 8. Why Event Loops Are Efficient

Event loops are efficient because they:

- Avoid thread creation
- Reduce context switching
- Rely on lightweight coroutines

### Memory Comparison

```text
Thread      ≈ ~1 MB stack
Coroutine   ≈ Few KB
```

This allows programs to handle:

```text
10,000+ concurrent tasks
```

with minimal overhead.

---

# 9. Real-World Systems Using Event Loops

Event loop architectures power many modern systems.

### Python Frameworks

- FastAPI
- Sanic
- Starlette

### Other Technologies

- Node.js
- Nginx
- Go Runtime Scheduler

These systems use event loops to support massive concurrency.

---

# 10. Summary

The event loop is the central component of asynchronous programming in Python.

Its main responsibilities include:

- Scheduling coroutines
- Managing I/O events
- Switching between tasks efficiently

Key functions provided by `asyncio` include:

- `asyncio.run()` → Starts the event loop
- `asyncio.create_task()` → Schedules concurrent tasks
- `asyncio.gather()` → Runs multiple tasks together

Internally, the event loop uses:

- Ready queues
- Priority heaps
- OS I/O selectors

to coordinate asynchronous execution.

By using event-driven scheduling and non-blocking I/O, Python applications can handle thousands of concurrent operations efficiently using a single thread.
