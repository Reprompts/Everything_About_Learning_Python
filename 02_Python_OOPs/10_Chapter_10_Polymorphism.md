# Python OOPs: Chapter 10 Polymorphism

Polymorphism is one of the core principles of Object-Oriented Programming (OOP). The term comes from two Greek words:

- **Poly** meaning *many*
- **Morph** meaning *forms*

Polymorphism therefore means **"many forms."**

In programming, polymorphism refers to the ability of a single interface, function, method, or operator to behave differently depending on the object or data type it operates on.

This allows developers to write generic, flexible, and reusable code that can operate on many types of objects.

In Python, polymorphism is a natural feature of the language because Python is dynamically typed and heavily object-oriented.

Polymorphism appears in several forms:

- Function polymorphism
- Method polymorphism
- Duck typing
- Operator polymorphism
- Method overriding polymorphism

---

# Concept of Polymorphism

Polymorphism allows the same operation to produce different behavior depending on the context.

For example:

A function called `draw()` might:

- Draw a circle
- Draw a rectangle
- Draw a triangle

Each object implements its own version of `draw()`.

## Example

```python
class Circle:
    def draw(self):
        print("Drawing a circle")


class Rectangle:
    def draw(self):
        print("Drawing a rectangle")
```

### Usage

```python
shapes = [Circle(), Rectangle()]

for shape in shapes:
    shape.draw()
```

### Output

```text
Drawing a circle
Drawing a rectangle
```

The same method `draw()` behaves differently depending on the object type.

This is polymorphism.

---

# Function Polymorphism

Function polymorphism occurs when the same function can operate on different types of data.

Python built-in functions demonstrate polymorphism extensively.

## Example: `len()` Function

The `len()` function works with many different objects.

```python
print(len("Python"))
print(len([1, 2, 3, 4]))
print(len((10, 20, 30)))
print(len({"a": 1, "b": 2}))
```

### Output

```text
6
4
3
2
```

The same function works with:

- Strings
- Lists
- Tuples
- Dictionaries

Each object defines how its length should be calculated.

---

## Example: `sum()` Function

```python
print(sum([1, 2, 3]))
print(sum((10, 20, 30)))
```

### Output

```text
6
60
```

Again, the same function works with different iterable types.

---

# Method Polymorphism

Method polymorphism occurs when different objects respond differently to the same method call.

## Example

```python
class Dog:
    def speak(self):
        print("Dog barks")


class Cat:
    def speak(self):
        print("Cat meows")
```

### Usage

```python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

### Output

```text
Dog barks
Cat meows
```

Both objects respond to the same method name but perform different actions.

This demonstrates method polymorphism.

---

# Duck Typing

Duck typing is a powerful polymorphism concept used in Python.

The principle comes from the phrase:

> "If it walks like a duck and quacks like a duck, it is a duck."

In Python, object type does not matter.

What matters is whether the object provides the required behavior.

If an object implements the required methods, it can be used.

---

## Example of Duck Typing

```python
class Dog:
    def speak(self):
        print("Dog barks")


class Robot:
    def speak(self):
        print("Robot speaking")
```

### Function

```python
def make_speak(entity):
    entity.speak()
```

### Usage

```python
make_speak(Dog())
make_speak(Robot())
```

### Output

```text
Dog barks
Robot speaking
```

The function works with both objects because they implement the `speak()` method.

Python does not require a common parent class.

This is duck typing.

---

# Duck Typing with a Real-World Example

Consider a logging system.

```python
class FileLogger:
    def write(self, message):
        print("Writing to file:", message)


class NetworkLogger:
    def write(self, message):
        print("Sending over network:", message)
```

### Generic Function

```python
def log(logger, message):
    logger.write(message)
```

### Usage

```python
log(FileLogger(), "System started")
log(NetworkLogger(), "System started")
```

### Output

```text
Writing to file: System started
Sending over network: System started
```

The function accepts any object that implements `write()`.

---

# Operator Polymorphism

Operators in Python behave differently depending on the operands.

This is called operator polymorphism.

## Example: `+`

The `+` operator performs different operations depending on the type.

```python
print(10 + 5)
print("Hello " + "World")
print([1, 2] + [3, 4])
```

### Output

```text
15
Hello World
[1, 2, 3, 4]
```

The same operator performs:

- Arithmetic addition
- String concatenation
- List concatenation

This is polymorphism.

---

# Operator Overloading

Python allows classes to define their own operator behavior using special methods.

## Example

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(
            self.x + other.x,
            self.y + other.y
        )

    def display(self):
        print(self.x, self.y)
```

### Usage

```python
v1 = Vector(2, 3)
v2 = Vector(4, 5)

v3 = v1 + v2

v3.display()
```

### Output

```text
6 8
```

The `+` operator works with custom objects.

---

# Method Overriding Polymorphism

Method overriding is another form of polymorphism.

When a child class overrides a method of the parent class, the method behaves differently depending on the object type.

## Example

```python
class Animal:
    def speak(self):
        print("Animal makes sound")


class Dog(Animal):
    def speak(self):
        print("Dog barks")


class Cat(Animal):
    def speak(self):
        print("Cat meows")
```

### Usage

```python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

### Output

```text
Dog barks
Cat meows
```

The same method `speak()` behaves differently depending on the class.

---

# Runtime Polymorphism

Python polymorphism is typically runtime polymorphism.

This means the method to execute is determined during program execution, not during compilation.

## Example

```python
def start_engine(vehicle):
    vehicle.start()
```

### Objects

```python
class Car:
    def start(self):
        print("Car engine started")


class Bike:
    def start(self):
        print("Bike engine started")
```

### Usage

```python
start_engine(Car())
start_engine(Bike())
```

### Output

```text
Car engine started
Bike engine started
```

The function adapts to the object type dynamically.

---

# Real-World Example of Polymorphism

Consider a payment processing system.

## Base Class

```python
class Payment:
    def process(self):
        raise NotImplementedError
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

## Generic Function

```python
def make_payment(payment):
    payment.process()
```

## Usage

```python
make_payment(CreditCardPayment())
make_payment(PayPalPayment())
```

### Output

```text
Processing credit card payment
Processing PayPal payment
```

The same function works with different payment types.

---

# Advantages of Polymorphism

## Code Reusability

Generic functions can operate on multiple object types.

## Flexibility

Programs become adaptable to new classes without modification.

## Extensibility

New classes can integrate easily without changing existing code.

## Cleaner Design

Polymorphism reduces complex conditional logic.

### Without Polymorphism

```python
if object is Dog:
    bark()
elif object is Cat:
    meow()
```

### With Polymorphism

```python
object.speak()
```

---

# Polymorphism vs Overloading

In many languages, polymorphism includes method overloading.

Python does not support traditional method overloading based on parameters.

### Example (Not Allowed in Python)

```text
add(int a, int b)
add(int a, int b, int c)
```

Instead, Python uses:

- Default arguments
- Variable arguments
- Dynamic typing

## Example

```python
def add(*numbers):
    return sum(numbers)
```

### Usage

```python
add(1, 2)
add(1, 2, 3)
```

---

# Best Practices

- Design functions to operate on interfaces rather than specific classes.
- Prefer duck typing for flexible systems.
- Avoid unnecessary type checking.

Instead of:

```python
if isinstance(obj, Dog):
    ...
```

Prefer:

```python
obj.speak()
```

- Ensure consistent method names across related classes.

---

# Summary

Polymorphism allows a single interface, method, or operator to exhibit different behavior depending on the object or data type it interacts with.

Python supports polymorphism through:

- Function polymorphism
- Method polymorphism
- Duck typing
- Operator polymorphism
- Method overriding

Because Python is dynamically typed, polymorphism is deeply integrated into the language design.

Duck typing enables flexible programming where objects are used based on their behavior rather than their type. Operator polymorphism allows built-in operators to behave differently for different data types.

Method overriding provides runtime polymorphism in class hierarchies, enabling subclasses to redefine inherited behavior.

Together, these mechanisms make Python programs more flexible, reusable, and extensible, forming an essential part of Python's object-oriented programming model.
