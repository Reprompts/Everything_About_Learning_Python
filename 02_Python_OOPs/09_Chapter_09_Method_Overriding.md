# Python OOPs: Chapter 9 Method Overriding

Method overriding is a fundamental concept in Object-Oriented Programming (OOP) that allows a subclass to provide its own implementation of a method already defined in its parent class.

Overriding enables a derived class to customize or extend the behavior of inherited methods while maintaining the same method interface.

This mechanism is essential for building flexible and extensible class hierarchies, where subclasses adapt inherited functionality to their specific needs.

In Python, method overriding occurs when:

- A child class defines a method with the same name
- The method has the same parameter structure
- The method replaces the behavior of the parent method when called through the child object

---

# Overriding Parent Methods

When a subclass defines a method with the same name as a method in its parent class, the subclass method overrides the parent method.

The overridden method in the parent class becomes hidden when the method is called through the child object.

## Basic Example

```python
class Animal:
    def speak(self):
        print("Animal makes a sound")


class Dog(Animal):
    def speak(self):
        print("Dog barks")
```

### Usage

```python
d = Dog()
d.speak()
```

### Output

```text
Dog barks
```

Even though the `Animal` class has a `speak()` method, the `Dog` class overrides it with its own implementation.

---

# How Python Resolves Overridden Methods

Python determines which method to execute using the **Method Resolution Order (MRO)**.

When a method is called:

1. Python checks the object's class.
2. Then searches parent classes according to the MRO.
3. The first matching method found is executed.

## Example

```python
print(Dog.__mro__)
```

### Typical Output

```text
(<class '__main__.Dog'>,
 <class '__main__.Animal'>,
 <class 'object'>)
```

### Search Order

1. Dog
2. Animal
3. object

---

# Redefining Behavior

The primary purpose of method overriding is changing inherited behavior.

## Example

```python
class Vehicle:
    def start(self):
        print("Vehicle engine starts")


class ElectricCar(Vehicle):
    def start(self):
        print("Electric car powers on silently")
```

### Usage

```python
car = ElectricCar()
car.start()
```

### Output

```text
Electric car powers on silently
```

Here, the `ElectricCar` modifies how the `start()` method works.

---

# Partial Method Overriding

Sometimes a subclass wants to extend the parent method instead of completely replacing it.

In such cases, the child method calls the parent method and then adds additional behavior.

## Example

```python
class Employee:
    def work(self):
        print("Employee performs general tasks")


class Manager(Employee):
    def work(self):
        print("Manager reviewing team work")
        print("Manager planning strategy")
```

### Output

```text
Manager reviewing team work
Manager planning strategy
```

The parent behavior is replaced entirely in this example.

However, often we want to preserve parent behavior.

---

# Calling Parent Methods

To reuse functionality from the parent method while overriding it, Python provides a way to explicitly call the parent method.

This allows the child method to:

- Execute parent logic
- Extend functionality

## Example

```python
class Employee:
    def work(self):
        print("Employee working")


class Manager(Employee):
    def work(self):
        Employee.work(self)
        print("Manager supervising team")
```

### Usage

```python
m = Manager()
m.work()
```

### Output

```text
Employee working
Manager supervising team
```

The parent method is explicitly called before adding new behavior.

---

# The `super()` Function

The recommended way to call parent methods is by using the `super()` function.

`super()` returns a proxy object that refers to the parent class.

It allows the child class to call methods defined in the parent class without explicitly naming it.

## Syntax

```python
super().method_name()
```

## Example

```python
class Employee:
    def work(self):
        print("Employee working")


class Manager(Employee):
    def work(self):
        super().work()
        print("Manager supervising team")
```

### Usage

```python
m = Manager()
m.work()
```

### Output

```text
Employee working
Manager supervising team
```

### Why Use `super()`?

- Automatically follows the Method Resolution Order (MRO)
- Supports multiple inheritance correctly
- Makes code easier to maintain

---

# How `super()` Works Internally

When `super()` is used:

1. Python determines the current class.
2. It checks the MRO.
3. It calls the next class in the inheritance chain.

## Example

```python
class A:
    def show(self):
        print("A")


class B(A):
    def show(self):
        super().show()
        print("B")
```

### Usage

```python
b = B()
b.show()
```

### Output

```text
A
B
```

The call order follows the MRO.

---

# Overriding Constructors

Constructors (`__init__`) can also be overridden in subclasses.

This is common when the subclass needs to initialize additional attributes.

## Example

```python
class Person:
    def __init__(self, name):
        self.name = name


class Student(Person):
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
```

### Usage

```python
s = Student("Alice", 101)

print(s.name)
print(s.student_id)
```

### Output

```text
Alice
101
```

However, this approach duplicates code from the parent constructor.

A better approach uses `super()`.

---

# Calling Parent Constructor with `super()`

## Example

```python
class Person:
    def __init__(self, name):
        self.name = name


class Student(Person):
    def __init__(self, name, student_id):
        super().__init__(name)
        self.student_id = student_id
```

### Usage

```python
s = Student("Alice", 101)

print(s.name)
print(s.student_id)
```

### Output

```text
Alice
101
```

This approach avoids repeating parent initialization logic.

---

# Constructor Overriding in Multi-Level Inheritance

## Example

```python
class A:
    def __init__(self):
        print("A constructor")


class B(A):
    def __init__(self):
        super().__init__()
        print("B constructor")


class C(B):
    def __init__(self):
        super().__init__()
        print("C constructor")
```

### Usage

```python
obj = C()
```

### Output

```text
A constructor
B constructor
C constructor
```

The constructors execute following the inheritance chain.

---

# Overriding Attributes

Attributes can also be overridden in subclasses.

## Example

```python
class Animal:
    sound = "Some sound"


class Dog(Animal):
    sound = "Bark"
```

### Usage

```python
print(Animal.sound)
print(Dog.sound)
```

### Output

```text
Some sound
Bark
```

The subclass defines its own attribute value.

---

# Overriding Instance Attributes

Instance attributes defined in the parent class can be replaced in the child class.

## Example

```python
class Animal:
    def __init__(self):
        self.type = "Animal"


class Dog(Animal):
    def __init__(self):
        self.type = "Dog"
```

### Usage

```python
d = Dog()
print(d.type)
```

### Output

```text
Dog
```

However, this again duplicates initialization logic.

### Better Version

```python
class Dog(Animal):
    def __init__(self):
        super().__init__()
        self.type = "Dog"
```

---

# Practical Real-World Example

Consider a payment processing system.

## Base Class

```python
class Payment:
    def process(self):
        print("Processing generic payment")
```

## Derived Classes

```python
class CreditCardPayment(Payment):
    def process(self):
        print("Processing credit card payment")


class PayPalPayment(Payment):
    def process(self):
        print("Processing PayPal payment")
```

### Usage

```python
p1 = CreditCardPayment()
p2 = PayPalPayment()

p1.process()
p2.process()
```

### Output

```text
Processing credit card payment
Processing PayPal payment
```

Each subclass overrides the generic payment behavior.

---

# Common Mistakes in Method Overriding

## Incorrect Method Name

```python
class Dog(Animal):
    def Speak(self):
        print("Dog barks")
```

Because Python is case-sensitive, this does **not** override `speak()`.

---

## Incorrect Parameter List

```python
class Dog(Animal):
    def speak(self, sound):
        print(sound)
```

The method signature differs from the parent method.

This may cause unexpected behavior when the method is called polymorphically.

---

## Forgetting `super()` in Constructor

Failing to call `super()` may leave parent attributes uninitialized.

Example:

```python
class Student(Person):
    def __init__(self, student_id):
        self.student_id = student_id
```

If `Person` initializes important attributes, they will not be created.

---

# Best Practices

- Use method overriding when a subclass needs specialized behavior.
- Always maintain method compatibility with the parent class.
- Prefer `super()` over direct parent class calls.
- Avoid overriding methods unnecessarily if the parent behavior already fits.
- Ensure constructors call `super()` when inheriting initialization logic.
- Follow the Method Resolution Order (MRO) when designing inheritance hierarchies.

---

# Summary

Method overriding allows a subclass to redefine behavior inherited from its parent class. When a child class defines a method with the same name as a parent method, Python executes the child version instead.

Overriding enables developers to customize inherited behavior, adapt base class functionality to specific scenarios, and implement polymorphic systems.

Python provides the `super()` function to safely call parent methods while respecting the Method Resolution Order (MRO). This is particularly important in multiple inheritance scenarios.

In addition to methods, subclasses can override constructors and attributes to customize initialization and behavior.

Proper use of method overriding leads to flexible, extensible, and maintainable object-oriented designs.
