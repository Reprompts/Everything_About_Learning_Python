# Python OOP: Chapter 28 Design Principles

Python's approach to Object-Oriented Programming is slightly different from traditional OOP languages like Java, C++, or C#. While those languages rely heavily on strict class hierarchies and static typing, Python emphasizes flexibility, readability, and simplicity.

This philosophy is often called **Pythonic OOP**—designing classes and systems in ways that follow Python's design philosophy.

The most important Pythonic OOP principles include:

* Composition over inheritance
* Duck typing
* EAFP vs LBYL
* Clean class design
* Separation of concerns

These principles are deeply connected to **The Zen of Python**:

> Beautiful is better than ugly.
> Simple is better than complex.
> Readability counts.
> There should be one obvious way to do it.

Let's explore each concept in extreme practical detail.

---

# 1. Composition Over Inheritance

## Concept

Composition means building complex objects by combining smaller objects instead of creating deep inheritance hierarchies.

In other words:

| Inheritance       | Composition        |
| ----------------- | ------------------ |
| Is-A Relationship | Has-A Relationship |

Example:

```text
Car IS a Vehicle        (inheritance)
Car HAS an Engine       (composition)
```

Python encourages composition because it produces more flexible and maintainable designs.

---

## Problem With Deep Inheritance

Consider this hierarchy:

```text
Animal
 ├── Mammal
 │    ├── Dog
 │    └── Cat
 └── Bird
```

Now suppose we want:

* Flying dogs
* Swimming cats

Inheritance becomes messy:

```text
FlyingDog
SwimmingDog
FlyingSwimmingDog
```

This creates a **class explosion**.

---

## Composition Solution

Instead of inheritance, we compose behavior.

### Behavior Classes

```python
class FlyBehavior:
    def fly(self):
        print("Flying in the air")


class SwimBehavior:
    def swim(self):
        print("Swimming in water")
```

### Animal Class

```python
class Animal:
    def __init__(self, fly_behavior=None, swim_behavior=None):
        self.fly_behavior = fly_behavior
        self.swim_behavior = swim_behavior

    def perform_fly(self):
        if self.fly_behavior:
            self.fly_behavior.fly()

    def perform_swim(self):
        if self.swim_behavior:
            self.swim_behavior.swim()
```

### Creating Different Animals

```python
dog = Animal(swim_behavior=SwimBehavior())

dog.perform_swim()
```

Output:

```text
Swimming in water
```

Another animal:

```python
bird = Animal(fly_behavior=FlyBehavior())

bird.perform_fly()
```

Output:

```text
Flying in the air
```

---

## Advantages

Composition provides:

* Flexibility
* Reusability
* Runtime behavior changes
* Loose coupling

We can even change behavior dynamically:

```python
dog.fly_behavior = FlyBehavior()

dog.perform_fly()
```

---

# 2. Duck Typing

## Concept

Duck typing is a core philosophy in Python.

It comes from the famous phrase:

> If it walks like a duck and quacks like a duck, then it is a duck.

In programming terms:

* Object type does not matter.
* Object behavior matters.

Python checks whether an object supports the required methods, not its class.

---

## Example Without Duck Typing

In statically typed languages:

```text
Method expects Bird
```

In Python:

```text
Method expects any object that has fly()
```

---

## Example

```python
class Bird:
    def fly(self):
        print("Bird flying")


class Airplane:
    def fly(self):
        print("Airplane flying")
```

Function using duck typing:

```python
def start_flying(obj):
    obj.fly()
```

Usage:

```python
bird = Bird()
plane = Airplane()

start_flying(bird)
start_flying(plane)
```

Output:

```text
Bird flying
Airplane flying
```

Python does not care about class type.

It only checks method availability.

---

## Real Python Example

File-like objects.

Many Python functions operate on any object that provides a `read()` method.

Examples:

* File objects
* Network streams
* String buffers

All behave similarly because they implement the same interface.

---

## Duck Typing vs Type Checking

Instead of writing:

```python
if isinstance(obj, Bird):
    obj.fly()
```

Python encourages:

```python
obj.fly()
```

This produces cleaner and more flexible code.

---

# 3. EAFP vs LBYL

These are two fundamental Python coding philosophies.

---

## LBYL (Look Before You Leap)

Check conditions before performing an action.

Example:

```python
if key in dictionary:
    value = dictionary[key]
```

---

## EAFP (Easier to Ask Forgiveness Than Permission)

Try the action and handle exceptions if it fails.

Example:

```python
try:
    value = dictionary[key]
except KeyError:
    value = None
```

---

## Why Python Prefers EAFP

Python encourages EAFP because:

* Code becomes cleaner
* Avoids race conditions
* Reduces redundant checks

---

## Example: File Handling

### LBYL Approach

```python
import os

if os.path.exists("data.txt"):
    file = open("data.txt")
```

Problem:

```text
File may be deleted between the check and open operation.
```

---

### EAFP Approach

```python
try:
    file = open("data.txt")
except FileNotFoundError:
    print("File not found")
```

This approach is safer and more Pythonic.

---

## Example: Duck Typing + EAFP

```python
def process(obj):
    try:
        obj.run()
    except AttributeError:
        print("Object cannot run")
```

This avoids strict type checking.

---

# 4. Clean Class Design

Python encourages simple, readable, and focused classes.

A good class should follow these rules:

* Single responsibility
* Clear interfaces
* Minimal complexity
* Readable methods

---

## Bad Class Design

```python
class UserManager:
    def create_user(self):
        pass

    def send_email(self):
        pass

    def connect_database(self):
        pass

    def generate_report(self):
        pass
```

This class does too many things.

---

## Clean Class Design

Separate responsibilities.

### Database Class

```python
class Database:
    def connect(self):
        print("Connecting to DB")
```

### User Service

```python
class UserService:
    def create_user(self):
        print("User created")
```

### Email Service

```python
class EmailService:
    def send_email(self):
        print("Email sent")
```

Now each class has a clear responsibility.

---

## Naming Conventions

### Classes

Use **PascalCase**:

```python
UserAccount
FileManager
DataProcessor
```

### Methods

Use **snake_case**:

```python
load_data()
process_file()
calculate_total()
```

---

## Avoid Large Classes

If a class grows too large:

* Split it into smaller classes
* Extract responsibilities

Large classes become difficult to maintain.

---

# 5. Separation of Concerns (SoC)

## Concept

Separation of Concerns (SoC) means dividing a program into distinct parts, each responsible for a specific functionality.

Each component should focus on one concern.

---

## Example Without Separation

```python
class OrderProcessor:
    def process(self):
        print("Processing order")
        print("Saving to database")
        print("Sending email")
```

This mixes:

* Business logic
* Database logic
* Notification logic

---

## Proper Separation

### Order Logic

```python
class OrderService:
    def process_order(self):
        print("Processing order")
```

### Database Logic

```python
class OrderRepository:
    def save(self):
        print("Saving order")
```

### Notification Logic

```python
class NotificationService:
    def send_email(self):
        print("Sending email")
```

### Controller

```python
class OrderController:
    def __init__(self):
        self.service = OrderService()
        self.repo = OrderRepository()
        self.notify = NotificationService()

    def execute(self):
        self.service.process_order()
        self.repo.save()
        self.notify.send_email()
```

Now each class has a single responsibility.

---

## Advantages

Separation of concerns improves:

* Maintainability
* Testability
* Readability
* Modularity
* Scalability

---

# Real Python Framework Examples

## Django

Django uses separation of concerns:

* Models → Database Layer
* Views → Business Logic
* Templates → Presentation Layer

---

## Flask

Flask uses decorators and composition extensively.

Example:

```python
@app.route("/home")
def home():
    return "Hello"
```

This is an example of decorator-based behavior extension.

---

## Pandas

Pandas uses duck typing extensively.

Functions work on any object that behaves like:

* Array
* Series
* DataFrame

---

# Combining All Pythonic Principles

A Pythonic design often combines all these ideas.

Example system:

* Strategy Pattern → Composition
* Duck Typing → Flexible Interfaces
* EAFP → Error Handling
* Clean Classes → Readability
* Separation of Concerns → Architecture

Together they produce clean, scalable, and maintainable Python software.

---

# Final Summary

Pythonic OOP focuses on practical and flexible design rather than rigid hierarchies.

## Key Principles

### Composition Over Inheritance

Prefer combining objects rather than building deep class hierarchies.

### Duck Typing

Focus on behavior rather than strict types.

### EAFP vs LBYL

Prefer `try/except` over excessive condition checking.

### Clean Class Design

Classes should be small, readable, and focused.

### Separation of Concerns

Different parts of the system should handle distinct responsibilities.

These principles form the foundation of professional Python architecture and are widely used in frameworks and libraries such as:

* Django
* Flask
* FastAPI
* Pandas
* TensorFlow

Mastering these ideas helps you write software that is more maintainable, scalable, testable, and Pythonic.
