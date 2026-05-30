# Python Programming: Chapter 26 Context Managers

Context managers are an important Python feature that help manage resources safely and efficiently. They ensure that resources such as:

* Files
* Database connections
* Network sockets
* Locks
* Temporary resources

are properly acquired and released, even if errors occur.

The most common use of context managers is with the `with` statement.

### Example

```python id="k8m2qv"
with open("data.txt") as file:
    content = file.read()
```

This guarantees that the file will be automatically closed, even if an exception occurs.

This article explains context managers in detail, covering:

* The `with` statement
* Context manager protocol
* `__enter__`
* `__exit__`
* Custom context managers
* `contextlib` utilities

---

# 1. The Problem Context Managers Solve

Many resources require setup and cleanup.

### Example without context managers

```python id="p2x8ua"
file = open("data.txt")
data = file.read()
file.close()
```

### Problem

If an exception occurs before `close()`, the file remains open.

```python id="l9wq0c"
file = open("data.txt")
data = file.read()
raise Exception("Error occurred")
file.close()
```

The `file.close()` line never executes → resource leak.

Context managers solve this problem.

---

# 2. The `with` Statement

The `with` statement ensures automatic cleanup.

### Example

```python id="a8kq3m"
with open("data.txt") as file:
    data = file.read()
```

### Behind the scenes

* Resource is acquired
* Code inside block runs
* Resource is released automatically

Even if exceptions occur.

---

# 3. Basic Syntax

```python id="t1q9pz"
with expression as variable:
    block
```

### Example

```python id="m7p2xv"
with open("data.txt") as f:
    print(f.read())
```

---

# 4. Context Manager Protocol

Objects used with `with` must implement:

* `__enter__()`
* `__exit__()`

These define:

* Setup behavior
* Cleanup behavior

---

# 5. The `__enter__` Method

Executed when entering the `with` block.

### Concept

```text id="x2w8ab"
resource = manager.__enter__()
```

The returned value becomes the variable after `as`.

---

# 6. The `__exit__` Method

Executed when the block ends.

### Signature

```python id="q8c1va"
def __exit__(self, exc_type, exc_value, traceback):
    ...
```

### Parameters

* `exc_type` → exception type
* `exc_value` → exception instance
* `traceback` → traceback object

If no exception occurs:

* All values are `None`

---

# 7. Internal Working of `with`

```python id="v1m8qz"
with manager as resource:
    block
```

Equivalent to:

```python id="r7n2kc"
manager = expression
resource = manager.__enter__()

try:
    block
finally:
    manager.__exit__()
```

This guarantees cleanup always occurs.

---

# 8. Built-in Context Managers

Examples:

* `open()` files
* `threading.Lock`
* `decimal.localcontext`
* `tempfile.TemporaryFile`

### Example

```python id="f3k9xp"
with open("file.txt") as f:
    data = f.read()
```

---

# 9. Creating Custom Context Managers

### Example

```python id="c9m2ld"
class Resource:
    def __enter__(self):
        print("Resource acquired")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Resource released")
```

### Usage

```python id="n4q7zx"
with Resource() as r:
    print("Using resource")
```

### Output

```text id="h8k1vm"
Resource acquired
Using resource
Resource released
```

---

# 10. Example: Database Connection Manager

```python id="z3p8wa"
class DatabaseConnection:
    def __enter__(self):
        print("Connecting to database")
        return self

    def query(self):
        print("Executing query")

    def __exit__(self, exc_type, exc_value, traceback):
        print("Closing database connection")
```

### Usage

```python id="u6k1md"
with DatabaseConnection() as db:
    db.query()
```

### Output

```text id="t9x2qa"
Connecting to database
Executing query
Closing database connection
```

---

# 11. Handling Exceptions in `__exit__`

If an exception occurs, it is passed to `__exit__`.

### Example

```python id="k2v9xn"
class Manager:
    def __enter__(self):
        print("Enter")

    def __exit__(self, exc_type, exc_value, traceback):
        print("Exit")
        if exc_type:
            print("Exception occurred:", exc_value)
```

### Usage

```python id="p8m1lc"
with Manager():
    raise ValueError("Test error")
```

### Output

```text id="w1q7nz"
Enter
Exit
Exception occurred: Test error
```

---

# 12. Suppressing Exceptions

If `__exit__` returns `True`, exceptions are suppressed.

### Example

```python id="a7k2pc"
class Manager:
    def __enter__(self):
        print("Start")

    def __exit__(self, exc_type, exc_value, traceback):
        print("Handled exception")
        return True
```

### Usage

```python id="m8x1qv"
with Manager():
    raise Exception("error")
```

The exception will not propagate.

---

# 13. Context Managers for Locks

```python id="r3n9wc"
import threading

lock = threading.Lock()

with lock:
    print("Critical section")
```

Automatically performs:

* `lock.acquire()`
* `lock.release()`

---

# 14. The `contextlib` Module

Provides utilities:

* `contextmanager`
* `closing`
* `suppress`
* `redirect_stdout`
* `ExitStack`

---

# 15. Creating Context Managers with `contextlib.contextmanager`

### Example

```python id="x9c2pw"
from contextlib import contextmanager

@contextmanager
def resource():
    print("Acquire resource")
    yield
    print("Release resource")
```

### Usage

```python id="v4m8lz"
with resource():
    print("Using resource")
```

### Output

```text id="k1p7qd"
Acquire resource
Using resource
Release resource
```

---

# 16. How `contextmanager` Works

Flow:

* Setup code runs
* `yield` pauses execution
* Cleanup runs after block ends

---

# 17. Example: Timer Context Manager

```python id="q2m8lx"
import time
from contextlib import contextmanager

@contextmanager
def timer():
    start = time.time()
    yield
    end = time.time()
    print("Elapsed time:", end - start)
```

### Usage

```python id="c7v9na"
with timer():
    for i in range(1000000):
        pass
```

---

# 18. `contextlib.suppress`

Suppress specific exceptions.

```python id="m9k2xp"
from contextlib import suppress

with suppress(FileNotFoundError):
    open("missing.txt")
```

---

# 19. `contextlib.redirect_stdout`

Redirect output.

```python id="u2p8xv"
import io
from contextlib import redirect_stdout

buffer = io.StringIO()

with redirect_stdout(buffer):
    print("Hello")

print(buffer.getvalue())
```

### Output

```text id="n8q1vm"
Hello
```

---

# 20. Best Practices

* Use `with` for resource management
* Prefer context managers over manual cleanup
* Use `contextlib` for simpler implementations
* Avoid leaving resources open

---

# 21. Summary

Context managers provide a structured way to manage resources safely.

## The `with` statement

Simplifies resource handling and guarantees cleanup.

## Context Manager Protocol

Requires:

* `__enter__`
* `__exit__`

## `__enter__`

Handles resource acquisition.

## `__exit__`

Handles cleanup and exception handling.

## Custom Context Managers

Enable reusable resource management logic.

## `contextlib`

Provides powerful utilities for easy implementation.

Context managers are essential for writing robust, safe, and maintainable Python programs, especially when working with files, network connections, databases, locks, and other critical resources.
