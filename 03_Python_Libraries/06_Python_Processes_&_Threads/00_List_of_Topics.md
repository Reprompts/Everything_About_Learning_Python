# Python Processes and Threads

## 1. Python Process Basics

A **process** is an independent program execution with its own memory space.

Python creates processes using the `multiprocessing` module.

### Key Characteristics

* Processes run completely isolated from each other.
* Each process has its own Python interpreter.
* Each process has its own Global Interpreter Lock (GIL).
* Best suited for CPU-bound tasks such as:

  * Mathematical computations
  * Data processing
  * Scientific simulations
  * Machine learning training

### Example

```python
import multiprocessing

def worker():
    print("Process is running")

if __name__ == "__main__":
    process = multiprocessing.Process(target=worker)
    process.start()
    process.join()
```

---

## 2. Creating Processes in Python

Processes are commonly created using:

* `multiprocessing.Process`
* `multiprocessing.Pool`

### Typical Workflow

1. Define a function.
2. Create a process object.
3. Start the process using `start()`.
4. Wait for completion using `join()`.

### Example

```python
import multiprocessing

def calculate_square(num):
    print(num * num)

if __name__ == "__main__":
    p = multiprocessing.Process(
        target=calculate_square,
        args=(5,)
    )

    p.start()
    p.join()
```

### Using a Process Pool

```python
from multiprocessing import Pool

def square(num):
    return num * num

if __name__ == "__main__":
    with Pool(4) as pool:
        results = pool.map(square, [1, 2, 3, 4, 5])

    print(results)
```

Processes can execute in parallel across multiple CPU cores.

---

## 3. Inter-Process Communication (IPC)

Processes do not share memory by default. To exchange data, Python provides Inter-Process Communication (IPC) mechanisms.

### IPC Methods

* Queues
* Pipes
* Shared Memory
* Managers

### Queue Example

```python
from multiprocessing import Process, Queue

def producer(queue):
    queue.put("Hello from process")

if __name__ == "__main__":
    q = Queue()

    p = Process(target=producer, args=(q,))
    p.start()

    print(q.get())

    p.join()
```

### Pipe Example

```python
from multiprocessing import Process, Pipe

def sender(conn):
    conn.send("Message from child")
    conn.close()

parent_conn, child_conn = Pipe()

p = Process(target=sender, args=(child_conn,))
p.start()

print(parent_conn.recv())

p.join()
```

### IPC Tools

* `multiprocessing.Queue`
* `multiprocessing.Pipe`
* `multiprocessing.Manager`
* `multiprocessing.shared_memory`

---

## 4. Python Threads Basics

A **thread** is a lightweight unit of execution within a process.

Unlike processes, threads share the same memory space.

Python threads are created using:

```python
threading.Thread
```

### Suitable For

Threads work best for I/O-bound tasks such as:

* Network requests
* File operations
* Database access
* Waiting for external APIs

### Example

```python
import threading

def worker():
    print("Thread running")

thread = threading.Thread(target=worker)

thread.start()
thread.join()
```

### Benefits

* Lightweight
* Fast creation
* Shared memory
* Efficient for waiting operations

---

## 5. Global Interpreter Lock (GIL)

The **Global Interpreter Lock (GIL)** is a mechanism in CPython that ensures only one thread executes Python bytecode at a time.

### Effects of the GIL

#### Good For

* I/O-bound applications
* File operations
* Network communication

#### Poor For

* CPU-intensive computations
* Numerical processing
* Parallel mathematical calculations

### Example Scenario

```python
# CPU-intensive task
for i in range(100000000):
    pass
```

Multiple threads running this task will not fully utilize multiple CPU cores because of the GIL.

### Solution

Use the `multiprocessing` module for CPU-bound workloads.

---

## 6. Thread Synchronization

Since threads share memory, synchronization is required to prevent race conditions and inconsistent data.

### Common Synchronization Tools

* `Lock`
* `RLock`
* `Semaphore`
* `Event`
* `Condition`

### Lock Example

```python
import threading

counter = 0
lock = threading.Lock()

def increment():
    global counter

    for _ in range(100000):
        with lock:
            counter += 1

threads = [
    threading.Thread(target=increment)
    for _ in range(2)
]

for t in threads:
    t.start()

for t in threads:
    t.join()

print(counter)
```

### Use Cases

* Protect shared variables
* Coordinate thread execution
* Prevent race conditions
* Limit resource access

---

## 7. Async Programming in Python

Async programming allows many tasks to run concurrently without creating multiple threads.

The central concept is the **Event Loop**.

### Common Libraries

* `asyncio`
* `aiohttp`
* `asyncpg`

### Benefits

* Handles thousands of connections efficiently
* Low memory usage
* Excellent for network-heavy applications

### Typical Use Cases

* Web servers
* API clients
* Web scraping
* Chat applications
* Real-time systems

---

## 8. Async Functions (`async` / `await`)

Async functions are declared using:

```python
async def my_function():
    ...
```

### How It Works

* `await` pauses the current coroutine.
* Control returns to the event loop.
* Other tasks can run while waiting.

### Example

```python
import asyncio

async def fetch_data():
    await asyncio.sleep(2)
    print("Data received")

asyncio.run(fetch_data())
```

### Multiple Coroutines

```python
import asyncio

async def task(name):
    await asyncio.sleep(1)
    print(name)

async def main():
    await asyncio.gather(
        task("A"),
        task("B"),
        task("C")
    )

asyncio.run(main())
```

---

## 9. Event Loop and Task Scheduling

The **Event Loop** is the core of asynchronous programming.

### Responsibilities

* Scheduling coroutines
* Managing I/O operations
* Switching between tasks
* Handling callbacks

### Common Functions

#### `asyncio.run()`

Starts and manages the event loop.

```python
asyncio.run(main())
```

#### `asyncio.create_task()`

Schedules a coroutine to run concurrently.

```python
task = asyncio.create_task(fetch_data())
```

#### `asyncio.gather()`

Runs multiple coroutines together.

```python
await asyncio.gather(task1(), task2())
```

### Example

```python
import asyncio

async def worker(name):
    await asyncio.sleep(2)
    print(name)

async def main():
    tasks = [
        asyncio.create_task(worker("Task 1")),
        asyncio.create_task(worker("Task 2"))
    ]

    await asyncio.gather(*tasks)

asyncio.run(main())
```

---

## 10. Comparison: Processes vs Threads vs Async

| Feature        | Processes        | Threads          | Async/Await             |
| -------------- | ---------------- | ---------------- | ----------------------- |
| Memory         | Separate         | Shared           | Shared                  |
| Parallelism    | True Parallelism | Limited (GIL)    | Cooperative Concurrency |
| Best For       | CPU Tasks        | I/O Tasks        | Massive I/O Workloads   |
| Creation Cost  | High             | Medium           | Low                     |
| Scaling        | Limited          | Moderate         | Very High               |
| Communication  | IPC Required     | Shared Variables | Shared Event Loop       |
| CPU Core Usage | Multiple Cores   | Usually One Core | Usually One Core        |
| Complexity     | Medium           | Medium           | High                    |

### Real-World Examples

#### Processes

* Image processing
* Machine learning training
* Video encoding
* Scientific computing

#### Threads

* File downloads
* Database access
* API requests
* Background tasks

#### Async Programming

* Web servers
* Chat applications
* Real-time messaging
* High-volume web scraping
* Streaming services

---

# Summary

### Processes

* Separate memory spaces
* True parallel execution
* Best for CPU-bound workloads

### Threads

* Shared memory
* Lightweight execution
* Best for I/O-bound workloads

### Async/Await

* Event-loop based concurrency
* Extremely scalable
* Ideal for handling large numbers of simultaneous I/O operations

### Rule of Thumb

* **CPU-heavy work → Multiprocessing**
* **Moderate I/O concurrency → Threads**
* **Massive I/O concurrency → Async/Await**
