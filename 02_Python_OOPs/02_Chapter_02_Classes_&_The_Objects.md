# Python OOPs: Chapter 2 Classes & Objects

Object-Oriented Programming revolves around two fundamental building blocks: **classes** and **objects**. These concepts allow programmers to design software systems that mirror real-world entities and interactions.

A **class** defines the structure and behavior of objects, while an **object** represents a specific instance of that structure.

Understanding classes and objects thoroughly is essential because nearly every advanced concept in Python's OOP system—such as inheritance, polymorphism, and abstraction—depends on them.

---

# Creating Classes

A class is a blueprint used to create objects.

It defines:

- Attributes (data stored in the object)
- Methods (functions that operate on the object)

In real life, a class is like a template, while objects are actual entities created from that template.

### Example Analogy

**Class:** Car

**Objects:**

- Car1
- Car2
- Car3

Each car has its own:

- brand
- speed
- color

But they all follow the same structure defined in the class.

---

# Basic Class Syntax

Python defines classes using the `class` keyword.

```python
class Car:
    pass
```

### Explanation

- `class` declares a new class.
- `Car` is the class name.
- `pass` means the class is empty.

This creates a class but does not yet define any attributes or methods.

---

# Adding Attributes and Methods

Classes usually define attributes and behaviors.

```python
class Car:
    def __init__(self, brand, speed):
        self.brand = brand
        self.speed = speed

    def accelerate(self):
        self.speed += 10
```

This class defines:

### Attributes

- `brand`
- `speed`

### Method

- `accelerate()`

---

# Creating Objects

An object is an instance of a class.

When a class is instantiated, Python allocates memory and creates a new object.

```python
car1 = Car("Toyota", 120)
car2 = Car("BMW", 150)
```

Now two objects exist:

| Object | Brand | Speed |
|----------|----------|----------|
| car1 | Toyota | 120 |
| car2 | BMW | 150 |

Each object maintains its own state.

---

# Instance Creation

When creating an object, Python internally performs several steps.

```python
car = Car("Tesla", 180)
```

## Step 1: Memory Allocation

Python allocates memory for a new object.

## Step 2: Object Creation

The object is created internally.

## Step 3: Constructor Execution

Python automatically calls the constructor:

```python
__init__()
```

Example:

```python
def __init__(self, brand, speed):
    self.brand = brand
    self.speed = speed
```

The constructor initializes the object's attributes.

## Step 4: Object Reference Returned

The variable `car` receives a reference to the created object.

---

# Object Identity

Every object in Python has a unique identity.

This identity represents the object's memory location.

You can view it using the `id()` function.

```python
car = Car("Tesla", 200)
print(id(car))
```

### Example Output

```python
140247583298192
```

This number represents the object's identity in memory.

---

# Identity vs Equality

Two objects may contain the same data but still be different objects.

```python
a = [1, 2, 3]
b = [1, 2, 3]
```

### Check Identity

```python
print(a is b)
```

Output:

```python
False
```

### Check Equality

```python
print(a == b)
```

Output:

```python
True
```

### Explanation

- `is` compares identity.
- `==` compares value.

---

# Object Lifecycle

Objects in Python go through a lifecycle from creation to destruction.

The lifecycle includes several stages.

## 1. Creation

Object created via class instantiation.

```python
user = User("Alice")
```

## 2. Usage

The object is used by calling methods and accessing attributes.

```python
user.login()
```

## 3. Reference Management

Python tracks references pointing to the object.

## 4. Garbage Collection

When no references remain, Python automatically removes the object from memory.

```python
obj = MyClass()
del obj
```

If no references remain, Python deletes the object.

---

# Object References

Variables in Python do not store objects directly.

They store references to objects.

```python
a = [1, 2, 3]
b = a
```

### Memory Model

```text
a ----> [1,2,3]
b ----/
```

Both variables refer to the same object.

Modifying through one variable affects the other.

```python
b.append(4)
print(a)
```

Output:

```python
[1, 2, 3, 4]
```

---

# Instance Attributes

Instance attributes belong to individual objects.

Each object stores its own values.

```python
class Person:
    def __init__(self, name):
        self.name = name
```

Creating objects:

```python
p1 = Person("Alice")
p2 = Person("Bob")
```

Attributes:

```text
p1.name = Alice
p2.name = Bob
```

Each object has independent data.

---

# Class Attributes

Class attributes belong to the class itself, not individual objects.

They are shared by all instances.

```python
class Dog:
    species = "Canine"

    def __init__(self, name):
        self.name = name
```

Creating objects:

```python
d1 = Dog("Buddy")
d2 = Dog("Max")
```

Accessing attribute:

```python
print(d1.species)
print(d2.species)
```

Output:

```python
Canine
Canine
```

The value is shared by all objects.

---

# Modifying Class Attributes

Changing the class attribute affects all objects.

```python
Dog.species = "Domestic Dog"
```

Now all instances see the new value.

---

# Instance Attribute Shadowing

If an instance modifies the attribute, it creates its own version.

```python
d1.species = "Wolf"
```

Now:

```text
d1.species -> Wolf
d2.species -> Domestic Dog
```

---

# Accessing Attributes

Attributes can be accessed using dot notation.

```python
person.name
car.speed
```

Example:

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

s = Student("Ravi", 90)

print(s.name)
print(s.marks)
```

---

# Modifying Attributes

Attributes can be modified after object creation.

```python
s.marks = 95
```

Example:

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

e = Employee("John", 50000)

e.salary = 60000
```

Output:

```text
salary updated to 60000
```

---

# Deleting Attributes

Attributes can also be removed using the `del` keyword.

```python
del object.attribute
```

Example:

```python
class Example:
    def __init__(self):
        self.value = 10

obj = Example()

print(obj.value)

del obj.value
```

Now accessing it causes:

```python
AttributeError: 'Example' object has no attribute 'value'
```

---

# Real Practical Example

Consider a simple Bank Account System.

```python
class BankAccount:
    bank_name = "National Bank"

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
        else:
            print("Insufficient balance")

    def display(self):
        print(self.owner, self.balance)
```

Using the class:

```python
acc1 = BankAccount("Alice", 1000)
acc2 = BankAccount("Bob", 500)

acc1.deposit(200)
acc2.withdraw(100)

acc1.display()
acc2.display()
```

Output:

```python
Alice 1200
Bob 400
```

---

# Visual Representation

### Conceptual Memory Layout

```text
Class: BankAccount
│
├── bank_name = "National Bank"
│
├── acc1 object
│      owner = Alice
│      balance = 1200
│
└── acc2 object
       owner = Bob
       balance = 400
```

The class defines the structure, while each object stores its own state.

---

# Summary

Classes and objects form the core foundation of Object-Oriented Programming in Python.

### Key Ideas

- Classes define blueprints for objects.
- Objects are instances created from classes.
- Object creation involves memory allocation and constructor execution.
- Every object has a unique identity in memory.
- Variables store references to objects.
- Instance attributes belong to individual objects.
- Class attributes are shared by all instances.
- Attributes can be accessed, modified, and deleted dynamically.

Understanding these concepts provides the groundwork for advanced Python OOP topics such as constructors, inheritance, polymorphism, and object lifecycle management.
