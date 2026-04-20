
# NumPy Hands-On Tutorial

## Chapter 19: Sorting & Searching

Sorting and searching operations are essential when working with numerical data. In real-world applications, we often need to:

- Arrange values in ascending or descending order  
- Identify the positions of elements  
- Find unique values in datasets  
- Search efficiently within sorted arrays  

NumPy provides powerful built-in functions that allow fast sorting and searching operations on arrays. These functions are optimized internally and work efficiently even with very large datasets.

Sorting and searching operations are widely used in:

- Data analysis  
- Machine learning preprocessing  
- Statistical computations  
- Database-like operations on arrays  
- Scientific computing  

This article explains the major sorting and searching functions available in NumPy.

---

## Sorting Arrays

Sorting arranges the elements of an array in a specific order, usually ascending order.

NumPy provides two main ways to sort arrays:

- `np.sort()` → returns a sorted copy of the array  
- `array.sort()` → sorts the array in-place  

---

### Using `np.sort()`
```python
np.sort(array)
````

This returns a new sorted array while leaving the original array unchanged.

### Example

```python
import numpy as np

arr = np.array([5,2,8,1,3])
sorted_arr = np.sort(arr)

print(sorted_arr)
```

**Output**

```
[1 2 3 5 8]
```

Original array remains unchanged:

```python
print(arr)
```

**Output**

```
[5 2 8 1 3]
```

---

### In-Place Sorting

```python
array.sort()
```

This modifies the original array.

### Example

```python
arr = np.array([7,4,9,2])
arr.sort()
print(arr)
```

**Output**

```
[2 4 7 9]
```

---

## Sorting Along Axes

For multi-dimensional arrays, sorting can be performed along rows or columns.

* `axis=0` → column-wise sorting
* `axis=1` → row-wise sorting

### Example Array

```python
import numpy as np

arr = np.array([
    [3,1,2],
    [6,4,5]
])
```

### Row-wise Sorting

```python
print(np.sort(arr, axis=1))
```

**Output**

```
[[1 2 3]
 [4 5 6]]
```

Each row is sorted independently.

---

### Column-wise Sorting

```python
print(np.sort(arr, axis=0))
```

**Output**

```
[[3 1 2]
 [6 4 5]]
```

Columns are sorted independently.

---

## Argsort Operations

Sometimes we need to know the indices that would sort an array, rather than the sorted values themselves.

Useful for:

* Ranking data
* Reordering multiple arrays consistently
* Finding sorted positions

### `argsort()`

```python
np.argsort(array)
```

### Example

```python
import numpy as np

arr = np.array([50,10,30,20])
indices = np.argsort(arr)

print(indices)
```

**Output**

```
[1 3 2 0]
```

**Explanation**

```
Sorted values: [10,20,30,50]
Indices:       [1,3,2,0]
```

---

### Using Argsort to Sort Another Array

```python
scores = np.array([88,75,90])
names = np.array(["Alice","Bob","Charlie"])

order = np.argsort(scores)
print(names[order])
```

**Output**

```
['Bob' 'Alice' 'Charlie']
```

Names sorted according to scores.

---

## Finding Unique Values

NumPy can identify unique elements in an array, removing duplicates.

### `unique()`

```python
np.unique(array)
```

### Example

```python
import numpy as np

arr = np.array([1,2,2,3,3,3,4,5])
unique_values = np.unique(arr)

print(unique_values)
```

**Output**

```
[1 2 3 4 5]
```

Duplicates removed.

---

### Unique Values with Counts

```python
values, counts = np.unique(arr, return_counts=True)

print(values)
print(counts)
```

**Output**

```
[1 2 3 4 5]
[1 2 3 1 1]
```

---

## Searching Sorted Arrays

NumPy provides a fast method to search where a value should be inserted in a sorted array.

### `searchsorted()`

```python
np.searchsorted(array, value)
```

The array must already be sorted.

### Example

```python
import numpy as np

arr = np.array([10,20,30,40])
index = np.searchsorted(arr, 25)

print(index)
```

**Output**

```
2
```

**Explanation**

```
25 should be inserted at index 2.
```

---

### Multiple Searches

```python
values = np.array([5,15,35])
print(np.searchsorted(arr, values))
```

**Output**

```
[0 1 3]
```

---

## Finding Indices of Elements

Sometimes we need to find where certain values occur in an array.

NumPy provides:

* `np.where()`
* `np.nonzero()`

---

### Using `where()`

```python
np.where(condition)
```

### Example

```python
import numpy as np

arr = np.array([10,20,30,20,40])
indices = np.where(arr == 20)

print(indices)
```

**Output**

```
(array([1, 3]),)
```

---

### Example with Condition

```python
indices = np.where(arr > 25)
print(indices)
```

**Output**

```
(array([2, 4]),)
```

Elements greater than 25.

---

### Using `nonzero()`

```python
np.nonzero(array)
```

### Example

```python
import numpy as np

arr = np.array([0,1,0,2,3,0])
print(np.nonzero(arr))
```

**Output**

```
(array([1,3,4]),)
```

Indices of non-zero values.

---

## Practical Example: Data Analysis

```python
import numpy as np

data = np.array([12,45,23,67,45,23,89])

print("Sorted:", np.sort(data))
print("Argsort:", np.argsort(data))
print("Unique values:", np.unique(data))
print("Indices of value 45:", np.where(data == 45))

sorted_data = np.sort(data)
print("Insert position for 50:", np.searchsorted(sorted_data, 50))
```

---

## Summary

Sorting and searching functions help organize and analyze datasets efficiently.

### Sorting Functions

| Operation        | Function       |
| ---------------- | -------------- |
| Sort array       | `np.sort()`    |
| In-place sorting | `array.sort()` |
| Sort indices     | `np.argsort()` |

---

### Searching Functions

| Operation                | Function            |
| ------------------------ | ------------------- |
| Unique values            | `np.unique()`       |
| Search insertion index   | `np.searchsorted()` |
| Find indices (condition) | `np.where()`        |
| Find non-zero indices    | `np.nonzero()`      |

---

## Why Sorting and Searching Are Important

These operations help:

* Organize datasets
* Perform ranking and ordering
* Remove duplicate values
* Locate elements efficiently
* Prepare data for machine learning algorithms

They are heavily used in data analysis pipelines, database-like queries, and scientific computations.

