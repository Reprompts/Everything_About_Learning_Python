
# Python Programming: Chapter 21 Exception Handling

Exception handling is a critical part of writing reliable and robust Python programs. In real-world applications, errors are inevitable: files may be missing, networks may fail, or users may provide invalid input. Instead of allowing programs to crash unexpectedly, Python provides a structured system called exception handling to detect, manage, and recover from errors.

This article explains Python exception handling in depth, covering:

- Exceptions  
- try statement  
- except blocks  
- else clause  
- finally clause  
- Raising exceptions  
- Custom exceptions  
- Exception chaining  

All concepts are explained with detailed practical examples.

---

## 1. What Are Exceptions?

An exception is an error that occurs during the execution of a program and interrupts the normal flow of instructions.

Example:

```python
print(10 / 0)
````

Output:

```
ZeroDivisionError: division by zero
```

Here:

* Python detects an illegal operation
* An exception object is created
* Program execution stops unless handled

---

## 2. Types of Errors in Python

Python errors are broadly divided into two categories:

### Syntax Errors

Errors in the structure of code.

```python
if x == 5
    print(x)
```

Output:

```
SyntaxError
```

These occur before execution begins.

---

### Runtime Errors (Exceptions)

Errors that occur during program execution.

```python
int("abc")
```

Output:

```
ValueError
```

These can be handled using exception handling mechanisms.

---

## 3. Common Built-in Exceptions

* ZeroDivisionError
* TypeError
* ValueError
* IndexError
* KeyError
* FileNotFoundError
* AttributeError
* ImportError
* StopIteration

Example:

```python
numbers = [1, 2, 3]
print(numbers[5])
```

Output:

```
IndexError
```

---

## 4. The try Statement

The `try` block contains code that may raise an exception.

Syntax:

```python
try:
    risky_code
except:
    error_handling
```

Example:

```python
try:
    x = 10 / 0
except:
    print("An error occurred")
```

Output:

```
An error occurred
```

---

## 5. Handling Specific Exceptions

```python
try:
    number = int("abc")
except ValueError:
    print("Invalid number format")
```

---

## 6. Multiple Except Blocks

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except ValueError:
    print("Input must be a number")
except ZeroDivisionError:
    print("Cannot divide by zero")
```

---

## 7. Catching Multiple Exceptions Together

```python
try:
    value = int("abc")
except (ValueError, TypeError):
    print("Conversion failed")
```

---

## 8. Capturing the Exception Object

```python
try:
    x = int("abc")
except ValueError as e:
    print("Error:", e)
```

Output:

```
Error: invalid literal for int()
```

---

## 9. The else Clause

The `else` block runs only if no exception occurs.

```python
try:
    x = int("10")
except ValueError:
    print("Conversion failed")
else:
    print("Conversion successful:", x)
```

---

## 10. The finally Clause

The `finally` block always executes.

```python
try:
    file = open("data.txt")
except FileNotFoundError:
    print("File not found")
finally:
    print("Execution finished")
```

---

### Practical Example: File Handling

```python
try:
    file = open("data.txt")
    content = file.read()
    print(content)
except FileNotFoundError:
    print("File not found")
finally:
    print("Closing program")
```

---

## 11. Raising Exceptions

```python
age = -5
if age < 0:
    raise ValueError("Age cannot be negative")
```

---

## 12. Re-raising Exceptions

```python
try:
    x = 10 / 0
except ZeroDivisionError:
    print("Logging error")
    raise
```

---

## 13. Custom Exceptions

```python
class InvalidAgeError(Exception):
    pass

age = -10
if age < 0:
    raise InvalidAgeError("Age cannot be negative")
```

---

## 14. Custom Exception with Attributes

```python
class BalanceError(Exception):
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__("Insufficient balance")
```

---

## 15. Exception Chaining

```python
try:
    int("abc")
except ValueError as e:
    raise RuntimeError("Conversion failed") from e
```

---

## 16. Practical Example: User Input Validation

```python
def divide():
    try:
        a = int(input("Enter numerator: "))
        b = int(input("Enter denominator: "))
        result = a / b
    except ValueError:
        print("Invalid input")
    except ZeroDivisionError:
        print("Cannot divide by zero")
    else:
        print("Result:", result)
    finally:
        print("Operation completed")

divide()
```

---

## 17. Exception Hierarchy

All exceptions derive from:

```
BaseException
```

Major subclasses:

* Exception

  * ArithmeticError
  * LookupError
  * ValueError
  * TypeError
  * RuntimeError

Example:

```python
except Exception:
    pass
```

---

## 18. Best Practices

* Catch specific exceptions
* Avoid bare `except:`
* Use `finally` for cleanup
* Avoid silent failure (`pass`)
* Use custom exceptions for domain logic

Bad example:

```python
try:
    risky()
except:
    pass
```

---

## 19. Real-World Example: File Processor

```python
def read_file(filename):
    try:
        file = open(filename)
        data = file.read()
    except FileNotFoundError:
        print("File does not exist")
    except PermissionError:
        print("Permission denied")
    else:
        print(data)
    finally:
        print("Read attempt completed")
```

---

## 20. Summary

Exception handling allows programs to manage runtime errors safely.

* **Exceptions** → runtime errors
* **try** → risky code block
* **except** → handles errors
* **else** → runs when no error occurs
* **finally** → always executes
* **raise** → manually trigger errors
* **custom exceptions** → domain-specific errors
* **chaining** → preserves error context

Proper exception handling improves reliability, debugging clarity, and software quality.


