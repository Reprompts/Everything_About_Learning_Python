# Python OOPs: Chapter 12 Magic Methods

Magic methods, also known as **special methods** or **dunder methods** (double underscore methods), are methods that begin and end with double underscores:

```python
__methodname__
```

These methods allow Python classes to integrate with Python's internal behavior and syntax. They define how objects interact with:

- Built-in functions
- Operators
- Object creation
- Attribute access
- Container behavior
- String representation

Magic methods are automatically invoked by Python when certain operations occur.

Example:

```python
a + b
```

Python internally calls:

```python
a.__add__(b)
```

Because of these methods, user-defined objects can behave like built-in Python types.

Magic methods power many advanced Python features including:

- Operator overloading
- Iteration
- Container objects
- Custom attribute handling
- Callable objects

---

# Object Creation Methods

Object creation in Python occurs in two stages:

1. Memory allocation
2. Object initialization

Two special methods control this process:

- `__new__`
- `__init__`

A third method, `__del__`, is involved in object destruction.

---

## `__new__` – Object Creation

`__new__` is responsible for creating a new instance of a class.

It is the first method executed when a new object is created.

### Syntax

```python
__new__(cls, ...)
```

### Parameters

- `cls` → The class being instantiated
- Additional arguments passed during object creation

### Example

```python
class Example:
    def __new__(cls):
        print("Creating instance")
        return super().__new__(cls)

    def __init__(self):
        print("Initializing instance")
```

Usage:

```python
obj = Example()
```

Output:

```text
Creating instance
Initializing instance
```

The process:

1. `__new__` creates the object
2. `__init__` initializes the object

---

## Practical Use of `__new__`

`__new__` is mainly used for:

- Immutable object customization
- Singleton patterns
- Controlling instance creation

### Example: Singleton Pattern

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

Usage:

```python
a = Singleton()
b = Singleton()

print(a is b)
```

Output:

```text
True
```

Both variables reference the same object.

---

## `__init__` – Object Initialization

`__init__` initializes the object after it has been created.

It is commonly referred to as the constructor, although technically `__new__` is the true constructor.

### Example

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Usage:

```python
p = Person("Alice", 25)

print(p.name)
print(p.age)
```

Output:

```text
Alice
25
```

---

## `__del__` – Object Destruction

`__del__` is called when an object is about to be destroyed by the garbage collector.

### Example

```python
class Demo:
    def __del__(self):
        print("Object destroyed")
```

Usage:

```python
d = Demo()
del d
```

Output:

```text
Object destroyed
```

> **Important:** Python's garbage collector controls destruction timing, so `__del__` should not be relied upon for critical logic.

---

# String Representation Methods

These methods define how objects are represented as strings.

Two key methods exist:

- `__str__`
- `__repr__`

---

## `__str__` – User-Friendly Representation

`__str__` returns a readable string for end users.

It is used by:

- `print(object)`
- `str(object)`

### Example

```python
class Book:
    def __init__(self, title):
        self.title = title

    def __str__(self):
        return f"Book: {self.title}"
```

Usage:

```python
b = Book("Python Programming")

print(b)
```

Output:

```text
Book: Python Programming
```

---

## `__repr__` – Developer Representation

`__repr__` returns a detailed representation intended for developers.

Used by:

- `repr(object)`

### Example

```python
class Book:
    def __init__(self, title):
        self.title = title

    def __repr__(self):
        return f"Book('{self.title}')"
```

Usage:

```python
b = Book("Python")

print(repr(b))
```

Output:

```text
Book('Python')
```

### Best Practice

`__repr__` should ideally produce a string that can recreate the object.

---

# Comparison Methods

These methods define how objects compare with each other.

| Method | Operator |
|----------|----------|
| `__eq__` | `==` |
| `__ne__` | `!=` |
| `__lt__` | `<` |
| `__le__` | `<=` |
| `__gt__` | `>` |
| `__ge__` | `>=` |

### Example

```python
class Product:
    def __init__(self, price):
        self.price = price

    def __eq__(self, other):
        return self.price == other.price

    def __lt__(self, other):
        return self.price < other.price
```

Usage:

```python
p1 = Product(100)
p2 = Product(200)

print(p1 == p2)
print(p1 < p2)
```

Output:

```text
False
True
```

---

# Arithmetic Methods

These methods define how objects behave with arithmetic operators.

| Method | Operator |
|----------|----------|
| `__add__` | `+` |
| `__sub__` | `-` |
| `__mul__` | `*` |
| `__truediv__` | `/` |
| `__floordiv__` | `//` |
| `__mod__` | `%` |
| `__pow__` | `**` |

### Example

```python
class Number:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return Number(self.value + other.value)

    def __mul__(self, other):
        return Number(self.value * other.value)
```

Usage:

```python
a = Number(5)
b = Number(3)

c = a + b
d = a * b

print(c.value)
print(d.value)
```

Output:

```text
8
15
```

---

# Unary Methods

Unary operators work with a single operand.

| Method | Operator |
|----------|----------|
| `__neg__` | `-` |
| `__pos__` | `+` |
| `__abs__` | `abs()` |

### Example

```python
class Temperature:
    def __init__(self, value):
        self.value = value

    def __neg__(self):
        return Temperature(-self.value)
```

Usage:

```python
t = Temperature(30)

cold = -t

print(cold.value)
```

Output:

```text
-30
```

---

# Container Methods

These methods allow objects to behave like lists or dictionaries.

| Method | Operation | Description |
|----------|----------|----------|
| `__len__` | `len(object)` | Returns length of object |
| `__getitem__` | `object[key]` | Access item using key/index |
| `__setitem__` | `object[key] = value` | Set item using key/index |
| `__delitem__` | `del object[key]` | Delete item using key/index |
| `__contains__` | `value in object` | Membership test |

### Example

```python
class MyList:
    def __init__(self):
        self.data = []

    def __len__(self):
        return len(self.data)

    def __getitem__(self, index):
        return self.data[index]

    def __setitem__(self, index, value):
        self.data[index] = value
```

Usage:

```python
ml = MyList()
ml.data = [10, 20, 30]

print(len(ml))
print(ml[1])
```

Output:

```text
3
20
```

---

## `__contains__`

Controls the `in` operator.

### Example

```python
class Bag:
    def __init__(self, items):
        self.items = items

    def __contains__(self, item):
        return item in self.items
```

Usage:

```python
bag = Bag(["apple", "banana"])

print("apple" in bag)
```

Output:

```text
True
```

---

# Callable Objects

Objects can behave like functions using `__call__`.

### Example

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, number):
        return number * self.factor
```

Usage:

```python
double = Multiplier(2)

print(double(5))
```

Output:

```text
10
```

The object behaves like a function.

---

# Attribute Access Methods

These methods control how attributes are accessed and modified.

| Method | Purpose |
|----------|----------|
| `__getattr__` | Called when attribute not found |
| `__setattr__` | Called when attribute is assigned |
| `__delattr__` | Called when attribute is deleted |
| `__getattribute__` | Called for every attribute access |

---

## `__getattr__`

Called only when the attribute does not exist.

### Example

```python
class Demo:
    def __getattr__(self, name):
        return "Attribute not found"
```

Usage:

```python
d = Demo()

print(d.unknown)
```

Output:

```text
Attribute not found
```

---

## `__setattr__`

Intercepts attribute assignment.

### Example

```python
class Demo:
    def __setattr__(self, name, value):
        print("Setting attribute:", name)
        super().__setattr__(name, value)
```

Usage:

```python
d = Demo()
d.x = 10
```

Output:

```text
Setting attribute: x
```

---

## `__delattr__`

Handles attribute deletion.

### Example

```python
class Demo:
    def __delattr__(self, name):
        print("Deleting attribute:", name)
        super().__delattr__(name)
```

Usage:

```python
d = Demo()
d.x = 5

del d.x
```

---

## `__getattribute__`

Called for every attribute access.

### Example

```python
class Demo:
    def __getattribute__(self, name):
        print("Accessing:", name)
        return super().__getattribute__(name)
```

Usage:

```python
d = Demo()
d.x = 10

print(d.x)
```

Output:

```text
Accessing: x
10
```

Because it intercepts all access, it must use `super()` carefully to avoid infinite recursion.

---

# Summary

Magic methods are special methods in Python that allow classes to integrate with Python's internal operations and syntax. These methods define how objects behave during creation, destruction, printing, comparison, arithmetic operations, container access, and attribute management.

The object lifecycle is controlled by methods such as `__new__`, `__init__`, and `__del__`. String representation is handled through `__str__` and `__repr__`, enabling readable and developer-oriented object representations.

Comparison and arithmetic methods allow objects to participate in logical and mathematical operations, while unary methods define behavior for single-operand operations. Container methods enable objects to behave like sequences or mappings.

Callable objects use the `__call__` method to behave like functions, and attribute access methods such as `__getattr__`, `__setattr__`, and `__getattribute__` allow deep customization of attribute management.

Together, magic methods form the foundation of Python's flexibility, enabling developers to build powerful, expressive, and highly customizable object-oriented systems.
