
# Python Programming: Chapter 18 Iteration Protocol

Iteration is a fundamental concept in Python. Many operations in Python involve traversing collections of data, such as lists, dictionaries, files, and generators. Python provides a powerful internal mechanism known as the iteration protocol, which standardizes how objects can be iterated over.

This system is what allows constructs such as:

```python
for item in collection:
    ...
````

to work with many different types of objects.

This article explains the iteration protocol in depth, covering:

* Iterables
* Iterators
* Iterator protocol
* `__iter__()`
* `__next__()`
* `StopIteration`
* Manual iteration using `iter()` and `next()`

---

## 1. What is Iteration?

Iteration is the process of accessing elements of a collection one by one.

```python id="iter_ex_1"
numbers = [1, 2, 3, 4]

for n in numbers:
    print(n)
```

### Output

```
1
2
3
4
```

Even though the syntax appears simple, Python internally performs several steps to iterate through the list. These steps rely on the iteration protocol.

---

## 2. Iterables

An iterable is any Python object capable of returning its elements one at a time.

Examples of iterables include:

* lists
* tuples
* strings
* sets
* dictionaries
* files
* generators
* ranges

```python id="iterable_ex_1"
text = "Python"

for letter in text:
    print(letter)
```

### Output

```
P
y
t
h
o
n
```

The string behaves as an iterable.

### How Python Recognizes Iterables

An object is considered iterable if it implements:

```python
__iter__()
```

This method returns an iterator object.

---

## 3. Iterators

An iterator is an object that produces values one at a time during iteration.

An iterator must implement:

* `__iter__()`
* `__next__()`

```python id="iterator_ex_1"
numbers = [1, 2, 3]

iterator = iter(numbers)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

### Output

```
1
2
3
```

Each call to `next()` retrieves the next element.

---

## 4. Iterable vs Iterator

| Concept  | Description                                 |
| -------- | ------------------------------------------- |
| Iterable | Object that can produce an iterator         |
| Iterator | Object that produces elements one at a time |

Example:

```python
numbers = [1, 2, 3]

iter_obj = iter(numbers)
```

Here:

* `numbers` → iterable
* `iter_obj` → iterator

---

## 5. Iterator Protocol

The iterator protocol defines how iteration works in Python.

An object must implement:

* `__iter__()`
* `__next__()`

Rules:

* `__iter__()` returns the iterator object
* `__next__()` returns the next element
* When no elements remain, `StopIteration` is raised

---

## 6. The `__iter__()` Method

The `__iter__()` method returns an iterator.

```python id="iter_method_ex"
numbers = [1, 2, 3]

iterator = numbers.__iter__()
```

Equivalent to:

```python
iterator = iter(numbers)
```

### Custom Iterable Example

```python id="custom_iterable_ex"
class CountDown:
    def __init__(self, start):
        self.start = start

    def __iter__(self):
        return iter(range(self.start, 0, -1))
```

### Usage

```python id="countdown_usage"
for i in CountDown(5):
    print(i)
```

### Output

```
5
4
3
2
1
```

---

## 7. The `__next__()` Method

The `__next__()` method returns the next value in the sequence.

```python id="next_method_ex"
numbers = [10, 20, 30]

iterator = iter(numbers)

print(iterator.__next__())
```

### Output

```
10
```

Equivalent to:

```python
next(iterator)
```

---

## 8. StopIteration Exception

When the iterator runs out of values, Python raises `StopIteration`.

```python id="stopiteration_ex"
numbers = [1, 2]

iterator = iter(numbers)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

### Output

```
1
2
StopIteration
```

This exception signals that iteration has finished.

---

## 9. Manual Iteration Using `iter()` and `next()`

```python id="manual_iter"
numbers = [10, 20, 30]

iterator = iter(numbers)

while True:
    try:
        value = next(iterator)
        print(value)
    except StopIteration:
        break
```

### Output

```
10
20
30
```

This is essentially what a `for` loop does internally.

---

## 10. How the for Loop Works Internally

A `for` loop internally performs the following steps:

* Calls `iter()` on the iterable
* Calls `next()` repeatedly
* Stops when `StopIteration` occurs

Equivalent code:

```python
iterator = iter(numbers)

while True:
    try:
        item = next(iterator)
        print(item)
    except StopIteration:
        break
```

---

## 11. Creating Custom Iterators

```python id="custom_iterator_class"
class Counter:
    def __init__(self, limit):
        self.limit = limit
        self.current = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.current < self.limit:
            self.current += 1
            return self.current
        else:
            raise StopIteration
```

### Usage

```python id="counter_usage"
counter = Counter(5)

for num in counter:
    print(num)
```

### Output

```
1
2
3
4
5
```

---

## 12. Infinite Iterators

```python id="infinite_iterator"
class Infinite:
    def __init__(self):
        self.value = 0

    def __iter__(self):
        return self

    def __next__(self):
        self.value += 1
        return self.value
```

### Usage

```python id="infinite_usage"
inf = Infinite()

print(next(inf))
print(next(inf))
print(next(inf))
```

### Output

```
1
2
3
```

---

## 13. Iteration Over Different Data Types

### Lists

```python
for item in [1, 2, 3]:
    print(item)
```

### Strings

```python
for char in "Python":
    print(char)
```

### Dictionaries

```python
data = {"a": 1, "b": 2}

for key in data:
    print(key)
```

### Files

```python
file = open("data.txt")

for line in file:
    print(line)
```

---

## 14. Practical Example: Custom Fibonacci Iterator

```python id="fib_iterator"
class Fibonacci:
    def __init__(self, limit):
        self.limit = limit
        self.a = 0
        self.b = 1
        self.count = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.count >= self.limit:
            raise StopIteration

        value = self.a
        self.a, self.b = self.b, self.a + self.b
        self.count += 1

        return value
```

### Usage

```python id="fib_usage"
for num in Fibonacci(7):
    print(num)
```

### Output

```
0
1
1
2
3
5
8
```

---

## 15. Summary

The Python iteration protocol defines how objects can be traversed sequentially.

### Iterables

Objects capable of returning an iterator.

### Iterators

Objects that produce elements one at a time.

### Iterator protocol

Defined using `__iter__()` and `__next__()` methods.

### `__iter__()`

Returns the iterator object.

### `__next__()`

Returns the next element in the sequence.

### StopIteration

Raised when the iterator has no more elements.

### Manual iteration

Performed using `iter()` and `next()`.

---

Understanding the iteration protocol is essential because it powers:

* for loops
* comprehensions
* generators
* many built-in functions

Mastering this mechanism allows developers to create custom iterable objects and advanced iteration behavior, making Python programs more flexible and powerful.

