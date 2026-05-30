# Python OOPs: Chapter 16 Aggregation

In object-oriented design, relationships between objects can vary in strength and ownership. Two closely related concepts used to model these relationships are **composition** and **aggregation**.

While composition represents strong ownership, aggregation represents a weaker association where objects collaborate but remain independent.

Aggregation is used when:

* One object uses another object
* The objects are not strongly bound
* Each object can exist independently

This tutorial explores aggregation in depth, including:

* The concept of aggregation
* Weak ownership relationships
* Object collaboration
* Independent object lifetimes
* Practical implementation examples

---

# Understanding Aggregation

Aggregation is a form of association where one class contains references to objects of another class, but those objects can exist independently of the container.

In simple terms:

> Aggregation means objects cooperate without owning each other completely.

### Example Relationships

* Teacher teaches Students
* Department contains Professors
* Team has Players
* Library has Books

Even if the team disappears, the players still exist.

This independence is what distinguishes aggregation from composition.

---

# Key Characteristics of Aggregation

Aggregation has several defining characteristics.

## Weak Ownership

The container does not fully control the lifetime of the contained objects.

## Independent Lifetimes

Objects exist independently and can be used elsewhere.

## Reusability

The same object may belong to multiple containers.

## Loose Coupling

Objects interact but remain independent.

---

# Aggregation vs Composition

Understanding the difference between aggregation and composition is important for proper object modeling.

| Feature         | Composition              | Aggregation      |
| --------------- | ------------------------ | ---------------- |
| Ownership       | Strong                   | Weak             |
| Object Lifetime | Dependent                | Independent      |
| Reusability     | Limited                  | High             |
| Relationship    | Part-of                  | Uses / Has-a     |
| Object Creation | Usually inside container | Usually external |

### Example

**Composition:** Car has Engine

**Aggregation:** Department has Professors

If a car is destroyed, its engine typically disappears with it.

But if a department is removed, professors still exist.

---

# Basic Aggregation Example

Consider a **Department** and **Professor** relationship.

Professors exist independently of departments.

## Professor Class

```python
class Professor:
    def __init__(self, name):
        self.name = name

    def teach(self):
        print(self.name, "is teaching")
```

## Department Class

```python
class Department:
    def __init__(self, name, professors):
        self.name = name
        self.professors = professors

    def show_professors(self):
        print("Department:", self.name)

        for professor in self.professors:
            print(professor.name)
```

## Usage

```python
p1 = Professor("Dr. Smith")
p2 = Professor("Dr. Johnson")

department = Department(
    "Computer Science",
    [p1, p2]
)

department.show_professors()
```

### Output

```text
Department: Computer Science
Dr. Smith
Dr. Johnson
```

### Important Observation

The professors were created outside the department and passed in.

They are **not owned** by the department.

---

# Weak Ownership Relationships

Aggregation represents weak ownership relationships.

The container object does not control the lifecycle of the contained objects.

### Examples

* School → Students
* Company → Employees
* Team → Players

Even if the container is destroyed, the objects still exist.

---

## Demonstration of Weak Ownership

```python
class Student:
    def __init__(self, name):
        self.name = name


class Course:
    def __init__(self, students):
        self.students = students
```

### Usage

```python
s1 = Student("Alice")
s2 = Student("Bob")

course = Course([s1, s2])
```

Delete the course:

```python
del course
```

Students still exist:

```python
print(s1.name)
```

### Output

```text
Alice
```

This demonstrates weak ownership.

---

# Object Collaboration

Aggregation enables objects to collaborate while remaining independent.

Objects cooperate to perform tasks without being tightly bound.

### Examples

* Customer → BankAccount
* Order → Product
* Playlist → Song

The collaborating objects interact through method calls.

---

# Example: Order and Product

## Product Class

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price
```

## Order Class

```python
class Order:
    def __init__(self, products):
        self.products = products

    def total_price(self):
        total = 0

        for product in self.products:
            total += product.price

        return total
```

## Usage

```python
p1 = Product("Laptop", 1000)
p2 = Product("Mouse", 50)

order = Order([p1, p2])

print(order.total_price())
```

### Output

```text
1050
```

Products collaborate with orders, but they remain independent objects.

---

# Independent Object Lifetimes

One of the most important properties of aggregation is independent object lifetimes.

This means:

* Objects exist independently
* They can be reused in multiple relationships
* Their lifecycle is not controlled by the container

---

# Example: Player and Team

## Player Class

```python
class Player:
    def __init__(self, name):
        self.name = name
```

## Team Class

```python
class Team:
    def __init__(self, name):
        self.name = name
        self.players = []

    def add_player(self, player):
        self.players.append(player)

    def show_players(self):
        for player in self.players:
            print(player.name)
```

## Usage

```python
player1 = Player("Rohit")
player2 = Player("Virat")

team = Team("India")

team.add_player(player1)
team.add_player(player2)

team.show_players()
```

### Output

```text
Rohit
Virat
```

If the team object is deleted:

```python
del team
```

The players still exist.

---

# Aggregation with Multiple Containers

An object in aggregation can belong to multiple containers simultaneously.

### Example

A student may enroll in multiple courses.

## Student Class

```python
class Student:
    def __init__(self, name):
        self.name = name
```

## Course Class

```python
class Course:
    def __init__(self, name):
        self.name = name
        self.students = []

    def enroll(self, student):
        self.students.append(student)
```

## Usage

```python
s = Student("Ananya")

math = Course("Mathematics")
physics = Course("Physics")

math.enroll(s)
physics.enroll(s)
```

Here:

* Student belongs to multiple courses
* The student object exists independently

This is a classic aggregation scenario.

---

# Real-World Example: Library System

Objects involved:

* Library
* Book
* Author

Books exist independently of libraries.

## Book Class

```python
class Book:
    def __init__(self, title):
        self.title = title
```

## Library Class

```python
class Library:
    def __init__(self, books):
        self.books = books

    def show_books(self):
        for book in self.books:
            print(book.title)
```

## Usage

```python
b1 = Book("Python Programming")
b2 = Book("Data Structures")

library = Library([b1, b2])

library.show_books()
```

### Output

```text
Python Programming
Data Structures
```

Books can exist without the library.

---

# Designing Systems with Aggregation

Aggregation is commonly used in:

* University systems
* Banking systems
* Inventory systems
* E-commerce platforms
* Enterprise applications

### Examples

* Order → Products
* Company → Employees
* Playlist → Songs
* Hospital → Doctors

Aggregation helps design loosely coupled systems.

---

# Advantages of Aggregation

## Loose Coupling

Objects remain independent.

## High Reusability

Objects can be reused in multiple contexts.

## Flexible System Design

Components can change independently.

## Easier Testing

Individual objects can be tested separately.

---

# Best Practices for Aggregation

### Use Aggregation When

* Objects should exist independently
* Multiple containers may share the same object
* Loose coupling is desired

### Prefer

Passing objects to containers through:

* Constructors
* Methods

rather than creating them internally.

### Avoid

* Tight coupling between aggregated objects
* Unnecessary ownership relationships

---

# Summary

Aggregation is a design technique used to model weak ownership relationships between objects. In this relationship, one object contains references to other objects but does not control their lifecycle.

Unlike composition, aggregated objects can exist independently and may belong to multiple containers simultaneously. This allows objects to collaborate while maintaining loose coupling.

Aggregation is widely used in real-world software systems to represent relationships such as students enrolled in courses, employees working in companies, or products included in orders.

By allowing independent object lifetimes and encouraging object collaboration, aggregation supports modular, reusable, and flexible object-oriented system design.
