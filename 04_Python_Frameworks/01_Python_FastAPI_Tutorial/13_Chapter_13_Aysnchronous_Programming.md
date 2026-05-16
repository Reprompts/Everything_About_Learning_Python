# FastAPI Hands-On Tutorial
## Chapter 13: Asynchronous Programming

# Asynchronous Programming

Asynchronous programming is a core concept in FastAPI that enables high performance and scalability. It allows the application to handle multiple requests concurrently without blocking execution. Instead of waiting for one task to complete before starting another, asynchronous code allows the system to switch between tasks efficiently, especially during I/O operations such as database queries, file access, or network calls.

At a system level, FastAPI is built on ASGI and uses Python's `asyncio` event loop. This means that instead of creating a new thread for each request, a single thread can manage multiple requests by suspending and resuming tasks as needed.

---

# `async` and `await`

The `async` and `await` keywords are used to define and control asynchronous functions.

## Basic Example

```python id="a1f9x2"
async def fetch_data():
    return "data"
```

An async function does not execute immediately. Instead, it returns a coroutine object that must be awaited.

```python id="k7m2q8"
result = await fetch_data()
```

## Internal Behavior

Internally:

1. `async` defines a coroutine function
2. When called, it returns a coroutine object
3. `await` pauses execution of the current function
4. Control is returned to the event loop
5. The event loop can execute other tasks while waiting

This is fundamentally different from synchronous execution, where each function blocks until completion.

---

# Async Endpoints

FastAPI allows you to define endpoints as asynchronous functions using `async def`.

## Example

```python id="z8r3w1"
from fastapi import FastAPI

app = FastAPI()

@app.get("/data")
async def get_data():
    return {"message": "async response"}
```

## Internal Workflow

When an async endpoint is used:

1. FastAPI schedules it in the event loop
2. If the function encounters an `await`, it yields control
3. Other requests can be processed during that time

---

# Synchronous vs Asynchronous Endpoints

## Synchronous Endpoint

```python id="m5u1b4"
@app.get("/sync")
def sync_route():
    return {"message": "sync"}
```

## Asynchronous Endpoint

```python id="n9v7c6"
@app.get("/async")
async def async_route():
    return {"message": "async"}
```

## System-Level Difference

| Synchronous                       | Asynchronous                          |
| --------------------------------- | ------------------------------------- |
| Blocks the worker thread          | Allows cooperative multitasking       |
| Waits until execution finishes    | Yields control during waiting         |
| Less scalable for I/O-heavy tasks | More scalable for concurrent requests |

FastAPI automatically handles both types, but async endpoints provide better scalability when dealing with I/O-bound tasks.

---

# Performance Considerations

Asynchronous programming improves performance, but only in specific scenarios.

## Key Principle

* Async is beneficial for **I/O-bound operations**
* Async does **not** improve **CPU-bound tasks**

---

# I/O-Bound Examples

Examples where async is beneficial:

* Database queries
* API calls
* File reading/writing
* Network communication

---

# CPU-Bound Examples

Examples where async is not beneficial:

* Heavy computations
* Data processing
* Complex algorithms

## Incorrect Usage

```python id="q2p8l0"
@app.get("/compute")
async def compute():
    result = 0

    for i in range(10**7):
        result += i

    return {"result": result}
```

This blocks the event loop and reduces performance.

## Correct Approach

* Use async for I/O operations
* Use multiprocessing or background workers for CPU-heavy tasks

---

# Concurrency vs Parallelism

## Concurrency

* Handling multiple tasks efficiently
* Tasks share execution time
* Achieved with async/event loop

## Parallelism

* Running multiple tasks simultaneously
* Uses multiple CPU cores or processes

### Important Note

Async provides **concurrency**, not true parallel execution.

---

# Async Database Calls

To fully benefit from async programming, database operations must also be asynchronous.

Using synchronous database calls inside async endpoints can block the event loop and reduce performance.

---

# Synchronous Database Usage (Not Ideal)

```python id="d4x7p1"
@app.get("/users")
async def get_users(db = Depends(get_db)):
    return db.query(User).all()
```

Here, the database call is blocking.

---

# Async SQLAlchemy Setup

```python id="r8y2t5"
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "postgresql+asyncpg://user:password@localhost/db"

engine = create_async_engine(DATABASE_URL)

AsyncSessionLocal = sessionmaker(
    engine,
    class_=AsyncSession
)

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session
```

---

# Async Query Example

```python id="w3n6k9"
@app.get("/users")
async def get_users(db: AsyncSession = Depends(get_db)):
    result = await db.execute("SELECT * FROM users")
    return result.fetchall()
```

## Internal Workflow

1. Query is sent asynchronously
2. While waiting for the database response, the event loop handles other requests
3. Once data is ready, execution resumes

This significantly improves throughput in high-concurrency environments.

---

# Blocking vs Non-Blocking Operations

Understanding blocking behavior is essential for correct async usage.

## Blocking Operation

* Stops execution until task completes
* Prevents handling other requests

## Non-Blocking Operation

* Yields control to the event loop
* Allows other tasks to execute

---

# Blocking Example

```python id="j5s8v0"
import time

@app.get("/blocking")
async def blocking():
    time.sleep(5)
    return {"message": "done"}
```

This blocks the event loop.

---

# Non-Blocking Example

```python id="u7c2m4"
import asyncio

@app.get("/non-blocking")
async def non_blocking():
    await asyncio.sleep(5)
    return {"message": "done"}
```

## What Happens Internally

1. `await asyncio.sleep()` yields control
2. The event loop processes other requests
3. Execution resumes after the sleep completes

---

# Mixing Sync and Async Code

FastAPI allows mixing synchronous and asynchronous functions, but care must be taken.

## Internal Behavior

* Sync functions run in a thread pool
* Async functions run directly in the event loop

## Example

```python id="h1p4r7"
@app.get("/mixed")
def sync_function():
    return {"message": "sync"}
```

FastAPI executes this in a thread pool to avoid blocking the event loop.

---

# Potential Problems with Excessive Sync Usage

Excessive use of sync functions can lead to:

* Increased thread usage
* Reduced scalability
* Higher memory consumption

## Best Practice

* Use async for I/O-heavy endpoints
* Use sync only when necessary

---

# End-to-End Async Flow

When an async request is processed:

1. Client sends request
2. Uvicorn receives request and passes it to the event loop
3. FastAPI schedules the async endpoint
4. Execution begins
5. When `await` is encountered, task is paused
6. Event loop switches to another task
7. Once awaited operation completes, task resumes
8. Response is generated and returned

This cooperative multitasking model allows efficient handling of thousands of concurrent requests.

---

# Key Takeaways

* Asynchronous programming in FastAPI enables efficient handling of concurrent requests
* FastAPI uses ASGI and Python's `asyncio` event loop
* `async` and `await` allow functions to pause and resume without blocking the system
* Async endpoints are best suited for I/O-bound workloads
* Blocking operations inside async code reduce performance
* Async database operations are essential for high scalability
* Async provides concurrency, not true parallelism
* Understanding blocking vs non-blocking operations is critical for building efficient APIs
