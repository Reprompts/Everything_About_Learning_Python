
# Python Programming: Chapter 11 Type System & Conversions

Python's type system determines how data is represented in memory, how operations behave on that data, and how different data types interact with each other. Understanding the type system is essential because Python programs constantly manipulate objects of various types.

This article explains in detail:

* Type checking
* type() function
* isinstance()
* Implicit type conversion
* Explicit type casting
* Duck typing
* Python's strong typing behavior

---

# 1. Understanding the Python Type System

In Python, everything is an object, and every object has a type.

```python
x = 10
y = 3.14
name = "Python"
```

Internally Python assigns types:

* 10 → int
* 3.14 → float
* "Python" → str

The type of an object determines:

* operations allowed on it
* internal memory representation
* how expressions are evaluated

Python uses **dynamic typing**, meaning:

* Types are determined at runtime
* Variables can be reassigned to different types

```python
x = 10
x = "hello"
```

---

# 2. Type Checking

Type checking determines the type of an object during execution.

```python
value = 10
if type(value) == int:
    print("Integer value")
```

However, Python prefers flexible design over strict type checking.

---

# 3. The type() Function

Returns the type of an object.

```python
x = 10
print(type(x))
```

Output:

```
<class 'int'>
```

Examples:

```python
print(type(3.14))
print(type("Python"))
print(type([1,2,3]))
```

Output:

```
<class 'float'>
<class 'str'>
<class 'list'>
```

---

# 4. The isinstance() Function

A safer and more flexible type check.

```python
x = 10
print(isinstance(x, int))
```

Output:

```
True
```

Multiple types:

```python
x = 3.14
print(isinstance(x, (int, float)))
```

Output:

```
True
```

### Why isinstance() is better

It supports inheritance:

```python
class Animal:
    pass

class Dog(Animal):
    pass

d = Dog()
print(isinstance(d, Animal))
```

Output:

```
True
```

But:

```python
type(d) == Animal
```

Output:

```
False
```

---

# 5. Implicit Type Conversion

Python automatically converts types when needed.

```python
a = 5
b = 2.5
result = a + b
print(result)
```

Output:

```
7.5
```

### Common conversions

* int + float → float
* bool + int → int

```python
True + 5
```

Output:

```
6
```

---

# 6. Explicit Type Casting

## 6.1 int()

```python
int("10")
int(3.7)
```

Output:

```
10
3
```

---

## 6.2 float()

```python
float("3.14")
```

Output:

```
3.14
```

---

## 6.3 str()

```python
str(100)
```

Output:

```
"100"
```

---

## 6.4 bool()

```python
bool(0)
bool(5)
```

Output:

```
False
True
```

---

## 6.5 Sequence Conversion

```python
list("abc")
tuple([1,2,3])
set([1,1,2,3])
```

---

# 7. Duck Typing

"If it looks like a duck and quacks like a duck, it is a duck."

```python
class Duck:
    def speak(self):
        print("Quack")

class Person:
    def speak(self):
        print("Hello")

def speak_twice(obj):
    obj.speak()
    obj.speak()

d = Duck()
p = Person()

speak_twice(d)
speak_twice(p)
```

Output:

```
Quack
Quack
Hello
Hello
```

---

# 8. Advantages of Duck Typing

* Flexible code
* Reusable functions
* Easier polymorphism
* Less rigid type constraints

---

# 9. Strong Typing in Python

Python prevents unsafe operations:

```python
"5" + 5
```

Error:

```
TypeError
```

Correct way:

```python
int("5") + 5
```

---

# 10. Strong vs Weak Typing

* Python → strong typing
* JavaScript → weak typing

Example:

```
"5" + 5 → "55"   (JavaScript)
```

Python avoids this ambiguity.

---

# 11. Type Conversion in Expressions

```python
a = 10
b = 3.5
print(a + b)
```

Output:

```
13.5
```

---

# 12. Type Hierarchy in Python

```
bool
  ↓
int
  ↓
float
  ↓
complex
```

Example:

```python
True + 2
```

Output:

```
3
```

---

# 13. Best Practices

* Use `isinstance()` instead of `type()`
* Prefer duck typing for flexibility
* Avoid unnecessary conversions
* Use explicit casting for:

  * user input
  * files
  * APIs

---

# 14. Real-World Example

```python
age = input("Enter age: ")
age = int(age)
print(age + 5)
```

Without conversion:

```
"20" + 5 → error
```

---

# 15. Summary

Python's type system combines:

* Dynamic typing
* Strong typing
* Flexible object behavior

Key ideas:

* Variables are references to objects
* Use `type()` or `isinstance()` for checking
* Python performs implicit conversions
* Explicit casting is done using int(), float(), etc.
* Duck typing focuses on behavior
* Strong typing prevents unsafe operations

---

