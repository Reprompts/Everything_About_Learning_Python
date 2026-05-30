# Python Processes & Threads: Chapter 2

Modern computers contain multiple CPU cores, allowing several tasks to run simultaneously. However, a normal Python program runs inside a single process, which means it usually uses only one CPU core.

To fully utilize modern hardware, Python provides the `multiprocessing` module, which allows programs to create multiple processes that run in parallel.

This article explains how processes are created in Python, the typical workflow, and the two primary mechanisms used:

* `multiprocessing.Process`
* `multiprocessing.Pool`

---

# 1. Why Create Multiple Processes?

A Python program normally runs like this:

```text
Single Process
      │
      └── Python Interpreter
              │
              └── Program Execution
```

Even if your machine has:

```text
CPU Core 1
CPU Core 2
CPU Core 3
CPU Core 4
```

a single process will typically use only one core.

By creating multiple processes, Python can distribute work across cores.

```text
Process 1 → CPU Core 1
Process 2 → CPU Core 2
Process 3 → CPU Core 3
Process 4 → CPU Core 4
```

This allows true parallel execution, which significantly improves performance for CPU-bound tasks.

### Examples

* Data processing
* Image rendering
* Scientific simulations
* Machine learning training
* Cryptography calculations

---

# 2. The multiprocessing Module

Python provides the `multiprocessing` module to manage processes.

It offers tools for:

```text
multiprocessing
│
├── Process        (manual process creation)
├── Pool           (process pool management)
├── Queue          (process communication)
├── Pipe           (process communication)
├── Lock           (synchronization)
├── Value / Array  (shared memory)
```

The two most commonly used components are:

* `multiprocessing.Process`
* `multiprocessing.Pool`

---

# 3. Typical Workflow for Creating a Process

Creating a process in Python usually follows four steps.

---

## Step 1 — Define a Function

The function contains the work that the process will perform.

### Example

```python
def task():
    print("Process is running")
```

---

## Step 2 — Create a Process Object

You create a process using:

```python
multiprocessing.Process
```

### Example

```python
from multiprocessing import Process

p = Process(target=task)
```

### Parameters

| Parameter | Meaning                             |
| --------- | ----------------------------------- |
| `target`  | Function the process should execute |
| `args`    | Arguments passed to the function    |

---

## Step 3 — Start the Process

The process begins execution using:

```python
p.start()
```

### What Happens Internally?

```text
Main Process
      │
      └── Creates Child Process
              │
              └── Executes Target Function
```

---

## Step 4 — Wait for Completion

The main program can wait for the process to finish using:

```python
p.join()
```

`join()` ensures that:

* The main program waits until the child process finishes.
* Execution continues only after the child process exits.

---

# 4. Complete Example

Here is the complete workflow:

```python
from multiprocessing import Process

def task():
    print("Child process executing")

if __name__ == "__main__":

    p = Process(target=task)

    p.start()
    p.join()

    print("Main process finished")
```

### Output

```text
Child process executing
Main process finished
```

---

# 5. Passing Arguments to a Process

Processes often need data.

Arguments can be passed using `args`.

### Example

```python
from multiprocessing import Process

def square(n):
    print(n * n)

if __name__ == "__main__":

    p = Process(
        target=square,
        args=(5,)
    )

    p.start()
    p.join()
```

### Output

```text
25
```

---

# 6. Running Multiple Processes

Multiple processes can be created to run tasks in parallel.

### Example

```python
from multiprocessing import Process

def worker(number):
    print("Processing:", number)

if __name__ == "__main__":

    processes = []

    for i in range(5):

        p = Process(
            target=worker,
            args=(i,)
        )

        processes.append(p)
        p.start()

    for p in processes:
        p.join()
```

### Execution Structure

```text
Main Process
     │
     ├── Process 1 → worker(0)
     ├── Process 2 → worker(1)
     ├── Process 3 → worker(2)
     ├── Process 4 → worker(3)
     └── Process 5 → worker(4)
```

All processes may run simultaneously depending on available CPU cores.

---

# 7. Process Lifecycle

A Python process goes through several stages.

```text
Process Created
      ↓
Process Started
      ↓
Function Executed
      ↓
Process Finishes
      ↓
Process Joined
```

### Internally

```text
Process()
   ↓
start()
   ↓
Running
   ↓
join()
   ↓
Terminated
```

---

# 8. Importance of `if __name__ == "__main__"`

When using multiprocessing, Python requires:

```python
if __name__ == "__main__":
```

### Example

```python
if __name__ == "__main__":
    p.start()
```

### Why Is It Necessary?

When a new process starts, Python imports the main script again.

Without this guard, the program may recursively create new processes forever.

### Especially Important On

* Windows
* macOS

Without it, multiprocessing programs may crash or create infinite child processes.

---

# 9. Process Pools

Creating processes manually works well for small tasks, but managing many processes becomes difficult.

Python provides **Process Pools** to simplify this.

A pool is a collection of worker processes that execute tasks.

```text
Task Queue
     │
     ▼
Process Pool
 ├── Worker 1
 ├── Worker 2
 ├── Worker 3
 └── Worker 4
```

Tasks are automatically distributed among available workers.

---

# 10. Creating a Process Pool

### Example

```python
from multiprocessing import Pool

def square(n):
    return n * n

if __name__ == "__main__":

    with Pool(4) as p:

        result = p.map(
            square,
            [1, 2, 3, 4, 5]
        )

    print(result)
```

### Output

```text
[1, 4, 9, 16, 25]
```

### Explanation

| Component  | Purpose                        |
| ---------- | ------------------------------ |
| `Pool(4)`  | Create four worker processes   |
| `map()`    | Distribute tasks automatically |
| `square()` | Function executed by workers   |

---

# 11. How Pool Distributes Tasks

Suppose we have:

```text
Tasks:    1 2 3 4 5 6 7 8
Workers:  4
```

Possible distribution:

```text
Worker 1 → 1, 5
Worker 2 → 2, 6
Worker 3 → 3, 7
Worker 4 → 4, 8
```

This keeps CPU cores busy and improves utilization.

---

# 12. Pool Methods

Several useful methods are available.

| Method          | Purpose                         |
| --------------- | ------------------------------- |
| `map()`         | Apply a function to an iterable |
| `imap()`        | Lazy iterator version of map    |
| `apply()`       | Execute a single task           |
| `apply_async()` | Execute asynchronously          |
| `close()`       | Stop accepting new tasks        |
| `join()`        | Wait for workers to finish      |

### Example

```python
pool.apply(square, (5,))
```

---

# 13. Process Parallelism and CPU Cores

Processes can execute across multiple CPU cores.

### Example Machine

```text
4-Core CPU
```

### Process Distribution

```text
Core 1 → Process A

Core 2 → Process B

Core 3 → Process C

Core 4 → Process D
```

This provides true parallel execution.

Unlike threads, processes are not restricted by a shared GIL.

---

# 14. Real-World Use Cases

## Data Processing

* Large dataset splitting
* Parallel transformations
* ETL pipelines

---

## Machine Learning

* Parallel feature engineering
* Model training
* Data preprocessing

---

## Scientific Computing

* Numerical calculations
* Simulations
* Computational research

---

## Image and Video Processing

* Frame processing
* Rendering
* Encoding pipelines

---

# 15. Advantages of Creating Processes

## True Parallelism

Multiple CPU cores can work simultaneously.

---

## Better Performance for CPU Tasks

Heavy computational workloads execute significantly faster.

---

## Fault Isolation

If one process crashes:

```text
Process A crashes
Process B continues running
```

The remaining processes are unaffected.

---

## Safe Memory Isolation

Processes cannot accidentally modify each other's memory.

---

# 16. Limitations of Processes

Processes also introduce overhead.

## Higher Memory Usage

Each process maintains its own memory space.

---

## Process Creation Cost

Creating a process is slower than creating a thread.

---

## Data Sharing Complexity

Data must be transferred using:

* Queues
* Pipes
* Shared memory

Communication is more complex than thread-based programs.

---

# 17. Summary

Creating processes in Python allows programs to run multiple tasks simultaneously using multiple CPU cores.

## Typical Workflow

1. Define a function
2. Create a `Process` object
3. Start the process using `start()`
4. Wait for completion using `join()`

---

## Python Provides Two Main Approaches

### `multiprocessing.Process`

* Manual process creation
* Full control over execution
* Suitable for custom workflows

### `multiprocessing.Pool`

* Automatic worker management
* Built-in task distribution
* Ideal for large workloads

---

Together, these tools allow Python programs to scale computational workloads efficiently, especially in data science, machine learning, scientific computing, and high-performance computing applications.
