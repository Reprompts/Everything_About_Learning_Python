
# Python Programming: Chapter 15 Comprehensions

Comprehensions are one of Python's most powerful and expressive features. They provide a compact and efficient way to create collections such as lists, dictionaries, sets, and even lazy generators.

Instead of writing long loops to build data structures, comprehensions allow developers to express the same logic in a concise and readable format.

This guide explains comprehensions in depth, covering:

- List comprehensions  
- Dictionary comprehensions  
- Set comprehensions  
- Generator expressions  
- Nested comprehensions  
- Conditional comprehensions  
- Performance considerations  

---

## 1. What Are Comprehensions?

A comprehension is a syntactic construct used to create collections using a single expression combined with iteration and optional filtering.

### Traditional approach:
```python
squares = []
for i in range(5):
    squares.append(i * i)
````

### Using comprehension:

```python
squares = [i * i for i in range(5)]
```

**Output:**

```text
[0, 1, 4, 9, 16]
```

Comprehensions improve:

* readability
* conciseness
* sometimes performance

---

## 2. List Comprehensions

List comprehensions are used to create lists in a compact way.

### 2.1 Syntax

```python
[expression for item in iterable]
```

### 2.2 Basic Example

```python id="4f9k2a"
numbers = [x for x in range(5)]
print(numbers)
```

**Output:**

```text
[0, 1, 2, 3, 4]
```

---

### 2.3 Transformation Example

```python id="x3c8lm"
squares = [x * x for x in range(5)]
```

**Output:**

```text
[0, 1, 4, 9, 16]
```

Equivalent loop:

```python
squares = []
for x in range(5):
    squares.append(x * x)
```

---

### 2.4 Using Existing Lists

```python id="m1v8kp"
numbers = [1, 2, 3, 4]
doubled = [n * 2 for n in numbers]
```

**Output:**

```text
[2, 4, 6, 8]
```

---

### 2.5 String Processing

```python id="t9q2dd"
words = ["python", "java", "go"]
upper = [w.upper() for w in words]
```

**Output:**

```text
['PYTHON', 'JAVA', 'GO']
```

---

### 2.6 Function Calls in Comprehensions

```python
import math
roots = [math.sqrt(x) for x in range(1, 6)]
```

---

## 3. Conditional List Comprehensions

### 3.1 Filtering

```python
numbers = range(10)
evens = [x for x in numbers if x % 2 == 0]
```

**Output:**

```text
[0, 2, 4, 6, 8]
```

---

### 3.2 Conditional Transformation

```python
labels = ["even" if x % 2 == 0 else "odd" for x in range(6)]
```

**Output:**

```text
['even', 'odd', 'even', 'odd', 'even', 'odd']
```

---

## 4. Nested List Comprehensions

### 4.1 Example

```python
pairs = [(x, y) for x in range(3) for y in range(2)]
```

**Output:**

```text
[(0, 0), (0, 1), (1, 0), (1, 1), (2, 0), (2, 1)]
```

Equivalent loop:

```python
pairs = []
for x in range(3):
    for y in range(2):
        pairs.append((x, y))
```

---

### 4.2 Matrix Flattening

```python
matrix = [[1, 2, 3], [4, 5, 6]]
flat = [num for row in matrix for num in row]
```

**Output:**

```text
[1, 2, 3, 4, 5, 6]
```

---

## 5. Dictionary Comprehensions

### 5.1 Syntax

```python
{key_expression: value_expression for item in iterable}
```

---

### 5.2 Basic Example

```python id="k2q9lm"
squares = {x: x * x for x in range(5)}
```

**Output:**

```text
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

---

### 5.3 Using Lists

```python
names = ["Alice", "Bob", "Charlie"]
lengths = {name: len(name) for name in names}
```

**Output:**

```text
{'Alice': 5, 'Bob': 3, 'Charlie': 7}
```

---

### 5.4 Conditional Dictionary Comprehension

```python
numbers = range(10)
even_squares = {x: x * x for x in numbers if x % 2 == 0}
```

---

## 6. Set Comprehensions

### 6.1 Syntax

```python
{expression for item in iterable}
```

---

### 6.2 Example

```python
squares = {x * x for x in range(5)}
```

**Output:**

```text
{0, 1, 4, 9, 16}
```

---

### 6.3 Removing Duplicates

```python
numbers = [1, 2, 2, 3, 3, 4]
unique = {x for x in numbers}
```

**Output:**

```text
{1, 2, 3, 4}
```

---

### 6.4 Conditional Sets

```python
evens = {x for x in range(10) if x % 2 == 0}
```

---

## 7. Generator Expressions

Generator expressions create lazy iterators.

### 7.1 Syntax

```python
(expression for item in iterable)
```

---

### 7.2 Example

```python
gen = (x * x for x in range(5))
```

---

### 7.3 Iteration

```python
for value in gen:
    print(value)
```

**Output:**

```text
0
1
4
9
16
```

---

### 7.4 Memory Efficiency

List comprehension:

```python
[x * x for x in range(1000000)]
```

Generator expression:

```python
(x * x for x in range(1000000))
```

Generators compute values on demand.

---

### 7.5 Using with Functions

```python
sum(x * x for x in range(10))
```

---

## 8. Nested Comprehensions

### Matrix Creation

```python
matrix = [[i * j for j in range(3)] for i in range(3)]
```

**Output:**

```text
[
 [0, 0, 0],
 [0, 1, 2],
 [0, 2, 4]
]
```

---

### Matrix Transpose

```python
matrix = [[1, 2, 3], [4, 5, 6]]
transpose = [[row[i] for row in matrix] for i in range(3)]
```

**Output:**

```text
[
 [1, 4],
 [2, 5],
 [3, 6]
]
```

---

## 9. Conditional Comprehensions

### Filtering

```python
[x for x in range(10) if x % 2 == 0]
```

### Conditional Expression

```python
["even" if x % 2 == 0 else "odd" for x in range(5)]
```

---

## 10. Combining Multiple Conditions

```python
[x for x in range(50) if x % 2 == 0 if x % 5 == 0]
```

**Output:**

```text
[0, 10, 20, 30, 40]
```

Equivalent:

```python
if x % 2 == 0 and x % 5 == 0
```

---

## 11. Performance Considerations

Comprehensions are often faster than loops.

### Loop approach:

```python
result = []
for x in range(1000):
    result.append(x * x)
```

### Comprehension:

```python
result = [x * x for x in range(1000)]
```

---

### 11.1 Generator Performance

Better for large datasets:

```python
sum(x * x for x in range(10000000))
```

---

## 12. When Not to Use Comprehensions

Avoid overly complex expressions:

```python
[x * y for x in range(5) for y in range(5) if x % 2 == 0 if y % 3 == 0]
```

Use loops for better readability.

---

## 13. Real-World Examples

### Filtering Files

```python
files = ["data.csv", "image.png", "report.csv"]
csv_files = [f for f in files if f.endswith(".csv")]
```

---

### Word Length Map

```python
words = ["python", "code", "ai"]
length_map = {w: len(w) for w in words}
```

---

### Unique Characters

```python
text = "programming"
unique = {c for c in text}
```

---

## 14. Summary

Comprehensions provide a powerful way to build collections in Python.

* **List comprehensions** → create lists
* **Dictionary comprehensions** → create dictionaries
* **Set comprehensions** → create sets
* **Generator expressions** → lazy sequences
* **Nested comprehensions** → multi-level iteration
* **Conditional comprehensions** → filtering and transformation

They make code:

* cleaner
* faster
* more expressive

But should be used carefully to maintain readability.


