
# NumPy Hands-On Tutorial

## Chapter 17: Array Reshaping & Manipulation 

In real-world data processing, arrays rarely remain in their original structure. Data often needs to be reorganized, reshaped, flattened, or rearranged to match the requirements of algorithms, machine learning models, or mathematical operations.

NumPy provides powerful functions for array reshaping and manipulation, allowing you to change the structure of arrays without modifying the underlying data whenever possible.

These operations are widely used in:

- Data preprocessing  
- Machine learning pipelines  
- Scientific computing  
- Image processing  
- Neural network inputs  
- Matrix computations  

This article explains the most important array manipulation operations in NumPy.

---

## Reshaping Arrays

Reshaping means changing the dimensions of an array while keeping the same data.

For example:
- 1D array → 2D matrix  
- 2D matrix → 3D tensor  

The number of elements must remain the same.

### `reshape()` Function
```python
array.reshape(new_shape)
````

### Example

```python
import numpy as np

arr = np.array([1,2,3,4,5,6])
reshaped = arr.reshape(2,3)
print(reshaped)
```

**Output**

```
[[1 2 3]
 [4 5 6]]
```

**Explanation**

```
Original shape: (6,)
New shape: (2,3)
Total elements remain: 2 × 3 = 6
```

---

### Automatic Dimension Calculation

```python
arr = np.array([1,2,3,4,5,6])
reshaped = arr.reshape(3,-1)
print(reshaped)
```

**Output**

```
[[1 2]
 [3 4]
 [5 6]]
```

**Explanation**

```
NumPy calculates the missing dimension automatically.
```

---

## Flattening Arrays

Flattening converts a multi-dimensional array into a 1D array.

This is useful when algorithms require linear input data.

### `flatten()` Function

```python
array.flatten()
```

Flatten returns a **copy** of the array.

### Example

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6]
])

flat = arr.flatten()
print(flat)
```

**Output**

```
[1 2 3 4 5 6]
```

### Important Property

```
Flatten creates a new independent array.
```

```python
flat[0] = 100
print(arr)
```

Original array remains unchanged.

---

## Raveling Arrays

Raveling is similar to flattening but returns a **view** instead of a copy whenever possible.

### `ravel()` Function

```python
array.ravel()
```

### Example

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6]
])

raveled = arr.ravel()
print(raveled)
```

**Output**

```
[1 2 3 4 5 6]
```

### Important Difference

```
Ravel returns a view of the original array.
```

```python
raveled[0] = 100
print(arr)
```

**Output**

```
[[100   2   3]
 [  4   5   6]]
```

The original array changes because `ravel` references the same memory.

---

## Flatten vs Ravel

| Feature            | `flatten()`          | `ravel()`      |
| ------------------ | -------------------- | -------------- |
| Returns            | Copy                 | View (usually) |
| Memory             | New memory allocated | Same memory    |
| Performance        | Slightly slower      | Faster         |
| Effect on original | No                   | Possible       |

---

## Transposing Arrays

Transposing means swapping rows and columns.

If matrix shape is:

```
(m × n)
```

Transpose becomes:

```
(n × m)
```

### `transpose()` Function

```python
np.transpose(array)
```

or

```python
array.T
```

### Example

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr.T)
```

**Output**

```
[[1 4]
 [2 5]
 [3 6]]
```

**Explanation**

```
Original shape: (2,3)
Transposed shape: (3,2)
```

---

## Swapping Axes

In higher dimensional arrays, we may want to swap specific axes.

This is useful for:

* Image processing
* Deep learning
* Tensor operations

### `swapaxes()`

```python
np.swapaxes(array, axis1, axis2)
```

### Example

```python
import numpy as np

arr = np.arange(8).reshape(2,2,2)
print(np.swapaxes(arr, 0, 2))
```

**Explanation**

```
This swaps axis 0 and axis 2.
```

---

## Expanding Dimensions

Sometimes we need to increase the number of dimensions of an array.

This is useful in:

* Machine learning input formatting
* Deep learning models
* Broadcasting operations

### `expand_dims()`

```python
np.expand_dims(array, axis)
```

### Example

```python
import numpy as np

arr = np.array([1,2,3])
expanded = np.expand_dims(arr, axis=0)
print(expanded)
```

**Output**

```
[[1 2 3]]
```

**Shape**

```
(1,3)
```

---

### Expand at Different Axis

```python
expanded = np.expand_dims(arr, axis=1)
print(expanded)
```

**Output**

```
[[1]
 [2]
 [3]]
```

**Shape**

```
(3,1)
```

---

## Squeezing Dimensions

Squeezing removes dimensions of size 1 from arrays.

### `squeeze()`

```python
np.squeeze(array)
```

### Example

```python
import numpy as np

arr = np.array([[[1,2,3]]])
print(arr.shape)
```

**Output**

```
(1,1,3)
```

Now squeeze:

```python
squeezed = np.squeeze(arr)
print(squeezed)
print(squeezed.shape)
```

**Output**

```
[1 2 3]
(3,)
```

All dimensions of size 1 were removed.

---

### Squeeze Specific Axis

```python
np.squeeze(arr, axis=0)
```

Removes only the specified axis.

---

## Practical Example: Full Manipulation Workflow

```python
import numpy as np

arr = np.arange(12)

# Reshape
matrix = arr.reshape(3,4)

# Transpose
transposed = matrix.T

# Flatten
flat = matrix.flatten()

# Expand dimension
expanded = np.expand_dims(flat, axis=0)

# Squeeze dimension
squeezed = np.squeeze(expanded)

print("Matrix:\n", matrix)
print("Transposed:\n", transposed)
print("Flattened:", flat)
print("Expanded Shape:", expanded.shape)
print("Squeezed Shape:", squeezed.shape)
```

---

## Summary

NumPy provides powerful tools for reshaping and manipulating arrays.

### Key Operations

| Operation         | Function              |
| ----------------- | --------------------- |
| Reshape array     | `reshape()`           |
| Flatten array     | `flatten()`           |
| Ravel array       | `ravel()`             |
| Transpose array   | `transpose()` or `.T` |
| Swap axes         | `swapaxes()`          |
| Expand dimensions | `expand_dims()`       |
| Remove dimensions | `squeeze()`           |

---

## Why These Operations Are Important

Array reshaping and manipulation help:

* Prepare data for machine learning models
* Reorganize matrices for linear algebra operations
* Convert multidimensional datasets
* Optimize data processing workflows

These operations are essential for advanced NumPy, Pandas, and deep learning frameworks like TensorFlow and PyTorch.


