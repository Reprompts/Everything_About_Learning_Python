# Python OOPs: Chapter 1 Introduction to Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm centered around the concept of **objects**, which are entities that combine data and behavior into a single unit. Instead of writing programs as a sequence of instructions that manipulate data structures independently, OOP organizes software into interacting objects that model real-world or conceptual systems.

In OOP, programs are designed around objects that communicate with each other. Each object contains:

- **State** — the data stored inside the object
- **Behavior** — the operations that the object can perform

For example, consider a **Bank Account** in a banking system.

The account has data such as:

- Account number
- Balance
- Account holder name

And it has behaviors such as:

- Deposit money
- Withdraw money
- Check balance

In Object-Oriented Programming, this concept would be modeled as a class, and each individual account would be an object created from that class.

---

# Procedural vs Object-Oriented Programming

Before understanding OOP deeply, it is important to understand how it differs from procedural programming, which is an older and simpler programming paradigm.

## Procedural Programming

Procedural programming focuses on procedures or functions that operate on data. Programs are written as sequences of instructions that manipulate variables and data structures.

### Characteristics of Procedural Programming

- Program organized around functions
- Data and functions are separate
- Functions operate on shared data
- Easier for small programs
- Difficult to maintain as programs grow larger

### Example (Procedural Style in Python)

```python
balance = 1000

def deposit(amount):
    global balance
    balance += amount

def withdraw(amount):
    global balance
    if balance >= amount:
        balance -= amount
```

### Problems with This Approach

- Data is globally accessible
- Any function can modify it
- Difficult to manage large systems
- Weak data protection

---

## Object-Oriented Programming

Object-Oriented Programming organizes programs around objects that contain both data and behavior.

Instead of separating functions and data, OOP bundles them together.

### Example

```python
class BankAccount:

    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if self.balance >= amount:
            self.balance -= amount
```

Now each account is an independent object.

### Example Usage

```python
account = BankAccount(1000)

account.deposit(200)
account.withdraw(150)
```

### Advantages

- Data is encapsulated
- Logic is organized around real-world models
- Code becomes modular and reusable
- Easier maintenance and scalability

---

# Key Differences

| Feature | Procedural Programming | Object-Oriented Programming |
|----------|----------------------|-----------------------------|
| Focus | Functions | Objects |
| Data Handling | Data separate from functions | Data bundled with functions |
| Code Structure | Sequence of procedures | Interacting objects |
| Data Protection | Weak | Strong via encapsulation |
| Reusability | Limited | High |
| Scalability | Difficult | Better for large systems |

---

# Object-Oriented Design Principles

Object-Oriented Programming is guided by several design principles that help create maintainable and scalable software systems.

These principles help developers design systems that are:

- Modular
- Reusable
- Flexible
- Easy to maintain

---

## 1. Abstraction

Abstraction means showing only essential features while hiding unnecessary implementation details.

### Example

When driving a car, the driver interacts with:

- Steering wheel
- Accelerator
- Brake

The driver does not need to understand the engine mechanics.

### In Programming

```python
class Car:

    def start(self):
        print("Car started")

    def stop(self):
        print("Car stopped")
```

The user simply calls:

```python
car.start()
```

They do not need to understand how the engine works internally.

---

## 2. Encapsulation

Encapsulation means restricting direct access to internal data and protecting it through controlled interfaces.

### Example

```python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        self._balance += amount

    def get_balance(self):
        return self._balance
```

Here the balance is accessed through methods instead of being manipulated directly.

### Benefits

- Prevents accidental modification
- Protects internal state
- Improves reliability

---

## 3. Inheritance

Inheritance allows a class to reuse properties and behaviors from another class.

### Example

```python
class Animal:

    def speak(self):
        print("Animal makes sound")


class Dog(Animal):

    def speak(self):
        print("Dog barks")
```

The `Dog` class inherits behavior from `Animal`.

### Advantages

- Code reuse
- Hierarchical relationships
- Extensibility

---

## 4. Polymorphism

Polymorphism means the same interface can represent different underlying forms.

### Example

```python
class Dog:

    def speak(self):
        print("Bark")


class Cat:

    def speak(self):
        print("Meow")


animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

Even though `Dog` and `Cat` are different classes, they share the same method interface.

This allows flexible and interchangeable code.

---

# Benefits of Object-Oriented Programming

Object-Oriented Programming offers several advantages for software development.

## 1. Modularity

Programs are divided into independent objects.

Each object manages its own data and behavior.

### Benefits

- Easier debugging
- Easier testing
- Clearer architecture

---

## 2. Reusability

Classes can be reused across different programs or modules.

### Example

A `User` class created for one application can be reused in another system.

---

## 3. Maintainability

Changes can be made in one class without affecting unrelated parts of the program.

### Example

Updating payment logic in a `PaymentProcessor` class will not affect other classes.

---

## 4. Scalability

Large systems are easier to extend.

New features can be added by:

- Extending classes
- Creating new subclasses

Without modifying existing code extensively.

---

## 5. Real-World Modeling

OOP maps naturally to real-world entities.

### Example System

**E-commerce Platform Objects**

- User
- Product
- Order
- Payment
- Cart

Each entity behaves independently but interacts with others.

---

# Core OOP Concepts

Object-Oriented Programming revolves around several fundamental concepts.

---

## Class

A class is a blueprint for creating objects.

It defines:

- Attributes (data)
- Methods (functions)

### Example

```python
class Car:

    def __init__(self, brand, speed):
        self.brand = brand
        self.speed = speed

    def accelerate(self):
        self.speed += 10
```

---

## Object

An object is an instance of a class.

### Example

```python
car1 = Car("Toyota", 100)
car2 = Car("BMW", 150)
```

Each object has its own data.

---

## Attribute

Attributes represent data stored inside objects.

### Example

```python
car.brand
car.speed
```

These define the object's state.

---

## Method

Methods are functions defined inside classes.

### Example

```python
car.accelerate()
```

Methods define behavior.

---

## Constructor

A constructor initializes an object when it is created.

In Python, the constructor is:

```python
__init__()
```

### Example

```python
def __init__(self, brand, speed):
    self.brand = brand
    self.speed = speed
```

---

## Self Parameter

The `self` parameter refers to the current instance of the object.

### Example

```python
def accelerate(self):
    self.speed += 10
```

`self.speed` refers to the object's own data.

---

# Objects and Classes in Python

Python implements OOP in a very flexible and dynamic way.

### Example Class

```python
class Person:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print("Hello, my name is", self.name)
```

### Creating Objects

```python
p1 = Person("Alice", 25)
p2 = Person("Bob", 30)
```

### Calling Methods

```python
p1.greet()
```

### Output

```text
Hello, my name is Alice
```

---

# Python's Approach to Object-Oriented Programming

Python follows a multi-paradigm design.

This means Python supports:

- Procedural programming
- Object-Oriented programming
- Functional programming

However, Python's OOP model has several unique characteristics.

---

## Dynamic Typing

Attributes can be added dynamically.

### Example

```python
class Example:
    pass

obj = Example()
obj.value = 10
```

Attributes can be created at runtime.

---

## Duck Typing

Python uses behavior-based typing instead of strict inheritance.

### Example

```python
class Duck:

    def speak(self):
        print("Quack")


class Person:

    def speak(self):
        print("Hello")


def make_sound(obj):
    obj.speak()


make_sound(Duck())
make_sound(Person())
```

If an object supports the required method, it can be used.

---

## Multiple Inheritance

Python allows classes to inherit from multiple parents.

### Example

```python
class Flyable:

    def fly(self):
        print("Flying")


class Swimmable:

    def swim(self):
        print("Swimming")


class Duck(Flyable, Swimmable):
    pass
```

---

## Everything Is Accessible

Python encourages the **"consenting adults"** philosophy.

Instead of strict restrictions, Python trusts developers to use objects responsibly.

---

# Everything in Python Is an Object

One of Python's most important design principles is that everything is an object.

This includes:

- Integers
- Floats
- Strings
- Lists
- Dictionaries
- Functions
- Classes
- Modules

### Example

```python
x = 10
```

The integer `10` is actually an object.

You can verify this:

```python
print(type(x))
```

### Output

```text
<class 'int'>
```

---

## Objects Have Attributes and Methods

### Example

```python
text = "hello"
```

String object methods:

```python
text.upper()
text.capitalize()
text.replace("h", "H")
```

---

## Functions Are Objects

Functions can be assigned to variables.

```python
def greet():
    print("Hello")

x = greet
x()
```

Functions can also be passed as arguments.

---

## Classes Are Objects

Even classes themselves are objects in Python.

### Example

```python
class Example:
    pass

print(type(Example))
```

### Output

```text
<class 'type'>
```

The class is an instance of `type`.

---

# Implications of Everything Being an Object

This design provides several advantages:

- Uniform behavior across data types
- Powerful introspection capabilities
- Flexible programming patterns
- Consistent method access

### Example

```python
dir(object)
```

Lists attributes and methods of any object.

---

# Summary

Object-Oriented Programming is a powerful paradigm that organizes programs around objects containing data and behavior. It provides a structured way to build complex software systems.

### Key Ideas

- Classes act as blueprints for objects
- Objects represent individual instances with state and behavior
- OOP promotes abstraction, encapsulation, inheritance, and polymorphism
- Python implements OOP in a flexible and dynamic way
- In Python, everything is treated as an object, including functions and classes

This model allows developers to build software that is modular, reusable, scalable, and easier to maintain.
