# NumPy Hands-On Tutorial

## Chapter 6: Array Indexing

Array indexing is one of the most essential skills when working with NumPy. It allows you to access, retrieve, and manipulate specific elements inside arrays efficiently.

Because NumPy arrays can contain multiple dimensions, indexing becomes more powerful compared to Python lists.

---

## 1. Basic Indexing

Indexing refers to accessing elements using their position.

Indexing starts from:

```
0
```

### Example

```python
import numpy as np
arr = np.array([10,20,30,40])
print(arr[0])
```

### Output

```
10
```

### Access Multiple Elements

```python
print(arr[1])
print(arr[2])
print(arr[3])
```

---

## 2. Accessing Elements by Position

Each element has a position (index).

### Example

```python
arr = np.array([5,10,15,20])
print(arr[2])
```

### Output

```
15
```

### Modifying Elements

```python
arr[1] = 100
print(arr)
```

### Output

```
[  5 100  15  20]
```

---

## 3. Indexing 1D Arrays

A 1D array behaves like a Python list.

### Example

```python
arr = np.array([10,20,30,40,50])
```

### Access Elements

```python
print(arr[0])
print(arr[2])
print(arr[4])
```

### Output

```
10
30
50
```

### Mathematical Operations

```python
result = arr[1] + arr[3]
print(result)
```

### Output

```
60
```

---

## 4. Indexing 2D Arrays

Use:

```python
array[row_index, column_index]
```

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])
```

### Access Single Element

```python
print(matrix[0,1])
```

### Output

```
20
```

### More Examples

```python
print(matrix[2,2])  # 90
print(matrix[1,0])  # 40
```

### Access Entire Row

```python
print(matrix[1])
```

### Output

```
[40 50 60]
```

### Access Entire Column

```python
print(matrix[:,1])
```

### Output

```
[20 50 80]
```

---

## 5. Indexing Multi-Dimensional Arrays

For multi-dimensional arrays, specify an index for each dimension.

### Example

```python
arr = np.array([
    [[1,2,3],[4,5,6]],
    [[7,8,9],[10,11,12]]
])
```

### Access Element

```python
print(arr[0,1,2])
```

### Output

```
6
```

### Another Example

```python
print(arr[1,0,1])
```

### Output

```
8
```

---

## 6. Negative Indexing

Negative indexing accesses elements from the end.

### Example

```python
arr = np.array([10,20,30,40])
print(arr[-1])
```

### Output

```
40
```

### More Examples

```python
print(arr[-2])  # 30
```

### 2D Example

```python
matrix = np.array([
    [1,2,3],
    [4,5,6]
])
print(matrix[-1,-1])
```

### Output

```
6
```

---

## 7. Selecting Specific Elements

### Manual Selection

```python
arr = np.array([10,20,30,40,50])
selected = [arr[0], arr[2], arr[4]]
print(selected)
```

### Output

```
[10, 30, 50]
```

### Using List of Indices (Advanced Indexing)

```python
arr = np.array([10,20,30,40,50])
indices = [0,2,4]
print(arr[indices])
```

### Output

```
[10 30 50]
```

### 2D Advanced Indexing

```python
matrix = np.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])
print(matrix[[0,2],[1,2]])
```

### Output

```
[2 9]
```

---

## Practical Example

```python
data = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

print("First Element:", data[0,0])
print("Last Element:", data[-1,-1])
print("Second Row:", data[1])
print("Third Column:", data[:,2])
```

### Output

```
First Element: 10
Last Element: 90
Second Row: [40 50 60]
Third Column: [30 60 90]
```

---

## Summary

| Concept                    | Description                              |
| -------------------------- | ---------------------------------------- |
| Basic indexing             | Access elements using position           |
| 1D indexing                | Single index access                      |
| 2D indexing                | Row and column access                    |
| Multi-dimensional indexing | Access elements across multiple axes     |
| Negative indexing          | Access elements from the end             |
| Selecting elements         | Retrieve specific elements using indices |

---

## Conclusion

Mastering indexing is essential because it enables:

* Data retrieval
* Data filtering
* Mathematical operations
* Efficient data manipulation

Almost every NumPy workflow relies heavily on array indexing techniques.

---
