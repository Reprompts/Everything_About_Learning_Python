# Python OOPs: Chpater 17 Data Model & Object Protocol

The Python Data Model defines how objects behave within the Python language. It specifies the mechanisms that allow objects to interact with the interpreter, operators, built-in functions, and language constructs.

Through the data model, Python allows developers to customize the behavior of objects by implementing special methods (often called **magic methods** or **dunder methods**). These methods form the **object protocol**, which determines how objects respond to operations such as:

- Arithmetic
- Comparisons
- Iteration
- Attribute access
- Function calls
- Container operations

Understanding the Python data model is essential for designing fully integrated Python objects that behave naturally with the language.

This tutorial covers:

- The Python data model
- Behavior customization
- Object protocol methods
- Operator protocols
- Practical examples

---

# The Python Data Model

The Python data model is the formal specification of how objects interact with the Python interpreter.

Every object in Python follows this model.

The data model defines:

- Object identity
- Object type
- Object value
- Special methods controlling behavior

Every Python object contains these fundamental properties.

---

## Object Identity

Identity refers to the unique memory address of an object.

### Example

```python
a = [1, 2, 3]
b = a

print(id(a))
print(id(b))
```

### Output

```python
140438283001920
140438283001920
```

Both variables reference the same object.

Identity is used internally to distinguish objects.

---

## Object Type

The type of an object determines its behavior and supported operations.

### Example

```python
x = 10
print(type(x))
```

### Output

```python
<class 'int'>
```

Each type defines its behavior through the data model methods.

---

## Object Value

The value represents the data stored in the object.

### Example

```python
x = 10
y = 20

print(x + y)
```

The addition operation internally calls special methods defined in the data model.

---

# Behavior Customization

One of the most powerful aspects of Python's data model is the ability to customize object behavior.

This is done by implementing special methods.

Special methods allow custom objects to behave like built-in types.

Examples include:

| Operation | Special Method |
|------------|---------------|
| Addition | `__add__` |
| Length | `__len__` |
| Index access | `__getitem__` |
| Function call | `__call__` |
| String representation | `__str__` |

These methods are automatically invoked by the interpreter.

---

## Example: Custom Object Behavior

```python
class Counter:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return Counter(self.value + other.value)

    def __str__(self):
        return f"Counter({self.value})"
```

### Usage

```python
c1 = Counter(5)
c2 = Counter(10)

result = c1 + c2

print(result)
```

### Output

```python
Counter(15)
```

The `+` operator calls `__add__()`.

---

# Object Protocol Methods

The object protocol defines how objects interact with Python's language features.

Protocol methods are special methods that allow objects to participate in language constructs.

Examples:

| Protocol | Method |
|-----------|---------|
| String conversion | `__str__` |
| Object representation | `__repr__` |
| Boolean evaluation | `__bool__` |
| Length protocol | `__len__` |
| Callable objects | `__call__` |
| Iteration | `__iter__` |

These protocols make objects compatible with built-in functions and syntax.

---

# String Representation Protocol

Objects should provide readable representations.

Two methods control this:

### `__str__`

User-friendly representation.

### `__repr__`

Developer representation.

### Example

```python
class Book:
    def __init__(self, title):
        self.title = title

    def __str__(self):
        return f"Book: {self.title}"

    def __repr__(self):
        return f"Book(title='{self.title}')"
```

### Usage

```python
b = Book("Python Guide")

print(b)
print(repr(b))
```

### Output

```python
Book: Python Guide
Book(title='Python Guide')
```

---

# Boolean Protocol

Objects can define how they behave in boolean contexts.

Method used:

```python
__bool__()
```

### Example

```python
class Wallet:
    def __init__(self, balance):
        self.balance = balance

    def __bool__(self):
        return self.balance > 0
```

### Usage

```python
w = Wallet(100)

if w:
    print("Wallet has money")
```

### Output

```python
Wallet has money
```

---

# Length Protocol

Objects that represent collections should implement the length protocol.

Method:

```python
__len__()
```

### Example

```python
class Playlist:
    def __init__(self, songs):
        self.songs = songs

    def __len__(self):
        return len(self.songs)
```

### Usage

```python
p = Playlist(["song1", "song2", "song3"])

print(len(p))
```

### Output

```python
3
```

---

# Callable Objects

Objects can behave like functions using the call protocol.

Method:

```python
__call__()
```

### Example

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, number):
        return number * self.factor
```

### Usage

```python
double = Multiplier(2)

print(double(5))
```

### Output

```python
10
```

The object behaves like a function.

---

# Operator Protocols

Operators in Python are implemented through special methods.

Each operator corresponds to a method.

| Operator | Method |
|-----------|---------|
| `+` | `__add__` |
| `-` | `__sub__` |
| `*` | `__mul__` |
| `/` | `__truediv__` |
| `==` | `__eq__` |
| `<` | `__lt__` |

When an operator is used, Python internally calls the corresponding method.

---

# Arithmetic Operator Protocol

### Example

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

    def __str__(self):
        return f"({self.x}, {self.y})"
```

### Usage

```python
v1 = Vector(2, 3)
v2 = Vector(5, 7)

v3 = v1 + v2

print(v3)
```

### Output

```python
(7, 10)
```

---

# Comparison Protocol

Objects can implement comparison operations.

Common methods:

- `__eq__`
- `__lt__`
- `__gt__`
- `__le__`
- `__ge__`
- `__ne__`

### Example

```python
class Product:
    def __init__(self, price):
        self.price = price

    def __lt__(self, other):
        return self.price < other.price
```

### Usage

```python
p1 = Product(100)
p2 = Product(200)

print(p1 < p2)
```

### Output

```python
True
```

---

# Container Protocol

Objects can behave like collections.

Methods used:

- `__getitem__`
- `__setitem__`
- `__delitem__`
- `__contains__`

### Example

```python
class ShoppingCart:
    def __init__(self):
        self.items = {}

    def __setitem__(self, key, value):
        self.items[key] = value

    def __getitem__(self, key):
        return self.items[key]

    def __contains__(self, item):
        return item in self.items
```

### Usage

```python
cart = ShoppingCart()

cart["apple"] = 3

print(cart["apple"])
print("apple" in cart)
```

### Output

```python
3
True
```

---

# Iteration Protocol

Objects can support iteration by implementing:

- `__iter__()`
- `__next__()`

### Example

```python
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

        raise StopIteration
```

### Usage

```python
for num in Counter(3):
    print(num)
```

### Output

```python
1
2
3
```

---

# Real-World Example: Custom Collection

```python
class Inventory:
    def __init__(self):
        self.items = {}

    def __setitem__(self, name, quantity):
        self.items[name] = quantity

    def __getitem__(self, name):
        return self.items[name]

    def __len__(self):
        return len(self.items)

    def __contains__(self, name):
        return name in self.items
```

### Usage

```python
inventory = Inventory()

inventory["Laptop"] = 5
inventory["Mouse"] = 20

print(len(inventory))
print("Laptop" in inventory)
```

### Output

```python
2
True
```

---

# Why the Python Data Model is Important

The Python data model provides several advantages.

## Language Integration

Custom objects work seamlessly with Python syntax.

## Flexible Behavior

Developers can define custom behavior for operators and built-in functions.

## Consistency

All objects follow a unified behavior specification.

## Extensibility

New types can be integrated into Python's ecosystem.

---

# Summary

The Python data model defines how objects behave and interact with the Python interpreter. It provides a framework that specifies object identity, type, and value, along with a set of special methods that control object behavior.

Through the data model, developers can customize how objects respond to operators, built-in functions, and language constructs. These behaviors are implemented through the object protocol, which includes methods such as `__str__`, `__len__`, `__call__`, and many others.

Operator protocols further allow objects to participate in arithmetic operations, comparisons, and container behaviors by implementing corresponding special methods like `__add__`, `__eq__`, and `__getitem__`.

By understanding and using the Python data model effectively, developers can create objects that behave naturally within the language, integrate seamlessly with Python's syntax, and support powerful abstractions in object-oriented program design.
