# Python OOPs: Chapter 5 Instance & Class Variables

## Introduction

In Object-Oriented Programming, variables inside classes are used to store the state of objects and the shared properties of the class.

Python supports two main types of variables within classes:

- Instance Variables
- Class Variables

Understanding how these variables behave, how they are accessed, and how Python resolves them internally is essential for writing correct and maintainable object-oriented programs.

These variables determine whether data belongs to individual objects or is shared across all instances of a class.

---

# Instance Variables

An instance variable is a variable that belongs to a specific object. Each object created from a class has its own copy of instance variables.

These variables store object-specific data, meaning that each instance maintains its own independent state.

Instance variables are usually defined inside the `__init__()` method using the `self` keyword.

---

## Defining Instance Variables

### Example

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
```

### Explanation

- `self.name` is an instance variable.
- `self.marks` is an instance variable.

Each object will store its own values for these variables.

---

## Creating Objects with Instance Variables

### Example

```python
s1 = Student("Ravi", 85)
s2 = Student("Neha", 92)
```

### Memory Representation (Conceptual)

```text
Class: Student

Object s1
    name = Ravi
    marks = 85

Object s2
    name = Neha
    marks = 92
```

Each object stores separate data.

---

## Accessing Instance Variables

Instance variables are accessed using dot notation.

### Example

```python
print(s1.name)
print(s2.marks)
```

### Output

```text
Ravi
92
```

---

## Modifying Instance Variables

Instance variables can be modified independently for each object.

### Example

```python
s1.marks = 90
```

Now:

```text
s1.marks = 90
s2.marks = 92
```

Only the specific object changes.

---

## Adding Instance Variables Dynamically

Python allows instance variables to be created dynamically outside the constructor.

### Example

```python
class Example:
    pass

obj = Example()
obj.value = 10
```

Here the variable `value` is added at runtime.

---

# Class Variables

A class variable is shared among all instances of a class.

It belongs to the class itself rather than to individual objects.

These variables are defined inside the class but outside methods.

They represent shared properties or configuration values.

---

## Defining Class Variables

### Example

```python
class Employee:
    company = "TechCorp"

    def __init__(self, name):
        self.name = name
```

Here:

```python
company
```

is a class variable.

---

## Accessing Class Variables

Class variables can be accessed in two ways.

### Access via Class

```python
print(Employee.company)
```

### Output

```text
TechCorp
```

---

### Access via Instance

```python
e = Employee("Alice")

print(e.company)
```

### Output

```text
TechCorp
```

Even though accessed through the object, the variable still belongs to the class.

---

# Shared vs Unique Data

Understanding the difference between shared and unique data is crucial.

| Variable Type | Ownership | Behavior |
|--------------|-----------|----------|
| Instance Variable | Object | Unique per object |
| Class Variable | Class | Shared by all objects |

### Example

```python
class Car:
    wheels = 4

    def __init__(self, brand):
        self.brand = brand
```

### Objects

```python
c1 = Car("Toyota")
c2 = Car("BMW")
```

### Conceptual Structure

```text
Class: Car
    wheels = 4

Object c1
    brand = Toyota

Object c2
    brand = BMW
```

All cars share the same number of wheels.

---

# Attribute Resolution in Python

Python follows a specific order when searching for attributes.

This process is called **attribute resolution**.

When accessing an attribute via an object, Python checks:

1. Instance attributes
2. Class attributes
3. Parent class attributes (if inheritance exists)

### Example

```python
class Example:
    value = 10
```

```python
obj = Example()

print(obj.value)
```

Python searches:

```text
obj.__dict__
Example.__dict__
Parent classes
```

---

## Demonstration

```python
class Demo:
    number = 50

    def __init__(self):
        self.number = 100
```

### Usage

```python
obj = Demo()

print(obj.number)
```

### Output

```text
100
```

Python found the instance variable first.

---

# Class Variable Modification

Class variables can be modified at the class level or the instance level.

However, these modifications behave differently.

---

## Modifying via Class

### Example

```python
class Counter:
    count = 0
```

Modify:

```python
Counter.count = 10
```

Now all objects see the new value.

---

## Modifying via Instance

### Example

```python
obj = Counter()

obj.count = 5
```

This creates a new instance variable instead of modifying the class variable.

Now:

```text
obj.count = 5
Counter.count = 10
```

---

## Example Demonstration

```python
class Test:
    value = 10

t1 = Test()
t2 = Test()

t1.value = 50

print(t1.value)
print(t2.value)
print(Test.value)
```

### Output

```text
50
10
10
```

### Explanation

- `t1.value` becomes an instance variable.
- `t2.value` still uses the class variable.

---

# Accessing Variables via Class and Instance

Python allows attributes to be accessed through either the class or an object.

However, the behavior depends on the variable type.

---

## Accessing Instance Variables via Instance

### Correct Usage

```python
obj.attribute
```

### Example

```python
class User:
    def __init__(self, name):
        self.name = name

u = User("Alice")

print(u.name)
```

### Output

```text
Alice
```

---

## Accessing Class Variables via Class

### Best Practice

```python
ClassName.variable
```

### Example

```python
print(Employee.company)
```

---

## Accessing Class Variables via Instance

Possible but less clear.

### Example

```python
emp = Employee("Bob")

print(emp.company)
```

This works but still refers to the class variable unless overridden.

---

# Practical Example

Consider a University System.

```python
class Student:
    university = "Global University"

    def __init__(self, name, course):
        self.name = name
        self.course = course
```

### Create Objects

```python
s1 = Student("Ravi", "Computer Science")
s2 = Student("Neha", "Physics")
```

### Access Attributes

```python
print(s1.name)
print(s2.course)
print(Student.university)
```

### Output

```text
Ravi
Physics
Global University
```

---

## Changing Class Variable

```python
Student.university = "International University"
```

Now:

```text
s1.university → International University
s2.university → International University
```

---

# Real-World Example: Product Inventory

```python
class Product:
    tax_rate = 0.18

    def __init__(self, name, price):
        self.name = name
        self.price = price

    def final_price(self):
        return self.price + (self.price * Product.tax_rate)
```

### Usage

```python
p1 = Product("Laptop", 50000)
p2 = Product("Phone", 20000)

print(p1.final_price())
print(p2.final_price())
```

### Output

```text
59000
23600
```

If the tax rate changes:

```python
Product.tax_rate = 0.20
```

All products automatically use the new value.

---

# Internal Representation

Conceptually, Python stores attributes like this:

### Class Namespace

```python
Student.__dict__

{
    "university": "Global University"
}
```

### Instance Namespace

```python
s1.__dict__ = {
    "name": "Ravi",
    "course": "Computer Science"
}
```

Attribute lookup checks these dictionaries.

---

# Best Practices

- Use instance variables for data unique to objects.
- Use class variables for shared configuration.
- Avoid modifying class variables through instances.
- Prefer accessing class variables through the class name.
- Keep shared constants as class variables.

---

# Summary

Instance and class variables are fundamental components of Python's object-oriented model.

Instance variables store object-specific data and are created using `self`. Each object has its own independent copy.

Class variables belong to the class itself and are shared among all instances. They represent shared properties or configuration values.

Python resolves attributes by first checking instance attributes, then class attributes, and finally parent classes.

Modifying class variables at the class level affects all objects, while modifying them through instances creates new instance attributes.

Understanding how Python handles instance and class variables ensures proper data organization and helps prevent subtle bugs in object-oriented programs.
