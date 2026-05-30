# Python OOPs: Chapter 19 Metaclasses

Metaclasses represent one of the most advanced features in Python's object system. They provide the ability to control how classes themselves are created, allowing developers to modify or extend the behavior of class definitions.

To understand metaclasses, it is essential to recognize an important concept in Python:

> Classes themselves are objects.

Because classes are objects, they are created using a class factory mechanism, and that factory is called a **metaclass**.

This tutorial explores the complete concept of metaclasses in depth, including:

- Classes as objects
- The class creation process
- The `type` metaclass
- Custom metaclasses
- `__new__` in metaclasses
- `__init__` in metaclasses
- Controlling class creation

---

# Classes as Objects

In Python, everything is an object, including classes.

A class behaves just like any other object. It can be:

- Assigned to variables
- Passed to functions
- Returned from functions
- Stored in data structures

## Example

```python
class Person:
    pass

print(type(Person))
```

### Output

```python
<class 'type'>
```

This means that the class `Person` itself is an object whose type is `type`.

---

## Assigning Classes to Variables

Because classes are objects, they can be assigned to variables.

```python
class Car:
    pass

vehicle = Car

obj = vehicle()

print(type(obj))
```

### Output

```python
<class '__main__.Car'>
```

The variable `vehicle` now refers to the class.

---

## Classes in Data Structures

Classes can also be stored in containers.

```python
class Dog:
    pass

class Cat:
    pass

animals = [Dog, Cat]

pet = animals[0]()

print(type(pet))
```

### Output

```python
<class '__main__.Dog'>
```

This demonstrates that classes are first-class objects.

---

# The Class Creation Process

When Python encounters a class definition, it does not immediately create a class object directly. Instead, it follows a multi-step class creation process.

## Steps Involved

1. Class body execution
2. Namespace creation
3. Class object creation
4. Binding class name

---

## Step 1: Class Body Execution

The code inside the class is executed first.

### Example

```python
class Example:
    x = 10

    def method(self):
        pass
```

The class body is executed like a normal block of code, producing a dictionary of attributes.

### Resulting Namespace

```python
{
    'x': 10,
    'method': <function>
}
```

---

## Step 2: Namespace Creation

Python creates a dictionary representing the class namespace.

### Example Namespace

```python
{
    '__module__': '__main__',
    '__qualname__': 'Example',
    'x': 10,
    'method': <function>
}
```

---

## Step 3: Class Object Creation

Python then calls the metaclass to construct the class object.

Conceptually:

```python
Example = metaclass("Example", bases, namespace)
```

The metaclass is responsible for building the final class object.

---

## Step 4: Binding the Class Name

The resulting class object is assigned to the name defined in the code.

```python
Example = created_class_object
```

---

# The `type` Metaclass

The default metaclass in Python is `type`.

The `type` class serves two roles:

1. It determines the type of objects.
2. It acts as a metaclass for creating classes.

## Example

```python
class Person:
    pass

print(type(Person))
```

### Output

```python
<class 'type'>
```

This means:

- `Person` is an instance of `type`

---

# Creating Classes Using `type`

Classes can be created dynamically using `type`.

## Syntax

```python
type(class_name, base_classes, attributes)
```

## Example

```python
Person = type(
    "Person",
    (),
    {"name": "Unknown"}
)

p = Person()

print(p.name)
```

### Output

```python
Unknown
```

This creates a class dynamically.

---

## Adding Methods Dynamically

```python
def greet(self):
    print("Hello")

Person = type(
    "Person",
    (),
    {"greet": greet}
)

p = Person()

p.greet()
```

### Output

```python
Hello
```

This demonstrates that `type` constructs classes.

---

# Custom Metaclasses

A custom metaclass is a class that inherits from `type`.

Custom metaclasses allow developers to control class creation behavior.

## Basic Structure

```python
class MyMeta(type):
    def __new__(cls, name, bases, namespace):
        return super().__new__(cls, name, bases, namespace)
```

## Using the Metaclass

```python
class MyClass(metaclass=MyMeta):
    pass
```

The metaclass intercepts the class creation process.

---

# `__new__` in Metaclasses

`__new__` is responsible for creating the class object.

## Signature

```python
__new__(metaclass, name, bases, namespace)
```

### Parameters

| Parameter | Description |
|------------|------------|
| `metaclass` | The metaclass itself |
| `name` | Class name |
| `bases` | Parent classes |
| `namespace` | Dictionary of attributes |

---

## Example: Logging Class Creation

```python
class MetaLogger(type):
    def __new__(cls, name, bases, namespace):
        print("Creating class:", name)
        return super().__new__(cls, name, bases, namespace)
```

### Using the Metaclass

```python
class Test(metaclass=MetaLogger):
    x = 10
```

### Output

```python
Creating class: Test
```

The metaclass intercepts class creation.

---

# `__init__` in Metaclasses

After the class object is created, Python calls the metaclass `__init__`.

## Signature

```python
__init__(cls, name, bases, namespace)
```

## Example

```python
class MetaInit(type):
    def __init__(cls, name, bases, namespace):
        print("Initializing class:", name)
        super().__init__(name, bases, namespace)
```

### Usage

```python
class Example(metaclass=MetaInit):
    pass
```

### Output

```python
Initializing class: Example
```

---

# Difference Between `__new__` and `__init__` in Metaclasses

| Method | Purpose |
|----------|----------|
| `__new__` | Creates class object |
| `__init__` | Initializes class object |

### Workflow

```text
__new__  → Class object created
__init__ → Class object configured
```

---

# Controlling Class Creation

Metaclasses allow developers to enforce rules during class creation.

---

## Example: Enforcing Uppercase Attributes

```python
class UpperAttrMeta(type):
    def __new__(cls, name, bases, namespace):
        new_namespace = {}

        for key, value in namespace.items():
            if not key.startswith("__"):
                key = key.upper()

            new_namespace[key] = value

        return super().__new__(cls, name, bases, new_namespace)
```

### Usage

```python
class Sample(metaclass=UpperAttrMeta):
    value = 10
```

### Test

```python
print(Sample.VALUE)
```

### Output

```python
10
```

The metaclass modifies class attributes.

---

## Example: Enforcing Required Methods

Metaclasses can ensure subclasses implement required methods.

```python
class InterfaceMeta(type):
    def __new__(cls, name, bases, namespace):
        if name != "Base":
            if "execute" not in namespace:
                raise TypeError(
                    "Class must implement execute()"
                )

        return super().__new__(
            cls,
            name,
            bases,
            namespace
        )
```

### Usage

```python
class Base(metaclass=InterfaceMeta):
    pass


class Task(Base):
    def execute(self):
        print("Task executed")
```

### Invalid Class

```python
class InvalidTask(Base):
    pass
```

### Error

```python
TypeError: Class must implement execute()
```

---

# Real-World Uses of Metaclasses

Metaclasses are used in many advanced frameworks.

## Common Applications

### ORMs

Frameworks like Django use metaclasses to process model definitions.

#### Example Concept

```python
class User(Model):
    name = CharField()
```

The metaclass collects fields.

---

### API Frameworks

Frameworks like FastAPI and Django REST Framework use metaclasses to inspect classes.

---

### Plugin Systems

Metaclasses can automatically register subclasses.

---

# Example: Automatic Class Registry

```python
class RegistryMeta(type):
    registry = {}

    def __new__(cls, name, bases, namespace):
        new_class = super().__new__(
            cls,
            name,
            bases,
            namespace
        )

        cls.registry[name] = new_class

        return new_class
```

### Usage

```python
class Plugin(metaclass=RegistryMeta):
    pass


class PluginA(Plugin):
    pass


class PluginB(Plugin):
    pass
```

### Check Registry

```python
print(RegistryMeta.registry)
```

### Output

```python
{
    'Plugin': <class '__main__.Plugin'>,
    'PluginA': <class '__main__.PluginA'>,
    'PluginB': <class '__main__.PluginB'>
}
```

---

# When to Use Metaclasses

Metaclasses should be used when you need to:

- Modify class creation behavior
- Enforce class structure rules
- Automatically register classes
- Build frameworks
- Implement DSL-like APIs

However, they should be used carefully because they add complexity.

---

# Summary

Metaclasses provide the mechanism through which Python creates classes. Since classes themselves are objects, they are constructed by a metaclass, with the default metaclass being `type`.

During class creation, Python executes the class body, builds a namespace, and then calls the metaclass to produce the final class object. Custom metaclasses allow developers to intercept and modify this process.

By overriding methods such as `__new__` and `__init__` in a metaclass, developers can:

- Enforce constraints
- Modify class attributes
- Automatically register classes
- Implement advanced framework behavior

Metaclasses are powerful tools used in sophisticated systems such as:

- ORMs
- Web frameworks
- Plugin architectures

While they are not required for most everyday programming tasks, understanding them provides deeper insight into Python's object model and enables advanced metaprogramming techniques.
