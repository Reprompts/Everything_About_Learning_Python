# Python OOPs: Chapter 15 Composition

Object-oriented design offers multiple ways to build relationships between classes. While inheritance is one of the most commonly known techniques, another equally powerful and often preferred approach is **composition**.

Composition focuses on building complex objects by combining simpler objects, rather than inheriting behavior from parent classes. This design promotes flexibility, modularity, and better software architecture.

This tutorial explores composition in depth, including:

- The concept of composition
- Composition vs inheritance
- The has-a relationship
- Building objects from other objects
- Delegation patterns
- Practical real-world examples

---

# Understanding Composition

**Composition** is a design technique where a class contains objects of other classes as its attributes.

Instead of extending behavior through inheritance, a class uses functionality from other classes by including them as components.

In simple terms:

> Composition means building complex objects using other objects.

### Example Concept

A car may contain:

- An engine
- Wheels
- A battery

```
Car
├── Engine
├── Wheels
└── Battery
```

The `Car` object is composed of several other objects.

---

# Basic Composition Example

Consider a system representing a computer.

A computer contains components like:

- CPU
- RAM
- Hard Drive

Each component can be represented by its own class.

## Component Classes

```python
class CPU:
    def process(self):
        print("CPU is processing instructions")


class RAM:
    def load(self):
        print("RAM is loading data")


class HardDrive:
    def read(self):
        print("Hard drive reading data")
```

## Composed Class

```python
class Computer:
    def __init__(self):
        self.cpu = CPU()
        self.ram = RAM()
        self.hard_drive = HardDrive()

    def start(self):
        print("Starting computer")
        self.cpu.process()
        self.ram.load()
        self.hard_drive.read()
```

## Usage

```python
pc = Computer()
pc.start()
```

### Output

```text
Starting computer
CPU is processing instructions
RAM is loading data
Hard drive reading data
```

The `Computer` class is composed of multiple component objects.

---

# Composition vs Inheritance

Both composition and inheritance establish relationships between classes, but they serve different purposes.

## Inheritance

Inheritance models an **is-a** relationship.

### Example

```python
class Animal:
    def eat(self):
        print("Animal eats")


class Dog(Animal):
    pass
```

Here:

```text
Dog is an Animal
```

---

## Composition

Composition models a **has-a** relationship.

### Example

```python
class Engine:
    def start(self):
        print("Engine started")


class Car:
    def __init__(self):
        self.engine = Engine()
```

Here:

```text
Car has an Engine
```

---

# Key Differences Between Composition and Inheritance

| Aspect | Inheritance | Composition |
|----------|-------------|-------------|
| Relationship | is-a | has-a |
| Coupling | Tightly coupled | Loosely coupled |
| Flexibility | Less flexible | Highly flexible |
| Reusability | Limited | Higher |
| Code modification | Harder | Easier |

Composition often leads to better modular design.

---

# The Has-a Relationship

A **has-a relationship** means one class owns or uses another object as a component.

### Examples

- Library has Books
- University has Departments
- Car has Wheels

In programming:

```python
object.attribute = another_object
```

---

## Example: Car and Engine

### Engine Class

```python
class Engine:
    def start(self):
        print("Engine started")

    def stop(self):
        print("Engine stopped")
```

### Car Class

```python
class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        print("Car starting")
        self.engine.start()

    def stop(self):
        print("Car stopping")
        self.engine.stop()
```

### Usage

```python
my_car = Car()

my_car.start()
my_car.stop()
```

### Output

```text
Car starting
Engine started
Car stopping
Engine stopped
```

The `Car` delegates engine-related behavior to the `Engine` object.

---

# Building Objects from Other Objects

Composition allows building hierarchies of objects.

## Example: House System

Components:

- House
- Room
- Door
- Window

---

## Component Classes

```python
class Door:
    def open(self):
        print("Door opened")


class Window:
    def open(self):
        print("Window opened")
```

---

## Room Class

```python
class Room:
    def __init__(self):
        self.door = Door()
        self.window = Window()

    def enter(self):
        print("Entering room")
        self.door.open()
```

---

## House Class

```python
class House:
    def __init__(self):
        self.living_room = Room()
        self.bedroom = Room()

    def enter_house(self):
        print("Entering house")
        self.living_room.enter()
```

---

## Usage

```python
house = House()
house.enter_house()
```

### Output

```text
Entering house
Entering room
Door opened
```

This demonstrates **multi-level composition**.

---

# Delegation Pattern

**Delegation** is a design pattern where an object forwards responsibility to another object.

Instead of implementing behavior directly, the object delegates work to its components.

### Examples

- Manager delegates tasks to employees
- Car delegates movement to engine
- Application delegates storage to database

---

## Delegation Example

### Worker Class

```python
class Worker:
    def perform_task(self):
        print("Worker performing task")
```

### Manager Class

```python
class Manager:
    def __init__(self):
        self.worker = Worker()

    def manage(self):
        print("Manager delegating task")
        self.worker.perform_task()
```

### Usage

```python
manager = Manager()
manager.manage()
```

### Output

```text
Manager delegating task
Worker performing task
```

The manager delegates the task to the worker.

---

# Dynamic Composition

Composition can also allow changing components dynamically.

## Example: Payment System

---

### Payment Processors

```python
class CreditCardPayment:
    def pay(self, amount):
        print("Paid using credit card:", amount)


class PayPalPayment:
    def pay(self, amount):
        print("Paid using PayPal:", amount)
```

---

### Shopping Cart Using Composition

```python
class ShoppingCart:
    def __init__(self, payment_method):
        self.payment_method = payment_method

    def checkout(self, amount):
        self.payment_method.pay(amount)
```

---

### Usage

```python
cart1 = ShoppingCart(CreditCardPayment())
cart1.checkout(100)

cart2 = ShoppingCart(PayPalPayment())
cart2.checkout(200)
```

### Output

```text
Paid using credit card: 100
Paid using PayPal: 200
```

The cart delegates payment behavior to different payment objects.

This demonstrates **flexible composition**.

---

# Real-World Example: Game Character System

A game character might have:

- Weapon
- Armor
- Abilities

---

## Component Classes

```python
class Weapon:
    def attack(self):
        print("Attacking with weapon")


class Armor:
    def defend(self):
        print("Defending with armor")
```

---

## Character Class

```python
class Character:
    def __init__(self):
        self.weapon = Weapon()
        self.armor = Armor()

    def fight(self):
        self.weapon.attack()
        self.armor.defend()
```

---

## Usage

```python
hero = Character()
hero.fight()
```

### Output

```text
Attacking with weapon
Defending with armor
```

The character is composed of weapon and armor objects.

---

# Advantages of Composition

## Better Flexibility

Components can be replaced or modified without changing the main class.

## Reduced Coupling

Classes depend on behavior rather than strict hierarchy.

## Easier Maintenance

Systems become easier to modify and extend.

## Better Code Reuse

Components can be reused in multiple classes.

---

# When to Use Composition

Composition should be preferred when:

- Objects contain other objects
- Behavior should be modular
- Inheritance relationships are unclear
- Flexibility is required

Many modern software architectures prefer composition over inheritance.

---

# Composition vs Inheritance in Design

A widely accepted object-oriented design principle is:

> Favor composition over inheritance.

### Why?

Inheritance creates tight coupling between classes, making systems harder to modify.

Composition promotes loose coupling and modularity, allowing easier evolution of systems.

---

# Visual Comparison

## Inheritance

```text
Animal
   ▲
   │
 Dog
```

Relationship:

```text
Dog is-an Animal
```

---

## Composition

```text
Car
 └── Engine
```

Relationship:

```text
Car has-an Engine
```

---

# Composition in Real Software

Composition is heavily used in modern software systems:

### Web Applications

```text
Controller
 ├── Service
 ├── Repository
 └── Logger
```

### Game Engines

```text
Character
 ├── Weapon
 ├── Armor
 ├── Inventory
 └── Skills
```

### E-Commerce Systems

```text
Order
 ├── Products
 ├── Customer
 └── Payment Method
```

### Banking Systems

```text
Account
 ├── Transaction History
 ├── Customer
 └── Notification Service
```

---

# Best Practices

### Prefer Composition for Reuse

Reuse behavior by combining objects rather than creating deep inheritance hierarchies.

### Keep Components Focused

Each component should have a single responsibility.

### Delegate Responsibilities

Allow specialized objects to handle their own logic.

### Use Dependency Injection

Pass components through constructors when appropriate.

Example:

```python
class Car:
    def __init__(self, engine):
        self.engine = engine
```

This makes the system more flexible and testable.

---

# Summary

Composition is a powerful object-oriented design technique that allows building complex objects by combining simpler objects. Instead of inheriting behavior, classes include other objects as components, creating a **has-a relationship**.

Through composition, objects delegate responsibilities to their internal components, promoting modularity and flexibility. This approach enables developers to construct scalable systems where individual components can evolve independently.

Compared to inheritance, composition reduces tight coupling and supports better code reuse. By organizing software systems into cooperating objects, composition forms the foundation of many modern design patterns and architectural practices in Python.

## Key Takeaways

- Composition models a **has-a** relationship.
- Objects are built from other objects.
- Delegation allows responsibilities to be forwarded to components.
- Composition promotes loose coupling and flexibility.
- Components can be reused across multiple systems.
- Modern software design often favors composition over inheritance.
- Composition is a foundation of scalable and maintainable architectures.
