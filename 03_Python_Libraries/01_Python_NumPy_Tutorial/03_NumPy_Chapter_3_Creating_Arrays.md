# NumPy Hands-On Tutorial

## Chapter 3: Ways of Creating NumPy Arrays

Creating arrays is one of the most fundamental operations in NumPy. Since NumPy is designed for efficient numerical computing, it provides many powerful methods for generating arrays quickly and efficiently.

These methods allow you to:

* Convert Python data structures into arrays
* Generate arrays filled with specific values
* Create sequences of numbers
* Generate evenly spaced numeric ranges
* Produce random datasets for simulations and machine learning

This article explains all major NumPy array creation methods with practical examples.

---

## 1. Creating Arrays from Python Lists

The most common way to create a NumPy array is by converting a Python list using the `np.array()` function.

### Example

```python
import numpy as np
data = [10, 20, 30, 40]
arr = np.array(data)
print(arr)
print(type(arr))
```

### Output

```
[10 20 30 40]
<class 'numpy.ndarray'>
```

### Why Convert Lists to Arrays?

**Python lists:**

* Store mixed data types
* Are slower for numerical operations

**NumPy arrays:**

* Store homogeneous data
* Support fast vectorized operations

### Creating Multi-Dimensional Arrays from Lists

```python
matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
print(matrix)
```

### Output

```
[[1 2 3]
 [4 5 6]]
```

---

## 2. Creating Arrays from Tuples

Tuples can also be converted into NumPy arrays.

### Example

```python
import numpy as np
data = (5, 10, 15, 20)
arr = np.array(data)
print(arr)
```

### Output

```
[ 5 10 15 20]
```

### Multi-Dimensional Tuple Example

```python
data = (
    (1,2,3),
    (4,5,6)
)
arr = np.array(data)
print(arr)
```

### Output

```
[[1 2 3]
 [4 5 6]]
```

---

## 3. Creating Arrays using `array()`

### Syntax

```python
np.array(object, dtype=None)
```

### Parameters

| Parameter | Description                    |
| --------- | ------------------------------ |
| object    | Input data (list, tuple, etc.) |
| dtype     | Optional data type             |

### Example

```python
import numpy as np
arr = np.array([1, 2, 3, 4], dtype=float)
print(arr)
print(arr.dtype)
```

### Output

```
[1. 2. 3. 4.]
float64
```

### Creating Higher Dimensional Arrays

```python
arr = np.array([
    [[1,2],[3,4]],
    [[5,6],[7,8]]
])
print(arr)
```

---

## 4. Creating Arrays with `zeros()`

### Syntax

```python
np.zeros(shape)
```

### Example: 1D Array

```python
arr = np.zeros(5)
print(arr)
```

### Output

```
[0. 0. 0. 0. 0.]
```

### Example: 2D Array

```python
arr = np.zeros((3,4))
print(arr)
```

---

## 5. Creating Arrays with `ones()`

### Syntax

```python
np.ones(shape)
```

### Example

```python
arr = np.ones(4)
print(arr)
```

### Output

```
[1. 1. 1. 1.]
```

---

## 6. Creating Arrays with `empty()`

### Syntax

```python
np.empty(shape)
```

### Example

```python
arr = np.empty(4)
print(arr)
```

### Note

Values are uninitialized and depend on memory.

---

## 7. Creating Arrays with `full()`

### Syntax

```python
np.full(shape, value)
```

### Example

```python
arr = np.full(5, 7)
print(arr)
```

### Output

```
[7 7 7 7 7]
```

---

## 8. Creating Arrays using `arange()`

### Syntax

```python
np.arange(start, stop, step)
```

### Example

```python
arr = np.arange(0, 10)
print(arr)
```

### Output

```
[0 1 2 3 4 5 6 7 8 9]
```

---

## 9. Creating Arrays using `linspace()`

### Syntax

```python
np.linspace(start, stop, num)
```

### Example

```python
arr = np.linspace(0, 1, 5)
print(arr)
```

### Output

```
[0.   0.25 0.5  0.75 1.  ]
```

---

## 10. Creating Arrays using `logspace()`

### Syntax

```python
np.logspace(start, stop, num)
```

### Example

```python
arr = np.logspace(1, 3, 4)
print(arr)
```

### Output

```
[   10.   100.  1000. 10000.]
```

---

## 11. Creating Identity Matrices

### Example

```python
arr = np.identity(4)
print(arr)
```

---

## 12. Creating Arrays with `eye()`

### Example

```python
arr = np.eye(3)
print(arr)
```

### Rectangular Example

```python
arr = np.eye(3,5)
print(arr)
```

---

## 13. Creating Arrays with Random Values

### Random Uniform

```python
arr = np.random.rand(3,3)
print(arr)
```

### Random Integers

```python
arr = np.random.randint(1,10,(3,3))
print(arr)
```

### Normal Distribution

```python
arr = np.random.randn(3,3)
print(arr)
```

### Setting Seed

```python
np.random.seed(42)
print(np.random.rand(3))
```

---

## Practical Example Combining Methods

```python
zeros_array = np.zeros((2,2))
ones_array = np.ones((2,2))
range_array = np.arange(0,10)
lin_array = np.linspace(0,1,5)
random_array = np.random.rand(2,2)

print("Zeros\n", zeros_array)
print("Ones\n", ones_array)
print("Range\n", range_array)
print("Linspace\n", lin_array)
print("Random\n", random_array)
```

---

## Summary

| Function         | Purpose                            |
| ---------------- | ---------------------------------- |
| array()          | Convert lists/tuples to arrays     |
| zeros()          | Create arrays filled with 0        |
| ones()           | Create arrays filled with 1        |
| empty()          | Create uninitialized arrays        |
| full()           | Fill arrays with a specific value  |
| arange()         | Generate numerical sequences       |
| linspace()       | Generate evenly spaced numbers     |
| logspace()       | Generate logarithmic scale numbers |
| identity()       | Create identity matrices           |
| eye()            | Create flexible identity matrices  |
| random functions | Generate arrays with random values |

---

## Practice Examples

### 21. np.empty()

```python
arr = np.empty(5)
print(arr)

arr[:] = [1,2,3,4,5]
print(arr)
```

---

### 22. np.full()

```python
arr = np.full((2,3), 7)
print(arr)
```

---

### 23. np.logspace()

```python
arr = np.logspace(1, 4, 4)
print(arr)
```

---

### 24. Identity vs Eye

```python
identity_matrix = np.identity(4)
eye_matrix = np.eye(3,5)

print(identity_matrix)
print(eye_matrix)
```

---

### 25. Random Arrays

```python
np.random.seed(42)

arr1 = np.random.rand(2,2)
arr2 = np.random.randint(1,10,(2,2))

print(arr1)
print(arr2)
```

---

**Thanks for reading! Follow RePromptsQuest for more articles.**
