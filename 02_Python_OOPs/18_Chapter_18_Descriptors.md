# Python OOPs: Chapter 18 Descriptors

Descriptors are one of the most powerful and foundational mechanisms in Python's object model. They provide a way to customize attribute access by defining how attributes are retrieved, assigned, or deleted.

Many important Python features are implemented internally using descriptors, including:

- Methods
- Properties
- Static methods
- Class methods
- Attribute access control
- ORM fields
- Validation systems

Understanding descriptors provides deep insight into how Python's attribute lookup and object behavior work internally.

This tutorial explains in detail:

- What descriptors are
- The descriptor protocol
- `__get__`, `__set__`, and `__delete__`
- Data descriptors
- Non-data descriptors
- Descriptor precedence
- Practical use cases

---

# What is a Descriptor?

A descriptor is any object that defines one or more of the following methods:

- `__get__()`
- `__set__()`
- `__delete__()`

These methods form the **descriptor protocol**.

Descriptors allow objects to control how attributes are:

- Accessed
- Modified
- Deleted

In other words, descriptors allow **custom attribute behavior**.

---

# Descriptor Protocol

The descriptor protocol consists of three special methods.

| Method | Purpose |
|----------|----------|
| `__get__` | Controls attribute retrieval |
| `__set__` | Controls attribute assignment |
| `__delete__` | Controls attribute deletion |

A class implementing these methods can act as a managed attribute.

---

# Basic Descriptor Structure

## Example

```python
class Descriptor:
    def __get__(self, instance, owner):
        pass

    def __set__(self, instance, value):
        pass

    def __delete__(self, instance):
        pass
```

### Parameters Explained

#### `instance`

The object instance accessing the attribute.

#### `owner`

The class where the descriptor is defined.

---

# How Descriptors Work

Descriptors must be assigned as class attributes.

## Example

```python
class MyClass:
    attr = Descriptor()
```

When accessing:

```python
obj.attr
```

Python internally calls:

```python
Descriptor.__get__(obj, MyClass)
```

Similarly:

```python
obj.attr = value
```

calls:

```python
Descriptor.__set__(obj, value)
```

---

# The `__get__()` Method

`__get__()` defines how attribute values are retrieved.

## Method Signature

```python
__get__(self, instance, owner)
```

### Parameters

| Parameter | Meaning |
|------------|----------|
| `self` | Descriptor object |
| `instance` | Object accessing attribute |
| `owner` | Class of the object |

---

## Example: Simple Descriptor

```python
class NameDescriptor:
    def __get__(self, instance, owner):
        print("Getting name")
        return instance._name

    def __set__(self, instance, value):
        print("Setting name")
        instance._name = value
```

### Using the Descriptor

```python
class Person:
    name = NameDescriptor()

    def __init__(self, name):
        self.name = name
```

### Usage

```python
p = Person("Alice")

print(p.name)
```

### Output

```python
Setting name
Getting name
Alice
```

The descriptor controls attribute access.

---

# The `__set__()` Method

`__set__()` controls attribute assignment.

## Signature

```python
__set__(self, instance, value)
```

---

## Example

```python
class AgeDescriptor:
    def __set__(self, instance, value):
        if value < 0:
            raise ValueError("Age cannot be negative")

        instance._age = value

    def __get__(self, instance, owner):
        return instance._age
```

### Usage

```python
class Person:
    age = AgeDescriptor()

    def __init__(self, age):
        self.age = age
```

### Test

```python
p = Person(25)

print(p.age)
```

### Invalid Assignment

```python
p.age = -10
```

### Error

```python
ValueError: Age cannot be negative
```

Descriptors enable validation logic.

---

# The `__delete__()` Method

`__delete__()` controls attribute deletion.

## Signature

```python
__delete__(self, instance)
```

---

## Example

```python
class AttributeDescriptor:
    def __get__(self, instance, owner):
        return instance._value

    def __set__(self, instance, value):
        instance._value = value

    def __delete__(self, instance):
        print("Deleting attribute")
        del instance._value
```

### Usage

```python
class Data:
    value = AttributeDescriptor()
```

### Test

```python
d = Data()

d.value = 100

del d.value
```

### Output

```python
Deleting attribute
```

---

# Data Descriptors

A **data descriptor** defines:

- `__get__()` and `__set__()`

or

- `__get__()` and `__delete__()`

Data descriptors control both reading and writing.

## Example

```python
class DataDescriptor:
    def __get__(self, instance, owner):
        print("get called")

    def __set__(self, instance, value):
        print("set called")
```

### Important Rule

Data descriptors override instance attributes.

---

# Non-Data Descriptors

A **non-data descriptor** defines only:

- `__get__()`

## Example

```python
class NonDataDescriptor:
    def __get__(self, instance, owner):
        print("Accessed descriptor")
```

### Important Rule

Non-data descriptors allow instance attributes to override them.

---

# Descriptor Precedence Rules

Python follows a specific order when resolving attributes.

## Attribute Lookup Order

1. Data descriptors
2. Instance attributes
3. Non-data descriptors
4. Class attributes
5. Parent classes

When evaluating:

```python
obj.attribute
```

Python searches in this order.

---

# Example Demonstrating Descriptor Precedence

```python
class Descriptor:
    def __get__(self, instance, owner):
        return "descriptor value"


class Test:
    attr = Descriptor()

    def __init__(self):
        self.attr = "instance value"
```

### Usage

```python
t = Test()

print(t.attr)
```

### Output

```python
instance value
```

Because instance attributes override non-data descriptors.

---

# Descriptors and Python Functions

Functions defined inside classes are actually descriptors.

## Example

```python
class MyClass:
    def greet(self):
        print("Hello")
```

Accessing:

```python
obj.greet
```

Triggers descriptor behavior that binds the method to the instance.

Internally:

```python
function.__get__(obj, MyClass)
```

This creates a **bound method**.

---

# Properties Implemented Using Descriptors

The `property()` function internally uses descriptors.

## Example

```python
class Person:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, value):
        self._name = value

    name = property(get_name, set_name)
```

### Usage

```python
p = Person("Alice")

print(p.name)

p.name = "Bob"
```

The property object implements descriptor methods.

---

# Descriptor Use Cases

Descriptors are widely used in advanced Python programming.

## Common Applications

- Attribute validation
- Computed attributes
- Lazy loading
- ORM field definitions
- Method binding
- Access control

---

# Example: Attribute Validation System

```python
class PositiveNumber:
    def __set__(self, instance, value):
        if value <= 0:
            raise ValueError(
                "Value must be positive"
            )

        instance._value = value

    def __get__(self, instance, owner):
        return instance._value
```

### Usage

```python
class Product:
    price = PositiveNumber()

    def __init__(self, price):
        self.price = price
```

### Test

```python
p = Product(100)

print(p.price)
```

### Invalid

```python
p.price = -50
```

### Result

```python
ValueError
```

---

# Example: Lazy Loading Descriptor

```python
class LazyProperty:
    def __init__(self, function):
        self.function = function

    def __get__(self, instance, owner):
        value = self.function(instance)

        setattr(
            instance,
            self.function.__name__,
            value
        )

        return value
```

### Usage

```python
class Data:
    @LazyProperty
    def expensive_computation(self):
        print("Computing value")
        return 42
```

### Test

```python
d = Data()

print(d.expensive_computation)

print(d.expensive_computation)
```

### Output

```python
Computing value
42
42
```

The value is computed only once.

---

# Real-World Use: ORM Systems

Descriptors are heavily used in Object Relational Mapping frameworks such as:

- Django ORM
- SQLAlchemy

---

## Example Conceptual Field

```python
class Field:
    def __get__(self, instance, owner):
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        instance.__dict__[self.name] = value
```

Used like:

```python
class User:
    name = Field()
    age = Field()
```

Descriptors control how fields behave.

---

# Advantages of Descriptors

Descriptors provide several benefits.

## Centralized Attribute Logic

Validation and behavior can be reused.

---

## Clean Object Interfaces

Attributes behave like normal variables.

---

## Code Reuse

Descriptors can be shared across classes.

---

## Powerful Metaprogramming

They enable advanced frameworks and libraries.

---

# Descriptor Lookup Flow

```text
obj.attribute
      |
      v
1. Data Descriptor?
      |
      v
2. Instance Attribute?
      |
      v
3. Non-Data Descriptor?
      |
      v
4. Class Attribute?
      |
      v
5. Parent Classes?
```

This order determines which value Python returns.

---

# Data Descriptor vs Non-Data Descriptor

| Feature | Data Descriptor | Non-Data Descriptor |
|----------|----------------|---------------------|
| `__get__` | Yes | Yes |
| `__set__` | Yes | No |
| `__delete__` | Optional | No |
| Overrides instance attribute | Yes | No |
| Typical use | Validation, managed attributes | Methods, computed values |

---

# Real Examples of Descriptors in Python

| Python Feature | Descriptor-Based |
|----------------|------------------|
| Instance methods | Yes |
| `property()` | Yes |
| `staticmethod()` | Yes |
| `classmethod()` | Yes |
| ORM model fields | Yes |
| Validation frameworks | Yes |

---

# Summary

Descriptors are a fundamental part of Python's object model that allow developers to customize attribute behavior. By implementing the descriptor protocol methods `__get__`, `__set__`, and `__delete__`, objects can control how attributes are accessed, assigned, and deleted.

Descriptors form the foundation for many built-in Python features, including:

- Properties
- Methods
- Static methods
- Class methods

They also power advanced frameworks such as ORMs and validation systems.

There are two main types of descriptors:

### Data Descriptors

Control both reading and writing of attributes.

### Non-Data Descriptors

Control only attribute access.

By understanding descriptors, developers gain deeper insight into Python's attribute lookup system and can design powerful abstractions that integrate seamlessly with the Python language.
