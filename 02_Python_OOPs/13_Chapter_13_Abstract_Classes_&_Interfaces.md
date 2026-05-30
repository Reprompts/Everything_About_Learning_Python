# Python OOPs: Chapter 13 Abstract Classes & Interfaces

In object-oriented programming, **abstract classes** and **interfaces** are used to define common structures and behaviors that multiple classes must follow.

These mechanisms are essential for designing large, maintainable, and extensible systems. They ensure that different classes follow a consistent contract, meaning certain methods must exist and be implemented.

Python implements abstraction primarily through the **Abstract Base Class (ABC)** mechanism, provided by the built-in `abc` module.

This article covers:

- Abstract base classes
- The `abc` module
- The `ABC` class
- The `@abstractmethod` decorator
- Enforcing method implementation
- Interface-like behavior in Python

---

# Concept of Abstraction

Before discussing abstract classes, it is important to understand **abstraction**.

Abstraction is the process of hiding internal implementation details while exposing only the necessary functionality.

For example:

Consider a payment processing system.

Every payment type must implement a method called:

```python
process_payment()
```

But the internal logic differs:

- Credit card payment
- PayPal payment
- Bank transfer

The system only cares that all payment types implement the required method.

Abstraction ensures this rule is enforced.

---

# Abstract Classes

An abstract class is a class that:

- Cannot be instantiated directly
- May contain abstract methods
- Serves as a template or blueprint for other classes

Abstract classes define what methods must exist, but they do not necessarily provide the full implementation.

Derived classes must implement the abstract methods.

## Example Without Abstract Classes

Without abstraction:

```python
class Dog:
    def speak(self):
        print("Dog barks")


class Cat:
    def speak(self):
        print("Cat meows")
```

Nothing forces every class to implement `speak()`.

A developer might accidentally forget it.

Abstract classes solve this problem.

---

# The `abc` Module

Python provides the `abc` module to support abstract base classes.

The module contains:

- `ABC`
- `ABCMeta`
- `abstractmethod`

The `abc` module allows developers to:

- Define abstract classes
- Declare abstract methods
- Enforce method implementation

Importing the module:

```python
from abc import ABC, abstractmethod
```

---

# The `ABC` Class

`ABC` stands for **Abstract Base Class**.

It is used as a base class for defining abstract classes.

## Syntax

```python
class MyClass(ABC):
    pass
```

### Example

```python
from abc import ABC

class Shape(ABC):
    pass
```

This defines an abstract base class.

However, it becomes truly abstract when it contains abstract methods.

---

# The `@abstractmethod` Decorator

`@abstractmethod` is used to declare a method as abstract.

An abstract method:

- Has no implementation in the base class
- Must be implemented by subclasses

### Example

```python
from abc import ABC, abstractmethod

class Shape(ABC):

    @abstractmethod
    def area(self):
        pass
```

This class cannot be instantiated.

---

# Attempting to Instantiate an Abstract Class

```python
shape = Shape()
```

Python raises an error:

```text
TypeError: Can't instantiate abstract class Shape with abstract method area
```

This ensures that subclasses must implement the method.

---

# Implementing Abstract Methods in Subclasses

```python
from abc import ABC, abstractmethod

class Shape(ABC):

    @abstractmethod
    def area(self):
        pass


class Rectangle(Shape):

    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height
```

### Usage

```python
r = Rectangle(10, 5)
print(r.area())
```

### Output

```text
50
```

The subclass implements the required method.

---

# Multiple Abstract Methods

Abstract classes can contain multiple abstract methods.

### Example

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):

    @abstractmethod
    def start(self):
        pass

    @abstractmethod
    def stop(self):
        pass
```

Subclasses must implement all abstract methods.

### Example

```python
class Car(Vehicle):

    def start(self):
        print("Car engine started")

    def stop(self):
        print("Car engine stopped")
```

### Usage

```python
c = Car()
c.start()
c.stop()
```

---

# Enforcing Method Implementation

The primary purpose of abstract classes is enforcing method implementation.

If a subclass does not implement all abstract methods, Python raises an error.

### Example

```python
class Bike(Vehicle):

    def start(self):
        print("Bike started")
```

Attempting to create an object:

```python
b = Bike()
```

### Error

```text
TypeError: Can't instantiate abstract class Bike with abstract method stop
```

This ensures that subclasses follow the contract.

---

# Abstract Methods With Partial Implementation

Abstract methods can contain partial implementation.

### Example

```python
from abc import ABC, abstractmethod

class Logger(ABC):

    @abstractmethod
    def log(self, message):
        print("Logging:", message)
```

### Subclass

```python
class FileLogger(Logger):

    def log(self, message):
        super().log(message)
        print("Writing to file")
```

### Usage

```python
logger = FileLogger()
logger.log("System started")
```

### Output

```text
Logging: System started
Writing to file
```

The abstract method provides base functionality.

---

# Abstract Properties

Abstract properties can also be defined.

### Example

```python
from abc import ABC, abstractmethod

class Employee(ABC):

    @property
    @abstractmethod
    def salary(self):
        pass
```

### Subclass

```python
class Developer(Employee):

    def __init__(self, salary):
        self._salary = salary

    @property
    def salary(self):
        return self._salary
```

### Usage

```python
d = Developer(70000)
print(d.salary)
```

### Output

```text
70000
```

---

# Interface-Like Behavior in Python

Languages like Java have explicit interfaces.

Python does not have a dedicated `interface` keyword.

However, abstract base classes can simulate interfaces.

An interface defines:

- Method signatures
- No implementation

### Example

```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):

    @abstractmethod
    def process_payment(self, amount):
        pass
```

### Implementations

```python
class CreditCardPayment(PaymentProcessor):

    def process_payment(self, amount):
        print("Processing credit card payment:", amount)


class PayPalPayment(PaymentProcessor):

    def process_payment(self, amount):
        print("Processing PayPal payment:", amount)
```

### Usage

```python
payments = [
    CreditCardPayment(),
    PayPalPayment()
]

for p in payments:
    p.process_payment(100)
```

### Output

```text
Processing credit card payment: 100
Processing PayPal payment: 100
```

All classes follow the same interface.

---

# Real-World Example: File Storage System

### Abstract Class

```python
from abc import ABC, abstractmethod

class Storage(ABC):

    @abstractmethod
    def save(self, data):
        pass

    @abstractmethod
    def load(self):
        pass
```

### Implementations

```python
class FileStorage(Storage):

    def save(self, data):
        print("Saving data to file")

    def load(self):
        print("Loading data from file")


class CloudStorage(Storage):

    def save(self, data):
        print("Saving data to cloud")

    def load(self):
        print("Loading data from cloud")
```

### Usage

```python
storage = FileStorage()

storage.save("data")
storage.load()
```

---

# Advantages of Abstract Classes

## 1. Enforces Design Contracts

Ensures subclasses implement required methods.

## 2. Improves Code Consistency

All derived classes follow the same structure.

## 3. Encourages Polymorphism

Objects can be treated generically.

## 4. Improves Maintainability

Systems become easier to extend and modify.

---

# Best Practices

- Use abstract classes when multiple classes share a common interface.
- Use them to enforce required methods in subclasses.
- Avoid overusing abstract classes in small projects.
- Combine abstract classes with inheritance and polymorphism.

---

# Summary

Abstract classes provide a structured way to define blueprints for other classes. They cannot be instantiated directly and may contain abstract methods that must be implemented by subclasses.

Python implements abstraction using the `abc` module, which provides the `ABC` base class and the `@abstractmethod` decorator. These tools allow developers to enforce method implementation and maintain consistent class interfaces.

Although Python does not have formal interfaces like some languages, abstract base classes can simulate interface-like behavior by defining method contracts that subclasses must follow.

Abstract classes are widely used in large systems such as frameworks, APIs, and plugin architectures. They help enforce design rules, promote code reuse, and enable polymorphic behavior across different implementations.
