# Python Processes & Threads: Chapter 3 

# Inter-Process Communication (IPC) in Python

When a Python program creates multiple processes using the `multiprocessing` module, each process runs independently with its own memory space.

This isolation improves safety and stability, but it also introduces a challenge:

> How do processes exchange data if they cannot access each other's memory?

The solution is **Inter-Process Communication (IPC)**.

IPC mechanisms allow processes to send, receive, and share data while remaining isolated.

Python's `multiprocessing` module provides several IPC tools:

* Queues
* Pipes
* Shared Memory
* Managers

Each method has different performance characteristics, complexity levels, and use cases.

---

# 1. Why Inter-Process Communication Is Needed

Processes do not share memory.

### Example

```text
Process A
Memory:
x = 10

Process B
Memory:
x = 0
```

If Process A modifies `x`, Process B will not see the change.

Therefore, processes must communicate using special operating-system-supported communication channels.

### Typical Communication Scenarios

* Sending tasks to worker processes
* Returning results to the parent process
* Sharing state across processes
* Coordinating parallel computations

---

# 2. IPC Mechanisms in Python Multiprocessing

Python provides four primary communication mechanisms.

```text
Inter-Process Communication
│
├── Queue
│
├── Pipe
│
├── Shared Memory
│
└── Manager
```

Each mechanism solves communication in a different way.

---

# 3. multiprocessing.Queue

A **Queue** is the most commonly used IPC mechanism.

It behaves like a FIFO (**First-In, First-Out**) message channel between processes.

## Concept

```text
Process A
    │
    ▼
 put(data)
    │
    ▼
   Queue
    │
    ▼
 get(data)
    │
    ▼
Process B
```

Processes place messages into the queue, and other processes retrieve them.

Internally, queues use:

* Pipes
* Locks
* Serialization (Pickling)

to safely transfer data between processes.

---

## Queue Workflow

```text
Producer Process
        │
        ▼
   queue.put(data)
        │
        ▼
      Queue
        │
        ▼
   queue.get()
        │
        ▼
Consumer Process
```

---

## Example: Using multiprocessing.Queue

```python
from multiprocessing import Process, Queue

def worker(q):
    q.put("Hello from worker")

if __name__ == "__main__":

    q = Queue()

    p = Process(
        target=worker,
        args=(q,)
    )

    p.start()

    message = q.get()

    p.join()

    print(message)
```

### Output

```text
Hello from worker
```

### Explanation

1. Worker process sends a message.
2. Message is placed into the queue.
3. Main process retrieves the message.

---

## Queue Features

Queues support multiple producers and consumers.

```text
Producer 1
Producer 2
Producer 3
     │
     ▼
    Queue
     │
     ▼
Consumer 1
Consumer 2
```

This pattern is widely used in parallel task-processing systems.

---

## Queue Advantages

* Easy to use
* Safe for multiple processes
* Automatic synchronization
* Excellent for task distribution

---

## Queue Limitations

* Data must be serialized (pickled)
* Large objects can slow communication

---

# 4. multiprocessing.Pipe

A **Pipe** provides a direct communication channel between two processes.

Unlike a queue, a pipe is generally designed for communication between exactly two processes.

---

## Pipe Concept

```text
Process A
     ↕
    Pipe
     ↕
Process B
```

Data can move in both directions if the pipe is duplex.

---

## Pipe Creation

```python
from multiprocessing import Pipe

parent_conn, child_conn = Pipe()
```

Two endpoints are created:

* `parent_conn`
* `child_conn`

Each endpoint can send and receive messages.

---

## Example: Pipe Communication

```python
from multiprocessing import Process, Pipe

def worker(conn):
    conn.send("Message from worker")
    conn.close()

if __name__ == "__main__":

    parent_conn, child_conn = Pipe()

    p = Process(
        target=worker,
        args=(child_conn,)
    )

    p.start()

    message = parent_conn.recv()

    p.join()

    print(message)
```

### Output

```text
Message from worker
```

---

## Pipe Communication Flow

```text
Worker Process
     │
     ▼
 conn.send(data)
     │
     ▼
     Pipe
     │
     ▼
 parent_conn.recv()
     │
     ▼
 Main Process
```

---

## Pipe Advantages

* Faster than queues
* Simple direct communication
* Low overhead

---

## Pipe Limitations

* Usually limited to two processes
* More difficult to scale to many producers and consumers

---

# 5. Shared Memory

Processes normally have separate memory spaces, but Python allows processes to share memory explicitly.

Shared memory enables multiple processes to access the same memory region.

---

## Shared Memory Concept

```text
       Shared Memory
             │
     ┌───────┴───────┐
     │               │
Process A       Process B
```

Both processes can read and write the same data.

---

## Shared Memory Objects

Python provides:

* `multiprocessing.Value`
* `multiprocessing.Array`

These objects allow processes to share primitive data types.

---

## Example: Shared Value

```python
from multiprocessing import Process, Value

def worker(num):
    num.value += 1

if __name__ == "__main__":

    number = Value('i', 0)

    p = Process(
        target=worker,
        args=(number,)
    )

    p.start()
    p.join()

    print(number.value)
```

### Output

```text
1
```

### Explanation

The worker process directly modifies the shared variable.

---

## Shared Arrays

```python
from multiprocessing import Array

arr = Array('i', [1, 2, 3])
```

Multiple processes can access and modify the same array.

---

## Shared Memory Advantages

* Extremely fast communication
* No serialization overhead
* Efficient for large datasets

---

## Shared Memory Limitations

* Requires careful synchronization
* Race conditions can occur
* More complex than queues and pipes

---

# 6. multiprocessing.Manager

A **Manager** provides a way to share complex Python objects between processes.

Managers create a dedicated server process that stores shared objects and allows other processes to access them.

---

## Manager Concept

```text
      Manager Server
             │
     ┌───────┴───────┐
     │               │
Process A       Process B
```

Both processes communicate with the manager process to access shared data.

---

## Objects Supported by Manager

Managers can share:

* Lists
* Dictionaries
* Sets
* Namespaces
* Locks
* Queues

---

## Example: Manager List

```python
from multiprocessing import Process, Manager

def worker(shared_list):
    shared_list.append("data")

if __name__ == "__main__":

    with Manager() as manager:

        shared_list = manager.list()

        p = Process(
            target=worker,
            args=(shared_list,)
        )

        p.start()
        p.join()

        print(shared_list)
```

### Output

```text
['data']
```

---

## Manager Advantages

* Supports complex Python objects
* Easy to use
* Familiar Python data structures

---

## Manager Limitations

* Slower than shared memory
* Uses a dedicated server process internally
* Additional communication overhead

---

# 7. IPC Method Comparison

| Method        | Communication Style  | Speed     | Best For                      |
| ------------- | -------------------- | --------- | ----------------------------- |
| Queue         | Message Passing      | Medium    | Task Distribution             |
| Pipe          | Direct Communication | Fast      | Two-Process Communication     |
| Shared Memory | Shared Data          | Very Fast | High-Performance Data Sharing |
| Manager       | Shared Objects       | Slower    | Complex Object Sharing        |

---

# 8. Typical IPC Architecture

A common multiprocessing architecture looks like this:

```text
Main Process
      │
      ▼
   Task Queue
      │
      ▼
Worker Processes
      │
      ▼
 Result Queue
      │
      ▼
Main Process Collects Results
```

This architecture is widely used in:

* Data pipelines
* Distributed task systems
* Machine learning preprocessing
* Parallel simulations

---

# 9. Real-World Use Cases

IPC is widely used in production systems.

## Parallel Data Processing

```text
Dataset Split
      ↓
Worker Processes
      ↓
Results Merged
```

---

## Web Servers

Worker processes communicate with a master process to handle requests efficiently.

---

## Machine Learning

IPC enables:

* Parallel feature extraction
* Data preprocessing
* Distributed model training

---

## Scientific Computing

Processes exchange intermediate results during simulations and large-scale numerical computations.

---

# 10. Summary

**Inter-Process Communication (IPC)** allows isolated processes to exchange data safely.

Python provides several IPC mechanisms through the `multiprocessing` module.

## Key Tools

### Queue

* Safe message passing
* Supports multiple producers and consumers
* Ideal for task distribution

### Pipe

* Direct communication channel
* Fast and lightweight
* Best for two-process communication

### Shared Memory

* Fastest communication mechanism
* No serialization overhead
* Suitable for performance-critical applications

### Manager

* Shares complex Python objects
* Easy to use
* Useful when working with lists, dictionaries, and other shared structures

---

Each IPC mechanism addresses different communication requirements.

Together, they enable Python programs to build scalable parallel systems that power modern applications in:

* Data Science
* Machine Learning
* Distributed Computing
* Scientific Computing
* High-Performance Computing (HPC)
