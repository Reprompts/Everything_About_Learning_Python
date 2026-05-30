# Python OOP: Chapter 27 Design Patterns

Object-Oriented Design Patterns are reusable solutions to common software design problems. They describe how classes and objects should interact to solve recurring design challenges in a flexible and maintainable way.

Although design patterns originated in languages like C++ and Java, they are extremely powerful in Python because Python supports:

* Dynamic typing
* First-class functions
* Flexible object models
* Decorators and metaprogramming

This article explains important OOP design patterns in Python, including:

* Singleton Pattern
* Factory Pattern
* Strategy Pattern
* Observer Pattern
* Adapter Pattern
* Decorator Pattern

Each pattern will be explained conceptually and practically.

---

# 1. Singleton Pattern

## Concept

The Singleton pattern ensures that only one instance of a class exists throughout the program and provides a global access point to it.

This is useful when a system requires a single shared resource.

### Examples

* Database connections
* Configuration managers
* Logging systems
* Thread pools

---

## Problem Without Singleton

Consider a configuration manager:

```python
class Config:
    pass
```

If we create multiple objects:

```python
config1 = Config()
config2 = Config()
```

Now we have multiple configuration objects, which may cause inconsistencies.

---

## Singleton Solution

Ensure only one instance exists.

### Implementation Using a Class Variable

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

## Real Example: Logger

```python
class Logger:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def log(self, message):
        print("LOG:", message)
```

### Usage

```python
logger1 = Logger()
logger2 = Logger()

logger1.log("Start")

print(logger1 is logger2)
```

---

## Alternative Singleton Using a Decorator

Python often uses decorators instead.

```python
def singleton(cls):
    instances = {}

    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)

        return instances[cls]

    return get_instance
```

### Usage

```python
@singleton
class Database:
    pass
```

---

# 2. Factory Pattern

## Concept

The Factory pattern creates objects without exposing the exact class used.

Instead of directly calling constructors, object creation is delegated to a factory function or class.

---

## Why Use the Factory Pattern?

Sometimes programs must create objects dynamically based on conditions.

### Examples

* File parsers
* Payment methods
* Notification services
* Database drivers

---

## Basic Factory Example

### Product Classes

```python
class Dog:
    def speak(self):
        return "Woof"


class Cat:
    def speak(self):
        return "Meow"
```

### Factory

```python
class AnimalFactory:
    def create_animal(self, animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
```

### Usage

```python
factory = AnimalFactory()

animal = factory.create_animal("dog")

print(animal.speak())
```

### Output

```text
Woof
```

---

## Advantages

The Factory pattern:

* Encapsulates object creation
* Reduces coupling
* Makes code extensible

New classes can be added without modifying client code significantly.

---

# 3. Strategy Pattern

## Concept

The Strategy pattern defines a family of algorithms and allows them to be interchangeable at runtime.

It lets the program choose behavior dynamically.

---

## Problem Without Strategy

Example: Payment system

```python
def pay(method):
    if method == "credit":
        process_credit()
    elif method == "paypal":
        process_paypal()
```

This creates large conditional blocks.

---

## Strategy Solution

Define separate classes for each algorithm.

### Strategy Interface

```python
class PaymentStrategy:
    def pay(self, amount):
        pass
```

### Concrete Strategies

```python
class CreditCardPayment(PaymentStrategy):
    def pay(self, amount):
        print("Paid", amount, "using credit card")


class PayPalPayment(PaymentStrategy):
    def pay(self, amount):
        print("Paid", amount, "using PayPal")
```

### Context Class

```python
class ShoppingCart:
    def __init__(self, strategy):
        self.strategy = strategy

    def checkout(self, amount):
        self.strategy.pay(amount)
```

### Usage

```python
cart = ShoppingCart(CreditCardPayment())

cart.checkout(100)
```

### Output

```text
Paid 100 using credit card
```

### Change Strategy Dynamically

```python
cart.strategy = PayPalPayment()

cart.checkout(200)
```

---

# 4. Observer Pattern

## Concept

The Observer pattern defines a one-to-many dependency between objects.

When one object changes state, all dependent objects are notified automatically.

---

## Real-World Examples

* Stock market systems
* Event systems
* GUI frameworks
* Publish-subscribe systems

---

## Components

* Subject (Publisher)
* Observers (Subscribers)

---

## Implementation

### Subject

```python
class Subject:
    def __init__(self):
        self.observers = []

    def attach(self, observer):
        self.observers.append(observer)

    def notify(self, message):
        for observer in self.observers:
            observer.update(message)
```

### Observer

```python
class Observer:
    def update(self, message):
        print("Received:", message)
```

### Usage

```python
subject = Subject()

obs1 = Observer()
obs2 = Observer()

subject.attach(obs1)
subject.attach(obs2)

subject.notify("New update available")
```

### Output

```text
Received: New update available
Received: New update available
```

---

# 5. Adapter Pattern

## Concept

The Adapter pattern allows incompatible interfaces to work together.

It converts one interface into another expected by the client.

---

## Real-World Examples

* Different APIs
* Legacy systems
* Hardware drivers
* Payment gateways

---

## Example Problem

Suppose we have a class with one interface:

```python
class EuropeanSocket:
    def voltage(self):
        return 230
```

But our device expects:

```python
power()
```

---

## Adapter Implementation

```python
class SocketAdapter:
    def __init__(self, socket):
        self.socket = socket

    def power(self):
        return self.socket.voltage()
```

### Usage

```python
socket = EuropeanSocket()

adapter = SocketAdapter(socket)

print(adapter.power())
```

### Output

```text
230
```

The adapter converts one interface into another.

---

# 6. Decorator Pattern

## Concept

The Decorator pattern allows behavior to be added to objects dynamically without modifying their code.

It wraps an object inside another object.

---

## Example Problem

Suppose we have a coffee class:

```python
class Coffee:
    def cost(self):
        return 5
```

Now we want to add:

* Milk
* Sugar
* Cream

Instead of modifying the class repeatedly, we use decorators.

---

## Decorator Implementation

### Base Class

```python
class Coffee:
    def cost(self):
        return 5
```

### Decorator Base

```python
class CoffeeDecorator:
    def __init__(self, coffee):
        self.coffee = coffee
```

### Milk Decorator

```python
class MilkDecorator(CoffeeDecorator):
    def cost(self):
        return self.coffee.cost() + 2
```

### Sugar Decorator

```python
class SugarDecorator(CoffeeDecorator):
    def cost(self):
        return self.coffee.cost() + 1
```

### Usage

```python
coffee = Coffee()

coffee = MilkDecorator(coffee)
coffee = SugarDecorator(coffee)

print(coffee.cost())
```

### Output

```text
8
```

---

## Decorator Pattern vs Python Decorators

Python function decorators are a practical implementation of the Decorator Pattern.

### Example

```python
def logger(func):
    def wrapper():
        print("Calling function")
        func()
    return wrapper
```

### Usage

```python
@logger
def hello():
    print("Hello")
```

---

# Design Pattern Comparison

| Pattern   | Purpose                     |
| --------- | --------------------------- |
| Singleton | Ensure a single instance    |
| Factory   | Centralized object creation |
| Strategy  | Interchangeable algorithms  |
| Observer  | Event notification system   |
| Adapter   | Interface compatibility     |
| Decorator | Dynamic behavior extension  |

---

# Real Systems Using These Patterns

These patterns appear throughout modern software systems.

### Django

* Factory patterns for models
* Observer patterns for signals

### Flask

* Decorator pattern for routes

### Logging Frameworks

* Singleton log managers

### Machine Learning Pipelines

* Strategy pattern for algorithm selection

---

# Final Summary

Design patterns provide proven architectural solutions for recurring software design problems.

The patterns discussed are among the most important in Python OOP:

* **Singleton** → Single shared instance
* **Factory** → Flexible object creation
* **Strategy** → Interchangeable algorithms
* **Observer** → Event-driven systems
* **Adapter** → Interface compatibility
* **Decorator** → Dynamic behavior extension

Understanding these patterns allows developers to build systems that are:

* Modular
* Maintainable
* Scalable
* Flexible

These patterns form the foundation of professional software architecture.

---

## What's Next?

Advanced design patterns include:

* Command Pattern
* Proxy Pattern
* Builder Pattern
* Prototype Pattern
* Composite Pattern

You can also explore:

* Real-world framework implementations of design patterns
* Pythonic alternatives to classic OOP patterns
* Functional design patterns in Python

Mastering these patterns significantly improves your ability to design robust, extensible, and maintainable software systems.
