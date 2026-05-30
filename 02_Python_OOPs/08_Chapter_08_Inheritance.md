# Python OOPs: Chapter 8 Inheritance

Inheritance is one of the core principles of Object-Oriented Programming (OOP). It allows a class to acquire properties and behaviors from another class, enabling code reuse, extensibility, and hierarchical design.

In real-world systems, many objects share common characteristics. Instead of rewriting the same code multiple times, inheritance allows developers to define common behavior once in a base class and reuse it in derived classes.

Inheritance helps model relationships such as:

- Animal → Dog
- Vehicle → Car
- Employee → Manager

In these relationships, the derived classes inherit common properties from the parent class while adding their own specialized functionality.

---

# Concept of Inheritance

Inheritance allows one class to reuse attributes and methods from another class.

The class that provides the inherited features is called the **base class**, while the class that receives them is called the **derived class**.

This relationship establishes an **"is-a" relationship**.

### Examples

- A Dog is an Animal
- A Car is a Vehicle
- A Manager is an Employee

This means the derived class automatically possesses the features of the base class.

### Basic Syntax

```python
class DerivedClass(BaseClass):
```

### Example

```python
class Animal:
    def speak(self):
        print("Animal makes a sound")


class Dog(Animal):
    def bark(self):
        print("Dog barks")
```

### Usage

```python
d = Dog()

d.speak()
d.bark()
```

### Output

```text
Animal makes a sound
Dog barks
```

The `Dog` class inherits the `speak()` method from `Animal`.

---

# Base Class

A base class (also called **parent class** or **superclass**) is the class whose attributes and methods are inherited by another class.

It typically contains common functionality shared by multiple subclasses.

### Example

```python
class Vehicle:
    def start(self):
        print("Vehicle started")

    def stop(self):
        print("Vehicle stopped")
```

This base class defines behavior that all vehicles share.

---

# Derived Class

A derived class (also called **child class** or **subclass**) inherits from the base class.

It can:

- Use parent methods directly
- Extend functionality
- Override parent behavior

### Example

```python
class Car(Vehicle):
    def drive(self):
        print("Car is driving")
```

### Usage

```python
c = Car()

c.start()
c.drive()
```

### Output

```text
Vehicle started
Car is driving
```

The `Car` class reuses the parent method `start()`.

---

# Reusing Parent Code

Inheritance significantly reduces code duplication.

### Without Inheritance

```python
class Car:
    def start(self):
        print("Engine started")


class Bike:
    def start(self):
        print("Engine started")
```

The same logic is repeated.

### Using Inheritance

```python
class Vehicle:
    def start(self):
        print("Engine started")


class Car(Vehicle):
    pass


class Bike(Vehicle):
    pass
```

Now both classes reuse the `start()` method.

### Benefits

- Reduced redundancy
- Easier maintenance
- Improved code organization

---

# Types of Inheritance

Python supports several forms of inheritance:

- Single inheritance
- Multiple inheritance
- Multilevel inheritance
- Hierarchical inheritance
- Hybrid inheritance

Each type represents a different class relationship structure.

---

# Single Inheritance

Single inheritance occurs when one child class inherits from a single parent class.

### Structure

```text
Parent
  │
  ▼
Child
```

### Example

```python
class Animal:
    def eat(self):
        print("Animal eats food")


class Dog(Animal):
    def bark(self):
        print("Dog barks")
```

### Usage

```python
d = Dog()

d.eat()
d.bark()
```

### Output

```text
Animal eats food
Dog barks
```

The `Dog` class inherits functionality from `Animal`.

Single inheritance is the simplest form of inheritance.

---

# Multiple Inheritance

Multiple inheritance occurs when a class inherits from more than one parent class.

### Structure

```text
Parent1     Parent2
    \         /
     \       /
      \     /
       Child
```

### Example

```python
class Flyer:
    def fly(self):
        print("Flying")


class Swimmer:
    def swim(self):
        print("Swimming")


class Duck(Flyer, Swimmer):
    def quack(self):
        print("Quack")
```

### Usage

```python
d = Duck()

d.fly()
d.swim()
d.quack()
```

### Output

```text
Flying
Swimming
Quack
```

The `Duck` class inherits behavior from both parent classes.

---

# Method Resolution Order (Brief Concept)

In multiple inheritance, Python determines which parent method to call using the **Method Resolution Order (MRO)**.

The MRO defines the order in which classes are searched for attributes.

### Example

```python
print(Duck.__mro__)
```

### Output (similar to)

```text
(Duck, Flyer, Swimmer, object)
```

Python searches methods in this order.

---

# Multilevel Inheritance

Multilevel inheritance occurs when a class inherits from another derived class, forming a chain.

### Structure

```text
Grandparent
     │
     ▼
Parent
     │
     ▼
Child
```

### Example

```python
class Animal:
    def eat(self):
        print("Animal eats")


class Dog(Animal):
    def bark(self):
        print("Dog barks")


class Puppy(Dog):
    def weep(self):
        print("Puppy weeps")
```

### Usage

```python
p = Puppy()

p.eat()
p.bark()
p.weep()
```

### Output

```text
Animal eats
Dog barks
Puppy weeps
```

The `Puppy` class inherits behavior from both parent classes.

---

# Hierarchical Inheritance

Hierarchical inheritance occurs when multiple child classes inherit from a single parent class.

### Structure

```text
        Parent
       /      \
      /        \
   Child1    Child2
```

### Example

```python
class Animal:
    def eat(self):
        print("Animal eats")


class Dog(Animal):
    def bark(self):
        print("Dog barks")


class Cat(Animal):
    def meow(self):
        print("Cat meows")
```

### Usage

```python
d = Dog()
c = Cat()

d.eat()
c.eat()
```

### Output

```text
Animal eats
Animal eats
```

Both classes reuse the `eat()` method.

---

# Hybrid Inheritance

Hybrid inheritance is a combination of multiple inheritance types.

### Structure

```text
        Animal
       /      \
  Mammal      Bird
       \      /
         Bat
```

### Example

```python
class Animal:
    def breathe(self):
        print("Animal breathes")


class Mammal(Animal):
    def walk(self):
        print("Mammal walks")


class Bird(Animal):
    def fly(self):
        print("Bird flies")


class Bat(Mammal, Bird):
    def hang(self):
        print("Bat hangs upside down")
```

### Usage

```python
b = Bat()

b.breathe()
b.walk()
b.fly()
b.hang()
```

### Output

```text
Animal breathes
Mammal walks
Bird flies
Bat hangs upside down
```

Hybrid inheritance combines multiple inheritance and hierarchical inheritance.

---

# Practical Real-World Example

Consider a Company Employee System.

### Base Class

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def work(self):
        print(self.name, "is working")
```

### Derived Class

```python
class Manager(Employee):
    def manage(self):
        print(self.name, "is managing the team")
```

### Usage

```python
m = Manager("Alice", 80000)

m.work()
m.manage()
```

### Output

```text
Alice is working
Alice is managing the team
```

The manager inherits common employee functionality.

---

# Advantages of Inheritance

Inheritance offers several benefits for software development.

## Code Reusability

Common code can be reused across multiple classes.

## Reduced Duplication

Shared functionality is written once in the parent class.

## Extensibility

New features can be added in derived classes without modifying existing code.

## Logical Hierarchy

Inheritance models real-world relationships between entities.

## Maintainability

Changes in base classes propagate to all derived classes.

---

# Best Practices for Using Inheritance

- Use inheritance only when there is a clear **"is-a" relationship**.
- Avoid deep inheritance hierarchies that become difficult to maintain.
- Prefer composition over inheritance when objects merely collaborate rather than represent hierarchical relationships.
- Ensure base classes contain generic behavior, while derived classes implement specialized functionality.

---

# Summary

Inheritance is a powerful mechanism in Python's object-oriented model that allows classes to reuse and extend behavior from other classes.

A base class provides common functionality, while derived classes inherit and extend that behavior. This mechanism supports hierarchical relationships between classes and reduces code duplication.

Python supports several forms of inheritance, including:

- Single inheritance
- Multiple inheritance
- Multilevel inheritance
- Hierarchical inheritance
- Hybrid inheritance

These inheritance models allow developers to build complex software systems with clear structure, modular design, and efficient reuse of code.
