
# Python Programming: Chapter 19 Generators

Generators are one of Python's most powerful features for efficient iteration and memory management. They allow programs to produce values one at a time instead of creating entire sequences in memory.

Generators are widely used in:
- large data processing
- streaming data
- pipelines
- asynchronous programming foundations
- coroutine patterns

This article explains generators in depth, covering:
- Generator functions
- yield keyword
- Generator expressions
- Lazy evaluation
- Generator state
- Generator methods (send, throw, close)
- Coroutine basics with generators

---

## 1. What Are Generators?

A generator is a special type of iterator that generates values on demand rather than storing all values in memory.

Example problem:
Create numbers from 1 to 1,000,000.

Using a list:
```python
numbers = [i for i in range(1000000)]
````

This stores 1 million values in memory.

Using a generator:

```python
numbers = (i for i in range(1000000))
```

Values are generated only when needed.

---

## 2. Generator Functions

A generator function is a function that returns a generator object instead of returning a single value. It uses the `yield` keyword instead of `return`.

### Basic Example

```python
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1
```

### Using the generator

```python
for number in count_up_to(5):
    print(number)
```

### Output

```
1
2
3
4
5
```

The function does not execute entirely at once. It pauses at every `yield`.

---

## 3. The `yield` Keyword

`yield` produces a value and pauses the function execution, saving its state.

```python
def simple_generator():
    print("Start")
    yield 1
    print("Middle")
    yield 2
    print("End")
```

### Usage

```python
g = simple_generator()

print(next(g))
print(next(g))
print(next(g))
```

### Output

```
Start
1
Middle
2
End
StopIteration
```

Each `next()` resumes execution from where it stopped.

---

## 4. Generator Execution Flow

Generators operate as follows:

* Generator function is called
* Function returns a generator object
* Execution starts when `next()` is called
* Execution pauses at `yield`
* State is saved
* Execution resumes on next call

---

## 5. Generator Expressions

Generator expressions are similar to list comprehensions, but they produce generators instead of lists.

### Syntax

```python
(expression for item in iterable)
```

### Example

```python
gen = (x*x for x in range(5))

for value in gen:
    print(value)
```

### Output

```
0
1
4
9
16
```

### Difference Between List and Generator

List comprehension:

```python
[x*x for x in range(5)]
```

Generator expression:

```python
(x*x for x in range(5))
```

Generators use far less memory.

---

## 6. Lazy Evaluation

Generators implement lazy evaluation: values are produced only when requested.

```python
def numbers():
    for i in range(1000000):
        yield i
```

### Usage

```python
gen = numbers()

print(next(gen))
print(next(gen))
```

### Output

```
0
1
```

---

## 7. Generator State

Generators maintain their internal state between executions.

```python
def counter():
    i = 0
    while True:
        yield i
        i += 1
```

### Usage

```python
c = counter()

print(next(c))
print(next(c))
print(next(c))
```

### Output

```
0
1
2
```

The variable `i` persists between yields.

---

## 8. Generator Methods

Generators support special methods:

* `send()`
* `throw()`
* `close()`

---

## 9. `send()` Method

`send()` sends a value into the generator.

```python
def echo():
    value = yield
    print(value)
```

### Usage

```python
g = echo()

next(g)
g.send("Hello")
```

### Output

```
Hello
```

### Practical Example

```python
def accumulator():
    total = 0
    while True:
        value = yield total
        total += value
```

### Usage

```python
g = accumulator()

print(next(g))
print(g.send(5))
print(g.send(10))
```

### Output

```
0
5
15
```

---

## 10. `throw()` Method

Raises an exception inside the generator.

```python
def generator():
    try:
        yield 1
    except ValueError:
        print("Exception caught")
```

### Usage

```python
g = generator()
next(g)
g.throw(ValueError)
```

### Output

```
Exception caught
StopIteration
```

---

## 11. `close()` Method

Stops the generator execution.

```python
def infinite():
    while True:
        yield "running"
```

### Usage

```python
g = infinite()
print(next(g))
g.close()
```

After closing, further iteration raises `StopIteration`.

---

## 12. Generator Pipelines

Generators can be combined into pipelines.

```python
def read_numbers():
    for i in range(10):
        yield i

def square(numbers):
    for n in numbers:
        yield n*n
```

### Usage

```python
pipeline = square(read_numbers())

for value in pipeline:
    print(value)
```

### Output

```
0
1
4
9
16
25
36
49
64
81
```

---

## 13. Coroutine Basics with Generators

Before `async/await`, generators were used as coroutines.

### Coroutine Example

```python
def coroutine():
    print("Started")
    value = yield
    print("Received:", value)
```

### Usage

```python
c = coroutine()
next(c)
c.send("Hello")
```

### Output

```
Started
Received: Hello
```

---

## 14. Running Average Example

```python
def running_average():
    total = 0
    count = 0
    while True:
        value = yield total / count if count else 0
        total += value
        count += 1
```

### Usage

```python
avg = running_average()

next(avg)
print(avg.send(10))
print(avg.send(20))
print(avg.send(30))
```

### Output

```
10
15
20
```

---

## 15. Generators vs Iterators

Generators automatically implement the iterator protocol.

Custom iterator:

```python
class Counter:
    def __iter__(self):
        return self
```

Generator equivalent:

```python
def counter():
    yield 1
```

Generators require far less code.

---

## 16. Real-World Use Cases

### Reading large files

```python
def read_lines(file):
    for line in file:
        yield line
```

### Data streaming

Processing logs or real-time streams.

### Infinite sequences

```python
def natural_numbers():
    n = 1
    while True:
        yield n
        n += 1
```

---

## 17. Summary

Generators provide a powerful mechanism for efficient iteration in Python.

* **Generator functions** → use `yield` and produce values lazily
* **Generator expressions** → compact syntax for generators
* **Lazy evaluation** → values generated on demand
* **Generator state** → execution resumes from last `yield`
* **Generator methods** → `send()`, `throw()`, `close()`
* **Coroutines** → generators that receive values and maintain state

Generators are widely used in:

* data pipelines
* streaming systems
* asynchronous frameworks
* high-performance Python programs

```
```
