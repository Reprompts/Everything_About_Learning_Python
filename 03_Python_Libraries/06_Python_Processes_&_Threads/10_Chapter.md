# Python Processes & Threads: Chapter 10

# Comparison: Processes vs Threads vs Async/Await in Python

Modern applications often need to handle multiple tasks at the same time. Python provides three primary concurrency models for this purpose:

- Processes
- Threads
- Async/Await (Asynchronous Programming)

Each model is designed for different types of workloads and has different performance characteristics, memory behavior, and scalability limits.

Understanding the differences between these approaches is essential when designing efficient and scalable systems.

---

# High-Level Comparison

Below is a simplified comparison of the three models.

| Feature | Processes | Threads | Async/Await |
|----------|-----------|----------|-------------|
| Memory | Separate | Shared | Shared |
| Parallelism | True Parallel | Limited (GIL) | Cooperative |
| Best For | CPU Tasks | I/O Tasks | Massive I/O |
| Creation Cost | High | Medium | Low |
| Scaling | Limited | Moderate | Very High |

Each model operates differently at the operating system and runtime level, which determines how efficiently it performs under different workloads.

---

# 1. Processes

A process is an independent program execution environment with its own:

- Memory space
- Python interpreter
- System resources
- Global Interpreter Lock (GIL)

Processes run completely isolated from each other.

## Memory Model

Processes use separate memory spaces.

### Example

```text
Process A Memory
----------------
Variables
Objects
Stack
Heap

Process B Memory
----------------
Variables
Objects
Stack
Heap
```

Because memory is isolated:

- Processes cannot directly access each other's data
- Communication requires Inter-Process Communication (IPC)

Examples:

- Queues
- Pipes
- Shared Memory
- Sockets

---

## True Parallelism

Processes achieve true parallel execution because each process has its own Python interpreter and its own GIL.

### Example on a 4-Core CPU

```text
Core 1 → Process A
Core 2 → Process B
Core 3 → Process C
Core 4 → Process D
```

All processes can run simultaneously.

---

## Creation Cost

Creating a process is relatively expensive because the OS must:

- Allocate new memory space
- Initialize a Python interpreter
- Duplicate resources
- Set up process tables

This results in higher startup overhead.

---

## Best Use Cases

Processes are ideal for CPU-intensive workloads.

Examples:

- Image processing
- Machine learning training
- Video encoding
- Scientific simulations
- Data analysis pipelines

### Common Tools

```python
multiprocessing
ProcessPoolExecutor
```

---

# 2. Threads

A thread is a lightweight execution unit inside a process.

Multiple threads run within the same process.

---

## Shared Memory

Threads share the same memory space.

### Example

```text
Process Memory
-----------------
Shared Heap
Shared Variables
Shared Objects

Thread A
Thread B
Thread C
```

### Advantages

- Easy data sharing
- Low memory usage

### Disadvantages

- Risk of race conditions
- Requires synchronization

Common synchronization tools:

- Locks
- Semaphores
- Condition variables

---

## The GIL Limitation

In CPython, threads are limited by the Global Interpreter Lock (GIL).

The GIL ensures:

> Only one thread executes Python bytecode at a time.

### Example

```text
Thread A executing
Thread B waiting
Thread C waiting
```

Even if multiple CPU cores exist, Python threads cannot run Python code simultaneously.

---

## Thread Scheduling

Thread switching occurs through preemptive multitasking managed by the operating system.

### Mechanism

```text
Time Slicing
```

### Example Timeline

```text
Thread A → runs for 5ms
Thread B → runs for 5ms
Thread C → runs for 5ms
```

However, the GIL prevents true parallel execution of Python code.

---

## Best Use Cases

Threads are useful for I/O-bound tasks where execution frequently waits for external events.

Examples:

- File reading
- Network requests
- Database queries
- Downloading files
- API communication

During I/O operations, Python releases the GIL, allowing other threads to run.

### Common Modules

```python
threading
concurrent.futures.ThreadPoolExecutor
```

---

# 3. Async/Await (Asynchronous Programming)

Async programming uses a completely different concurrency model based on an event loop.

Instead of running multiple threads, async programs run many tasks inside a single thread.

---

## Shared Memory

Async tasks operate within the same process and thread.

### Memory Model

```text
Single Process
Single Thread
Multiple Coroutines
```

All tasks share memory.

---

## Cooperative Multitasking

Async uses cooperative scheduling.

Tasks switch only when they voluntarily yield control.

This happens at:

```python
await
```

### Example

```text
Task A starts

await network request

Task A pauses

Task B executes
```

This is different from threads where switching occurs automatically.

---

## Event Loop Architecture

Async execution is controlled by an event loop.

### Structure

```text
Event Loop
   │
   ├── Coroutine A
   ├── Coroutine B
   ├── Coroutine C
   └── Coroutine D
```

The event loop:

- Schedules tasks
- Monitors I/O events
- Resumes tasks when results are ready

---

## Creation Cost

Async tasks are extremely lightweight.

### Memory Usage

```text
Thread     → ~1 MB stack
Coroutine  → Few KB
```

This allows programs to manage thousands or even hundreds of thousands of tasks.

---

## Best Use Cases

Async programming is ideal for massive I/O concurrency.

Examples:

- Web servers
- Chat servers
- Web scraping
- Real-time applications
- API gateways
- Microservices

### Popular Libraries

```python
asyncio
aiohttp
asyncpg
FastAPI
```

---

# 4. Scalability Comparison

The scalability of each model differs significantly.

```text
Processes   → Tens to Hundreds
Threads     → Hundreds
Async Tasks → Tens of Thousands
```

### Example

A web server handling 10,000 connections:

```text
Processes → Impractical
Threads   → Heavy memory usage
Async     → Efficient
```

This is why most modern web frameworks use async architecture.

---

# 5. Performance Comparison

Each concurrency model excels in different scenarios.

---

## CPU-Bound Workloads

### Best Choice

**Processes**

Reason:

- Bypass the GIL
- Utilize multiple CPU cores

---

## I/O-Bound Workloads (Moderate Concurrency)

### Best Choice

**Threads**

Reason:

- Threads can run while others wait for I/O

---

## Massive I/O Workloads

### Best Choice

**Async/Await**

Reason:

- Handles thousands of simultaneous operations efficiently

---

# 6. Real-World Examples

## Processes

Used in systems performing heavy computation.

Examples:

- Machine learning model training
- Image rendering
- Data compression
- Video encoding

---

## Threads

Used in systems performing multiple blocking operations.

Examples:

- Downloading multiple files
- Interacting with APIs
- Database operations

---

## Async

Used in highly scalable network applications.

Examples:

- Web servers
- Chat servers
- Streaming platforms
- Large-scale web scrapers

### Framework Examples

- FastAPI
- Node.js
- Nginx

---

# 7. Choosing the Right Model

A simple rule for selecting the correct approach:

```text
CPU-heavy work          → Processes
Blocking I/O            → Threads
Massive network I/O     → Async
```

Sometimes real systems combine all three.

### Example Architecture

```text
Async Web Server
        │
        ▼
Thread Pool
(for blocking operations)
        │
        ▼
Process Pool
(for CPU-heavy tasks)
```

This hybrid architecture is common in modern backend systems.

---

# 8. Final Summary

Python provides three major concurrency models.

---

## Processes

- Separate memory space
- True parallelism
- High creation cost
- Ideal for CPU-bound tasks

---

## Threads

- Shared memory
- Limited by the GIL
- Medium overhead
- Ideal for I/O-bound workloads

---

## Async/Await

- Single-threaded concurrency
- Cooperative scheduling
- Extremely scalable
- Ideal for massive network I/O

---

## Quick Decision Guide

| Scenario | Recommended Approach |
|-----------|---------------------|
| Machine Learning Training | Processes |
| Video Encoding | Processes |
| Scientific Simulations | Processes |
| API Requests | Threads |
| Database Queries | Threads |
| File Downloads | Threads |
| High-Traffic Web APIs | Async |
| Chat Servers | Async |
| Real-Time Notifications | Async |
| Large-Scale Web Scraping | Async |

---

Understanding these models allows developers to design high-performance Python systems that use system resources efficiently while achieving maximum scalability.

The key takeaway is simple:

```text
CPU Work        → Processes
Blocking I/O    → Threads
Massive I/O     → Async/Await
```

Choosing the correct concurrency model is one of the most important architectural decisions when building scalable Python applications.
