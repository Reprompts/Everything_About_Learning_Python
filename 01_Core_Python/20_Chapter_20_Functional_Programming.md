
# Python Programming: Chapter 20 Functional Programming

Python supports multiple programming paradigms, including procedural programming, object-oriented programming, and functional programming. Functional programming focuses on transforming data using functions, avoiding unnecessary state changes and side effects.

Python provides several built-in tools that make functional-style programming practical and efficient.

This article explains the following tools in detail:

- map  
- filter  
- reduce  
- zip  
- enumerate  
- any  
- all  
- sorted  
- min and max  

Each tool is explained with detailed examples and practical applications.

---

## 1. map()

### Concept
`map()` applies a function to every element of an iterable and returns an iterator containing the results.

### Syntax
```python id="map_syntax"
map(function, iterable)
````

### Example

```python id="map_example"
numbers = [1, 2, 3, 4]

def square(x):
    return x * x

result = map(square, numbers)
print(list(result))
```

Output:

```
[1, 4, 9, 16]
```

### Using Lambda with map

```python id="map_lambda"
numbers = [1, 2, 3, 4]
result = map(lambda x: x * 2, numbers)
print(list(result))
```

Output:

```
[2, 4, 6, 8]
```

### Multiple Iterables with map

```python id="map_multi"
a = [1, 2, 3]
b = [4, 5, 6]

result = map(lambda x, y: x + y, a, b)
print(list(result))
```

Output:

```
[5, 7, 9]
```

---

## 2. filter()

### Concept

`filter()` selects elements from an iterable based on a condition.

### Syntax

```python id="filter_syntax"
filter(function, iterable)
```

The function must return True or False.

### Example

```python id="filter_example"
numbers = [1,2,3,4,5,6]

def is_even(x):
    return x % 2 == 0

result = filter(is_even, numbers)
print(list(result))
```

Output:

```
[2,4,6]
```

### Using Lambda

```python id="filter_lambda"
numbers = [1,2,3,4,5]
result = filter(lambda x: x > 2, numbers)
print(list(result))
```

Output:

```
[3,4,5]
```

---

## 3. reduce()

### Concept

`reduce()` repeatedly applies a function to elements of an iterable to produce a single result.

It is part of the `functools` module.

### Syntax

```python id="reduce_syntax"
reduce(function, iterable)
```

### Example

```python id="reduce_example"
from functools import reduce

numbers = [1,2,3,4]
result = reduce(lambda x, y: x + y, numbers)
print(result)
```

Output:

```
10
```

### Product Example

```python id="reduce_product"
from functools import reduce

numbers = [2,3,4]
product = reduce(lambda x,y: x*y, numbers)
print(product)
```

Output:

```
24
```

### Practical Example

```python id="reduce_strings"
from functools import reduce

words = ["Python","is","powerful"]
sentence = reduce(lambda x,y: x + " " + y, words)
print(sentence)
```

Output:

```
Python is powerful
```

---

## 4. zip()

### Concept

`zip()` combines multiple iterables into pairs or tuples.

### Syntax

```python id="zip_syntax"
zip(iterable1, iterable2, ...)
```

### Example

```python id="zip_example"
names = ["Alice","Bob","Charlie"]
scores = [85,90,78]

result = zip(names, scores)
print(list(result))
```

Output:

```
[('Alice', 85), ('Bob', 90), ('Charlie', 78)]
```

### Converting to Dictionary

```python id="zip_dict"
names = ["Alice","Bob"]
scores = [90,80]

data = dict(zip(names, scores))
print(data)
```

Output:

```
{'Alice': 90, 'Bob': 80}
```

### Unzipping

```python id="zip_unzip"
pairs = [('a',1),('b',2)]

letters, numbers = zip(*pairs)
print(letters)
print(numbers)
```

Output:

```
('a','b')
(1,2)
```

---

## 5. enumerate()

### Concept

`enumerate()` adds a counter to an iterable.

### Syntax

```python id="enumerate_syntax"
enumerate(iterable, start=0)
```

### Example

```python id="enumerate_example"
fruits = ["apple","banana","mango"]

for index, value in enumerate(fruits):
    print(index, value)
```

Output:

```
0 apple
1 banana
2 mango
```

### Starting Index

```python id="enumerate_start"
for index, value in enumerate(fruits, start=1):
    print(index, value)
```

---

## 6. any()

### Concept

`any()` returns True if at least one element is True.

### Syntax

```python id="any_syntax"
any(iterable)
```

### Example

```python id="any_example"
values = [False, False, True]
print(any(values))
```

Output:

```
True
```

---

## 7. all()

### Concept

`all()` returns True only if every element is True.

### Syntax

```python id="all_syntax"
all(iterable)
```

### Example

```python id="all_example"
values = [True, True, False]
print(all(values))
```

Output:

```
False
```

---

## 8. sorted()

### Concept

`sorted()` returns a new sorted list from an iterable.

### Syntax

```python id="sorted_syntax"
sorted(iterable, key=None, reverse=False)
```

### Example

```python id="sorted_example"
numbers = [5,3,8,1]
print(sorted(numbers))
```

Output:

```
[1,3,5,8]
```

### Reverse Sorting

```python id="sorted_reverse"
print(sorted(numbers, reverse=True))
```

### Sorting with Key Function

```python id="sorted_key"
words = ["apple","banana","kiwi"]
print(sorted(words, key=len))
```

### Sorting Objects

```python id="sorted_objects"
students = [
    ("Alice",90),
    ("Bob",85),
    ("Charlie",95)
]

print(sorted(students, key=lambda x: x[1]))
```

---

## 9. min() and max()

### Concept

`min()` and `max()` return the smallest or largest value.

### Syntax

```python id="min_max_syntax"
min(iterable)
max(iterable)
```

### Example

```python id="min_max_example"
numbers = [10,3,45,7]

print(min(numbers))
print(max(numbers))
```

Output:

```
3
45
```

### Using Key Function

```python id="min_key"
words = ["apple","banana","kiwi"]
print(min(words, key=len))
```

---

## 10. Functional Programming Pipeline Example

```python id="pipeline_example"
from functools import reduce

numbers = [1,2,3,4,5,6]

result = reduce(
    lambda x,y: x+y,
    filter(
        lambda n: n%2==0,
        map(lambda n: n*n, numbers)
    )
)

print(result)
```

Steps:

* square numbers
* filter even squares
* sum them

Output:

```
56
```

---

## 11. Summary

Python includes several built-in functional tools for transforming and processing data efficiently.

* **map()** → applies a function to every element
* **filter()** → selects elements based on conditions
* **reduce()** → aggregates values into a single result
* **zip()** → combines iterables into tuples
* **enumerate()** → adds indices
* **any()** → checks if at least one value is True
* **all()** → checks if all values are True
* **sorted()** → returns sorted list with rules
* **min() / max()** → find smallest or largest values

These tools enable concise, expressive, and efficient data processing in Python.

```

`
