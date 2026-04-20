# NumPy Hands-On Tutorial

## Chapter 9: Fancy Indexing

Fancy indexing (also called advanced indexing) is a powerful NumPy feature that allows selecting array elements using arrays of indices instead of simple slices or single positions.

Unlike normal indexing:

* **Basic indexing** selects elements using numbers or slices
* **Fancy indexing** selects elements using lists or NumPy arrays

This makes it possible to:

* Select multiple arbitrary elements
* Extract specific rows or columns
* Reorder arrays
* Perform complex data selection

---

## 1. Selecting Elements Using Index Arrays

### Syntax

```python
array[[index1, index2, index3]]
```

### Example

```python
import numpy as np
arr = np.array([10, 20, 30, 40, 50])

result = arr[[0, 2, 4]]
print(result)
```

### Output

```
[10 30 50]
```

### Using NumPy Index Arrays

```python
indices = np.array([1,3])
print(arr[indices])
```

### Output

```
[20 40]
```

---

## 2. Reordering Arrays

### Example

```python
arr = np.array([10,20,30,40])
result = arr[[3,1,0,2]]
print(result)
```

### Output

```
[40 20 10 30]
```

---

## 3. Selecting Rows

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90],
    [100,110,120]
])

rows = matrix[[0,2]]
print(rows)
```

### Output

```
[[10 20 30]
 [70 80 90]]
```

### Custom Order

```python
rows = matrix[[3,1]]
print(rows)
```

---

## 4. Selecting Columns

### Syntax

```python
array[:, [col1, col2]]
```

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

columns = matrix[:, [0,2]]
print(columns)
```

### Output

```
[[10 30]
 [40 60]
 [70 90]]
```

---

## 5. Multi-Dimensional Fancy Indexing

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

rows = [0,1,2]
cols = [2,1,0]

result = matrix[rows, cols]
print(result)
```

### Output

```
[30 50 70]
```

---

## 6. Selecting Submatrices

### Example

```python
matrix = np.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])

rows = [0,2]
cols = [1,2]

result = matrix[rows][:, cols]
print(result)
```

### Output

```
[[2 3]
 [8 9]]
```

---

## 7. Random Selection

### Example

```python
data = np.array([10,20,30,40,50])
random_indices = np.random.randint(0,5,3)
print(data[random_indices])
```

---

## 8. Fancy Indexing vs Slicing

| Feature                   | Slicing | Fancy Indexing |
| ------------------------- | ------- | -------------- |
| Select ranges             | Yes     | No             |
| Select arbitrary elements | No      | Yes            |
| Reorder elements          | No      | Yes            |
| Use index arrays          | No      | Yes            |

---

## 9. Copy vs View

Fancy indexing always returns a **copy**, not a view.

### Example

```python
arr = np.array([10,20,30])
result = arr[[0,1]]

result[0] = 999
print(arr)
```

### Output

```
[10 20 30]
```

---

## 10. Practical Example

```python
marks = np.array([
    [85,90,88],
    [70,65,72],
    [95,92,96],
    [60,58,63]
])
```

### Select Top Students

```python
top_students = marks[[0,2]]
print(top_students)
```

### Output

```
[[85 90 88]
 [95 92 96]]
```

### Select Subjects

```python
subjects = marks[:, [0,2]]
print(subjects)
```

### Output

```
[[85 88]
 [70 72]
 [95 96]
 [60 63]]
```

---

## Summary

| Concept                    | Description                    |
| -------------------------- | ------------------------------ |
| Index arrays               | Select elements using arrays   |
| Row selection              | Extract rows                   |
| Column selection           | Extract columns                |
| Multi-dimensional indexing | Use row-column pairs           |
| Submatrix extraction       | Combine row & column selection |
| Copy behavior              | Always returns a copy          |

---

## Conclusion

Fancy indexing is a powerful technique for:

* Machine learning datasets
* Data sampling
* Feature selection
* Matrix manipulation
* Advanced data processing

Mastering it allows flexible and efficient data selection in NumPy.

---
