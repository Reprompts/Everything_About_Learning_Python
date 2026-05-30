# Python Processes & Threads: Chapter 7

# Async Programming in Python

Modern applications often need to handle thousands of simultaneous operations, especially in areas like web services, APIs, and data processing pipelines. Traditional approaches like threads and processes can become inefficient or resource-heavy when managing large numbers of tasks.

Python provides **asynchronous programming (async programming)** to solve this problem. It allows programs to handle many tasks concurrently using a single thread, making it extremely efficient for I/O-heavy workloads such as network communication and database queries.

This article explains async programming in Python, the role of the event loop, commonly used modules like `asyncio`, `aiohttp`, and `asyncpg`, and why async programming is powerful for high-concurrency systems.

---

# 1. What Is Asynchronous Programming?

Asynchronous programming is a programming model that allows a program to start a task and continue doing other work while waiting for that task to complete.

Instead of blocking the program while waiting for slow operations (such as network responses), the program suspends the task and switches to another one.

## Example Problem

Imagine a program that downloads 100 web pages.

A synchronous program would do this:

```text
Download page 1
(wait)

Download page 2
(wait)

Download page 3
(wait)
```

This wastes time because the program sits idle while waiting for network responses.

## Async Solution

Async programming allows this:

```text
Start download page 1
Start download page 2
Start download page 3
Start download page 4
...
Handle results as they complete
```

All tasks are in progress at the same time, even though only one thread is used.

---

# 2. Concurrency Without Multiple Threads

Async programming achieves concurrency without creating multiple threads.

Instead of threads, it uses:

* Coroutines
* Event Loop
* Non-blocking I/O

## Conceptual View

```text
Single Thread
      │
      ▼
  Event Loop
      │
      ├── Task A
      ├── Task B
      ├── Task C
      └── Task D
```

When one task waits for I/O, the event loop switches to another task.

This allows the program to handle thousands of operations efficiently.

---

# 3. The Event Loop (Core Concept)

The **event loop** is the heart of asynchronous programming.

It acts as a task scheduler, deciding which coroutine should run at any moment.

## Event Loop Responsibilities

* Start and manage asynchronous tasks
* Pause tasks when they wait for I/O
* Resume tasks when results are available
* Coordinate task switching

## Event Loop Workflow

```text
Event Loop
     │
     ▼
Execute Task A
     │
     ▼
Task A waits for network response
     │
     ▼
Switch to Task B
     │
     ▼
Task B waits for database query
     │
     ▼
Switch to Task C
```

Once the network response arrives:

```text
Event Loop resumes Task A
```

This switching happens very quickly, creating the illusion of parallel execution.

---

# 4. Coroutines in Python

Async functions in Python are called **coroutines**.

They are defined using the `async` keyword.

## Example

```python
async def fetch_data():
    print("Fetching data")
```

Coroutines do not run immediately.

They must be scheduled in the event loop.

## The `await` Keyword

Inside a coroutine, the `await` keyword pauses execution.

### Example

```python
async def fetch_data():
    data = await network_request()
```

When `await` is reached:

```text
Coroutine pauses
        │
        ▼
Event loop runs other tasks
        │
        ▼
Result arrives
        │
        ▼
Coroutine resumes
```

---

# 5. The asyncio Module

Python provides the **asyncio** module for asynchronous programming.

It is the core framework for managing:

* Event loops
* Coroutines
* Asynchronous tasks
* Non-blocking I/O

## asyncio Architecture

```text
asyncio
│
├── Event Loop
├── Coroutines
├── Tasks
├── Futures
└── Asynchronous I/O Operations
```

## Basic Async Example

```python
import asyncio

async def hello():
    print("Hello")
    await asyncio.sleep(1)
    print("World")

asyncio.run(hello())
```

## Execution Flow

```text
Event Loop starts
      │
      ▼
Run hello()
      │
      ▼
Print "Hello"
      │
      ▼
Pause for 1 second
      │
      ▼
Resume coroutine
      │
      ▼
Print "World"
```

---

# 6. Running Multiple Async Tasks

Async programming becomes powerful when handling multiple concurrent tasks.

## Example

```python
import asyncio

async def task(name):
    print(name, "started")
    await asyncio.sleep(2)
    print(name, "finished")

async def main():
    await asyncio.gather(
        task("Task1"),
        task("Task2"),
        task("Task3")
    )

asyncio.run(main())
```

## Execution

Instead of:

```text
Task1 → wait
Task2 → wait
Task3 → wait
```

Async runs them concurrently:

```text
Task1 started
Task2 started
Task3 started

(wait)

Task1 finished
Task2 finished
Task3 finished
```

---

# 7. aiohttp — Async HTTP Client

`aiohttp` is a popular library for asynchronous HTTP requests.

It allows programs to make many network requests concurrently.

## Example: Async Web Request

```python
import aiohttp
import asyncio

async def fetch(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def main():
    html = await fetch("https://example.com")
    print(html[:100])

asyncio.run(main())
```

## Why aiohttp Is Powerful

Instead of waiting for each request sequentially:

```text
Request 1 → wait
Request 2 → wait
Request 3 → wait
```

Async can perform many requests simultaneously:

```text
Request 1
Request 2
Request 3
Request 4
Request 5

All waiting concurrently
```

### Common Uses

* Web scraping
* API clients
* Microservices
* Data collection systems

---

# 8. asyncpg — Async PostgreSQL Client

`asyncpg` is a high-performance asynchronous PostgreSQL driver.

It allows Python applications to perform database queries without blocking the event loop.

## Example

```python
import asyncpg
import asyncio

async def fetch_data():
    conn = await asyncpg.connect(
        user='user',
        password='pass',
        database='db',
        host='localhost'
    )

    rows = await conn.fetch('SELECT * FROM users')

    await conn.close()

    print(rows)

asyncio.run(fetch_data())
```

## Benefits

Async database drivers allow applications to:

* Handle thousands of database queries concurrently
* Avoid blocking the application while waiting for database responses
* Improve scalability of backend systems

### Common Use Cases

* High-traffic APIs
* Real-time applications
* Large-scale backend services

---

# 9. Async Programming vs Threads

```text
+-------------------+-----------------------+------------------+
| Feature           | Async                 | Threads          |
+-------------------+-----------------------+------------------+
| Execution Model   | Event Loop            | OS Threads       |
| Memory Usage      | Low                   | Higher           |
| Context Switching | Very Fast             | Slower           |
| Best For          | I/O Tasks             | Mixed Workloads  |
| Parallel CPU      | No                    | Limited by GIL   |
+-------------------+-----------------------+------------------+
```

Async excels at handling large numbers of waiting tasks efficiently.

---

# 10. Real-World Applications

Async programming is widely used in modern software systems.

## Web Servers

Frameworks such as:

* FastAPI
* Starlette
* Sanic

use async programming to handle thousands of requests concurrently.

---

## Web Scraping

Download hundreds or thousands of pages simultaneously.

---

## Microservices

Async APIs communicate with multiple services concurrently.

---

## Real-Time Applications

Examples include:

* Chat systems
* Live notifications
* Streaming systems
* Multiplayer applications

---

# 11. Benefits of Async Programming

## Massive Concurrency

Async systems can handle thousands of simultaneous tasks efficiently.

---

## Efficient Resource Usage

Uses a single-threaded event loop instead of creating many threads.

---

## Fast Context Switching

Switching between coroutines is extremely lightweight.

---

## Scalable Network Applications

Perfect for:

* Web servers
* API clients
* Data pipelines
* Distributed systems

---

# 12. Limitations of Async Programming

## Complex Programming Model

Async code can be more difficult to understand and debug.

---

## Not Suitable for CPU-Heavy Work

Async programming does not improve computationally intensive tasks.

Examples:

* Machine learning training
* Image processing
* Scientific simulations

For these workloads, multiprocessing is usually a better choice.

---

## Requires Async-Compatible Libraries

Blocking libraries interrupt the event loop and reduce async performance.

To gain the full benefits of async programming, libraries must support asynchronous operations.

---

# Summary

Async programming in Python enables highly efficient concurrency without relying on multiple threads.

It is built around three core concepts:

* Coroutines
* Event Loops
* Non-blocking I/O

Key libraries include:

* **asyncio** — Core asynchronous framework
* **aiohttp** — Asynchronous HTTP client/server library
* **asyncpg** — High-performance asynchronous PostgreSQL driver

Async programming is especially powerful for applications that handle large numbers of network operations, including:

* Web APIs
* Web scraping systems
* Distributed services
* Real-time applications
* Cloud-native backends

By leveraging asynchronous programming, Python applications can scale to thousands of concurrent operations while using minimal system resources, making async one of the most important tools for modern high-performance software development.
