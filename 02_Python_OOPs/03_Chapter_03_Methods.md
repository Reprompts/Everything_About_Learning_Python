# Python OOPs: Chapter 3 Methods in Python Classes

## Introduction

In Object-Oriented Programming, methods define the behavior of objects. While attributes store data inside an object, methods define what actions the object can perform.

In Python, methods are simply functions defined inside a class. However, Python supports several types of methods, each designed for different responsibilities.

The primary method categories are:

- Instance Methods
- Class Methods
- Static Methods

In addition, Python supports method visibility conventions such as:

- Public Methods
- Protected Methods
- Private Methods

Understanding how these method types work is essential for designing clean, maintainable, and reusable object-oriented code.

---

# Instance Methods

An instance method is the most common type of method in Python classes. It operates on a specific instance (object) of a class.

Instance methods can:

- Access instance attributes
- Modify object state
- Perform operations related to the object's data

Each object has its own copy of instance attributes, so instance methods operate on individual object data.

---

## Instance Method Definition

Instance methods are defined inside a class like normal functions but always include `self` as the first parameter.

### Example

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print("Hello, my name is", self.name)
```

### Explanation

- `greet()` is an instance method
- It operates on a specific object
- It accesses the attribute `self.name`

---

## Calling Instance Methods

Instance methods are called through objects.

### Example

```python
p1 = Person("Alice", 25)
p2 = Person("Bob", 30)

p1.greet()
p2.greet()
```

### Output

```text
Hello, my name is Alice
Hello, my name is Bob
```

Each object uses its own data.

---

# The `self` Parameter

The `self` parameter refers to the current instance of the class.

It allows methods to access and modify object data.

### Important Characteristics

- `self` is not a keyword
- It is a naming convention
- It must be the first parameter in instance methods

### Example

```python
class Car:
    def __init__(self, brand, speed):
        self.brand = brand
        self.speed = speed

    def accelerate(self):
        self.speed += 10
```

### Usage

```python
car = Car("Tesla", 100)

car.accelerate()

print(car.speed)
```

### Output

```text
110
```

Behind the scenes:

```python
car.accelerate()
```

is equivalent to:

```python
Car.accelerate(car)
```

The object is passed automatically as the first argument.

---

# Accessing Instance Data

Instance methods can access instance attributes using `self`.

### Example

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def display(self):
        print("Name:", self.name)
        print("Marks:", self.marks)
```

### Usage

```python
s = Student("Ravi", 85)

s.display()
```

### Output

```text
Name: Ravi
Marks: 85
```

---

# Modifying Instance Data

Instance methods can also modify object state.

### Example

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

### Usage

```python
account = BankAccount(1000)

account.deposit(200)

print(account.balance)
```

### Output

```text
1200
```

---

# Class Methods

A class method operates on the class itself rather than on individual objects.

Class methods are useful when working with class-level data or behavior shared by all instances.

### Key Features

- Access class attributes
- Modify class state
- Create alternative constructors

---

## `@classmethod` Decorator

Class methods are declared using the `@classmethod` decorator.

### Example

```python
class Employee:
    company = "TechCorp"

    @classmethod
    def change_company(cls, new_name):
        cls.company = new_name
```

### Explanation

- `cls` refers to the class itself
- `company` is a class attribute

---

# The `cls` Parameter

`cls` is similar to `self`, but instead of referring to the instance, it refers to the class.

### Example

```python
class Example:
    value = 10

    @classmethod
    def show(cls):
        print(cls.value)
```

### Usage

```python
Example.show()
```

### Output

```text
10
```

---

# Class-Level Operations

Class methods are commonly used for operations that involve shared class state.

### Example

```python
class Product:
    tax_rate = 0.18

    def __init__(self, price):
        self.price = price

    @classmethod
    def set_tax_rate(cls, rate):
        cls.tax_rate = rate
```

### Usage

```python
Product.set_tax_rate(0.20)
```

All objects now use the new tax rate.

---

# Class Method as Alternative Constructor

Class methods can create objects in different ways.

### Example

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_birth_year(cls, name, birth_year):
        age = 2025 - birth_year
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

# Static Methods

Static methods belong to a class but do not depend on instance or class data.

They behave like normal functions but are logically grouped within the class.

### Static Methods

- Do not receive `self`
- Do not receive `cls`
- Used for utility functions related to the class

---

## `@staticmethod` Decorator

Static methods are defined using the `@staticmethod` decorator.

### Example

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
```

### Usage

```python
result = MathUtils.add(3, 5)

print(result)
```

### Output

```text
8
```

---

# Utility Functions Inside Classes

Static methods are useful when a function logically belongs to a class but does not require instance data.

### Example

```python
class TemperatureConverter:
    @staticmethod
    def celsius_to_fahrenheit(c):
        return (c * 9 / 5) + 32
```

### Usage

```python
print(
    TemperatureConverter.celsius_to_fahrenheit(30)
)
```

### Output

```text
86
```

---

# Comparison of Method Types

| Method Type | First Parameter | Access Instance Data | Access Class Data |
|------------|----------------|----------------------|-------------------|
| Instance Method | `self` | Yes | Yes |
| Class Method | `cls` | No | Yes |
| Static Method | None | No | No |

---

# Special Method Types (Visibility in Python)

Unlike languages such as Java or C++, Python does not enforce strict access control.

Instead, Python follows naming conventions to indicate method visibility.

Three common method types exist:

- Public Methods
- Protected Methods
- Private Methods

---

# Public Methods

Public methods are accessible from anywhere.

They have no special naming convention.

### Example

```python
class Calculator:
    def add(self, a, b):
        return a + b
```

### Usage

```python
calc = Calculator()

print(calc.add(3, 4))
```

### Output

```text
7
```

Public methods are the default method type.

---

# Protected Methods

Protected methods are intended for internal use within the class or subclasses.

They use a single underscore prefix.

### Example

```python
class Example:
    def _internal_method(self):
        print("This is a protected method")
```

### Usage

```python
obj = Example()

obj._internal_method()
```

Although accessible, the underscore signals:

> "This method is meant for internal use."

---

# Private Methods

Private methods are meant to be used only inside the class.

They use a double underscore prefix.

### Example

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def __calculate_fee(self):
        return self.balance * 0.02
```

### Usage Inside the Class

```python
def withdraw(self, amount):
    fee = self.__calculate_fee()
```

---

# Name Mangling

Python modifies private method names internally to avoid accidental access.

### Example

```python
__calculate_fee
```

becomes:

```python
_BankAccount__calculate_fee
```

### Accessing It Manually

```python
account._BankAccount__calculate_fee()
```

However, this is discouraged.

---

# Practical Example Combining All Method Types

```python
class BankAccount:
    bank_name = "National Bank"

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        fee = self.__calculate_fee()

        if amount + fee <= self.balance:
            self.balance -= (amount + fee)

    @classmethod
    def change_bank(cls, new_name):
        cls.bank_name = new_name

    @staticmethod
    def bank_policy():
        print("Minimum balance required is 100")

    def __calculate_fee(self):
        return self.balance * 0.01
```

### Usage

```python
acc = BankAccount("Alice", 1000)

acc.deposit(200)
acc.withdraw(100)

BankAccount.bank_policy()
BankAccount.change_bank("Global Bank")
```

---

# Internal View of Method Types

```text
Class
│
├── Instance Methods
│     └── Work with object data
│
├── Class Methods
│     └── Work with class data
│
└── Static Methods
      └── Utility functions
```

---

# Best Practices

### Use Instance Methods When

- Working with object-specific data
- Modifying object state
- Accessing instance attributes

### Use Class Methods When

- Working with shared class data
- Creating alternative constructors
- Managing class-level configuration

### Use Static Methods When

- No access to object or class data is required
- Implementing utility/helper functions

### Visibility Guidelines

- Public methods for external APIs
- Protected methods for subclass/internal use
- Private methods for implementation details

---

# Summary

Methods define the behavior of objects in Python classes.

Python supports several types of methods:

### Instance Methods

- Operate on individual objects
- Use the `self` parameter
- Access and modify instance data

### Class Methods

- Operate on the class itself
- Use the `cls` parameter
- Useful for class-level operations and alternative constructors

### Static Methods

- Behave like regular functions
- Do not receive `self` or `cls`
- Used for utility operations

Python also follows naming conventions for method visibility:

### Public Methods

- Accessible everywhere

### Protected Methods

- Use a single underscore (`_`)
- Intended for internal or subclass use

### Private Methods

- Use double underscores (`__`)
- Name-mangled to prevent accidental access

Understanding these method types allows developers to design classes with clear responsibilities, proper data handling, and well-structured behavior in object-oriented Python programs.
