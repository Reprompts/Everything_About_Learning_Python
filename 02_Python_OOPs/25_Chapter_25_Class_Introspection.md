# Python OOP: Chapter 25 Class Introspection

Class introspection is the ability of a program to examine the structure, type, attributes, and relationships of objects and classes at runtime. Python strongly supports introspection because it is a dynamic language where objects and classes are first-class objects.

Through introspection, programs can:

* Discover available attributes and methods
* Determine an object's type
* Check relationships between classes
* Analyze class hierarchies
* Dynamically adapt behavior

Introspection is widely used in:

* Frameworks (Django, Flask)
* ORMs
* Debugging tools
* Dynamic libraries
* Plugin systems
* IDEs and documentation tools

This article explores in depth:

* Inspecting classes and objects
* `dir()`
* `type()`
* `isinstance()`
* `issubclass()`

with practical explanations.

---

# 1. Class Introspection Concept

## Definition

Introspection is the process of examining the internal structure of an object or class at runtime.

This includes discovering:

* Attributes
* Methods
* Type information
* Inheritance hierarchy
* Module origin
* Metadata

### Example

```python
class User:
    def login(self):
        pass

u = User()
```

Using introspection, Python can answer questions like:

* What attributes does `u` have?
* What class does `u` belong to?
* Is `u` an instance of `User`?
* What methods exist in the class?
* What parent classes exist?

These capabilities allow programs to inspect themselves while running.

---

# 2. Inspecting Classes and Objects

Before exploring specific functions, it is important to understand what information can be inspected.

Python objects contain several metadata attributes.

### Example

```python
class Product:
    pass

p = Product()
```

### Important Attributes

| Attribute    | Purpose                       |
| ------------ | ----------------------------- |
| `__class__`  | Class of object               |
| `__dict__`   | Attributes of object          |
| `__module__` | Module where class is defined |
| `__bases__`  | Parent classes                |
| `__mro__`    | Method Resolution Order       |

### Example

```python
print(p.__class__)
```

**Output**

```python
<class '__main__.Product'>
```

---

## Inspecting Instance Attributes

### Example

```python
class User:
    def __init__(self):
        self.name = "Alice"
        self.age = 25

u = User()

print(u.__dict__)
```

**Output**

```python
{'name': 'Alice', 'age': 25}
```

This shows the instance attribute dictionary.

---

## Inspecting Class Attributes

### Example

```python
class Car:
    wheels = 4

    def drive(self):
        pass

print(Car.__dict__)
```

Output includes:

```python
{
    'wheels': 4,
    'drive': <function>,
    '__init__': ...
}
```

This reveals the internal structure of the class.

---

# 3. The `dir()` Function

## Concept

`dir()` returns a list of attributes and methods available on an object or class.

### Syntax

```python
dir(object)
```

This includes:

* Instance attributes
* Class attributes
* Methods
* Special methods
* Inherited attributes

---

## Example

```python
class User:
    def login(self):
        pass

u = User()

print(dir(u))
```

Example output:

```python
[
    '__class__',
    '__delattr__',
    '__dict__',
    '__dir__',
    '__doc__',
    '__eq__',
    '__format__',
    '__getattribute__',
    'login'
]
```

---

## Understanding the Output

The list contains:

### Special Methods

* `__init__`
* `__str__`
* `__repr__`
* `__eq__`

These are built-in object methods.

### User-Defined Methods

* `login`

### Object Metadata

* `__class__`
* `__dict__`

---

## `dir()` Without Arguments

When called without parameters:

```python
dir()
```

It returns names in the current scope.

### Example

```python
a = 10
b = 20

print(dir())
```

**Output**

```python
['a', 'b']
```

---

## Practical Use Case

Developers often use `dir()` to discover available methods.

### Example

```python
dir(list)
```

Shows methods such as:

* `append`
* `extend`
* `insert`
* `remove`
* `sort`

This is helpful when learning new libraries.

---

# 4. The `type()` Function

## Concept

`type()` returns the type (class) of an object.

### Syntax

```python
type(object)
```

### Example

```python
x = 10

print(type(x))
```

**Output**

```python
<class 'int'>
```

---

## Example with Different Objects

```python
print(type("hello"))
print(type([1, 2, 3]))
print(type({"a": 1}))
```

**Output**

```python
<class 'str'>
<class 'list'>
<class 'dict'>
```

---

## Type of Custom Objects

### Example

```python
class Animal:
    pass

a = Animal()

print(type(a))
```

**Output**

```python
<class '__main__.Animal'>
```

This confirms that `a` belongs to class `Animal`.

---

## Using `type()` for Comparisons

### Example

```python
if type(x) == int:
    print("Integer")
```

However, this approach has limitations.

### Why?

Because it ignores inheritance.

---

# 5. `isinstance()` Function

## Concept

`isinstance()` checks whether an object is an instance of a class or its subclasses.

### Syntax

```python
isinstance(object, class)
```

---

## Example

```python
x = 10

print(isinstance(x, int))
```

**Output**

```python
True
```

---

## Multiple Type Checking

`isinstance()` can check multiple types.

### Example

```python
x = 10

print(isinstance(x, (int, float)))
```

**Output**

```python
True
```

This checks whether `x` is either an integer or a float.

---

## Why `isinstance()` is Better Than `type()`

Consider inheritance.

### Example

```python
class Animal:
    pass


class Dog(Animal):
    pass


d = Dog()
```

Using `type()`:

```python
type(d) == Animal
```

**Output**

```python
False
```

Using `isinstance()`:

```python
isinstance(d, Animal)
```

**Output**

```python
True
```

Because `Dog` is a subclass of `Animal`.

### Key Difference

`isinstance()` respects inheritance relationships.

---

## Practical Example

```python
def process_number(x):
    if isinstance(x, (int, float)):
        print("Valid number")
    else:
        print("Invalid input")
```

This is a common validation pattern.

---

# 6. `issubclass()` Function

## Concept

`issubclass()` checks whether a class inherits from another class.

### Syntax

```python
issubclass(class_name, parent_class)
```

---

## Example

```python
class Animal:
    pass


class Dog(Animal):
    pass


print(issubclass(Dog, Animal))
```

**Output**

```python
True
```

Because:

```text
Dog → Animal
```

---

## Multiple Parent Check

### Example

```python
issubclass(Dog, (Animal, object))
```

**Output**

```python
True
```

---

## Checking Built-in Hierarchies

### Example

```python
print(issubclass(bool, int))
```

**Output**

```python
True
```

Because internally:

```text
object
  │
 number
  │
  int
  │
 bool
```

---

# 7. Combining Introspection Tools

These tools often work together.

### Example

```python
class Vehicle:
    pass


class Car(Vehicle):
    pass


c = Car()

print(type(c))
print(isinstance(c, Vehicle))
print(issubclass(Car, Vehicle))
print(dir(c))
```

Output reveals:

* Object type
* Instance relationship
* Class inheritance
* Available attributes and methods

---

# 8. Introspection in Real Frameworks

Introspection powers many advanced systems.

---

## Django ORM

Django inspects model classes to discover fields.

### Example

```python
class User(models.Model):
    name = models.CharField()
    age = models.IntegerField()
```

Django internally scans attributes using introspection.

---

## Flask Routing

Flask inspects function signatures dynamically.

### Example

```python
@app.route("/user/<id>")
def get_user(id):
    ...
```

The framework examines parameters automatically.

---

## Testing Frameworks

Tools like `pytest` inspect functions to detect tests automatically.

### Example

```python
def test_login():
    ...
```

The test runner scans modules using introspection.

---

# 9. Debugging and Development

Introspection is extremely useful for debugging.

### Example

```python
print(dir(object))
```

Shows all available methods on the base object class.

---

## Dynamic Attribute Checking

```python
if hasattr(obj, "method"):
    obj.method()
```

This enables safe dynamic execution.

---

# 10. Relationship Between These Functions

| Function       | Purpose                                    |
| -------------- | ------------------------------------------ |
| `dir()`        | List attributes and methods                |
| `type()`       | Determine object's exact class             |
| `isinstance()` | Check object type with inheritance support |
| `issubclass()` | Check class inheritance                    |

Together they allow deep runtime inspection of objects and classes.

---

# 11. Introspection vs Reflection

In many languages:

* **Introspection** → Examine structure
* **Reflection** → Modify structure

Python supports both.

### Examples of Reflection

```python
getattr()
setattr()
delattr()
```

However, introspection primarily focuses on observing object structure.

---

# Final Summary

Class introspection enables Python programs to analyze objects and classes during execution.

## Core Introspection Tools

| Tool           | Purpose                 |
| -------------- | ----------------------- |
| `dir()`        | List attributes         |
| `type()`       | Identify object's class |
| `isinstance()` | Check object type       |
| `issubclass()` | Check inheritance       |

These tools allow Python programs to:

* Discover object capabilities
* Adapt behavior dynamically
* Build flexible frameworks
* Perform runtime analysis

They are fundamental to how modern Python frameworks, libraries, IDEs, testing tools, and debugging systems operate.
