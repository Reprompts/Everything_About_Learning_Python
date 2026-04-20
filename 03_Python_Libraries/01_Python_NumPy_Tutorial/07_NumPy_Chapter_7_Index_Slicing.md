# NumPy Hands-On Tutorial

## Chapter 7: Index Slicing

Array slicing is a powerful feature in NumPy that allows you to extract portions of an array without copying the entire data structure. Instead of accessing a single element, slicing lets you select ranges of elements, rows, columns, or multi-dimensional sections.

Slicing is widely used in:

* Data preprocessing
* Machine learning
* Image processing
* Scientific computing
* Data filtering

---

## 1. Basic Slicing

### Syntax

```python
array[start : stop : step]
```

| Parameter | Meaning                   |
| --------- | ------------------------- |
| start     | Starting index (included) |
| stop      | Ending index (excluded)   |
| step      | Step size                 |

### Example

```python
import numpy as np
arr = np.array([10,20,30,40,50])
print(arr[1:4])
```

### Output

```
[20 30 40]
```

### Omitting Start

```python
print(arr[:3])
```

### Output

```
[10 20 30]
```

### Omitting Stop

```python
print(arr[2:])
```

### Output

```
[30 40 50]
```

---

## 2. Row Slicing

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])
```

### Select One Row

```python
print(matrix[1])
```

### Output

```
[40 50 60]
```

### Slice Multiple Rows

```python
print(matrix[0:2])
```

### Output

```
[[10 20 30]
 [40 50 60]]
```

---

## 3. Column Slicing

### Syntax

```python
array[:, column_range]
```

### Example

```python
print(matrix[:,1])
```

### Output

```
[20 50 80]
```

### Multiple Columns

```python
print(matrix[:,0:2])
```

### Output

```
[[10 20]
 [40 50]
 [70 80]]
```

### Last Column

```python
print(matrix[:,-1])
```

### Output

```
[30 60 90]
```

---

## 4. Multi-Dimensional Slicing

### Example

```python
arr = np.array([
    [[1,2,3],[4,5,6]],
    [[7,8,9],[10,11,12]]
])
```

### Access Block

```python
print(arr[0])
```

### Output

```
[[1 2 3]
 [4 5 6]]
```

### Slice Inside Block

```python
print(arr[0, :, 1:])
```

### Output

```
[[2 3]
 [5 6]]
```

---

## 5. Step Slicing

### Example

```python
arr = np.array([10,20,30,40,50,60])
print(arr[::2])
```

### Output

```
[10 30 50]
```

### Custom Step

```python
print(arr[1:6:2])
```

### Output

```
[20 40 60]
```

### 2D Step Slicing

```python
matrix = np.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])
print(matrix[::2])
```

### Output

```
[[1 2 3]
 [7 8 9]]
```

---

## 6. Reversing Arrays

### Example

```python
arr = np.array([10,20,30,40])
print(arr[::-1])
```

### Output

```
[40 30 20 10]
```

### Reverse Rows

```python
print(matrix[::-1])
```

### Output

```
[[7 8 9]
 [4 5 6]
 [1 2 3]]
```

### Reverse Columns

```python
print(matrix[:, ::-1])
```

### Output

```
[[3 2 1]
 [6 5 4]
 [9 8 7]]
```

---

## 7. Copy vs View in Slicing

### View (Default Behavior)

```python
arr = np.array([1,2,3,4,5])
slice_arr = arr[1:4]
slice_arr[0] = 100
print(arr)
```

### Output

```
[  1 100   3   4   5]
```

### Copy (Independent Array)

```python
arr = np.array([1,2,3,4,5])
slice_arr = arr[1:4].copy()
slice_arr[0] = 100
print(arr)
```

### Output

```
[1 2 3 4 5]
```

### View vs Copy Summary

| Feature                 | View    | Copy      |
| ----------------------- | ------- | --------- |
| Memory shared           | Yes     | No        |
| Changes affect original | Yes     | No        |
| Created by              | Slicing | `.copy()` |

---

## Practical Example

```python
data = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

print("First two rows:")
print(data[:2])

print("\nLast column:")
print(data[:,-1])

print("\nEvery second row:")
print(data[::2])

print("\nReversed rows:")
print(data[::-1])
```

---

## Summary

| Concept                   | Description                  |
| ------------------------- | ---------------------------- |
| Basic slicing             | Select ranges of elements    |
| Row slicing               | Extract rows                 |
| Column slicing            | Extract columns              |
| Multi-dimensional slicing | Slice across axes            |
| Step slicing              | Select elements at intervals |
| Reverse slicing           | Reverse arrays               |
| Copy vs view              | Memory sharing control       |

---

## Conclusion

Array slicing is one of the most powerful features of NumPy for extracting and manipulating subsets of data.

It is essential for:

* Data preprocessing
* Feature selection
* Machine learning pipelines
* Image and signal processing

Mastering slicing enables efficient handling of large NumPy datasets.

---
