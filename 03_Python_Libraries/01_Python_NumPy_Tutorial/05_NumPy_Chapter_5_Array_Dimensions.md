# NumPy Hands-On Tutorial

## Chapter 5: NumPy Array & Dimensions

Understanding array shape and dimensions is fundamental when working with NumPy. These concepts describe how data is organized inside arrays and are essential for operations like reshaping, matrix computations, machine learning preprocessing, and numerical simulations.

NumPy arrays can represent data in multiple dimensions, from simple one-dimensional lists to complex multi-dimensional tensors used in scientific computing and AI.

---

## 1. Understanding Array Dimensions

The dimension of a NumPy array refers to the number of axes (directions) required to index the data.

This is represented by:

```python
ndim
```

### General Rule

* 0D → Scalar
* 1D → Vector
* 2D → Matrix
* 3D+ → Tensor

### Example

```python
import numpy as np
arr = np.array([1,2,3,4])
print(arr.ndim)
```

### Output

```
1
```

---

## 2. 1D Arrays

A 1-dimensional array is the simplest type of NumPy array.

### Example

```python
arr = np.array([10,20,30,40])
print(arr)
```

### Output

```
[10 20 30 40]
```

### Dimension

```python
print(arr.ndim)
```

### Shape

```python
print(arr.shape)
```

### Output

```
(4,)
```

### Uses

* Feature vectors
* Time series data
* Single variable datasets

---

## 3. 2D Arrays

A 2D array represents rows and columns.

### Example

```python
matrix = np.array([
    [1,2,3],
    [4,5,6]
])
print(matrix)
```

### Output

```
[[1 2 3]
 [4 5 6]]
```

### Dimension

```python
print(matrix.ndim)
```

### Shape

```python
print(matrix.shape)
```

### Output

```
(2, 3)
```

---

## 4. 3D Arrays

A 3D array contains multiple 2D arrays.

### Example

```python
arr = np.array([
    [[1,2],[3,4]],
    [[5,6],[7,8]]
])
print(arr)
```

### Output

```
[[[1 2]
  [3 4]]
 [[5 6]
  [7 8]]]
```

### Shape

```python
print(arr.shape)
```

### Output

```
(2, 2, 2)
```

---

## 5. Higher Dimensional Arrays

NumPy supports N-dimensional arrays.

### Example

```python
arr = np.zeros((2,3,4,5))
print(arr.ndim)
```

### Output

```
4
```

### Meaning of Shape `(2,3,4,5)`

* 2 blocks
* 3 matrices
* 4 rows
* 5 columns

### Applications

* Machine learning (batches of data)
* Deep learning tensors
* Scientific simulations
* Medical imaging

---

## 6. Getting Array Shape

The shape describes the structure of the array.

### Syntax

```python
array.shape
```

### Example

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])
print(arr.shape)
```

### Output

```
(2, 3)
```

---

## 7. Changing Array Shape

Use `reshape()` to change shape.

### Example

```python
arr = np.arange(12)
new_arr = arr.reshape(3,4)
print(new_arr)
```

### Output

```
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```

### Rule

Total elements must remain the same.

---

## 8. Flattening Arrays

Convert multi-dimensional array into 1D.

### Example

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])
flat = arr.flatten()
print(flat)
```

### Output

```
[1 2 3 4 5 6]
```

---

## 9. Reshaping Arrays

### Example

```python
arr = np.arange(8)
new_arr = arr.reshape(2,4)
print(new_arr)
```

### Automatic Dimension

```python
arr = np.arange(12)
new_arr = arr.reshape(3,-1)
print(new_arr)
```

### 3D Reshape

```python
arr = np.arange(24)
new_arr = arr.reshape(2,3,4)
print(new_arr)
```

---

## Practical Example

```python
data = np.arange(16)

print("Original Array:")
print(data)

matrix = data.reshape(4,4)
print("\nReshaped 4x4 Matrix:")
print(matrix)

print("\nFlattened Array:")
print(matrix.flatten())
```

---

## Summary

| Concept   | Meaning                |
| --------- | ---------------------- |
| ndim      | Number of dimensions   |
| shape     | Structure of array     |
| 1D array  | Vector                 |
| 2D array  | Matrix                 |
| 3D array  | Tensor                 |
| reshape() | Change array structure |
| flatten() | Convert to 1D          |

---

## Conclusion

Array shape and dimensions define how data is structured in NumPy arrays.

These operations are essential in:

* Data science
* Machine learning
* Image processing
* Scientific simulations

Understanding shape manipulation is critical because most NumPy operations depend on compatible shapes.

---
