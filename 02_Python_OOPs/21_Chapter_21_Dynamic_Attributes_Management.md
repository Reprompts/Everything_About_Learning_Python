# Python OOPs: Chapter 21 Dynamic Attribute Management

Dynamic Attribute Management is one of the most powerful and flexible aspects of Python's object model. Python objects are not rigid containers with fixed attributes like many statically typed languages. Instead, attributes can be created, modified, intercepted, redirected, or delegated dynamically at runtime.

This capability enables frameworks, ORMs, proxies, lazy loaders, dynamic APIs, and many advanced programming patterns.

Below is a deep and practical explanation of the key aspects of Dynamic Attribute Management.

---

# Dynamic Attribute Management in Python

Dynamic attribute management refers to the ability of Python objects to:

- Create attributes at runtime
- Attach methods dynamically
- Intercept attribute access
- Redirect attribute lookup to other objects

This flexibility is possible because Python objects internally store attributes in dictionaries (`__dict__`) and allow overriding attribute access mechanisms.

---

# 1. Dynamic Attribute Creation

## Concept

Dynamic attribute creation means attributes can be added to objects while the program is running, even if they were not defined in the class.

Unlike languages like Java or C++, Python objects are open for modification.

### Example

```python
class User:
    pass

u = User()

u.name = "Alice"
u.age = 25

print(u.name)
print(u.age)
```

**Output**

```python
Alice
25
```

Here:

```text
User class -> initially empty
Instance u -> attributes added dynamically
```

Internally:

```python
u.__dict__ = {
    "name": "Alice",
    "age": 25
}
```

---

## Real Internal Behavior

Every Python object maintains an attribute dictionary.

```text
object
  └── __dict__
        ├── name
        ├── age
        └── other attributes
```

You can inspect it directly.

```python
print(u.__dict__)
```

**Output**

```python
{'name': 'Alice', 'age': 25}
```

---

## Using `setattr()`

Python provides a built-in function for dynamic attribute creation.

```python
setattr(object, attribute_name, value)
```

### Example

```python
class Product:
    pass

p = Product()

setattr(p, "price", 500)
setattr(p, "stock", 20)

print(p.price)
print(p.stock)
```

---

## Dynamic Attributes from Data

Dynamic attribute creation is very useful when converting JSON or database records into objects.

### Example

```python
class User:
    pass

data = {
    "name": "John",
    "email": "john@email.com",
    "age": 30
}

u = User()

for key, value in data.items():
    setattr(u, key, value)

print(u.name)
print(u.email)
print(u.age)
```

This is exactly how many frameworks convert API responses into objects.

---

## Real-World Use Case: ORM

ORM libraries like SQLAlchemy or Django ORM dynamically create attributes based on database fields.

### Example Concept

```text
table: users

columns:
id
name
email
```

ORM converts a row into an object:

```python
User(
    id=1,
    name="Alice",
    email="alice@email.com"
)
```

Attributes correspond directly to database columns.

---

# 2. Dynamic Method Binding

Methods can also be attached to objects dynamically.

In Python, functions are first-class objects, so they can be assigned as attributes.

## Example

```python
class Robot:
    pass

r = Robot()

def speak(self):
    print("Hello, I am a robot")

r.speak = speak

r.speak(r)
```

**Output**

```python
Hello, I am a robot
```

Here:

```text
function -> attached as attribute
```

But this is not yet a proper bound method.

---

## Proper Method Binding with `types.MethodType`

Python provides a proper way to bind methods.

```python
import types

class Robot:
    pass

r = Robot()

def speak(self):
    print("Hello, I am", self.name)

r.name = "R2D2"

r.speak = types.MethodType(speak, r)

r.speak()
```

**Output**

```python
Hello, I am R2D2
```

Now `self` is automatically bound.

---

## Dynamic Class Method Injection

You can also attach methods to the class itself.

```python
class Car:
    pass

def drive(self):
    print("Driving", self.model)

Car.drive = drive

c = Car()
c.model = "Tesla"

c.drive()
```

**Output**

```python
Driving Tesla
```

Now all objects have the method.

---

## Real-World Use Cases

Dynamic method binding is used in:

### Plugin Systems

Modules dynamically extend existing classes.

```text
core system
   |
plugins attach new behavior
```

### Monkey Patching

Modifying behavior of existing libraries.

Examples:

- Framework patches
- Runtime bug fixes
- Test mocks

```python
import math

math.square = lambda x: x * x

print(math.square(5))
```

---

# 3. Attribute Interception

Sometimes we want to intercept attribute access to control behavior.

Python allows this through special methods:

- `__getattr__`
- `__getattribute__`
- `__setattr__`
- `__delattr__`

These methods allow custom attribute management logic.

---

## `__getattr__()`

Called when an attribute is not found normally.

### Example

```python
class DynamicObject:
    def __getattr__(self, name):
        return f"{name} attribute does not exist"

obj = DynamicObject()

print(obj.test)
print(obj.value)
```

**Output**

```python
test attribute does not exist
value attribute does not exist
```

Useful for:

- Lazy loading
- Virtual attributes
- Dynamic APIs

---

## `__getattribute__()`

Intercepts every attribute access.

### Example

```python
class Logger:
    def __getattribute__(self, name):
        print("Accessing:", name)
        return object.__getattribute__(self, name)

    def hello(self):
        print("Hello")

obj = Logger()

obj.hello()
```

**Output**

```python
Accessing: hello
Hello
```

### Important

Always call:

```python
object.__getattribute__()
```

Otherwise infinite recursion occurs.

---

## `__setattr__()`

Intercepts attribute assignment.

### Example

```python
class Protected:
    def __setattr__(self, name, value):
        if name == "password":
            raise ValueError("Cannot set password directly")

        super().__setattr__(name, value)

obj = Protected()

obj.name = "Admin"
```

This allows:

- Validation
- Data protection
- Logging

---

## `__delattr__()`

Intercepts attribute deletion.

### Example

```python
class Secure:
    def __delattr__(self, name):
        print("Deleting:", name)
        super().__delattr__(name)

obj = Secure()

obj.x = 10

del obj.x
```

---

# 4. Attribute Delegation

Attribute delegation means redirecting attribute access to another object.

This pattern is used heavily in:

- Wrappers
- Proxies
- Adapters
- Decorators

---

## Basic Delegation Example

```python
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self):
        self.engine = Engine()

    def __getattr__(self, name):
        return getattr(self.engine, name)

c = Car()

c.start()
```

**Output**

```python
Engine started
```

Here:

```text
Car delegates attribute lookup to Engine
```

---

## Delegation Flow

```text
c.start()

Python search order:

1. class Car
2. instance attributes
3. __getattr__()

delegates to:

engine.start()
```

---

## Proxy Pattern Example

A proxy object controls access to another object.

```python
class Database:
    def query(self):
        print("Running query")

class DBProxy:
    def __init__(self):
        self.db = Database()

    def __getattr__(self, name):
        print("Logging access")
        return getattr(self.db, name)

proxy = DBProxy()

proxy.query()
```

**Output**

```python
Logging access
Running query
```

This enables:

- Logging
- Security
- Caching
- Lazy loading

---

# Delegation in Large Frameworks

Attribute delegation is used heavily in:

## Django ORM

```python
model.user.profile.address
```

Objects delegate attribute access internally.

---

## SQLAlchemy

Database fields are descriptors that dynamically manage attribute access.

---

## Flask Request Objects

Attributes like:

```python
request.args
request.json
request.headers
```

are dynamically delegated properties.

---

# Combined Example (Dynamic System)

Here is a complete dynamic attribute system.

```python
class DynamicContainer:
    def __init__(self):
        self._data = {}

    def __setattr__(self, name, value):
        if name == "_data":
            super().__setattr__(name, value)
        else:
            print("Setting:", name)
            self._data[name] = value

    def __getattr__(self, name):
        print("Getting:", name)
        return self._data.get(name, None)

obj = DynamicContainer()

obj.name = "Alice"
obj.age = 30

print(obj.name)
print(obj.age)
```

**Output**

```python
Setting: name
Setting: age

Getting: name
Alice

Getting: age
30
```

This pattern is used in:

- ORM frameworks
- Configuration systems
- Dynamic API clients

---

# Performance Considerations

Dynamic attribute handling is powerful but has costs.

### Potential Issues

- Slower attribute lookup
- Harder debugging
- Unexpected behavior

### Best Practice

Use dynamic techniques when:

- Building frameworks
- Implementing proxies
- Building DSLs
- Dynamic configuration systems

Avoid excessive use in simple business logic.

---

# Summary

Dynamic Attribute Management enables extremely flexible object behavior in Python.

| Concept | Purpose |
|----------|----------|
| Dynamic attribute creation | Add attributes at runtime |
| Dynamic method binding | Attach methods dynamically |
| Attribute interception | Control attribute access |
| Attribute delegation | Forward attribute requests |

These mechanisms power many advanced systems including:

- Django ORM
- SQLAlchemy
- Flask
- Proxy objects
- Lazy loaders
- Plugin systems
- Dynamic APIs
