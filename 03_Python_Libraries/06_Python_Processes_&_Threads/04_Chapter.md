# Python Processes & Threads: Chapter 4

# Python Threads

In modern software systems, programs often need to perform multiple tasks at the same time. For example, a web application might need to:

* Handle user requests
* Fetch data from a database
* Read files from disk
* Communicate with external APIs

Instead of executing these tasks one after another, Python can run them concurrently using **threads**.

Threads are a fundamental concept in concurrent programming, allowing a single process to perform multiple operations simultaneously.

---

# 1. What Is a Thread?

A **thread** is the smallest unit of execution inside a program.

It is often called a **lightweight process** because it behaves similarly to a process but with much lower overhead.

A thread runs inside a process and shares the process resources.

## Basic Idea

```text
Process
   │
   ├── Thread 1
   ├── Thread 2
   ├── Thread 3
   └── Thread 4
```

All threads belong to the same process.

---

# 2. Threads vs Processes

Understanding the difference between threads and processes is essential.

| Feature       | Process         | Thread                    |
| ------------- | --------------- | ------------------------- |
| Memory        | Separate Memory | Shared Memory             |
| Creation Cost | High            | Low                       |
| Communication | Complex (IPC)   | Simple (Shared Variables) |
| Isolation     | Strong          | Weak                      |
| Execution     | Parallel        | Concurrent                |

## Visualization

### Processes

```text
Process A
   └── Memory Space A

Process B
   └── Memory Space B
```

### Threads

```text
Single Process
   ├── Thread 1
   ├── Thread 2
   └── Thread 3

All Share the Same Memory
```

Because threads share memory, they can communicate easily, but they must also be careful about synchronization.

---

# 3. Why Use Threads?

Threads allow programs to perform multiple operations concurrently.

This improves performance for tasks that spend time waiting for external resources.

## Examples

* Waiting for network responses
* Reading files
* Querying databases
* Downloading data

Without threads, a program may block and wait, wasting CPU time.

---

# 4. Python Threading Module

Python provides built-in support for threads through the `threading` module.

This module allows developers to:

* Create threads
* Start threads
* Manage thread execution
* Synchronize threads

## Main Components

```text
threading
│
├── Thread
├── Lock
├── RLock
├── Event
├── Semaphore
├── Condition
└── Timer
```

The most important class is:

```python
threading.Thread
```

---

# 5. Creating Threads in Python

Threads are created using the `Thread` class.

## Basic Steps

1. Define a function
2. Create a thread object
3. Start the thread
4. Optionally wait using `join()`

---

# 6. Basic Thread Example

## Step 1 — Import Module

```python
import threading
```

## Step 2 — Define Function

```python
def worker():
    print("Thread is running")
```

## Step 3 — Create Thread

```python
t = threading.Thread(target=worker)
```

## Step 4 — Start Thread

```python
t.start()
```

## Step 5 — Wait for Completion

```python
t.join()
```

## Full Example

```python
import threading

def worker():
    print("Thread execution started")

t = threading.Thread(target=worker)

t.start()
t.join()

print("Main program finished")
```

### Output

```text
Thread execution started
Main program finished
```

---

# 7. Running Multiple Threads

Threads become powerful when multiple tasks run concurrently.

## Example

```python
import threading

def worker(num):
    print("Thread", num, "running")

threads = []

for i in range(5):
    t = threading.Thread(
        target=worker,
        args=(i,)
    )

    threads.append(t)
    t.start()

for t in threads:
    t.join()
```

## Execution Structure

```text
Main Thread
     │
     ├── Thread 0
     ├── Thread 1
     ├── Thread 2
     ├── Thread 3
     └── Thread 4
```

All threads run within the same process.

---

# 8. Shared Memory Between Threads

Threads share the same memory space.

## Example

```python
import threading

counter = 0

def increment():
    global counter
    counter += 1

threads = []

for i in range(10):
    t = threading.Thread(target=increment)

    threads.append(t)
    t.start()

for t in threads:
    t.join()

print(counter)
```

All threads modify the same variable.

This shared-memory model enables very fast communication but can lead to race conditions if synchronization is not used.

---

# 9. Python Global Interpreter Lock (GIL)

Python has a special mechanism called the **Global Interpreter Lock (GIL)**.

The GIL ensures that only one thread executes Python bytecode at a time.

## Visualization

```text
Process
   │
   ├── Thread 1
   ├── Thread 2
   └── Thread 3

Only ONE thread executes Python code at a time
```

Because of the GIL:

* Threads do not provide true parallelism for CPU-intensive tasks.
* Only one thread executes Python bytecode at a given moment.

However, threads release the GIL during I/O operations, allowing other threads to run.

---

# 10. Best Use Case: I/O-Bound Tasks

Threads are ideal for **I/O-bound workloads**.

An I/O-bound task spends most of its time waiting for external resources.

## Network Calls

* Downloading data from APIs
* HTTP requests
* Web scraping

## File Reading

* Reading large files
* Log processing
* Streaming data

## Database Queries

* Fetching records
* Executing SQL queries
* Waiting for database responses

### Example Scenario

Imagine a program downloading 10 web pages.

### Without Threads

```text
Download Page 1
(wait)

Download Page 2
(wait)

Download Page 3
(wait)
```

### With Threads

```text
Thread 1 → Download Page 1
Thread 2 → Download Page 2
Thread 3 → Download Page 3
Thread 4 → Download Page 4
```

Multiple downloads can occur simultaneously.

---

# 11. Thread Lifecycle

Threads pass through several states.

```text
New
 ↓
Runnable
 ↓
Running
 ↓
Waiting / Blocked
 ↓
Terminated
```

## Python Thread Lifecycle

```text
Thread Created
      ↓
start()
      ↓
Thread Running
      ↓
join()
      ↓
Thread Finished
```

---

# 12. Thread Communication

Threads communicate through shared variables.

### Example

```text
Thread A updates variable
          ↓
Shared Memory
          ↓
Thread B reads variable
```

Because memory is shared, communication is extremely fast.

However, synchronization tools are often required to prevent data corruption.

---

# 13. Advantages of Threads

## Lightweight

Threads are cheaper to create than processes.

---

## Shared Memory

Threads can easily exchange data without IPC mechanisms.

---

## Efficient for I/O Tasks

Threads improve performance when programs spend time waiting for external resources.

---

## Faster Context Switching

Switching between threads is generally faster than switching between processes.

---

# 14. Limitations of Threads

## Global Interpreter Lock (GIL)

Only one thread executes Python bytecode at a time.

---

## Race Conditions

Shared memory can cause conflicts when multiple threads modify the same data simultaneously.

---

## Harder Debugging

Concurrent execution can make bugs difficult to reproduce and diagnose.

---

# 15. Real-World Applications of Threads

Threads are widely used in modern software systems.

## Web Servers

Handling multiple client connections simultaneously.

---

## Web Scraping

Downloading many web pages concurrently.

---

## File Processing

Reading and processing large datasets.

---

## GUI Applications

Running background tasks without freezing the user interface.

---

## Networking Programs

Managing multiple socket connections.

---

# 16. Threads vs Multiprocessing (Quick Comparison)

| Feature       | Threads        | Processes        |
| ------------- | -------------- | ---------------- |
| Memory        | Shared         | Separate         |
| Creation Cost | Low            | High             |
| Best For      | I/O Tasks      | CPU Tasks        |
| Parallelism   | Limited by GIL | True Parallelism |

---

# 17. Summary

Threads are lightweight execution units inside a process that allow programs to perform multiple tasks concurrently.

## Key Points

* Threads run inside a single process.
* They share memory.
* Created using `threading.Thread`.
* Ideal for I/O-bound workloads.
* Limited by Python's Global Interpreter Lock (GIL).

## Common Use Cases

* Network requests
* File I/O
* Database queries
* Web scraping
* Concurrent server handling

Threads are an essential tool for building responsive, efficient, and scalable Python applications.
