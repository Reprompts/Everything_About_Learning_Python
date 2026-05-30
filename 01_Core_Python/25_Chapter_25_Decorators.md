# Python Programming: Chapter 25 Decorators

Decorators are one of the most powerful features of Python. They allow developers to modify or extend the behavior of functions or classes without changing their source code.

Decorators are widely used in real-world Python frameworks and libraries such as:

* Flask
* Django
* FastAPI
* pytest
* functools

They are built on several Python concepts:

* Functions as first-class objects
* Higher-order functions
* Closures

This article explains decorators in detail, covering:

* Function decorators
* Decorator syntax
* Nested decorators
* Parameterized decorators
* Decorator closures

Each concept is explained with practical examples.

---

# 1. What is a Decorator?

A decorator is a function that takes another function as input and returns a new function with modified behavior.

### Basic Concept

```text id="dec1"
function → decorator → enhanced function
```

Conceptually:

```python id="dec2"
def original_function():
    pass

decorated_function = decorator(original_function)
```

Decorators are commonly used to add:

* Logging
* Validation
* Authentication
* Caching
* Timing
* Access control

without modifying the original function.

---

# 2. Functions as First-Class Objects

Decorators are possible because functions in Python are first-class objects.

This means functions can:

* Be assigned to variables
* Be passed as arguments
* Be returned from functions

### Example

```python id="dec3"
def greet():
    print("Hello")

message = greet
message()
```

### Output

```text id="dec4"
Hello
```

---

# 3. Functions Returning Functions

Decorators rely on functions that return other functions.

### Example

```python id="dec5"
def outer():
    def inner():
        print("Inner function")
    return inner

func = outer()
func()
```

### Output

```text id="dec6"
Inner function
```

---

# 4. Basic Function Decorator

A decorator wraps another function.

### Example

```python id="dec7"
def decorator(func):
    def wrapper():
        print("Before function execution")
        func()
        print("After function execution")
    return wrapper
```

Function to decorate:

```python id="dec8"
def say_hello():
    print("Hello")
```

### Applying decorator manually

```python id="dec9"
say_hello = decorator(say_hello)
say_hello()
```

### Output

```text id="dec10"
Before function execution
Hello
After function execution
```

---

# 5. Decorator Syntax (@)

Python provides a cleaner syntax for decorators.

Instead of:

```python id="dec11"
say_hello = decorator(say_hello)
```

You can write:

```python id="dec12"
@decorator
def say_hello():
    print("Hello")
```

This is equivalent to:

```text id="dec13"
say_hello = decorator(say_hello)
```

### Execution

```python id="dec14"
say_hello()
```

### Output

```text id="dec15"
Before function execution
Hello
After function execution
```

---

# 6. Decorators with Function Arguments

The wrapper function must accept arguments.

### Example

```python id="dec16"
def decorator(func):
    def wrapper(a, b):
        print("Adding numbers")
        result = func(a, b)
        print("Result computed")
        return result
    return wrapper
```

### Function

```python id="dec17"
@decorator
def add(x, y):
    return x + y
```

### Execution

```python id="dec18"
print(add(5, 3))
```

### Output

```text id="dec19"
Adding numbers
Result computed
8
```

---

# 7. Generic Decorators Using *args and **kwargs

To support any function signature:

```python id="dec20"
def decorator(func):
    def wrapper(*args, **kwargs):
        print("Function started")
        result = func(*args, **kwargs)
        print("Function finished")
        return result
    return wrapper
```

### Example

```python id="dec21"
@decorator
def multiply(a, b):
    return a * b
```

---

# 8. Practical Example: Logging Decorator

```python id="dec22"
def logger(func):
    def wrapper(*args, **kwargs):
        print("Calling function:", func.__name__)
        result = func(*args, **kwargs)
        print("Function completed")
        return result
    return wrapper
```

### Usage

```python id="dec23"
@logger
def greet(name):
    print("Hello", name)

greet("Alice")
```

### Output

```text id="dec24"
Calling function: greet
Hello Alice
Function completed
```

---

# 9. Nested Decorators

Multiple decorators can be applied to the same function.

### Example

```python id="dec25"
@decorator1
@decorator2
def function():
    pass
```

### Execution Order

```text id="dec26"
function = decorator1(decorator2(function))
```

---

### Example

```python id="dec27"
def uppercase(func):
    def wrapper():
        return func().upper()
    return wrapper

def add_exclamation(func):
    def wrapper():
        return func() + "!"
    return wrapper
```

### Applying decorators

```python id="dec28"
@uppercase
@add_exclamation
def greet():
    return "hello"
```

### Execution

```python id="dec29"
print(greet())
```

### Steps

```text id="dec30"
greet → add_exclamation → uppercase
```

### Output

```text id="dec31"
HELLO!
```

---

# 10. Decorator Closures

Decorators rely on closures.

A closure occurs when an inner function remembers variables from the outer function.

### Example

```python id="dec32"
def decorator(func):
    def wrapper():
        print("Function name:", func.__name__)
        func()
    return wrapper
```

The wrapper remembers `func` even after the outer function finishes.

---

# 11. Parameterized Decorators

Sometimes decorators require parameters.

### Example usage

```python id="dec33"
@repeat(3)
def greet():
    print("Hello")
```

### Implementation

```python id="dec34"
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)
        return wrapper
    return decorator
```

### Execution

```python id="dec35"
greet()
```

### Output

```text id="dec36"
Hello
Hello
Hello
```

---

# 12. Decorator Execution Flow

When Python encounters:

```python id="dec37"
@decorator
def function():
    pass
```

It performs:

1. Function object is created
2. Decorator is called
3. Function passed to decorator
4. Decorator returns wrapper
5. Original function replaced by wrapper

Equivalent:

```python id="dec38"
function = decorator(function)
```

---

# 13. Practical Example: Authentication Decorator

```python id="dec39"
def authenticate(func):
    def wrapper(user):
        if user != "admin":
            print("Access denied")
            return
        return func(user)
    return wrapper
```

### Usage

```python id="dec40"
@authenticate
def dashboard(user):
    print("Welcome to dashboard")
```

### Execution

```python id="dec41"
dashboard("guest")
dashboard("admin")
```

### Output

```text id="dec42"
Access denied
Welcome to dashboard
```

---

# 14. Timing Decorator (Performance Measurement)

```python id="dec43"
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print("Execution time:", end - start)
        return result
    return wrapper
```

### Usage

```python id="dec44"
@timer
def compute():
    total = 0
    for i in range(1000000):
        total += i
    return total

compute()
```

---

# 15. Preserving Function Metadata

Decorators can hide original function metadata.

### Problem

```python id="dec45"
print(function.__name__)
```

### Fix using `functools.wraps`

```python id="dec46"
from functools import wraps

def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

---

# 16. Real-World Examples of Decorators

Decorators are widely used in frameworks:

### Flask

```python id="dec47"
@app.route("/home")
def home():
    return "Welcome"
```

### pytest

```python id="dec48"
@pytest.mark.slow
def test_function():
    pass
```

### Caching

```python id="dec49"
@lru_cache
def fibonacci(n):
    pass
```

---

# 17. Advantages of Decorators

* Code reuse
* Separation of concerns
* Cleaner architecture
* Easy extension of functionality
* Non-invasive modification of functions

---

# 18. Summary

Decorators are functions that modify the behavior of other functions.

## Function decorators

Wrap and enhance functions.

## Decorator syntax (@)

Provides a clean way to apply decorators.

## Nested decorators

Allow multiple decorators to stack.

## Parameterized decorators

Allow decorators to accept arguments.

## Decorator closures

Enable wrappers to remember the decorated function.

Decorators are widely used in:

* Web frameworks
* Testing frameworks
* Caching systems
* Logging systems
* Performance monitoring

They are one of the most powerful abstraction tools in Python.
