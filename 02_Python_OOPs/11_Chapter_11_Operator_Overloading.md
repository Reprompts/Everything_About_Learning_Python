# Python OOPs: Chapter 11 Operator Overloading

Operator overloading is an advanced feature of Python's object-oriented model that allows custom objects to define how standard operators behave when used with them.

In everyday programming, operators such as:

- `+`
- `-`
- `*`
- `/`
- `==`
- `<`
- `>`

are normally used with built-in data types like integers, floats, and strings. However, Python allows programmers to extend these operators so they work with user-defined objects as well.

This is accomplished by implementing special methods (also called **magic methods** or **dunder methods**, meaning double underscore methods).

Operator overloading is a form of **polymorphism**, where the same operator behaves differently depending on the objects involved.

---

# Why Operator Overloading Exists

Without operator overloading, working with complex objects would require explicit method calls.

Example without operator overloading:

```python
vector3 = vector1.add(vector2)
```

With operator overloading:

```python
vector3 = vector1 + vector2
```

The second approach is more intuitive and readable.

Operator overloading allows user-defined classes to behave like built-in types.

---

# Special Methods for Operators

Python internally maps operators to special methods.

| Operator | Special Method | Description |
|-----------|----------------|-------------|
| `+` | `__add__` | Addition |
| `-` | `__sub__` | Subtraction |
| `*` | `__mul__` | Multiplication |
| `/` | `__truediv__` | True Division |
| `==` | `__eq__` | Equal To |
| `<` | `__lt__` | Less Than |
| `>` | `__gt__` | Greater Than |
| `<=` | `__le__` | Less Than or Equal To |
| `>=` | `__ge__` | Greater Than or Equal To |
| `!=` | `__ne__` | Not Equal To |
| `-` (unary) | `__neg__` | Unary Negation |
| `+` (unary) | `__pos__` | Unary Positive |
| `+=` | `__iadd__` | In-place Addition |

When an operator is used with objects, Python automatically calls the corresponding special method.

Example:

```python
a + b
```

Python internally executes:

```python
a.__add__(b)
```

---

# Basic Example of Operator Overloading

Consider a simple class representing a 2D vector.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

Without operator overloading:

```python
def add(v1, v2):
    return Vector(v1.x + v2.x, v1.y + v2.y)
```

With operator overloading:

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(
            self.x + other.x,
            self.y + other.y
        )
```

Usage:

```python
v1 = Vector(2, 3)
v2 = Vector(4, 5)

v3 = v1 + v2

print(v3.x, v3.y)
```

Output:

```text
6 8
```

The `+` operator now works with `Vector` objects.

---

# Arithmetic Operator Overloading

Arithmetic operators can be customized using special methods.

## Common Arithmetic Methods

| Operator | Method | Description |
|-----------|---------|-------------|
| `+` | `__add__` | Addition |
| `-` | `__sub__` | Subtraction |
| `*` | `__mul__` | Multiplication |
| `/` | `__truediv__` | True Division |
| `//` | `__floordiv__` | Floor Division |
| `%` | `__mod__` | Modulus |
| `**` | `__pow__` | Power / Exponent |

### Example: Arithmetic Operators

```python
class Number:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return Number(self.value + other.value)

    def __sub__(self, other):
        return Number(self.value - other.value)

    def __mul__(self, other):
        return Number(self.value * other.value)

    def __truediv__(self, other):
        return Number(self.value / other.value)

    def show(self):
        print(self.value)
```

Usage:

```python
a = Number(20)
b = Number(5)

c = a + b
d = a - b
e = a * b
f = a / b

c.show()
d.show()
e.show()
f.show()
```

Output:

```text
25
15
100
4.0
```

---

# Comparison Operator Overloading

Comparison operators determine relationships between objects.

## Comparison Methods

| Operator | Method |
|-----------|---------|
| `==` | `__eq__` |
| `!=` | `__ne__` |
| `<` | `__lt__` |
| `>` | `__gt__` |
| `<=` | `__le__` |
| `>=` | `__ge__` |

### Example: Comparison Operators

```python
class Student:
    def __init__(self, marks):
        self.marks = marks

    def __eq__(self, other):
        return self.marks == other.marks

    def __lt__(self, other):
        return self.marks < other.marks

    def __gt__(self, other):
        return self.marks > other.marks
```

Usage:

```python
s1 = Student(85)
s2 = Student(90)

print(s1 == s2)
print(s1 < s2)
print(s1 > s2)
```

Output:

```text
False
True
False
```

The comparison operators now compare student marks.

---

# Unary Operator Overloading

Unary operators operate on a single operand.

Examples include:

- `-a`
- `+a`
- `~a`

## Unary Methods

| Operator | Method |
|-----------|---------|
| `-` | `__neg__` |
| `+` | `__pos__` |
| `~` | `__invert__` |

### Example: Unary Operator

```python
class Temperature:
    def __init__(self, value):
        self.value = value

    def __neg__(self):
        return Temperature(-self.value)

    def show(self):
        print(self.value)
```

Usage:

```python
t = Temperature(25)

cold = -t

cold.show()
```

Output:

```text
-25
```

The unary `-` operator changes the temperature sign.

---

# In-place Operator Overloading

In-place operators modify objects directly rather than creating new objects.

Examples:

- `+=`
- `-=`
- `*=`
- `/=`

## In-place Methods

| Operator | Method |
|-----------|---------|
| `+=` | `__iadd__` |
| `-=` | `__isub__` |
| `*=` | `__imul__` |
| `/=` | `__itruediv__` |

### Example: In-place Operator

```python
class Counter:
    def __init__(self, value):
        self.value = value

    def __iadd__(self, other):
        self.value += other
        return self
```

Usage:

```python
c = Counter(10)

c += 5

print(c.value)
```

Output:

```text
15
```

The object itself is modified.

---

# Custom Object Arithmetic

Operator overloading allows complex domain-specific objects to behave naturally.

Example: Vector Mathematics.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(
            self.x + other.x,
            self.y + other.y
        )

    def __sub__(self, other):
        return Vector(
            self.x - other.x,
            self.y - other.y
        )

    def __mul__(self, scalar):
        return Vector(
            self.x * scalar,
            self.y * scalar
        )

    def __str__(self):
        return f"Vector({self.x}, {self.y})"
```

Usage:

```python
v1 = Vector(2, 3)
v2 = Vector(4, 5)

v3 = v1 + v2
v4 = v1 - v2
v5 = v1 * 3

print(v3)
print(v4)
print(v5)
```

Output:

```text
Vector(6, 8)
Vector(-2, -2)
Vector(6, 9)
```

This enables natural mathematical expressions with objects.

---

# Real-World Example: Money Class

```python
class Money:
    def __init__(self, amount):
        self.amount = amount

    def __add__(self, other):
        return Money(self.amount + other.amount)

    def __sub__(self, other):
        return Money(self.amount - other.amount)

    def __str__(self):
        return f"${self.amount}"
```

Usage:

```python
m1 = Money(50)
m2 = Money(30)

total = m1 + m2

print(total)
```

Output:

```text
$80
```

Financial values behave naturally with operators.

---

# Important Rules for Operator Overloading

## Operators Cannot Be Created

Python allows overloading existing operators, but new operators cannot be defined.

## Maintain Logical Meaning

Operators should behave intuitively.

Bad design example:

```python
__add__
```

performing subtraction.

## Return Correct Types

Operator methods usually return new objects rather than modifying existing ones.

## Handle Type Compatibility

Operator methods should check object compatibility.

Example:

```python
if not isinstance(other, Vector):
    return NotImplemented
```

---

# Best Practices

- Implement operator overloading only when it improves clarity and usability.
- Avoid excessive operator overloading that makes code confusing.
- Ensure overloaded operators behave consistently with their mathematical meaning.
- Document operator behavior clearly in class documentation.

---

# Summary

Operator overloading allows Python classes to define custom behavior for built-in operators. This is achieved by implementing special methods such as `__add__`, `__sub__`, `__eq__`, and others.

These methods enable objects to interact using familiar operators like `+`, `-`, `*`, and comparison operators. Python internally maps operator usage to these special methods.

Operator overloading supports arithmetic operations, comparisons, unary operations, and in-place modifications. It allows user-defined objects such as vectors, financial values, or mathematical structures to behave naturally with operators.

When used properly, operator overloading improves code readability, expressiveness, and usability, making custom objects integrate seamlessly with Python's syntax and programming style.
