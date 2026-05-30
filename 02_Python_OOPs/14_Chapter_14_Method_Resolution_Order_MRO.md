# Python OOPs: Chapter 14 Method Resolution Order - MRO

When working with object-oriented programming, especially with inheritance and multiple inheritance, Python must determine which method or attribute should be used when multiple classes define the same name.

This decision process is called **Method Resolution Order (MRO)**.

MRO defines the order in which Python searches classes for methods and attributes.

Understanding MRO is critical when dealing with:

- Multiple inheritance
- Method overriding
- `super()`
- Attribute lookup
- Complex class hierarchies

This tutorial explains in depth:

- Attribute lookup process
- Method Resolution Order (MRO)
- C3 Linearization algorithm
- Diamond inheritance problem
- `mro()` method
- `__mro__` attribute
- Practical examples

---

# Attribute Lookup Process in Python

Before understanding MRO, it is important to understand how Python finds attributes and methods.

When you access an attribute like:

```python
obj.attribute
```

Python performs a lookup process.

The search order is:

1. Instance namespace
2. Class namespace
3. Parent classes (following MRO)
4. Base class `object`

---

## Example

```python
class Person:
    species = "Human"

    def speak(self):
        print("Person speaking")


p = Person()

print(p.species)
p.speak()
```

### Lookup Steps

When Python evaluates:

```python
p.species
```

It searches:

```text
p.__dict__
Person.__dict__
Parent classes
```

Since `species` exists in the class, Python returns it.

---

# Instance Attribute vs Class Attribute

```python
class Person:
    role = "Human"


p = Person()

p.role = "Developer"

print(p.role)
print(Person.role)
```

### Output

```text
Developer
Human
```

### Why?

Because attribute lookup first checks:

```text
Instance attributes
        ↓
Class attributes
```

The instance attribute shadows the class attribute.

---

# What is Method Resolution Order (MRO)?

**Method Resolution Order (MRO)** defines the order in which Python searches classes for methods and attributes when inheritance is involved.

For simple inheritance:

```text
Child → Parent → object
```

With multiple inheritance, determining the order becomes more complex.

---

# Simple Inheritance MRO

### Example

```python
class A:
    def show(self):
        print("A")


class B(A):
    pass


b = B()
b.show()
```

Python searches:

```text
B → A → object
```

Since `show()` exists in `A`, it is executed.

---

# Viewing MRO

Python allows inspection of MRO using:

```python
ClassName.mro()
```

### Example

```python
class A:
    pass


class B(A):
    pass


print(B.mro())
```

### Output

```python
[
    <class '__main__.B'>,
    <class '__main__.A'>,
    <class 'object'>
]
```

---

# Multiple Inheritance

Multiple inheritance occurs when a class inherits from multiple parent classes.

### Example

```python
class A:
    def show(self):
        print("A")


class B:
    def show(self):
        print("B")


class C(A, B):
    pass


c = C()
c.show()
```

### Output

```text
A
```

### Why?

Python follows this MRO:

```text
C → A → B → object
```

Since `show()` exists in `A`, Python stops searching.

---

# Diamond Inheritance Problem

One of the classic inheritance issues is the **Diamond Problem**.

It occurs when two classes inherit from the same base class and another class inherits from both.

### Structure

```text
        A
       / \
      B   C
       \ /
        D
```

### Example

```python
class A:
    def greet(self):
        print("Hello from A")


class B(A):
    pass


class C(A):
    pass


class D(B, C):
    pass


d = D()
d.greet()
```

Question:

```text
Which path should Python follow?

D → B → A
or
D → C → A
```

Python resolves this using **C3 Linearization**.

---

# C3 Linearization

Python uses an algorithm called **C3 Linearization** to determine the MRO.

The algorithm guarantees three important properties.

---

## 1. Local Precedence Order

The order of parent classes listed in the class definition is preserved.

Example:

```python
class D(B, C):
    pass
```

Means:

```text
B comes before C
```

---

## 2. Monotonicity

The ordering of classes remains consistent throughout the hierarchy.

A child class cannot violate the ordering established by its parents.

---

## 3. No Duplicate Classes

Each class appears only once in the final MRO.

---

# MRO Example with Diamond Problem

```python
class A:
    def greet(self):
        print("A")


class B(A):
    pass


class C(A):
    pass


class D(B, C):
    pass
```

Inspect MRO:

```python
print(D.mro())
```

### Output

```python
[
    D,
    B,
    C,
    A,
    object
]
```

### Lookup Order

```text
D → B → C → A → object
```

Notice that `A` appears only once.

---

# Practical Diamond Example with Methods

```python
class A:
    def process(self):
        print("Processing in A")


class B(A):
    def process(self):
        print("Processing in B")


class C(A):
    def process(self):
        print("Processing in C")


class D(B, C):
    pass


d = D()
d.process()
```

### Output

```text
Processing in B
```

### Why?

MRO:

```text
D → B → C → A → object
```

Python finds `process()` in `B` first.

---

# MRO with `super()`

`super()` follows the MRO order.

### Example

```python
class A:
    def greet(self):
        print("A")


class B(A):
    def greet(self):
        print("B")
        super().greet()


class C(A):
    def greet(self):
        print("C")
        super().greet()


class D(B, C):
    def greet(self):
        print("D")
        super().greet()
```

### Usage

```python
d = D()
d.greet()
```

### Output

```text
D
B
C
A
```

### Execution Flow

```text
D
 ↓
B
 ↓
C
 ↓
A
```

Each class calls `super()` to continue along the MRO chain.

---

# Visualizing `super()`

MRO:

```text
[D, B, C, A, object]
```

Execution:

```text
D.greet()
    ↓
B.greet()
    ↓
C.greet()
    ↓
A.greet()
```

This is called **cooperative multiple inheritance**.

---

# `mro()` Method

Python provides the `mro()` method to inspect the method resolution order.

### Syntax

```python
ClassName.mro()
```

### Example

```python
class A:
    pass


class B(A):
    pass


class C(B):
    pass


print(C.mro())
```

### Output

```python
[
    <class '__main__.C'>,
    <class '__main__.B'>,
    <class '__main__.A'>,
    <class 'object'>
]
```

This shows the exact search order Python uses.

---

# `__mro__` Attribute

Python also stores the MRO in a special class attribute.

### Example

```python
print(C.__mro__)
```

### Output

```python
(
    <class '__main__.C'>,
    <class '__main__.B'>,
    <class '__main__.A'>,
    <class 'object'>
)
```

---

# Difference Between `mro()` and `__mro__`

| Feature | `mro()` Method | `__mro__` Attribute |
|----------|---------------|--------------------|
| Type | Method | Attribute |
| Return Type | List | Tuple |
| Access | `Class.mro()` | `Class.__mro__` |
| Mutability | List (returned object) | Immutable tuple |
| Purpose | Computes/returns MRO | Stores MRO internally |
| Example | `A.mro()` | `A.__mro__` |

Both display the same resolution order.

---

# Real Example: Logging System

Consider multiple logging behaviors.

```python
class Logger:
    def log(self):
        print("Logging message")


class FileLogger(Logger):
    def log(self):
        print("File logging")
        super().log()


class DatabaseLogger(Logger):
    def log(self):
        print("Database logging")
        super().log()


class ApplicationLogger(FileLogger, DatabaseLogger):
    pass
```

### Usage

```python
logger = ApplicationLogger()
logger.log()
```

### Output

```text
File logging
Database logging
Logging message
```

### MRO

```text
ApplicationLogger
    ↓
FileLogger
    ↓
DatabaseLogger
    ↓
Logger
    ↓
object
```

Each method cooperates through `super()`.

---

# Inspecting MRO in Practice

```python
print(ApplicationLogger.mro())
```

### Output

```python
[
    ApplicationLogger,
    FileLogger,
    DatabaseLogger,
    Logger,
    object
]
```

Inspecting MRO is one of the best debugging tools for complex inheritance hierarchies.

---

# Common MRO Pitfalls

## Direct Parent Calls

Avoid:

```python
Parent.method(self)
```

This bypasses MRO.

Prefer:

```python
super().method()
```

---

## Forgetting Cooperative Inheritance

Bad:

```python
class B(A):
    def greet(self):
        print("B")
```

Good:

```python
class B(A):
    def greet(self):
        print("B")
        super().greet()
```

Without `super()`, the MRO chain stops.

---

## Complex Multiple Inheritance

Avoid unnecessary inheritance trees like:

```text
A
├── B
├── C
├── D
└── E
      \
       F
```

Complex hierarchies are harder to reason about and debug.

---

# Best Practices for Working with MRO

### Use `super()` Instead of Direct Parent Calls

```python
super().method()
```

---

### Design Cooperative Classes

Every class in a multiple inheritance hierarchy should call `super()` when appropriate.

---

### Keep Inheritance Hierarchies Simple

Deep and complex inheritance trees increase confusion.

---

### Inspect MRO Frequently

Use:

```python
ClassName.mro()
```

when debugging inheritance behavior.

---

### Understand the Diamond Problem

Always be aware of how methods will be resolved when multiple parents share common ancestors.

---

# Summary

**Method Resolution Order (MRO)** defines the order in which Python searches for methods and attributes in a class hierarchy. This becomes particularly important in multiple inheritance scenarios.

Python resolves this order using the **C3 Linearization Algorithm**, which ensures consistent and predictable lookup behavior. The algorithm preserves local precedence order, avoids duplicate classes, and maintains hierarchy consistency.

The classic **Diamond Inheritance Problem** is solved by MRO, allowing Python to determine a single, unambiguous path for method and attribute lookup.

Developers can inspect MRO using:

```python
ClassName.mro()
```

or

```python
ClassName.__mro__
```

Understanding MRO is essential for designing robust class hierarchies, using `super()` correctly, and avoiding subtle inheritance bugs in Python applications.

---

# Key Takeaways

- MRO determines how Python searches for methods and attributes.
- Attribute lookup starts from the instance and then follows the class hierarchy.
- Python uses the **C3 Linearization Algorithm**.
- Multiple inheritance relies heavily on MRO.
- The diamond inheritance problem is solved through MRO.
- `super()` follows the MRO chain.
- `ClassName.mro()` displays the lookup order.
- `ClassName.__mro__` stores the MRO internally.
- Cooperative inheritance requires proper use of `super()`.
- Understanding MRO helps prevent complex inheritance bugs.
