
# NumPy Hands-On Tutorial

## Chapter 18: Joining & Splitting Arrays

In real-world data processing, arrays are frequently combined (joined) or divided (split). For example:

- Combining multiple datasets into one structure  
- Merging training data batches  
- Dividing datasets for training and testing  
- Organizing matrices for mathematical operations  

NumPy provides powerful functions for joining arrays together and splitting arrays into smaller parts.

These operations are essential in:

- Data preprocessing  
- Machine learning workflows  
- Scientific computing  
- Image and signal processing  
- Large dataset manipulation  

This article explains the major methods used for joining and splitting arrays in NumPy.

---

## Concatenating Arrays

Concatenation means joining arrays along an existing axis.

The arrays must have compatible shapes, meaning all dimensions must match except the axis being joined.

### `concatenate()`
```python
np.concatenate((array1, array2), axis)
````

* `axis=0` → join rows (vertical)
* `axis=1` → join columns (horizontal)

---

### Example: Concatenate 1D Arrays

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

result = np.concatenate((a,b))
print(result)
```

**Output**

```
[1 2 3 4 5 6]
```

---

### Example: Concatenate 2D Arrays (Vertical)

```python
a = np.array([
    [1,2],
    [3,4]
])

b = np.array([
    [5,6],
    [7,8]
])

result = np.concatenate((a,b), axis=0)
print(result)
```

**Output**

```
[[1 2]
 [3 4]
 [5 6]
 [7 8]]
```

---

### Example: Concatenate Horizontally

```python
result = np.concatenate((a,b), axis=1)
print(result)
```

**Output**

```
[[1 2 5 6]
 [3 4 7 8]]
```

---

## Stacking Arrays Vertically

Vertical stacking means placing arrays on top of each other.

### `vstack()`

```python
np.vstack((array1, array2))
```

Equivalent to:

```
concatenate(axis=0)
```

### Example

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

result = np.vstack((a,b))
print(result)
```

**Output**

```
[[1 2 3]
 [4 5 6]]
```

---

### Example with 2D Arrays

```python
a = np.array([
    [1,2],
    [3,4]
])

b = np.array([
    [5,6]
])

print(np.vstack((a,b)))
```

**Output**

```
[[1 2]
 [3 4]
 [5 6]]
```

---

## Stacking Arrays Horizontally

Horizontal stacking means placing arrays side-by-side.

### `hstack()`

```python
np.hstack((array1, array2))
```

Equivalent to:

```
concatenate(axis=1)
```

### Example

```python
import numpy as np

a = np.array([
    [1,2],
    [3,4]
])

b = np.array([
    [5,6],
    [7,8]
])

result = np.hstack((a,b))
print(result)
```

**Output**

```
[[1 2 5 6]
 [3 4 7 8]]
```

---

## Column Stacking

Column stacking stacks arrays as columns into a 2D array.

### `column_stack()`

```python
np.column_stack((array1, array2))
```

### Example

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

result = np.column_stack((a,b))
print(result)
```

**Output**

```
[[1 4]
 [2 5]
 [3 6]]
```

**Explanation**

```
The arrays become columns in a matrix.
```

---

## Row Stacking

Row stacking stacks arrays as rows in a matrix.

### `row_stack()`

```python
np.row_stack((array1, array2))
```

This is similar to `vstack()`.

### Example

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

result = np.row_stack((a,b))
print(result)
```

**Output**

```
[[1 2 3]
 [4 5 6]]
```

---

## Splitting Arrays

Splitting divides an array into multiple smaller arrays.

Useful for:

* Dividing datasets
* Creating training/testing sets
* Parallel processing

---

### `split()`

```python
np.split(array, number_of_parts)
```

### Example

```python
import numpy as np

arr = np.array([1,2,3,4,5,6])
result = np.split(arr, 3)
print(result)
```

**Output**

```
[array([1, 2]), array([3, 4]), array([5, 6])]
```

**Explanation**

```
Array split into 3 equal parts.
```

---

### Splitting at Specific Indexes

```python
result = np.split(arr, [2,4])
print(result)
```

**Output**

```
[array([1,2]), array([3,4]), array([5,6])]
```

---

## Splitting Arrays Vertically

Vertical splitting divides arrays row-wise.

### `vsplit()`

```python
np.vsplit(array, sections)
```

Works only for 2D arrays.

### Example

```python
import numpy as np

arr = np.array([
    [1,2],
    [3,4],
    [5,6],
    [7,8]
])

result = np.vsplit(arr, 2)
print(result)
```

**Output**

```
[array([[1,2],
        [3,4]]),
 array([[5,6],
        [7,8]])]
```

**Explanation**

```
Matrix split into two vertical parts.
```

---

## Splitting Arrays Horizontally

Horizontal splitting divides arrays column-wise.

### `hsplit()`

```python
np.hsplit(array, sections)
```

### Example

```python
import numpy as np

arr = np.array([
    [1,2,3,4],
    [5,6,7,8]
])

result = np.hsplit(arr, 2)
print(result)
```

**Output**

```
[array([[1,2],
        [5,6]]),
 array([[3,4],
        [7,8]])]
```

**Explanation**

```
Columns split into two groups.
```

---

## Practical Example: Dataset Manipulation

```python
import numpy as np

data1 = np.array([
    [1,2],
    [3,4]
])

data2 = np.array([
    [5,6],
    [7,8]
])

# Combine datasets
combined = np.vstack((data1,data2))

# Split dataset
train, test = np.vsplit(combined,2)

print("Combined Data:\n", combined)
print("Train Data:\n", train)
print("Test Data:\n", test)
```

---

## Summary

NumPy provides many functions for joining and splitting arrays efficiently.

### Joining Operations

| Operation           | Function         |
| ------------------- | ---------------- |
| Concatenate arrays  | `concatenate()`  |
| Vertical stacking   | `vstack()`       |
| Horizontal stacking | `hstack()`       |
| Column stacking     | `column_stack()` |
| Row stacking        | `row_stack()`    |

---

### Splitting Operations

| Operation        | Function   |
| ---------------- | ---------- |
| General split    | `split()`  |
| Vertical split   | `vsplit()` |
| Horizontal split | `hsplit()` |

---

## Why These Operations Matter

Joining and splitting arrays help:

* Combine datasets
* Divide data for training/testing
* Rearrange matrices
* Manage large numerical datasets
* Build efficient data pipelines

These operations are heavily used in NumPy, Pandas, machine learning libraries, and scientific computing frameworks.

