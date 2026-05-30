
# Python Programming: Chapter 14 Control Flow

Control flow defines how a program executes instructions — whether it makes decisions, repeats tasks, or skips certain operations.

This guide provides a complete and practical understanding of:
- Conditional statements (if, elif, else, nested conditions)
- Loops (for, while)
- Loop variables
- Loop control (break, continue, pass)
- Loop else
- Iteration utilities (range, enumerate, zip)

---

## 1. Conditional Statements

Conditional statements allow programs to execute code based on conditions (True/False).

### 1.1 The `if` Statement

**Syntax**
```python
if condition:
    statements
````

**Example**

```python
age = 20
if age >= 18:
    print("Adult")
```

**Execution**

* Condition is evaluated
* If True, block executes
* If False, skipped

---

### 1.2 The `if-else` Statement

```python
if condition:
    statements
else:
    statements
```

**Example**

```python
num = 5
if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

---

### 1.3 The `elif` Ladder

```python
if condition1:
    ...
elif condition2:
    ...
elif condition3:
    ...
else:
    ...
```

**Example**

```python
marks = 82

if marks >= 90:
    print("A")
elif marks >= 80:
    print("B")
elif marks >= 70:
    print("C")
else:
    print("D")
```

---

### 1.4 Nested Conditions

```python
age = 25
citizen = True

if age >= 18:
    if citizen:
        print("Eligible to vote")
```

---

## 2. Loops in Python

Loops are used to repeat operations.

Types:

* `for` loop (iteration over sequences)
* `while` loop (condition-based)

---

## 3. The `for` Loop

**Syntax**

```python
for variable in iterable:
    statements
```

**Example**

```python
for i in [1, 2, 3]:
    print(i)
```

---

## 4. The `range()` Function

`range()` generates sequences of numbers.

### 4.1 Syntax

```python
range(start, stop, step)
```

* start → default 0
* stop → required
* step → default 1

---

### 4.2 Examples

```python
for i in range(5):
    print(i)
```

Output:

```
0 1 2 3 4
```

---

### Custom Start and Step

```python
for i in range(2, 10, 2):
    print(i)
```

Output:

```
2 4 6 8
```

---

### Reverse Loop

```python
for i in range(5, 0, -1):
    print(i)
```

Output:

```
5 4 3 2 1
```

---

## 5. Loop Variables

Loop variables store the current value during iteration.

```python
for i in range(3):
    print(i)
```

i changes:

```
0 → 1 → 2
```

---

## 6. The `while` Loop

**Syntax**

```python
while condition:
    statements
```

**Example**

```python
count = 0

while count < 3:
    print(count)
    count += 1
```

---

## 7. The `enumerate()` Function

Adds index to an iterable.

### 7.1 Syntax

```python
enumerate(iterable, start=0)
```

---

### 7.2 Example

```python
names = ["Alice", "Bob", "Charlie"]

for index, name in enumerate(names):
    print(index, name)
```

Output:

```
0 Alice
1 Bob
2 Charlie
```

---

### 7.3 Custom Start Index

```python
for i, name in enumerate(names, start=1):
    print(i, name)
```

---

### 7.4 Practical Use

```python
for i, value in enumerate([10, 20, 30]):
    print(f"Index {i} → Value {value}")
```

---

## 8. The `zip()` Function

Combines multiple iterables.

### 8.1 Syntax

```python
zip(iterable1, iterable2, ...)
```

---

### 8.2 Example

```python
names = ["Alice", "Bob"]
scores = [85, 90]

for name, score in zip(names, scores):
    print(name, score)
```

---

### 8.3 Unequal Length Behavior

```python
zip([1, 2, 3], [10, 20])
```

Stops at shortest iterable.

---

### 8.4 Convert to List

```python
list(zip([1, 2], [3, 4]))
```

Output:

```
[(1, 3), (2, 4)]
```

---

### 8.5 Unzipping

```python
pairs = [(1, 2), (3, 4)]
a, b = zip(*pairs)

print(a)
print(b)
```

Output:

```
(1, 3)
(2, 4)
```

---

## 9. Loop Control Statements

### 9.1 break

Terminates loop.

```python
for i in range(5):
    if i == 3:
        break
    print(i)
```

---

### 9.2 continue

Skips current iteration.

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
```

---

### 9.3 pass

Placeholder.

```python
for i in range(3):
    pass
```

---

## 10. Loop `else` Clause

Runs when loop completes normally.

### Without break

```python
for i in range(3):
    print(i)
else:
    print("Done")
```

### With break

```python
for i in range(5):
    if i == 3:
        break
else:
    print("Done")
```

`else` does not execute.

---

## 11. Combining range, enumerate, zip

### Example 1: Index + Value

```python
numbers = [10, 20, 30]

for i, value in enumerate(numbers):
    print(i, value)
```

---

### Example 2: Parallel Iteration

```python
names = ["A", "B"]
scores = [90, 80]

for i, (name, score) in enumerate(zip(names, scores)):
    print(i, name, score)
```

---

### Example 3: Matrix Traversal

```python
matrix = [[1, 2], [3, 4]]

for i, row in enumerate(matrix):
    for j, val in enumerate(row):
        print(i, j, val)
```

---

## 12. Nested Loops

```python
for i in range(2):
    for j in range(2):
        print(i, j)
```

---

## 13. Real-World Example

Student report system:

```python
names = ["Alice", "Bob"]
marks = [85, 90]

for i, (name, mark) in enumerate(zip(names, marks), start=1):
    print(f"{i}. {name} scored {mark}")
```

---

## 14. Common Mistakes

* Infinite loops:

```python
while True:
    pass
```

* Wrong reverse range:

```python
range(5, 1)  # does nothing
```

* Misusing zip lengths:
  Shorter iterable limits output.

---

## 15. Summary

Control flow structures define the execution path of Python programs.

* Conditional statements → decision-making
* Loops → repetition
* range() → numeric sequences
* enumerate() → index-value pairs
* zip() → combine iterables
* break/continue/pass → loop control
* else in loops → executes on normal completion

Understanding these concepts allows building efficient, readable, and logically structured programs, forming the foundation of real-world software systems.

```

