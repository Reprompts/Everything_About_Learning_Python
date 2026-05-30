# Python OOP: Chapter 22 Slots & Memory Optimization

Python objects are very flexible by default, but that flexibility comes with memory overhead. When many objects are created (millions in large systems), the default behavior can consume significant memory.

To optimize this, Python provides `__slots__`, which allows developers to control how attributes are stored, reduce memory usage, and improve attribute access speed in some cases.

This section explores:

* `__slots__`
* Memory optimization in classes
* Attribute storage control

in extremely deep and practical detail.

---

# 1. Default Attribute Storage in Python Classes

Before understanding `__slots__`, we must understand how Python normally stores attributes.

## Standard Object Layout

When you create a class instance, Python stores attributes inside a dictionary called `__dict__`.

### Example

```python
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

u = User("Alice", 25)
```

Now inspect:

```python
print(u.__dict__)
```

Output:

```python
{'name': 'Alice', 'age': 25}
```

This means:

```text
User object
   |
   └── __dict__
         ├── name -> "Alice"
         └── age  -> 25
```

Every instance has its own dictionary.

---

## Why Python Uses Dictionaries

Python uses dictionaries for attributes because they allow:

* Dynamic attribute creation
* Easy attribute lookup
* Flexible object modification

### Example

```python
u.email = "alice@email.com"
```

The dictionary becomes:

```python
{
    'name': 'Alice',
    'age': 25,
    'email': 'alice@email.com'
}
```

This flexibility is extremely powerful.

However, it has memory costs.

---

# 2. Memory Overhead of Default Objects

Each object contains:

```text
Object Structure
-----------------
Object header
Reference counters
Pointer to type
Pointer to __dict__
Pointer to weakrefs
```

Then the dictionary contains:

```text
Dictionary Structure
--------------------
Hash table
Key objects
Value objects
Pointers
```

This results in large overhead per object.

---

## Approximate Memory Usage

### Example

```python
import sys

class User:
    def __init__(self):
        self.name = "Alice"
        self.age = 25

u = User()

print(sys.getsizeof(u))
print(sys.getsizeof(u.__dict__))
```

Typical result:

```text
48 bytes (object)
112+ bytes (dictionary)
```

Total:

```text
~160+ bytes per object
```

If you create:

```text
10 million objects
```

memory usage becomes enormous.

---

# 3. What is `__slots__`?

`__slots__` is a class attribute that tells Python to allocate fixed storage for instance attributes instead of using `__dict__`.

Instead of a dictionary, attributes are stored in predefined slots in memory.

## Basic Syntax

```python
class User:
    __slots__ = ("name", "age")

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Now:

```python
u = User("Alice", 25)

print(u.__dict__)
```

Result:

```python
AttributeError: 'User' object has no attribute '__dict__'
```

Because dictionary storage is disabled.

---

# 4. How `__slots__` Works Internally

Instead of storing attributes in a dictionary:

```text
instance.__dict__["name"]
instance.__dict__["age"]
```

Python creates fixed memory slots.

Memory layout:

```text
User Object
-----------
slot 0 -> name
slot 1 -> age
```

Attributes become direct memory references rather than dictionary entries.

---

## Visualization

### Without `__slots__`

```text
User instance
   |
   └── __dict__
        ├── "name" -> "Alice"
        └── "age"  -> 25
```

### With `__slots__`

```text
User instance
   |
   ├── slot[0] -> name
   └── slot[1] -> age
```

No dictionary is involved.

---

# 5. Memory Optimization Example

Let's compare both approaches.

## Normal Class

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

Creating many objects:

```python
points = [Point(i, i) for i in range(1_000_000)]
```

Memory cost:

```text
Object + Dictionary per instance
```

---

## Slotted Class

```python
class Point:
    __slots__ = ("x", "y")

    def __init__(self, x, y):
        self.x = x
        self.y = y
```

Memory usage is dramatically reduced.

Typical savings:

```text
30% to 60% less memory
```

---

# 6. Performance Improvements

Using `__slots__` can slightly improve:

* Attribute lookup speed
* Memory locality
* CPU cache efficiency

Because attribute access becomes:

```text
Direct pointer lookup
```

instead of:

```text
Dictionary hash lookup
```

---

## Access Comparison

### Normal Object

```text
object
   ↓
dictionary
   ↓
hash lookup
   ↓
value
```

### Slotted Object

```text
object
   ↓
slot index
   ↓
value
```

---

# 7. Attribute Restrictions

A key behavior of `__slots__`:

> Only predefined attributes can exist.

### Example

```python
class User:
    __slots__ = ("name", "age")

u = User()

u.name = "Alice"
u.email = "alice@email.com"
```

Error:

```python
AttributeError: 'User' object has no attribute 'email'
```

Because `email` was not declared in `__slots__`.

This provides stronger control over object structure.

---

# 8. Allowing `__dict__` with Slots

Sometimes we want slots plus dynamic attributes.

### Example

```python
class User:
    __slots__ = ("name", "age", "__dict__")

u = User()

u.name = "Alice"
u.email = "alice@email.com"
```

Now dynamic attributes work.

However, memory savings are reduced.

---

# 9. Weak Reference Support

Objects with slots cannot support weak references unless enabled.

Add:

```python
__weakref__
```

### Example

```python
class User:
    __slots__ = ("name", "age", "__weakref__")
```

This enables weak-reference support.

---

# 10. Slots with Inheritance

Slots interact with inheritance in specific ways.

### Example

```python
class A:
    __slots__ = ("x",)

class B(A):
    __slots__ = ("y",)
```

Object layout:

```text
B instance
-----------
slot A.x
slot B.y
```

Usage:

```python
b = B()

b.x = 10
b.y = 20
```

Works correctly.

---

## Child Class Without Slots

```python
class A:
    __slots__ = ("x",)

class B(A):
    pass
```

Now the subclass regains a `__dict__`.

Result:

```text
A → Slot storage
B → Dictionary storage
```

---

# 11. Slots and Multiple Inheritance

Multiple inheritance can create slot conflicts.

### Example

```python
class A:
    __slots__ = ("x",)

class B:
    __slots__ = ("y",)

class C(A, B):
    __slots__ = ("z",)
```

Python must combine slot layouts.

Complex multiple inheritance may lead to errors.

---

# 12. Real-World Use Cases

`__slots__` is valuable when millions of objects exist.

---

## Data Models

Example:

```python
class Point:
    __slots__ = ("x", "y")
```

Used in:

* Graphics engines
* GIS systems
* Physics simulations

---

## Networking Libraries

Objects representing:

* Packets
* Frames
* Protocol messages

Examples:

* TCP packets
* IP headers
* DNS records

Slots significantly reduce memory usage.

---

## Game Engines

Games create thousands or millions of objects:

* NPCs
* Particles
* Bullets
* Entities

Slots help optimize memory.

---

## Machine Learning Pipelines

Objects representing:

* Features
* Samples
* Vectors
* Nodes

Slots reduce object overhead.

---

# 13. Slots with Dataclasses

Python dataclasses support slots directly.

### Example

```python
from dataclasses import dataclass

@dataclass(slots=True)
class User:
    name: str
    age: int
```

This automatically generates:

```python
__slots__ = ("name", "age")
```

Benefits:

* Less memory
* Faster object creation
* Cleaner syntax

---

# 14. Measuring Memory Savings

### Example

```python
import sys

class Normal:
    def __init__(self):
        self.x = 1
        self.y = 2

class Slotted:
    __slots__ = ("x", "y")

    def __init__(self):
        self.x = 1
        self.y = 2

n = Normal()
s = Slotted()

print(sys.getsizeof(n))
print(sys.getsizeof(s))
```

Typical results:

```text
Normal object  ~56 bytes + __dict__
Slotted object ~48 bytes
```

Savings multiply across large numbers of objects.

---

# 15. Limitations of `__slots__`

While powerful, slots have limitations.

## Reduced Flexibility

Cannot dynamically add attributes.

---

## Complex Inheritance

Multiple inheritance can become difficult to manage.

---

## Compatibility Issues

Some frameworks expect `__dict__`.

Examples:

* Serialization libraries
* ORM frameworks
* Debugging tools

---

# 16. Best Practices

## Use `__slots__` When

* Creating millions of objects
* Objects have fixed attributes
* Memory efficiency is critical

---

## Avoid `__slots__` When

* Objects need dynamic attributes
* Frameworks rely on `__dict__`
* Flexibility is more important

---

# 17. Real Example: Memory-Efficient Data Model

## Without Slots

```python
class Employee:
    def __init__(self, id, name, salary):
        self.id = id
        self.name = name
        self.salary = salary
```

---

## Optimized Version

```python
class Employee:
    __slots__ = ("id", "name", "salary")

    def __init__(self, id, name, salary):
        self.id = id
        self.name = name
        self.salary = salary
```

This significantly reduces memory usage in large systems.

---

# Final Summary

`__slots__` is a memory optimization mechanism in Python.

| Feature            | Normal Class            | Slotted Class  |
| ------------------ | ----------------------- | -------------- |
| Attribute Storage  | Dictionary (`__dict__`) | Fixed Slots    |
| Memory Usage       | Higher                  | Lower          |
| Dynamic Attributes | Allowed                 | Restricted     |
| Attribute Lookup   | Hash Lookup             | Direct Pointer |
| Flexibility        | High                    | Limited        |

---

## Core Benefits

* Reduces memory usage
* Speeds up attribute access
* Prevents accidental attributes
* Improves large-scale object efficiency

---

## Key Takeaway

`__slots__` is most beneficial when working with large numbers of objects that have a fixed set of attributes. It trades flexibility for memory efficiency and can significantly improve the performance characteristics of memory-intensive applications.
