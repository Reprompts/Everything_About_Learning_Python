# Python Processes & Threads: Chapter 1

When a Python program runs, it does not simply execute code line by line inside a void. Instead, it runs inside an operating system structure called a **process**.

Understanding processes is extremely important for building high-performance, scalable, and parallel applications, especially when dealing with CPU-heavy computations, data processing pipelines, and scientific workloads.

This article explains Python processes in deep detail, covering how they work internally, why they are important, how Python creates them, and when they should be used.

---

# 1. What Is a Process?

A **process** is an independent program execution environment created by the operating system.

When you run a Python script like:

```bash
python program.py
```

the operating system creates a new process that runs the Python interpreter.

That process contains:

* Its own memory space
* Its own CPU execution state
* Its own stack and heap
* Its own resources

Each process runs independently of other processes.

## Conceptual View

```text
Operating System
│
├── Process 1 (Python Program A)
│      ├── Memory Space
│      ├── Python Interpreter
│      └── Threads
│
├── Process 2 (Python Program B)
│      ├── Memory Space
│      ├── Python Interpreter
│      └── Threads
│
└── Process 3 (Browser)
       ├── Memory Space
       └── Threads
```

Every process behaves like a separate computer program even though they share the same machine.

---

# 2. Key Characteristics of a Process

A process has several important properties.

## 2.1 Independent Memory Space

Each process has its own memory.

```text
Process A Memory
----------------
variable_x = 10

Process B Memory
----------------
variable_x = 10 (different copy)
```

Changing a variable in one process does not affect another process.

### Example

```text
Process A modifies x → 20
Process B still sees x → 10
```

This isolation makes processes safe and stable.

---

## 2.2 Process Isolation

Processes are fully isolated from each other.

They cannot directly access:

* Another process's variables
* Another process's memory
* Another process's internal state

To communicate, they must use Inter-Process Communication (IPC) mechanisms such as:

* Pipes
* Queues
* Shared memory
* Sockets

---

## 2.3 Separate Resource Allocation

Each process gets:

* CPU time
* Memory allocation
* File handles
* Network connections

The operating system manages these resources using a process scheduler.

---

## 2.4 Process Identification

Each process has a unique identifier called a **PID (Process ID)**.

### Example

```text
PID 1012 → Python Program
PID 1023 → Browser
PID 1045 → Text Editor
```

You can view the current process ID in Python:

```python
import os

print(os.getpid())
```

---

# 3. How Python Programs Run as Processes

When a Python program starts, the following happens internally.

## Step 1: OS Creates a Process

You run:

```bash
python script.py
```

The operating system:

1. Creates a new process
2. Allocates memory
3. Loads the Python interpreter

---

## Step 2: Python Interpreter Starts

Inside the process:

```text
Process
 └── Python Interpreter
        └── script.py execution
```

The interpreter:

* Loads modules
* Initializes runtime
* Executes the script

---

## Step 3: Program Execution

Python executes code in the following sequence:

```text
Top-level code
      ↓
Functions
      ↓
Objects created
      ↓
Program runs
```

All of this happens inside one process unless additional processes are created.

---

# 4. Creating Multiple Processes in Python

Python allows programs to create additional processes using the `multiprocessing` module.

This module enables true parallel execution.

## Basic Process Creation

```python
from multiprocessing import Process

def worker():
    print("Worker process running")

p = Process(target=worker)

p.start()
p.join()
```

### Execution Flow

```text
Main Process
     │
     ├── Worker Process
     │       └── worker() runs
     │
     └── Main waits until worker finishes
```

### Output

```text
Worker process running
```

---

# 5. Multiprocessing Module

The `multiprocessing` module allows Python programs to run multiple processes simultaneously.

It provides tools for:

* Process creation
* Process management
* Inter-process communication
* Shared memory
* Synchronization

## Main Components

```text
multiprocessing
│
├── Process
├── Pool
├── Queue
├── Pipe
├── Lock
├── Event
└── Value / Array (shared memory)
```

---

# 6. Each Process Has Its Own Python Interpreter

This is one of the most important concepts.

Each process contains a separate Python interpreter instance.

```text
Process A
 └── Python Interpreter A

Process B
 └── Python Interpreter B
```

This means:

* Separate memory
* Separate objects
* Separate execution

### Example

```text
Process A variable → x = 5

Process B variable → x = 5
(independent copy)
```

---

# 7. Global Interpreter Lock (GIL) and Processes

Python uses something called the **Global Interpreter Lock (GIL)**.

The GIL allows only one thread to execute Python bytecode at a time inside a process.

## Thread Limitation

Inside one process:

```text
Process
 ├── Thread 1
 ├── Thread 2
 └── Thread 3
```

Only one thread executes Python code at a given moment.

This limits performance for CPU-intensive tasks.

---

## Processes Bypass the GIL

Since each process has:

* Its own interpreter
* Its own GIL

multiple processes can execute simultaneously.

```text
CPU Core 1 → Process A → Python Interpreter → GIL A

CPU Core 2 → Process B → Python Interpreter → GIL B

CPU Core 3 → Process C → Python Interpreter → GIL C
```

This enables true parallelism.

---

# 8. Processes vs Threads

Understanding the difference is essential.

| Feature       | Process                  | Thread  |
| ------------- | ------------------------ | ------- |
| Memory        | Separate                 | Shared  |
| GIL           | Separate GIL per Process | One GIL |
| Communication | Slower                   | Faster  |
| Safety        | High                     | Lower   |
| Creation Cost | Higher                   | Lower   |

## Visualization

### Processes

```text
Process A
   ├── Memory
   └── Interpreter

Process B
   ├── Memory
   └── Interpreter
```

### Threads

```text
Single Process
   ├── Thread 1
   ├── Thread 2
   └── Thread 3

All share memory
```

---

# 9. Why Processes Are Good for CPU-Bound Tasks

A CPU-bound task is one where the program spends most of its time performing calculations.

### Examples

* Matrix multiplication
* Image processing
* Data compression
* Machine learning training
* Scientific simulations

Threads struggle with CPU-bound workloads because of the GIL.

Processes solve this problem by allowing multiple CPU cores to work simultaneously.

## Example CPU Task

Calculating prime numbers.

### Without Multiprocessing

```text
1 CPU Core Used
↓
Slow Execution
```

### With Multiprocessing

```text
Core 1 → Part 1
Core 2 → Part 2
Core 3 → Part 3
Core 4 → Part 4
```

Execution becomes significantly faster.

---

# 10. Example: Parallel Computation

```python
from multiprocessing import Process

def square(n):
    print(n * n)

numbers = [1, 2, 3, 4]

processes = []

for num in numbers:
    p = Process(target=square, args=(num,))
    processes.append(p)
    p.start()

for p in processes:
    p.join()
```

### Execution

```text
Process 1 → square(1)
Process 2 → square(2)
Process 3 → square(3)
Process 4 → square(4)
```

### Output

```text
1
4
9
16
```

The processes may execute simultaneously depending on the available CPU cores.

---

# 11. Process Lifecycle

A process goes through several stages.

```text
Created
   ↓
Ready
   ↓
Running
   ↓
Waiting (optional)
   ↓
Terminated
```

## Python Process Lifecycle

```text
Process() object created
       ↓
start()
       ↓
Process begins execution
       ↓
join() waits for completion
       ↓
Process exits
```

---

# 12. Process Communication (IPC)

Because processes are isolated, they must use **Inter-Process Communication (IPC)** to exchange data.

Python supports:

## Queue

```text
Process A → Queue → Process B
```

## Pipe

```text
Process A ↔ Pipe ↔ Process B
```

## Shared Memory

```text
Shared Variable
Accessible to Multiple Processes
```

### Example

```python
from multiprocessing import Queue
```

---

# 13. Process Pools

For many tasks, Python provides a **Process Pool**.

Instead of manually creating processes, a pool manages them automatically.

## Example

```python
from multiprocessing import Pool

def square(n):
    return n * n

with Pool(4) as p:
    result = p.map(square, [1, 2, 3, 4])

print(result)
```

### Output

```text
[1, 4, 9, 16]
```

The pool distributes tasks across multiple worker processes.

---

# 14. Real-World Use Cases of Python Processes

## Data Science

* Parallel data processing
* Large dataset transformations

## Machine Learning

* Model training
* Data preprocessing

## Scientific Computing

* Simulations
* Numerical calculations

## Web Servers

Some web servers use multiple processes:

* Gunicorn
* uWSGI

Each worker process handles requests independently.

---

# 15. Advantages of Using Processes

## True Parallel Execution

Processes can utilize multiple CPU cores simultaneously.

---

## Safe Memory Isolation

No accidental memory corruption between independent tasks.

---

## Fault Isolation

If one process crashes:

```text
Process A crashes
Process B continues running
```

The overall system remains stable.

---

# 16. Limitations of Processes

Processes also have drawbacks.

## Higher Memory Usage

Each process maintains its own memory space.

---

## Slower Creation

Creating a process is slower than creating a thread.

---

## Communication Overhead

Data must often be serialized and transferred between processes.

---

# 17. Summary

Python processes are independent execution environments created by the operating system.

## Key Takeaways

* A process runs a separate Python interpreter.
* Each process has its own memory space.
* Processes are isolated and safe.
* Communication requires IPC mechanisms.
* Multiprocessing enables true parallelism.
* Processes are best suited for CPU-bound workloads.

In modern Python applications, processes are a core tool for scaling computational performance, especially in data science, machine learning, scientific computing, and high-performance systems.
