# Python OOPs: Chapter 20 Class Decorators

Class decorators are a powerful metaprogramming feature in Python that allow developers to modify, enhance, or transform classes dynamically at the time they are defined. They work similarly to function decorators but operate on class objects instead of functions.

Class decorators are often used to:

- Modify class attributes
- Inject methods
- Register classes automatically
- Enforce rules on class definitions
- Implement reusable behaviors

Understanding class decorators requires familiarity with how Python treats classes as objects and how decorators intercept object creation.

This article explores class decorators in depth, including:

- Decorating classes
- Modifying class behavior
- Enhancing classes dynamically
- Practical patterns and real-world use cases

---

# Understanding Decorators

Before focusing on class decorators, it is important to understand how decorators work in general.

A decorator is simply a function that receives another object and returns a modified object.

Basic concept:

```text
decorator(original_object) → modified_object
```

In Python syntax:

```python
@decorator
def function():
    pass
```

is equivalent to:

```python
function = decorator(function)
```

The same concept applies to classes.

---

# What is a Class Decorator?

A class decorator is a function that receives a class as an argument and returns either:

- The same class (possibly modified)
- A completely new class

### Syntax

```python
@decorator
class MyClass:
    pass
```

This is equivalent to:

```python
class MyClass:
    pass

MyClass = decorator(MyClass)
```

The decorator function is executed immediately after the class is created.

---

# Basic Example of a Class Decorator

Example decorator that prints information about the class.

```python
def log_class(cls):
    print("Creating class:", cls.__name__)
    return cls

@log_class
class Person:
    pass
```

### Output

```python
Creating class: Person
```

### Execution Steps

1. Python creates the class `Person`
2. The decorator `log_class` is called with `Person`
3. The returned class replaces the original class

---

# Decorating Classes

Decorating a class means intercepting the class object after it is created and modifying it.

## Example: Adding Attributes Dynamically

```python
def add_version(cls):
    cls.version = "1.0"
    return cls

@add_version
class Application:
    pass
```

### Test

```python
print(Application.version)
```

### Output

```python
1.0
```

The decorator injected a new class attribute.

---

# Adding Methods Using Class Decorators

Decorators can dynamically attach methods to classes.

### Example

```python
def add_greet_method(cls):
    def greet(self):
        print("Hello from", self.__class__.__name__)

    cls.greet = greet
    return cls

@add_greet_method
class User:
    pass
```

### Usage

```python
u = User()
u.greet()
```

### Output

```python
Hello from User
```

The decorator dynamically added a method to the class.

---

# Modifying Class Behavior

Class decorators can change how class instances behave by modifying attributes or methods.

## Example: Modifying Method Behavior

```python
def uppercase_methods(cls):
    for name, value in cls.__dict__.items():
        if callable(value):
            def wrapper(self, *args, func=value, **kwargs):
                result = func(self, *args, **kwargs)

                if isinstance(result, str):
                    return result.upper()

                return result

            setattr(cls, name, wrapper)

    return cls
```

### Applying Decorator

```python
@uppercase_methods
class Message:
    def text(self):
        return "hello world"
```

### Usage

```python
m = Message()
print(m.text())
```

### Output

```python
HELLO WORLD
```

The decorator modified all methods returning strings.

---

# Adding Validation to Classes

Class decorators can enforce validation rules.

## Example: Requiring Certain Attributes

```python
def require_name(cls):
    if "name" not in cls.__dict__:
        raise TypeError("Class must define 'name' attribute")

    return cls
```

### Valid Usage

```python
@require_name
class Product:
    name = "Laptop"
```

### Invalid Usage

```python
@require_name
class InvalidProduct:
    pass
```

### Error

```python
TypeError: Class must define 'name' attribute
```

Decorators can enforce structural rules.

---

# Enhancing Classes Dynamically

Class decorators allow developers to dynamically enhance class functionality without modifying the original code.

## Example: Automatic Method Logging

```python
def log_methods(cls):
    for name, method in cls.__dict__.items():
        if callable(method):
            def wrapper(self, *args, func=method, **kwargs):
                print("Calling:", func.__name__)
                return func(self, *args, **kwargs)

            setattr(cls, name, wrapper)

    return cls
```

### Using the Decorator

```python
@log_methods
class Calculator:
    def add(self, a, b):
        return a + b
```

### Usage

```python
c = Calculator()

print(c.add(3, 4))
```

### Output

```python
Calling: add
7
```

The decorator adds logging behavior.

---

# Class Decorator vs Metaclass

Both class decorators and metaclasses modify classes, but they operate at different stages.

| Feature | Class Decorator | Metaclass |
|----------|----------------|------------|
| Complexity | Simpler | More advanced |
| Timing | After class creation | During class creation |
| Usage | Modifying existing class | Controlling class creation |
| Syntax | `@decorator` | `metaclass=` |

### Example Comparison

#### Class Decorator

```text
class → decorator → modified class
```

#### Metaclass

```text
class definition → metaclass → class creation
```

Most use cases that require modifying classes can be handled with decorators instead of metaclasses.

---

# Chaining Multiple Class Decorators

Multiple decorators can be applied to a class.

### Example

```python
def decorator_a(cls):
    print("Decorator A")
    return cls

def decorator_b(cls):
    print("Decorator B")
    return cls

@decorator_a
@decorator_b
class Example:
    pass
```

### Execution Order

```text
decorator_b
decorator_a
```

Decorators are applied from bottom to top.

Equivalent to:

```python
Example = decorator_a(decorator_b(Example))
```

---

# Parameterized Class Decorators

Decorators can accept parameters.

### Example

```python
def add_attribute(attr_name, value):
    def decorator(cls):
        setattr(cls, attr_name, value)
        return cls

    return decorator
```

### Usage

```python
@add_attribute("version", "2.0")
class Library:
    pass
```

### Test

```python
print(Library.version)
```

### Output

```python
2.0
```

This pattern allows configurable decorators.

---

# Automatically Registering Classes

Class decorators are often used for plugin systems.

## Example: Class Registry

```python
registry = {}

def register(cls):
    registry[cls.__name__] = cls
    return cls
```

### Usage

```python
@register
class PluginA:
    pass

@register
class PluginB:
    pass
```

### Registry Contents

```python
print(registry)
```

### Output

```python
{
    'PluginA': <class '__main__.PluginA'>,
    'PluginB': <class '__main__.PluginB'>
}
```

This pattern is used in many frameworks.

---

# Wrapping Classes with Another Class

A decorator can also replace the class with another class.

### Example: Singleton Decorator

```python
def singleton(cls):
    instance = None

    def get_instance(*args, **kwargs):
        nonlocal instance

        if instance is None:
            instance = cls(*args, **kwargs)

        return instance

    return get_instance
```

### Usage

```python
@singleton
class Database:
    pass
```

Now:

```python
a = Database()
b = Database()

print(a is b)
```

### Output

```python
True
```

The decorator converted the class into a singleton factory.

---

# Real-World Uses of Class Decorators

Class decorators are widely used in modern frameworks.

## Common Applications

### Data Validation Frameworks

Libraries like `dataclasses` and `Pydantic` use decorators to process class fields.

Example:

```python
@dataclass
class User:
    name: str
    age: int
```

The decorator automatically generates:

- `__init__`
- `__repr__`
- `__eq__`

---

### Web Frameworks

Frameworks often use decorators to register controllers or routes.

Example concept:

```python
@controller
class UserAPI:
    pass
```

---

### ORM Systems

Decorators may register database models.

---

### Plugin Systems

Decorators automatically track implementations.

---

# Best Practices for Class Decorators

When using class decorators:

## Prefer Decorators When

- Modifying classes after creation
- Injecting reusable behavior
- Building simple class frameworks

## Avoid Decorators When

- Deep control over class creation is needed
- Inheritance behavior must be modified

In such cases, metaclasses may be more appropriate.

---

# Summary

Class decorators provide a mechanism to intercept and modify classes after they are defined. Because classes are objects in Python, decorators can receive the class object, modify its attributes or methods, and return the modified class.

Class decorators can:

- Inject new functionality
- Enforce structural constraints
- Register classes in systems
- Dynamically enhance behavior

They are simpler than metaclasses and are widely used in real-world frameworks for tasks such as:

- Automatic registration
- Configuration
- Validation
- Code generation

By understanding class decorators, developers gain access to a powerful metaprogramming technique that enables flexible, reusable, and dynamic class design.
