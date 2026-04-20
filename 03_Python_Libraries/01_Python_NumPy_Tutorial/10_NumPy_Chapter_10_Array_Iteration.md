# NumPy Hands-On Tutorial

## Chapter 10: Array Iteration

Iteration means traversing elements of an array one by one. While NumPy encourages vectorized operations (which are faster and preferred), iteration is still useful when:

* Processing elements individually
* Performing custom operations
* Debugging array contents
* Working with complex multi-dimensional structures

NumPy provides multiple ways to iterate through arrays:

* Basic iteration
* Row iteration
* Column iteration
* Using `nditer()`
* Multi-dimensional iteration

---

## 1. Iterating Through Arrays

### Example

```python
import numpy as np
arr = np.array([10,20,30,40])

for element in arr:
    print(element)
```

### Output

```
10
20
30
40
```

---

## 2. Iterating Over Rows

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

for row in matrix:
    print(row)
```

### Output

```
[10 20 30]
[40 50 60]
[70 80 90]
```

### Iterating Elements Inside Rows

```python
for row in matrix:
    for element in row:
        print(element)
```

---

## 3. Iterating Over Columns

Use transpose (`.T`):

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

for column in matrix.T:
    print(column)
```

### Output

```
[10 40 70]
[20 50 80]
[30 60 90]
```

---

## 4. Using `nditer()`

### Example

```python
arr = np.array([[1,2],[3,4]])

for element in np.nditer(arr):
    print(element)
```

### Output

```
1
2
3
4
```

---

## 5. Modifying Elements with `nditer()`

### Example

```python
arr = np.array([1,2,3,4])

for x in np.nditer(arr, op_flags=['readwrite']):
    x[...] = x * 2

print(arr)
```

### Output

```
[2 4 6 8]
```

---

## 6. Multi-Dimensional Iteration

### Example

```python
arr = np.array([
    [[1,2],[3,4]],
    [[5,6],[7,8]]
])

for element in np.nditer(arr):
    print(element)
```

### Output

```
1
2
3
4
5
6
7
8
```

---

## 7. Iterating with Index Tracking

Use `ndenumerate()`:

### Example

```python
arr = np.array([[10,20],[30,40]])

for index, value in np.ndenumerate(arr):
    print(index, value)
```

### Output

```
(0, 0) 10
(0, 1) 20
(1, 0) 30
(1, 1) 40
```

---

## 8. Practical Example

```python
data = np.array([
    [40,60,70],
    [20,55,90]
])

for x in np.nditer(data, op_flags=['readwrite']):
    if x > 50:
        x[...] = x + 10

print(data)
```

### Output

```
[[ 40  70  80]
 [ 20  65 100]]
```

---

## 9. Iteration vs Vectorization

### Iteration

```python
for x in np.nditer(arr):
    x = x * 2
```

### Vectorized

```python
arr = arr * 2
```

### Advantages of Vectorization

* Faster
* Cleaner code
* Memory efficient

---

## Summary

| Concept                     | Description                           |
| --------------------------- | ------------------------------------- |
| Basic iteration             | Loop through 1D arrays                |
| Row iteration               | Default for 2D arrays                 |
| Column iteration            | Use transpose                         |
| nditer()                    | Efficient multi-dimensional iteration |
| Multi-dimensional iteration | Traverse complex arrays               |
| Element modification        | Use `readwrite` mode                  |
| ndenumerate()               | Iterate with indices                  |

---

## Conclusion

Iteration is helpful for:

* Custom element processing
* Debugging
* Multi-dimensional traversal
* Complex transformations

However, for most operations, **vectorized NumPy operations are preferred** for better performance.

---
