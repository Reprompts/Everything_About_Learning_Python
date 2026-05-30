# Python Programming: Chapter 28 Advanced Functions

Python treats functions as **first-class objects**, meaning functions can be:

* Assigned to variables
* Passed as arguments
* Returned from other functions
* Stored in data structures

Because of this flexibility, Python supports several advanced functional programming concepts such as:

* Partial functions
* Currying
* Function composition

These concepts are widely used in:

* Functional programming patterns
* Data processing pipelines
* Reusable abstractions
* API design
* Mathematical programming

This article explains these concepts in depth with practical examples.

---

# 1. Partial Functions

## 1.1 What is a Partial Function?

A partial function is a new function created by fixing some arguments of an existing function.

Instead of providing all parameters each time, some parameters are pre-filled.

### Concept

```text
f(a, b, c)

partial(f, a=10)

→ new function g(b, c)
```

This technique simplifies repeated calls with common parameters.

---

## 1.2 Creating Partial Functions Using `functools.partial`

Python provides built-in support through the `functools` module.

### Import

```python
from functools import partial
```

### Example: Basic Partial Function

Original function:

```python
def power(base, exponent):
    return base ** exponent
```

Create a partial function:

```python
from functools import partial

square = partial(power, exponent=2)
```

Now `square` automatically uses `exponent = 2`.

### Usage

```python
print(square(5))
print(square(10))
```

### Output

```text
25
100
```

Here:

```text
square(x) → power(x, 2)
```

---

## 1.3 Partial with Multiple Fixed Parameters

### Example

```python
def volume(length, width, height):
    return length * width * height
```

Create partial:

```python
from functools import partial

volume_with_length_10 = partial(volume, 10)
```

Usage:

```python
print(volume_with_length_10(5, 2))
```

Equivalent to:

```python
volume(10, 5, 2)
```

---

## 1.4 Practical Example: Logging Function

Original function:

```python
def log(message, level):
    print(f"[{level}] {message}")
```

Create partial functions:

```python
from functools import partial

info = partial(log, level="INFO")
error = partial(log, level="ERROR")
```

Usage:

```python
info("System started")
error("Database failure")
```

### Output

```text
[INFO] System started
[ERROR] Database failure
```

---

## 1.5 Advantages of Partial Functions

Partial functions provide:

* Simplified function calls
* Reduced repetition
* Reusable configurations
* Cleaner API design

They are commonly used in:

* GUI event handlers
* Web frameworks
* Configuration-driven systems

---

# 2. Currying

## 2.1 What is Currying?

Currying is a technique where a function that takes multiple arguments is transformed into a sequence of functions, each taking a single argument.

### Concept

Original function:

```text
f(a, b, c)
```

Curried form:

```text
f(a)(b)(c)
```

Each function returns another function until all arguments are provided.

---

## 2.2 Simple Currying Example

Original function:

```python
def add(a, b):
    return a + b
```

Curried version:

```python
def add(a):
    def inner(b):
        return a + b
    return inner
```

Usage:

```python
add_five = add(5)

print(add_five(10))
```

### Output

```text
15
```

Equivalent call:

```python
add(5)(10)
```

---

## 2.3 Multi-Level Currying

### Example

```python
def multiply(a):
    def second(b):
        def third(c):
            return a * b * c
        return third
    return second
```

Usage:

```python
result = multiply(2)(3)(4)

print(result)
```

### Output

```text
24
```

---

## 2.4 Practical Example: Pricing System

### Example

```python
def price_with_tax(tax_rate):
    def calculate(price):
        return price + price * tax_rate
    return calculate
```

Create specialized functions:

```python
vat_10 = price_with_tax(0.10)
vat_20 = price_with_tax(0.20)
```

Usage:

```python
print(vat_10(100))
print(vat_20(100))
```

### Output

```text
110
120
```

This pattern is common in financial calculations.

---

## 2.5 Advantages of Currying

Currying enables:

* Incremental parameter application
* Highly reusable functions
* Cleaner functional pipelines
* Configuration-based functions

Currying is widely used in functional programming languages such as Haskell.

---

# 3. Partial Functions vs Currying

These concepts are similar but not identical.

### Partial Functions

```text
f(a, b, c)

partial(f, a=1)
```

### Currying

```text
f(a)(b)(c)
```

### Difference

| Partial Functions                     | Currying                                  |
| ------------------------------------- | ----------------------------------------- |
| Fix parameters directly               | Transform functions into chains           |
| Produces a specialized function       | Produces nested single-argument functions |
| Implemented using `functools.partial` | Implemented through nested functions      |
| Useful for configuration              | Useful for functional pipelines           |

---

# 4. Function Composition

## 4.1 What is Function Composition?

Function composition means combining multiple functions into a single function.

### Mathematical Concept

```text
f(g(x))
```

Where:

1. `g(x)` executes first
2. `f()` uses the result

---

## 4.2 Basic Composition Example

Functions:

```python
def double(x):
    return x * 2

def increment(x):
    return x + 1
```

Composition:

```python
result = double(increment(5))
```

### Steps

```text
increment(5) → 6
double(6)    → 12
```

### Output

```text
12
```

---

## 4.3 Creating a Composition Function

### Example

```python
def compose(f, g):
    def inner(x):
        return f(g(x))
    return inner
```

Usage:

```python
double_then_increment = compose(double, increment)

print(double_then_increment(5))
```

---

## 4.4 Composition with Multiple Functions

### Example

```python
def compose(*functions):
    def inner(value):
        for f in reversed(functions):
            value = f(value)
        return value
    return inner
```

Usage:

```python
def square(x):
    return x * x

def double(x):
    return x * 2

def add_one(x):
    return x + 1

pipeline = compose(square, double, add_one)

print(pipeline(3))
```

### Execution Order

```text
add_one → double → square
```

### Steps

```text
3 + 1 = 4
4 * 2 = 8
8 * 8 = 64
```

### Output

```text
64
```

---

## 4.5 Real-World Example: Data Processing Pipeline

Functions:

```python
def clean(x):
    return x.strip()

def to_int(x):
    return int(x)

def square(x):
    return x * x
```

Compose them:

```python
pipeline = compose(square, to_int, clean)

print(pipeline(" 5 "))
```

### Output

```text
25
```

---

# 5. Composition with `map`

### Example

```python
numbers = [1, 2, 3]

result = map(
    lambda x: double(increment(x)),
    numbers
)

print(list(result))
```

### Output

```text
[4, 6, 8]
```

---

# 6. Functional Pipelines

Composition enables pipelines:

```text
data → transform → filter → aggregate
```

### Example

```python
numbers = [1, 2, 3, 4, 5]

result = map(
    lambda x: x * x,
    filter(lambda x: x % 2 == 0, numbers)
)

print(list(result))
```

### Output

```text
[4, 16]
```

---

# 7. Advanced Composition Example

```python
def compose(*functions):
    def inner(value):
        for f in functions[::-1]:
            value = f(value)
        return value
    return inner
```

Usage:

```python
pipeline = compose(str, abs)

print(pipeline(-5))
```

### Output

```python
"5"
```

---

# 8. Summary

Advanced function concepts extend Python's functional programming capabilities.

## Partial Functions

Allow fixing some arguments of functions to create specialized versions.

## Currying

Transforms multi-argument functions into chains of single-argument functions.

## Function Composition

Combines multiple functions into pipelines where the output of one becomes the input of another.

These concepts enable:

* Cleaner abstractions
* Reusable function pipelines
* Flexible configuration of behavior
* Efficient data processing architectures

They are widely used in:

* Functional programming patterns
* Data science pipelines
* Advanced software architecture

Mastering these techniques helps developers write expressive, modular, and reusable Python programs.
