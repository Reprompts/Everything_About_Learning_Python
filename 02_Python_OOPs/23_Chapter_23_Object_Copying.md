# Python OOP: Chapter 23 Object Copying

Object copying is an essential concept in Python because variables do not store objects themselves; they store **references** to objects.

This means when you assign one variable to another, Python does not create a new object. Instead, it creates another reference to the same object.

Understanding object copying is critical when working with:

* Nested data structures
* Mutable objects
* Data models
* Object-oriented systems
* Frameworks and libraries

In this article, we will explore in depth:

* Object copying fundamentals
* Shallow copy
* Deep copy
* The `copy` module
* Custom copy behavior in classes

with practical explanations and examples.

---

# 1. Understanding Object References

Before discussing copying, it is important to understand how Python handles objects.

When you create an object:

```python
a = [1, 2, 3]
```

Memory representation:

```text
Variable a
   |
   v
[1, 2, 3]   (list object in memory)
```

Now assign:

```python
b = a
```

Memory representation:

```text
a ──┐
    ├──> [1, 2, 3]
b ──┘
```

Both variables point to the same object.

If we modify the object:

```python
b.append(4)

print(a)
```

Output:

```python
[1, 2, 3, 4]
```

Because no new object was created.

This is why copying mechanisms exist.

---

# 2. Object Copying Concept

Object copying means creating a new object based on an existing object.

Two main types exist:

* Shallow Copy
* Deep Copy

The difference becomes important when objects contain nested mutable objects.

Example nested structure:

```text
list
 ├── element 1
 ├── element 2
 └── inner list
        ├── value
        └── value
```

Copying may either:

* Copy only the outer object (shallow copy)
* Copy everything recursively (deep copy)

---

# 3. Shallow Copy

## Concept

A shallow copy creates a new container object but does not copy the objects inside it.

Instead, it copies references to the same internal objects.

### Visualization

Original:

```text
A
 |
 v
[1, 2, [3,4]]
       |
       v
    inner list
```

Shallow copy:

```text
A ───> [1, 2, [3,4]]
             ^
             |
B ───> [1, 2, [3,4]]
```

Both lists share the same nested list.

---

## Creating a Shallow Copy

### Method 1: Using Slicing

```python
a = [1, 2, 3]

b = a[:]

print(a is b)
```

Output:

```python
False
```

The lists are different objects.

---

### Method 2: Using `list()` Constructor

```python
a = [1, 2, 3]

b = list(a)
```

---

### Method 3: Using `copy()` Method

```python
a = [1, 2, 3]

b = a.copy()
```

---

## Shallow Copy with Nested Structures

### Example

```python
a = [1, 2, [3, 4]]

b = a.copy()

b[2].append(5)

print(a)
print(b)
```

Output:

```python
[1, 2, [3, 4, 5]]
[1, 2, [3, 4, 5]]
```

Because the inner list reference is shared.

### Memory Diagram

```text
a ----┐
      v
    [1,2,*]
          |
b ----┐   |
      v   v
    [1,2,*] ---> [3,4,5]
```

Only the outer list is copied.

---

# 4. Deep Copy

## Concept

A deep copy creates a new object and recursively copies all objects inside it.

### Visualization

Original:

```text
A ---> [1,2,[3,4]]
```

Deep copy:

```text
A ---> [1,2,[3,4]]

B ---> [1,2,[3,4]]
```

Even the inner list is duplicated.

---

## Example

```python
import copy

a = [1, 2, [3, 4]]

b = copy.deepcopy(a)

b[2].append(5)

print(a)
print(b)
```

Output:

```python
[1, 2, [3, 4]]
[1, 2, [3, 4, 5]]
```

The copied structure is completely independent.

---

## Deep Copy Algorithm Concept

Deep copy works recursively.

Pseudo-process:

```text
function deep_copy(object):
    create new object

    for each attribute:
        if immutable:
            copy reference

        if mutable:
            recursively deep copy
```

---

# 5. The `copy` Module

Python provides a standard module for copying operations.

```python
copy
```

It provides two main functions:

* `copy.copy()`
* `copy.deepcopy()`

---

## Importing the Module

```python
import copy
```

---

## `copy.copy()`

Performs a shallow copy.

### Example

```python
import copy

a = [1, 2, [3, 4]]

b = copy.copy(a)
```

Equivalent to:

```python
a.copy()
```

---

## `copy.deepcopy()`

Performs a deep copy.

### Example

```python
import copy

a = [1, 2, [3, 4]]

b = copy.deepcopy(a)
```

---

## Example Comparison

```python
import copy

a = [[1, 2], [3, 4]]

b = copy.copy(a)

c = copy.deepcopy(a)

a[0].append(99)

print(b)
print(c)
```

Output:

```python
[[1, 2, 99], [3, 4]]
[[1, 2], [3, 4]]
```

Shallow copy shares inner lists.

Deep copy duplicates them.

---

# 6. Immutable Objects and Copying

Immutable objects generally do not require copying.

Examples:

* `int`
* `float`
* `str`
* `tuple`
* `frozenset`

### Example

```python
import copy

a = 10

b = copy.copy(a)

print(a is b)
```

Output:

```python
True
```

Because immutable objects can safely be reused.

---

# 7. Copying Custom Objects

When copying custom objects, Python copies instance attributes.

### Example

```python
class User:
    def __init__(self, name, hobbies):
        self.name = name
        self.hobbies = hobbies

import copy

u1 = User("Alice", ["reading", "coding"])

u2 = copy.copy(u1)

u2.hobbies.append("music")

print(u1.hobbies)
```

Output:

```python
['reading', 'coding', 'music']
```

Because the list reference is shared.

---

# 8. Custom Copy Behavior

Classes can define how they should be copied.

Python provides two special methods:

* `__copy__()`
* `__deepcopy__()`

---

# 9. Implementing `__copy__()`

### Example

```python
import copy

class User:
    def __init__(self, name):
        self.name = name

    def __copy__(self):
        print("Custom shallow copy")
        return User(self.name)

u1 = User("Alice")

u2 = copy.copy(u1)
```

Output:

```text
Custom shallow copy
```

This allows custom logic during copying.

---

# 10. Implementing `__deepcopy__()`

### Example

```python
import copy

class User:
    def __init__(self, name, hobbies):
        self.name = name
        self.hobbies = hobbies

    def __deepcopy__(self, memo):
        name_copy = copy.deepcopy(self.name, memo)
        hobbies_copy = copy.deepcopy(self.hobbies, memo)

        return User(name_copy, hobbies_copy)
```

Now deep copying behaves correctly.

---

# 11. Memo Dictionary in Deep Copy

Deep copy uses a **memo dictionary** to track objects already copied.

This prevents:

* Infinite recursion
* Duplicate copying

### Example Circular Structure

```python
a = []

a.append(a)
```

Structure:

```text
a
 |
 v
[a -> itself]
```

Without memo tracking:

```text
Infinite recursion
```

Deep copy handles this automatically using the memo dictionary.

---

# 12. Copying Objects with Circular References

### Example

```python
import copy

a = []

a.append(a)

b = copy.deepcopy(a)

print(b)
```

Deep copy safely reproduces the circular structure.

---

# 13. Performance Considerations

Deep copy can be expensive.

Reasons include:

* Recursive traversal
* Object reconstruction
* Memory allocation

For large object graphs, deep copying can be slow.

Examples:

* Large nested dictionaries
* Complex object trees
* ORM models
* Graph structures

---

# 14. Real-World Use Cases

Object copying is widely used in modern software systems.

---

## Data Processing

Examples:

* Data pipelines
* Dataset transformations

Copying ensures original data remains unchanged.

---

## Game Development

Game states may be copied during:

* AI simulations
* Undo systems
* State rollback

---

## Machine Learning

Deep copies may be used when:

* Duplicating model configurations
* Experiment branching
* Hyperparameter testing

---

## Web Frameworks

Frameworks often copy request or context objects to avoid modifying originals.

---

## Undo Systems

Applications store copies of object states.

Examples:

* Text editors
* Drawing applications
* CAD tools

---

# 15. Comparison Summary

| Feature       | Shallow Copy      | Deep Copy         |
| ------------- | ----------------- | ----------------- |
| Outer Object  | Copied            | Copied            |
| Inner Objects | Shared            | Duplicated        |
| Performance   | Fast              | Slower            |
| Memory Usage  | Lower             | Higher            |
| Best For      | Simple structures | Nested structures |

---

# 16. Practical Rule of Thumb

## Use Shallow Copy When

* Objects contain only immutable items
* Nested objects should remain shared
* Performance is important

---

## Use Deep Copy When

* Objects contain nested mutable objects
* Complete independence is required
* Modifying nested objects should not affect the original

---

# Final Summary

Object copying is a fundamental concept in Python's memory and object model.

## Key Ideas

* Variables store references, not objects
* Shallow copy duplicates only the outer container
* Deep copy duplicates the entire object graph
* The `copy` module provides standardized copying functions
* Custom objects can define their own copy behavior

---

## Core Functions

```python
copy.copy()       # Shallow copy
copy.deepcopy()   # Deep copy
```

---

## Why It Matters

Understanding object copying is critical when working with:

* Nested data structures
* Object-oriented systems
* Frameworks
* Data models
* Large-scale applications

A solid understanding of shallow and deep copying helps prevent unintended side effects, improves program correctness, and ensures safe manipulation of complex object structures.
