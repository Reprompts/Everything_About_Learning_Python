# Python Processes & Threads: Chapter 6

# Thread Synchronization in Multithreading

When multiple threads run inside the same process, they share the same memory space. This shared memory allows threads to communicate efficiently, but it also creates the possibility of **race conditions**, where multiple threads try to access or modify the same data at the same time.

To prevent inconsistent data and ensure correct program behavior, **thread synchronization mechanisms** are used.

---

# What Is Thread Synchronization?

**Thread synchronization** is the technique used to coordinate multiple threads so that shared resources are accessed safely and in the correct order.

It ensures that:

* Only one thread modifies critical data at a time
* Threads execute in a controlled sequence when necessary
* Shared data remains consistent and error-free

Without synchronization, programs that use multiple threads may produce unpredictable results.

---

# Why Synchronization Is Necessary

Threads may simultaneously access shared resources such as:

* Variables
* Files
* Databases
* Memory buffers
* Data structures (lists, queues, dictionaries)

If two threads modify the same variable at the same time, the final result may be incorrect.

---

## Example of a Race Condition

Suppose two threads increment a shared variable:

```python
counter = counter + 1
```

Execution might occur like this:

```text
Thread A reads counter = 5
Thread B reads counter = 5

Thread A writes counter = 6
Thread B writes counter = 6
```

Final value becomes:

```text
6
```

instead of:

```text
7
```

Synchronization prevents such problems.

---

# Common Thread Synchronization Tools

Modern programming languages provide several synchronization primitives.

Commonly used synchronization tools include:

* Lock
* RLock
* Semaphore
* Event
* Condition

Each serves a different purpose in controlling thread execution.

---

# 1. Lock (Mutex Lock)

A **Lock** is the most basic synchronization primitive.

It ensures that only one thread can access a critical section at a time.

A thread must acquire the lock before accessing shared resources and release it afterward.

---

## Working Principle

```text
Thread attempts to acquire lock
           │
           ▼
If lock is free
           │
           ▼
Thread proceeds
           │
           ▼
Critical section executes
           │
           ▼
Lock released
```

If the lock is already held:

```text
Thread waits until lock becomes available
```

---

## Example (Conceptual)

```python
lock.acquire()

# Critical section
shared_variable += 1

lock.release()
```

---

## Advantages

* Simple to understand and use
* Prevents simultaneous access to shared data
* Reduces race conditions

---

## Limitation

If the same thread attempts to acquire the lock again before releasing it, a deadlock may occur.

---

# 2. RLock (Reentrant Lock)

An **RLock** (Reentrant Lock) allows the same thread to acquire the lock multiple times without blocking itself.

It internally tracks:

* Which thread owns the lock
* How many times the lock has been acquired

The lock must be released the same number of times it was acquired.

---

## Example Use Case

Suppose a function acquires a lock and then calls another function that also needs the same lock.

```python
lock.acquire()

functionA()

def functionA():
    lock.acquire()

    # Do work

    lock.release()

lock.release()
```

With a normal `Lock`, this can cause a deadlock.

With an `RLock`, it works safely because the same thread can re-enter the lock.

---

## Advantages

* Prevents self-deadlocks
* Useful for nested function calls
* Simplifies recursive locking scenarios

---

## Limitation

* Slightly slower than a regular lock
* More complex internal bookkeeping

---

# 3. Semaphore

A **Semaphore** controls access to a resource that has a limited number of instances.

Unlike a lock, which allows only one thread at a time, a semaphore allows a fixed number of threads to access the resource simultaneously.

---

## Example

Suppose a system has:

```text
3 Database Connections
```

A semaphore initialized with value `3` ensures that only three threads can use the database concurrently.

---

## Working Principle

```text
acquire() → Decrease Counter
release() → Increase Counter
```

When the counter reaches zero:

```text
Additional threads must wait
```

until another thread releases the semaphore.

---

## Example (Conceptual)

```python
semaphore.acquire()

# Access shared resource

semaphore.release()
```

---

## Real-World Examples

* Database connection pools
* Limited hardware devices
* API request limits
* Resource management systems

---

# 4. Event

An **Event** is used for thread signaling.

It allows one thread to notify other threads that something important has happened.

An event object maintains an internal flag that can be set or cleared.

---

## Key Methods

| Method    | Purpose                       |
| --------- | ----------------------------- |
| `set()`   | Set flag to True              |
| `clear()` | Set flag to False             |
| `wait()`  | Block until flag becomes True |

---

## Example Scenario

One thread loads data while another waits for the data to become available.

### Waiting Thread

```python
event.wait()

process_data()
```

### Worker Thread

```python
load_data()

event.set()
```

This guarantees that the processing thread does not execute before the data is ready.

---

## Advantages

* Simple thread signaling
* Easy coordination between threads
* Useful for startup and shutdown synchronization

---

# 5. Condition

A **Condition** object is used for advanced thread coordination.

It allows threads to wait until a specific condition becomes true.

Conditions are typically used together with locks.

---

## Common Methods

| Method         | Purpose                  |
| -------------- | ------------------------ |
| `wait()`       | Thread waits             |
| `notify()`     | Wake one waiting thread  |
| `notify_all()` | Wake all waiting threads |

---

## Typical Example: Producer–Consumer Problem

### Producer Thread

```text
Add Item to Buffer
        │
        ▼
Notify Consumer
```

### Consumer Thread

```text
Wait for Data
        │
        ▼
Consume Item
```

Conditions allow consumers to sleep until producers provide new data.

---

## Advantages

* Efficient coordination
* Prevents busy waiting
* Ideal for producer-consumer architectures

---

# Example Use Cases of Thread Synchronization

## 1. Protect Shared Variables

When multiple threads update the same variable, synchronization prevents incorrect results.

### Examples

* Bank account balances
* Inventory counters
* Shared statistics
* Application metrics

Locks are commonly used in these situations.

---

## 2. Control Thread Execution Order

Sometimes threads must execute in a specific sequence.

### Example Workflow

```text
Load Data
     ↓
Process Data
     ↓
Save Results
```

Events and conditions can ensure that each step waits for the previous one to complete.

---

## 3. Resource Pool Management

When resources are limited, semaphores regulate access.

### Examples

* Database connections
* Printer queues
* Thread pools
* API rate limits

---

## 4. Producer–Consumer Systems

Producer-consumer patterns are common in:

* Task queues
* Message-processing systems
* Streaming applications
* Background job schedulers

Conditions allow producers to notify consumers when new work is available.

---

# Benefits of Thread Synchronization

Proper synchronization provides several important benefits.

## Data Consistency

Ensures shared data remains accurate and reliable.

---

## Controlled Resource Access

Prevents resource conflicts and overuse.

---

## Predictable Program Behavior

Threads execute in a well-defined manner.

---

## Safe Concurrent Execution

Allows multiple threads to work together without corrupting data.

---

## Easier Maintenance

Well-synchronized code is easier to reason about and debug.

---

# Challenges of Synchronization

Although synchronization is necessary, excessive synchronization can create problems.

## Deadlocks

Two or more threads wait indefinitely for each other.

```text
Thread A waits for Thread B
Thread B waits for Thread A
```

---

## Reduced Performance

Too much locking can reduce concurrency.

---

## Increased Complexity

Synchronization logic can make programs harder to understand and maintain.

---

# Conclusion

Thread synchronization is essential in multithreaded programming because threads share memory and resources. Without proper coordination, race conditions, inconsistent data, and unpredictable behavior can occur.

Synchronization tools such as:

* `Lock`
* `RLock`
* `Semaphore`
* `Event`
* `Condition`

provide mechanisms for safely managing shared resources and coordinating thread execution.

By understanding and applying these synchronization primitives correctly, developers can build efficient, reliable, and scalable concurrent applications that behave predictably even under heavy workloads.
