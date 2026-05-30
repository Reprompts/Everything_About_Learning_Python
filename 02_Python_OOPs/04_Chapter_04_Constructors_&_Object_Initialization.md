# Python OOPs: Chapter 4 Constructors & Object Initialization

## Introduction

In Object-Oriented Programming, objects must be created and initialized properly before they can be used. Python provides mechanisms that automatically run when an object is created to initialize its attributes and prepare it for use.

This process involves constructors and object initialization mechanisms.

In Python, object creation involves two important special methods:

- `__new__()` – Responsible for creating the object
- `__init__()` – Responsible for initializing the object

Understanding how these methods work internally is essential for mastering Python's object model.

---

# The `__init__()` Method

The `__init__()` method is commonly referred to as the constructor in Python.

Technically, `__init__()` is **not the actual constructor**, but rather an **initializer** that runs immediately after an object is created.

Its purpose is to initialize the object's attributes.

### Basic Syntax

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

### Example Usage

```python
p = Person("Alice", 25)

print(p.name)
print(p.age)
```

### Output

```text
Alice
25
```

When the object `p` is created, Python automatically calls:

```python
Person.__init__(p, "Alice", 25)
```

The object `p` becomes the `self` parameter.

---

# Object Initialization Process

When an object is created in Python, a series of steps occur internally.

### Example

```python
obj = MyClass()
```

Python performs the following steps.

## Step 1: Call `__new__()`

Python first calls:

```python
MyClass.__new__(MyClass)
```

The `__new__()` method:

- Creates a new instance
- Allocates memory
- Returns the object

---

## Step 2: Call `__init__()`

After the object is created, Python calls:

```python
MyClass.__init__(obj)
```

The `__init__()` method:

- Initializes the object
- Sets attributes
- Prepares the object for use

---

## Step 3: Object Reference Assigned

The created object reference is assigned to the variable.

```text
obj → reference to new object
```

---

# Default Constructors

A default constructor is a constructor that takes no arguments except `self`.

It initializes objects with default values.

### Example

```python
class Car:
    def __init__(self):
        self.brand = "Unknown"
        self.speed = 0
```

### Creating Objects

```python
c1 = Car()
c2 = Car()

print(c1.brand, c1.speed)
```

### Output

```text
Unknown 0
```

All objects receive the same initial values.

---

# Practical Example of Default Constructor

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1
```

### Usage

```python
counter = Counter()

counter.increment()
counter.increment()

print(counter.count)
```

### Output

```text
2
```

The constructor ensures the counter starts at zero.

---

# Parameterized Constructors

A parameterized constructor accepts arguments that initialize object attributes.

### Example

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
```

### Creating Objects

```python
s1 = Student("Ravi", 85)
s2 = Student("Neha", 90)
```

Each object receives its own data.

### Conceptual Result

```text
s1 → Ravi, 85
s2 → Neha, 90
```

---

# Practical Example

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

### Usage

```python
acc = BankAccount("Alice", 1000)

acc.deposit(200)

print(acc.balance)
```

### Output

```text
1200
```

---

# Multiple Constructors (Patterns)

Unlike some languages, Python does not support multiple constructors directly.

However, similar behavior can be achieved using several patterns.

Common approaches include:

- Default arguments
- Class methods
- Variable arguments

---

## Pattern 1: Using Default Arguments

One constructor can behave differently depending on arguments.

### Example

```python
class Rectangle:
    def __init__(self, width=1, height=1):
        self.width = width
        self.height = height
```

### Usage

```python
r1 = Rectangle()
r2 = Rectangle(5, 10)
```

### Results

```text
r1 → width=1 height=1
r2 → width=5 height=10
```

---

## Pattern 2: Using Class Methods

Alternative constructors can be implemented using `@classmethod`.

### Example

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_birth_year(cls, name, year):
        age = 2025 - year
        return cls(name, age)
```

### Usage

```python
p = Person.from_birth_year("Alice", 2000)

print(p.name, p.age)
```

### Output

```text
Alice 25
```

---

## Pattern 3: Using Variable Arguments

Constructors can accept flexible parameters.

### Example

```python
class Point:
    def __init__(self, *args):
        if len(args) == 2:
            self.x = args[0]
            self.y = args[1]

        elif len(args) == 3:
            self.x = args[0]
            self.y = args[1]
            self.z = args[2]
```

### Usage

```python
p2 = Point(3, 4)
p3 = Point(3, 4, 5)
```

---

# Constructor Chaining

Constructor chaining occurs when one constructor calls another constructor.

This is common when using inheritance.

### Example

```python
class Animal:
    def __init__(self, name):
        self.name = name
```

### Child Class

```python
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
```

### Usage

```python
d = Dog("Buddy", "Labrador")

print(d.name, d.breed)
```

### Output

```text
Buddy Labrador
```

`super()` calls the parent class constructor.

---

# Why Constructor Chaining Is Important

Benefits:

- Avoids duplicate initialization code
- Ensures parent attributes are initialized
- Maintains inheritance hierarchy

---

# `__new__()` vs `__init__()`

Python object creation involves two separate methods:

| Method | Purpose |
|----------|----------|
| `__new__()` | Creates the object |
| `__init__()` | Initializes the object |

---

# The `__new__()` Method

`__new__()` is responsible for creating and returning a new instance.

It is a static method internally.

### Example

```python
class Example:
    def __new__(cls):
        print("Creating object")
        return super().__new__(cls)

    def __init__(self):
        print("Initializing object")
```

### Usage

```python
obj = Example()
```

### Output

```text
Creating object
Initializing object
```

### Execution Order

```text
__new__() → __init__()
```

---

# When to Override `__new__()`

Normally developers override `__init__()`, not `__new__()`.

`__new__()` is used in special cases:

- Immutable objects
- Implementing singletons
- Controlling instance creation
- Metaprogramming

---

## Example: Singleton Pattern

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)

        return cls._instance
```

### Usage

```python
a = Singleton()
b = Singleton()

print(a is b)
```

### Output

```text
True
```

Both variables reference the same object.

---

# Practical Example Combining Everything

```python
class User:
    def __new__(cls, *args, **kwargs):
        print("Allocating memory")
        return super().__new__(cls)

    def __init__(self, name, email):
        print("Initializing object")
        self.name = name
        self.email = email

    @classmethod
    def from_string(cls, data):
        name, email = data.split(",")
        return cls(name, email)
```

### Usage

```python
u1 = User("Alice", "alice@email.com")

u2 = User.from_string(
    "Bob,bob@email.com"
)
```

### Output

```text
Allocating memory
Initializing object

Allocating memory
Initializing object
```

---

# Object Creation Flow Diagram

```text
User Creates Object
        │
        ▼
      __new__()
(Create & Allocate Memory)
        │
        ▼
      __init__()
(Initialize Attributes)
        │
        ▼
 Object Ready To Use
```

---

# Summary

Constructors and object initialization are fundamental mechanisms that prepare objects for use in Python programs.

### Key Points

- `__init__()` initializes object attributes.
- Object creation follows the sequence:
  
  ```text
  __new__() → __init__()
  ```

- Default constructors initialize objects with preset values.
- Parameterized constructors allow flexible initialization.
- Python simulates multiple constructors using:
  - Default parameters
  - Class methods
  - Variable arguments
- Constructor chaining enables proper initialization in inheritance hierarchies.
- `__new__()` controls instance creation, while `__init__()` configures the instance.

Understanding these mechanisms provides deep insight into how Python objects are created, initialized, and managed in memory, which is essential for advanced object-oriented programming and framework development.
