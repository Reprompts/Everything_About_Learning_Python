# Python Programming: Chapter 5 Python Data Types: Lists

## 1. Introduction to Lists in Python

A list is one of the most important and widely used data structures in Python. Lists are used to store collections of items in a single variable. Unlike some other data structures, lists are highly flexible and can store elements of different types.

### Key Characteristics of Python Lists

- Ordered collection of elements
- Mutable (elements can be modified)
- Can contain elements of different data types
- Supports indexing and slicing
- Allows duplicate values

### Example

```python
numbers = [1, 2, 3, 4, 5]
```

In this example:

- `[1, 2, 3, 4, 5]` is a list object
- `numbers` is a reference to that list

Lists are implemented internally as **dynamic arrays**, meaning their size can grow or shrink during program execution.

---

## 2. List Creation

Lists can be created in several different ways.

### 2.1 Using Square Brackets

The most common way to create a list is by using square brackets.

```python
numbers = [1, 2, 3, 4]
```

Lists can contain different types of objects:

```python
mixed = [10, "Python", 3.14, True]
```

### 2.2 Creating an Empty List

```python
empty_list = []
```

### 2.3 Using the `list()` Constructor

Lists can also be created using the `list()` function.

```python
numbers = list((1, 2, 3, 4))
```

From a string:

```python
letters = list("Python")
print(letters)
```

**Output**

```python
['P', 'y', 't', 'h', 'o', 'n']
```

### 2.4 List Comprehensions

List comprehensions allow creating lists dynamically.

```python
numbers = [x for x in range(5)]
```

**Output**

```python
[0, 1, 2, 3, 4]
```

---

## 3. List Indexing

Lists are ordered sequences, meaning each element has a position called an index.

### Example

```python
items = ["apple", "banana", "cherry"]
```

| Element | Index |
|----------|--------|
| apple | 0 |
| banana | 1 |
| cherry | 2 |

### 3.1 Accessing Elements

```python
items[0]
```

**Output**

```python
apple
```

### 3.2 Negative Indexing

Python allows negative indexing to access elements from the end.

```python
items[-1]
```

**Output**

```python
cherry
```

| Element | Negative Index |
|----------|----------------|
| apple | -3 |
| banana | -2 |
| cherry | -1 |

---

## 4. List Slicing

Slicing allows extracting a portion of a list.

### Syntax

```python
list[start:stop:step]
```

### Example

```python
numbers = [1, 2, 3, 4, 5]

print(numbers[1:4])
```

**Output**

```python
[2, 3, 4]
```

### 4.1 Omitting Slice Parameters

```python
numbers[:3]    # First three elements
numbers[2:]    # From index 2 to end
numbers[:]     # Copy of entire list
```

### 4.2 Using Step

```python
numbers = [1, 2, 3, 4, 5, 6]

numbers[::2]
```

**Output**

```python
[1, 3, 5]
```

### 4.3 Reversing a List

```python
numbers[::-1]
```

**Output**

```python
[5, 4, 3, 2, 1]
```

---

## 5. List Mutability

Lists are mutable, meaning their contents can be modified after creation.

### Example

```python
numbers = [1, 2, 3]

numbers[0] = 10

print(numbers)
```

**Output**

```python
[10, 2, 3]
```

This behavior differs from immutable types such as strings and tuples.

### 5.1 Adding Elements

```python
numbers.append(4)
```

**Output**

```python
[1, 2, 3, 4]
```

### 5.2 Removing Elements

```python
numbers.remove(2)
```

**Output**

```python
[1, 3, 4]
```

---

## 6. List Methods

Python provides many built-in methods to modify, search, and organize lists.

### Sample List

```python
numbers = [1, 2, 3, 4]
```

### 6.1 `append()`

Adds an element to the end of the list.

```python
numbers = [1, 2, 3]

numbers.append(4)

print(numbers)
```

**Output**

```python
[1, 2, 3, 4]
```

---

### 6.2 `extend()`

Adds multiple elements from another iterable.

```python
numbers = [1, 2, 3]

numbers.extend([4, 5, 6])

print(numbers)
```

**Output**

```python
[1, 2, 3, 4, 5, 6]
```

---

### 6.3 `insert()`

Inserts an element at a specific index.

```python
numbers = [1, 2, 3]

numbers.insert(1, 10)

print(numbers)
```

**Output**

```python
[1, 10, 2, 3]
```

---

### 6.4 `remove()`

Removes the first occurrence of a value.

```python
numbers = [1, 2, 3, 2]

numbers.remove(2)

print(numbers)
```

**Output**

```python
[1, 3, 2]
```

---

### 6.5 `pop()`

Removes and returns an element.

#### Remove Last Element

```python
numbers = [1, 2, 3]

numbers.pop()

print(numbers)
```

**Output**

```python
[1, 2]
```

#### Remove Element at Specific Index

```python
numbers = [1, 2, 3]

numbers.pop(1)

print(numbers)
```

**Output**

```python
[1, 3]
```

---

### 6.6 `clear()`

Removes all elements from the list.

```python
numbers = [1, 2, 3]

numbers.clear()

print(numbers)
```

**Output**

```python
[]
```

---

### 6.7 `index()`

Returns the index of the first occurrence of a value.

```python
numbers = [10, 20, 30, 20]

print(numbers.index(20))
```

**Output**

```python
1
```

---

### 6.8 `count()`

Counts how many times a value appears.

```python
numbers = [1, 2, 2, 3, 2]

print(numbers.count(2))
```

**Output**

```python
3
```

---

### 6.9 `sort()`

Sorts the list in ascending order.

```python
numbers = [4, 2, 1, 3]

numbers.sort()

print(numbers)
```

**Output**

```python
[1, 2, 3, 4]
```

#### Descending Order

```python
numbers.sort(reverse=True)
```

**Output**

```python
[4, 3, 2, 1]
```

---

### 6.10 `reverse()`

Reverses the order of elements.

```python
numbers = [1, 2, 3]

numbers.reverse()

print(numbers)
```

**Output**

```python
[3, 2, 1]
```

---

### 6.11 `copy()`

Creates a shallow copy of the list.

```python
numbers = [1, 2, 3]

new_list = numbers.copy()

print(new_list)
```

**Output**

```python
[1, 2, 3]
```

---

### Complete List of Python List Methods

- `append()`
- `clear()`
- `copy()`
- `count()`
- `extend()`
- `index()`
- `insert()`
- `pop()`
- `remove()`
- `reverse()`
- `sort()`

---

## 7. Nested Lists

A list can contain other lists as elements.

### Example

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

This structure is commonly used to represent matrices or tables.

### 7.1 Accessing Nested Elements

```python
matrix[1][2]
```

**Output**

```python
6
```

---

## 8. List Copying

Copying lists requires careful understanding because lists store references to objects.

### 8.1 Assignment Copy

```python
a = [1, 2, 3]

b = a
```

Both variables reference the same list.

```python
b.append(4)

print(a)
```

**Output**

```python
[1, 2, 3, 4]
```

---

## 9. Shallow Copy

A shallow copy creates a new list but does not copy nested objects.

### Methods

```python
b = a.copy()
```

or

```python
b = list(a)
```

or

```python
b = a[:]
```

### Example

```python
a = [[1, 2], [3, 4]]

b = a.copy()

b[0][0] = 100
```

**Output**

```python
a = [[100, 2], [3, 4]]
```

Nested objects remain shared.

---

## 10. Deep Copy

A deep copy duplicates the entire structure, including nested objects.

This requires the `copy` module.

```python
import copy

b = copy.deepcopy(a)
```

### Example

```python
import copy

a = [[1, 2], [3, 4]]

b = copy.deepcopy(a)

b[0][0] = 100

print(a)
print(b)
```

**Output**

```python
a = [[1, 2], [3, 4]]
b = [[100, 2], [3, 4]]
```

Both lists are now independent.

---

## 11. When to Use Shallow vs Deep Copy

### Shallow Copy is Sufficient When:

- List elements are immutable
- Nested structures are not modified

### Deep Copy is Required When:

- Working with nested data structures
- Complete independence is required

---

## 12. Performance Considerations

Lists are optimized for:

- Fast indexing
- Dynamic resizing
- Efficient iteration

### However

- Inserting in the middle is slower
- Removing many elements may require shifting memory

---

## 13. Summary

Python lists are one of the most versatile and frequently used data structures.

### Key Features

- Ordered collection
- Mutable structure
- Supports indexing and slicing
- Allows heterogeneous data types
- Provides powerful built-in methods
- Supports nested structures
- Enables shallow and deep copying

Understanding lists is essential because they form the foundation for many higher-level data structures and algorithms used throughout Python programming.
