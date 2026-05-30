
# Python Programming: Chapter 13 Expressions & Evaluations

Expressions are at the heart of every Python program. They define how values are computed, how conditions are evaluated, and how decisions are made.

This guide explains:
- Expression types
- Short-circuit evaluation
- Truth value testing
- Boolean context
- Chained comparisons

with deep practical understanding and examples.

---

## 1. What is an Expression?

An expression is any piece of code that produces a value.

### Examples:
```python
5 + 3
x * 2
len("hello")
a > b
````

Each produces a result:

* `5 + 3 → 8`
* `a > b → True or False`

---

### 1.1 Expression vs Statement

* **Expression → returns a value**
* **Statement → performs an action**

```python
x = 5      # statement
x + 2      # expression
```

---

## 2. Types of Expressions

Python supports multiple types of expressions.

---

### 2.1 Arithmetic Expressions

Perform mathematical operations.

```python
a = 10
b = 5
result = a + b * 2
```

Evaluation:

* `b * 2 → 10`
* `a + 10 → 20`

---

### 2.2 Relational (Comparison) Expressions

Compare values and return boolean.

```python
x = 10
print(x > 5)
```

Output:

```
True
```

---

### 2.3 Logical Expressions

Combine multiple conditions.

```python
x = 10
print((x > 5) and (x < 20))
```

Output:

```
True
```

---

### 2.4 Bitwise Expressions

Operate at binary level.

```python
print(5 & 3)
```

Output:

```
1
```

---

### 2.5 Assignment Expressions (Walrus Operator)

Assignment inside expressions using `:=`.

```python
if (n := len("hello")) > 3:
    print(n)
```

Output:

```
5
```

---

### 2.6 Conditional Expressions (Ternary Operator)

Compact if-else.

```python
x = 10
result = "Even" if x % 2 == 0 else "Odd"
```

---

### 2.7 Lambda Expressions

Anonymous functions.

```python
square = lambda x: x * x
print(square(5))
```

Output:

```
25
```

---

### 2.8 Membership Expressions

```python
print(3 in [1, 2, 3])
```

Output:

```
True
```

---

### 2.9 Identity Expressions

Checks if both refer to same object.

```python
a is b
```

---

## 3. Expression Evaluation

Python evaluates expressions based on:

* operator precedence
* associativity
* parentheses

```python
2 + 3 * 4
```

Evaluation:

* `3 * 4 → 12`
* `2 + 12 → 14`

---

## 4. Short-Circuit Evaluation

Python stops evaluating as soon as result is determined.

---

### 4.1 Short-Circuit in `and`

Rule:

* If A is False → return A
* If A is True → evaluate B

```python
x = 0
print(x and 10)
```

Output:

```
0
```

---

#### Practical Example

```python
def check():
    print("Checked")
    return True

False and check()
```

No output (function not called)

---

### 4.2 Short-Circuit in `or`

Rule:

* If A is True → return A
* If A is False → evaluate B

```python
print(10 or 20)
```

Output:

```
10
```

---

#### Practical Example

```python
True or check()
```

`check()` is never called.

---

### 4.3 Practical Use Case

Default value handling:

```python
name = input("Enter name: ") or "Guest"
```

If input is empty → `"Guest"`

---

## 5. Truth Value Testing

Python evaluates values as True or False in conditions.

---

### 5.1 Truthy Values

* non-zero numbers
* non-empty sequences
* objects

```python
bool(10)      # True
bool("hello") # True
bool([1,2])   # True
```

---

### 5.2 Falsy Values

* False
* None
* 0, 0.0
* "", [], {}, set()

```python
bool(0)   # False
bool("")  # False
```

---

## 6. Boolean Context

A boolean context is where Python expects True/False.

---

### 6.1 Examples

#### if statement

```python
if "hello":
    print("True")
```

#### while loop

```python
while 0:
    print("Never runs")
```

#### logical operations

```python
if [] or [1]:
    print("Executed")
```

---

### 6.2 Custom Objects

```python
class Test:
    def __bool__(self):
        return False

t = Test()

if t:
    print("True")
else:
    print("False")
```

Output:

```
False
```

---

## 7. Chained Comparisons

---

### 7.1 Syntax

```python
a < b < c
```

Equivalent:

```python
(a < b) and (b < c)
```

---

### 7.2 Example

```python
x = 10
print(5 < x < 20)
```

Output:

```
True
```

---

### 7.3 Evaluation Behavior

Python evaluates middle value only once.

---

### 7.4 Practical Example

```python
age = 25

if 18 <= age <= 60:
    print("Working age")
```

---

### 7.5 Mixed Comparisons

```python
print(1 < 2 == 2)
```

Equivalent:

```python
(1 < 2) and (2 == 2)
```

Output:

```
True
```

---

## 8. Combined Example

```python
x = 10

result = (x > 5) and (x < 20) or (x == 100)
print(result)
```

Step-by-step:

* `x > 5 → True`
* `x < 20 → True`
* `True and True → True`
* `True or False → True`

---

## 9. Common Pitfalls

### 9.1 `and` vs `&`

```python
True and False  # False
True & False    # False
```

But behavior differs for non-boolean values.

---

### 9.2 Truthy Values

```python
if [0]:
    print("True")
```

Output:

```
True
```

---

### 9.3 Incorrect Chaining

```python
x = 5
print(x == 5 == True)
```

Output:

```
False
```

Because:

```
(5 == 5) and (5 == True)
```

---

## 10. Real-World Example

```python
username = input("Enter username: ")

if username and len(username) >= 3:
    print("Valid username")
```

* empty string → False
* ensures minimum length

---

## 11. Performance Insight

Short-circuit evaluation improves performance:

```python
expensive_function() and condition
```

If first is False → second is skipped.

---

## 12. Summary

Expressions form the core execution model of Python.

### Key concepts:

* Expression types → arithmetic, logical, relational, lambda, conditional
* Short-circuit evaluation → stops early in `and` / `or`
* Truth value testing → determines boolean behavior
* Boolean context → where expressions act as conditions
* Chained comparisons → clean multi-condition checks

Mastering these concepts enables writing efficient, readable, and logically correct Python programs.

```
