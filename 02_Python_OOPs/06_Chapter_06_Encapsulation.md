# Python OOPs: Chapter 6 Encapsulation

## Encapsulation in Python

Encapsulation is one of the fundamental principles of Object-Oriented Programming (OOP). It refers to the practice of bundling data and the methods that operate on that data within a single unit, typically a class, while controlling access to that data.

Encapsulation helps enforce data integrity, modularity, and controlled interaction with objects.

In simple terms, encapsulation answers two key questions:

- Where should the data live?
- Who is allowed to modify or access it?

By encapsulating data within classes and controlling access through methods, programmers can prevent unintended interference with an object's internal state.

---

## Concept of Encapsulation

Encapsulation combines data (attributes) and behavior (methods) into a single structure: the class.

This concept ensures that:

- Internal data is protected from accidental modification.
- Interaction with data happens through well-defined methods.
- Implementation details are hidden from the outside world.

Consider a Bank Account system. The account balance should not be modified directly by any part of the program.

Instead, it should only change through specific operations such as:

- deposit
- withdraw

### Example

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
```

In this design, the balance is managed through methods rather than arbitrary modifications.

Encapsulation ensures that object data remains consistent and protected.

---

## Public Members

Public members are attributes or methods that are accessible from anywhere in the program.

In Python, all attributes and methods are public by default.

### Example

```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print("Hello", self.name)
```

### Usage

```python
p = Person("Alice")

print(p.name)
p.greet()
```

### Output

```text
Alice
Hello Alice
```

Both the attribute `name` and method `greet()` are publicly accessible.

Public members are typically used when data or behavior must be accessible to other parts of the system.

---

## Protected Members

Protected members are intended to be used only within the class and its subclasses.

In Python, protected members follow a naming convention using a single underscore prefix.

### Example

```python
_variable
```

### Implementation

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self._salary = salary
```

### Usage

```python
emp = Employee("John", 50000)

print(emp._salary)
```

This is still technically accessible.

However, the underscore signals:

> This variable is meant for internal use and should not be accessed directly.

Protected members are commonly used when designing class hierarchies and inheritance structures.

---

## Private Members

Private members are intended to be accessible only inside the class where they are defined.

Python marks private members using double underscore prefixes.

### Example

```python
__variable
```

### Implementation

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance
```

### Attempting Direct Access

```python
account = BankAccount(1000)

print(account.__balance)
```

### Output

```text
AttributeError: 'BankAccount' object has no attribute '__balance'
```

This prevents accidental access to sensitive data.

---

## Name Mangling

Python does not enforce strict private access like some other languages. Instead, it uses a mechanism called **name mangling**.

Name mangling modifies private attribute names internally to prevent accidental access or name conflicts.

When Python encounters:

```python
__balance
```

It transforms the name internally to:

```python
_ClassName__balance
```

### Example

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance
```

Internally stored as:

```python
_BankAccount__balance
```

### Accessing It Manually

```python
account._BankAccount__balance
```

### Example

```python
account = BankAccount(1000)

print(account._BankAccount__balance)
```

### Output

```text
1000
```

Although possible, this approach is discouraged and should only be used for debugging.

Name mangling primarily exists to avoid accidental attribute overrides in subclasses.

---

## Data Hiding

Data hiding is a core objective of encapsulation.

It refers to the practice of restricting direct access to object data so that it can only be modified through controlled interfaces.

This protects the object's internal state from invalid modifications.

### Example Problem Without Data Hiding

```python
class Account:
    def __init__(self):
        self.balance = 1000
```

Anyone can modify it:

```python
account.balance = -5000
```

This creates an invalid state.

Encapsulation solves this by hiding the data.

### Example

```python
class Account:
    def __init__(self):
        self.__balance = 1000
```

Now `balance` cannot be directly modified.

---

## Getter and Setter Patterns

To safely access and modify private data, classes often provide getter and setter methods.

These methods control how attributes are accessed and changed.

### Getter Methods

A getter method retrieves the value of a private attribute.

#### Example

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance
```

#### Usage

```python
account = BankAccount(1000)

print(account.get_balance())
```

#### Output

```text
1000
```

---

### Setter Methods

A setter method modifies the value of a private attribute with validation.

#### Example

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance

    def set_balance(self, amount):
        if amount >= 0:
            self.__balance = amount
        else:
            print("Invalid balance")
```

#### Usage

```python
account = BankAccount(1000)

account.set_balance(1500)
```

---

## Complete Example

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.__balance = balance

    def get_balance(self):
        return self.__balance

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount

    def withdraw(self, amount):
        if amount <= self.__balance:
            self.__balance -= amount
        else:
            print("Insufficient funds")
```

### Usage

```python
acc = BankAccount("Alice", 1000)

acc.deposit(200)
print(acc.get_balance())

acc.withdraw(500)
print(acc.get_balance())
```

### Output

```text
1200
700
```

---

## Advantages of Getter and Setter Methods

Getter and setter patterns allow developers to:

- Validate input before modifying data
- Prevent invalid object states
- Log changes to sensitive attributes
- Apply business rules during data modification
- Maintain backward compatibility

---

## Encapsulation with Property Decorators (Modern Python)

Modern Python often uses property decorators instead of explicit getter/setter methods.

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
temp = Temperature(25)

print(temp.value)

temp.value = 30
```

This provides clean syntax while maintaining encapsulation.

---

## Practical Real-World Example

Consider a User Authentication System.

```python
class User:
    def __init__(self, username, password):
        self.username = username
        self.__password = password

    def verify_password(self, password):
        return self.__password == password

    def change_password(self, new_password):
        if len(new_password) >= 8:
            self.__password = new_password
        else:
            print("Password too short")
```

### Usage

```python
user = User("admin", "secret123")

print(user.verify_password("secret123"))

user.change_password("newpassword")
```

Encapsulation ensures the password is never accessed directly.

---

## Internal Structure Representation

Conceptually, a class with encapsulation might look like this:

```text
Class: BankAccount

Public:
    owner
    deposit()
    withdraw()

Private:
    __balance
```

External code interacts with public methods only.

---

## Best Practices for Encapsulation

- Keep sensitive attributes private or protected.
- Expose only necessary methods for interaction.
- Use getters and setters to enforce validation.
- Avoid unnecessary exposure of internal implementation details.
- Use property decorators for cleaner interfaces in modern Python.

---

## Summary

Encapsulation is a foundational principle of object-oriented programming that promotes data protection and modular design.

It involves bundling data and behavior within a class and controlling access to internal attributes.

Python supports encapsulation through public, protected, and private members. Public members are accessible everywhere, protected members are intended for internal use within class hierarchies, and private members use name mangling to restrict direct access.

Name mangling transforms private attribute names internally to prevent accidental conflicts and access.

Data hiding ensures that sensitive information remains protected and can only be accessed through controlled interfaces such as getter and setter methods.

Encapsulation leads to more reliable, maintainable, and secure software by enforcing clear boundaries between an object's internal state and external interactions.
