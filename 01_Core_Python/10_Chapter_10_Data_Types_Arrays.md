# Python Programming: Chapter 10 Python Data Types: Python Arrays

Arrays are one of the most fundamental data structures used in programming for storing collections of elements in contiguous memory. In Python, arrays exist in multiple forms depending on the level of abstraction and performance requirements.

Python provides two major types of arrays:

- Built-in Arrays using the `array` module
- NumPy Arrays using the `numpy` library

Although Python lists can behave like arrays, true arrays provide memory efficiency, type safety, and high-performance numerical computation.

This article explains both types in depth, including internal behavior, methods, and practical usage.

---

# 1. What is an Array?

An array is a collection of elements stored in contiguous memory locations.

### Key Characteristics

- Stores multiple elements under a single variable
- Elements are accessed using indexes
- Elements typically share the same data type
- Memory layout is compact and efficient

### Example Conceptual Array

```text
Index:  0   1   2   3
Value: 10  20  30  40
```

Accessing an element:

```python
array[2]  # 30
```

---

# 2. Built-in Python Arrays (`array` Module)

Python provides a built-in module called:

```python
array
```

This module allows creation of typed arrays.

Unlike lists, arrays require all elements to be of the same data type.

---

# 3. Importing the Array Module

Before using arrays, the module must be imported.

```python
import array
```

---

# 4. Creating Arrays

Arrays are created using the `array()` constructor.

### Syntax

```python
array.array(typecode, initializer)
```

### Example

```python
import array

numbers = array.array('i', [1, 2, 3, 4])
```

### Explanation

- `'i'` → typecode representing integer
- `[1, 2, 3, 4]` → initial elements

---

# 5. Array Type Codes

Arrays require specifying the type of elements.

### Common Type Codes

| Type Code | Description |
|------------|------------|
| `'b'` | Signed char |
| `'B'` | Unsigned char |
| `'i'` | Signed integer |
| `'I'` | Unsigned integer |
| `'f'` | Float |
| `'d'` | Double |

### Example

```python
array.array('f', [1.5, 2.7, 3.2])
```

---

# 6. Accessing Array Elements

Array elements are accessed using indexing.

### Example

```python
numbers = array.array('i', [10, 20, 30])

print(numbers[1])
```

**Output**

```text
20
```

---

# 7. Array Traversal

Arrays can be iterated using loops.

```python
for value in numbers:
    print(value)
```

---

# 8. Modifying Array Elements

Elements can be modified using index assignment.

```python
numbers[0] = 100
```

Result:

```python
[100, 20, 30]
```

---

# 9. Built-in Array Methods

The `array` module provides several useful methods.

## 9.1 `append()`

Adds an element at the end.

```python
numbers.append(40)
```

Result:

```python
[10, 20, 30, 40]
```

---

## 9.2 `extend()`

Adds multiple elements.

```python
numbers.extend([50, 60])
```

---

## 9.3 `insert()`

Inserts an element at a specific index.

```python
numbers.insert(1, 15)
```

Result:

```python
[10, 15, 20, 30]
```

---

## 9.4 `remove()`

Removes the first occurrence of a value.

```python
numbers.remove(20)
```

---

## 9.5 `pop()`

Removes an element at a given index.

```python
numbers.pop()
```

Removes the last element.

---

## 9.6 `index()`

Returns the index of a value.

```python
numbers.index(30)
```

---

## 9.7 `count()`

Counts occurrences of a value.

```python
numbers.count(10)
```

---

## 9.8 `reverse()`

Reverses the array.

```python
numbers.reverse()
```

---

## 9.9 `buffer_info()`

Returns memory address and length.

```python
numbers.buffer_info()
```

Example output:

```text
(address, length)
```

---

## 9.10 `tolist()`

Converts an array to a list.

```python
numbers.tolist()
```

---

## 9.11 `fromlist()`

Adds elements from a list.

```python
numbers.fromlist([70, 80])
```

---

## 9.12 `tobytes()`

Converts an array to bytes.

```python
numbers.tobytes()
```

---

## 9.13 `frombytes()`

Loads array data from bytes.

```python
numbers.frombytes(byte_data)
```

---

# 10. Limitations of Built-in Arrays

Built-in arrays are limited because:

- Only support 1D arrays
- No advanced mathematical operations
- Not suitable for scientific computing

For high-performance numerical work, Python uses NumPy arrays.

---

# 11. NumPy Arrays

NumPy stands for:

> Numerical Python

It is the most widely used library for scientific computing in Python.

### NumPy Arrays Provide

- Multi-dimensional arrays
- Vectorized operations
- High-performance numerical computation
- Efficient memory usage

---

# 12. Installing NumPy

```bash
pip install numpy
```

Import:

```python
import numpy as np
```

---

# 13. Creating NumPy Arrays

Using the `array()` function:

```python
import numpy as np

arr = np.array([1, 2, 3, 4])
```

**Output**

```text
[1 2 3 4]
```

---

# 14. Multi-Dimensional Arrays

NumPy supports multi-dimensional arrays.

### Example: 2D Array

```python
matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

Representation:

```text
1 2 3
4 5 6
```

---

# 15. Array Attributes

## `arr.ndim`

Number of dimensions.

```python
arr.ndim
```

---

## `arr.shape`

Dimensions of the array.

Example:

```python
(2, 3)
```

---

## `arr.size`

Total number of elements.

```python
arr.size
```

---

## `arr.dtype`

Data type of array elements.

```python
arr.dtype
```

---

# 16. NumPy Array Creation Functions

## `zeros()`

Creates an array filled with zeros.

```python
np.zeros((3, 3))
```

---

## `ones()`

Creates an array filled with ones.

```python
np.ones((2, 2))
```

---

## `arange()`

Creates a range-based array.

```python
np.arange(0, 10, 2)
```

**Output**

```text
[0 2 4 6 8]
```

---

## `linspace()`

Creates evenly spaced values.

```python
np.linspace(0, 1, 5)
```

---

## `eye()`

Creates an identity matrix.

```python
np.eye(3)
```

---

# 17. Array Indexing

```python
arr = np.array([10, 20, 30, 40])

arr[2]
```

**Output**

```text
30
```

---

# 18. Array Slicing

```python
arr[1:3]
```

**Output**

```text
[20 30]
```

---

# 19. Vectorized Operations

NumPy performs operations on entire arrays.

### Example

```python
arr + 10
```

**Output**

```text
[11 12 13 14]
```

### Multiplication

```python
arr * 2
```

---

# 20. Mathematical Operations

```python
np.sum(arr)
np.mean(arr)
np.max(arr)
np.min(arr)
np.std(arr)
```

---

# 21. Array Reshaping

Change the shape of an array.

```python
arr.reshape(2, 2)
```

Example:

```text
[[1 2]
 [3 4]]
```

---

# 22. Array Broadcasting

Broadcasting allows operations on arrays of different shapes.

### Example

```python
arr = np.array([1, 2, 3])

arr + 10
```

**Output**

```text
[11 12 13]
```

---

# 23. NumPy Performance Advantage

NumPy operations run faster because they are implemented in C and optimized machine code.

### Python Loop

```python
for i in range(len(arr)):
    arr[i] += 10
```

### NumPy Vectorization

```python
arr += 10
```

Vectorization is significantly faster.

---

# 24. Practical Applications

NumPy arrays are widely used in:

- Data science
- Machine learning
- Image processing
- Scientific computing
- Statistics
- Simulations

### Example: Matrix Multiplication

```python
np.dot(A, B)
```

---

# 25. Summary

Python arrays exist in two main forms.

## Built-in Arrays

- Provided by the `array` module
- Typed elements
- Memory efficient
- Limited functionality

## NumPy Arrays

- Multi-dimensional
- Fast numerical operations
- Vectorized computation
- Widely used in scientific computing

Understanding arrays is essential for working with large datasets, numerical computations, and performance-critical Python applications.
