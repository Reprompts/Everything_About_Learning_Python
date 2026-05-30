# Python OOPs: Chapter 7 Properties

In Object-Oriented Programming, it is often necessary to control how attributes are accessed, modified, or deleted. Python provides a powerful mechanism called **properties** that allows developers to manage attribute access while preserving simple attribute-like syntax.

Properties enable a class to:

- Validate data before assigning it
- Compute values dynamically
- Protect internal state
- Maintain backward compatibility
- Encapsulate internal logic

Instead of requiring users to call explicit getter and setter methods like:

```python
obj.get_value()
obj.set_value(x)
```

Python properties allow the same functionality while keeping natural attribute access:

```python
obj.value
obj.value = x
```

Internally, the class controls what happens when these operations occur.

Properties are implemented using:

- `@property` decorator
- Property getter
- Property setter
- Property deleter

---

# Why Properties Exist

Without properties, encapsulation usually requires explicit getter and setter methods.

### Example Without Properties

```python
class Temperature:
    def __init__(self, value):
        self._value = value

    def get_value(self):
        return self._value

    def set_value(self, new_value):
        self._value = new_value
```

### Usage

```python
t = Temperature(25)

print(t.get_value())

t.set_value(30)
```

This approach works but introduces problems:

- More verbose
- Less natural syntax
- Breaks compatibility if attribute access previously existed

Properties solve this by allowing controlled attribute access with simple syntax.

---

# The `@property` Decorator

The `@property` decorator transforms a method into a getter for an attribute.

This allows the method to be accessed like a normal attribute.

### Example

```python
class Temperature:
    def __init__(self, value):
        self._value = value

    @property
    def value(self):
        return self._value
```

### Usage

```python
t = Temperature(25)

print(t.value)
```

### Output

```text
25
```

Even though `value` is implemented as a method, it behaves like an attribute.

Internally Python calls:

```python
Temperature.value(t)
```

but hides the method-call syntax.

---

# Property Getter

A property getter retrieves the value of a managed attribute.

The getter defines how the attribute should be accessed.

### Example

```python
class Rectangle:
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def area(self):
        return self._width * self._height
```

### Usage

```python
r = Rectangle(5, 4)

print(r.area)
```

### Output

```text
20
```

Here `area` is not stored as an attribute. It is computed dynamically.

---

# Property Setter

A property setter allows controlled modification of a property value.

It is defined using the `.setter` decorator attached to the property.

### Example

```python
class Temperature:
    def __init__(self, value):
        self._value = value

    @property
    def value(self):
        return self._value

    @value.setter
    def value(self, new_value):
        if new_value < -273:
            raise ValueError("Temperature below absolute zero")

        self._value = new_value
```

### Usage

```python
t = Temperature(25)

t.value = 30

print(t.value)
```

### Output

```text
30
```

The setter ensures that invalid temperatures cannot be assigned.

---

# Property Deleter

A property deleter defines what happens when an attribute is deleted.

It is implemented using the `.deleter` decorator.

### Example

```python
class User:
    def __init__(self, username):
        self._username = username

    @property
    def username(self):
        return self._username

    @username.deleter
    def username(self):
        print("Deleting username")
        del self._username
```

### Usage

```python
u = User("admin")

del u.username
```

### Output

```text
Deleting username
```

This allows controlled deletion behavior.

---

# Managed Attributes

A managed attribute is an attribute whose access is controlled through property methods.

Instead of directly accessing data, Python automatically routes operations through:

- Getter
- Setter
- Deleter

### Example

```python
class Account:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    @balance.setter
    def balance(self, amount):
        if amount < 0:
            raise ValueError("Negative balance not allowed")

        self._balance = amount
```

### Usage

```python
acc = Account(1000)

acc.balance = 1500

print(acc.balance)
```

### Output

```text
1500
```

The attribute behaves normally, but validation logic runs automatically.

---

# Computed Attributes

A computed attribute is a property whose value is calculated dynamically instead of stored.

These attributes depend on other data.

### Example

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def area(self):
        return 3.14159 * self.radius ** 2
```

### Usage

```python
c = Circle(5)

print(c.area)
```

### Output

```text
78.53975
```

The area is calculated every time it is accessed.

---

# Practical Example: Product Pricing

```python
class Product:
    def __init__(self, price, tax_rate):
        self._price = price
        self.tax_rate = tax_rate

    @property
    def final_price(self):
        return self._price + (self._price * self.tax_rate)
```

### Usage

```python
p = Product(100, 0.18)

print(p.final_price)
```

### Output

```text
118
```

`final_price` is a computed attribute.

---

# Property-Based Data Validation Example

```python
class Person:
    def __init__(self, age):
        self.age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")

        self._age = value
```

### Usage

```python
p = Person(25)

p.age = 30

print(p.age)
```

### Invalid Assignment

```python
p.age = -5
```

### Result

```text
ValueError: Age cannot be negative
```

---

# Internal Mechanism of Properties

When using properties, Python internally converts attribute access.

### Example

```python
obj.value
```

becomes:

```python
obj.__class__.value.__get__(obj)
```

Similarly:

```python
obj.value = x
```

becomes:

```python
obj.__class__.value.__set__(obj, x)
```

This mechanism is part of Python's **descriptor protocol**.

Properties are actually descriptor objects.

---

# Property Object Equivalent

The property decorator is equivalent to creating a `property()` object manually.

### Example

```python
class Example:
    def get_x(self):
        return self._x

    def set_x(self, value):
        self._x = value

    x = property(get_x, set_x)
```

The decorator syntax simply provides a cleaner way.

---

# Advantages of Properties

Properties provide several important benefits.

## Encapsulation

They hide internal implementation details.

## Validation

They ensure only valid data can be assigned.

## Backward Compatibility

Attributes can be converted to properties without changing external code.

## Computed Values

Values can be calculated dynamically.

## Cleaner Syntax

They maintain natural attribute access instead of explicit method calls.

---

# Best Practices for Using Properties

- Use properties when attribute access requires validation or computation.
- Avoid excessive property logic that makes attribute access expensive.
- Use properties to maintain compatibility when changing class internals.
- Prefer properties over explicit getter/setter methods in modern Python code.

---

# Real-World Example: Bank Account System

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    @balance.setter
    def balance(self, amount):
        if amount < 0:
            raise ValueError("Balance cannot be negative")

        self._balance = amount

    @balance.deleter
    def balance(self):
        print("Deleting account balance")
        del self._balance
```

### Usage

```python
acc = BankAccount(1000)

print(acc.balance)

acc.balance = 1500

del acc.balance
```

---

# Conceptual Flow

```text
User accesses attribute
        │
        ▼
Property intercepts access
        │
        ▼
Getter / Setter / Deleter executes
        │
        ▼
Value returned or updated
```

---

# Summary

Properties in Python provide a powerful mechanism for controlling attribute access while maintaining simple and natural syntax.

The `@property` decorator converts methods into managed attributes, enabling developers to define getters that behave like regular attributes.

Property setters allow controlled modification of attributes, enabling validation and enforcement of business rules. Property deleters define custom deletion behavior.

Managed attributes use properties to control access to internal state, ensuring data integrity and encapsulation.

Computed attributes dynamically calculate values based on other attributes, eliminating the need to store redundant data.

Properties are widely used in modern Python applications because they allow classes to expose clean interfaces while retaining full control over internal behavior and data management.
