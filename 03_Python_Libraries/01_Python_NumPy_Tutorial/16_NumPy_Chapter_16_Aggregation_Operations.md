
# NumPy Hands-On Tutorial  
## Chapter 16: Aggregation Operations

Aggregation operations are used to summarize data by combining multiple values into a single result. Instead of analyzing every element individually, aggregation functions help compute overall statistics, totals, or cumulative values efficiently.

NumPy provides many powerful aggregation functions that operate on entire arrays or along specific axes.

Aggregation operations are widely used in:

- Data science  
- Machine learning  
- Statistical analysis  
- Financial modeling  
- Scientific simulations  
- Data preprocessing  

Because NumPy arrays are vectorized and optimized internally, aggregation functions operate much faster than traditional Python loops.

This article explains the most important aggregation operations in NumPy.

---

## Sum Operations

The sum operation calculates the total of all elements in an array.

### Mathematical Formula
```

Sum = x₁ + x₂ + x₃ + … + xₙ

````

### Function
```python
np.sum()
````

or

```python
array.sum()
```

### Example

```python
import numpy as np

arr = np.array([10, 20, 30, 40])
result = np.sum(arr)
print(result)
```

**Output**

```
100
```

**Explanation**

```
10 + 20 + 30 + 40 = 100
```

---

### Sum Along Axis

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(np.sum(arr, axis=0))
```

**Output**

```
[5 7 9]
```

**Explanation**

```
Column sums:
1+4 = 5
2+5 = 7
3+6 = 9
```

---

### Row-wise Sum

```python
print(np.sum(arr, axis=1))
```

**Output**

```
[6 15]
```

---

## Product Operations

The product operation multiplies all elements in an array.

### Mathematical Formula

```
Product = x₁ × x₂ × x₃ × … × xₙ
```

### Function

```python
np.prod()
```

### Example

```python
import numpy as np

arr = np.array([1,2,3,4])
print(np.prod(arr))
```

**Output**

```
24
```

**Explanation**

```
1 × 2 × 3 × 4 = 24
```

---

### Product Along Axis

```python
arr = np.array([
    [1,2],
    [3,4]
])

print(np.prod(arr, axis=0))
```

**Output**

```
[3 8]
```

**Explanation**

```
1×3 = 3
2×4 = 8
```

---

## Cumulative Sum

The cumulative sum calculates the running total of array elements.

Each element represents the sum of all previous elements including itself.

### Function

```python
np.cumsum()
```

### Example

```python
import numpy as np

arr = np.array([1,2,3,4])
print(np.cumsum(arr))
```

**Output**

```
[ 1  3  6 10]
```

**Explanation**

```
1
1+2 = 3
1+2+3 = 6
1+2+3+4 = 10
```

---

### Cumulative Sum in 2D Arrays

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(np.cumsum(arr, axis=0))
```

**Output**

```
[[1 2 3]
 [5 7 9]]
```

**Explanation**

```
Running totals column-wise.
```

---

## Cumulative Product

The cumulative product calculates the running product of elements.

Each element represents the product of all previous elements including itself.

### Function

```python
np.cumprod()
```

### Example

```python
import numpy as np

arr = np.array([1,2,3,4])
print(np.cumprod(arr))
```

**Output**

```
[ 1  2  6 24]
```

**Explanation**

```
1
1×2 = 2
1×2×3 = 6
1×2×3×4 = 24
```

---

### Cumulative Product Along Axis

```python
arr = np.array([
    [1,2],
    [3,4]
])

print(np.cumprod(arr, axis=0))
```

**Output**

```
[[1 2]
 [3 8]]
```

---

## Finding Minimum Values

The minimum operation returns the smallest element in an array.

### Function

```python
np.min()
```

or

```python
array.min()
```

### Example

```python
import numpy as np

arr = np.array([15, 7, 22, 3, 10])
print(np.min(arr))
```

**Output**

```
3
```

---

### Minimum Along Axis

```python
arr = np.array([
    [5,9,2],
    [7,1,6]
])

print(np.min(arr, axis=0))
```

**Output**

```
[5 1 2]
```

**Explanation**

```
Minimum of each column.
```

---

## Finding Maximum Values

The maximum operation returns the largest element in an array.

### Function

```python
np.max()
```

or

```python
array.max()
```

### Example

```python
import numpy as np

arr = np.array([10, 35, 22, 8])
print(np.max(arr))
```

**Output**

```
35
```

---

### Maximum Along Axis

```python
arr = np.array([
    [5,9,2],
    [7,1,6]
])

print(np.max(arr, axis=1))
```

**Output**

```
[9 7]
```

**Explanation**

```
Maximum of each row.
```

---

## Argmin and Argmax Operations

Sometimes we need to know where the minimum or maximum value occurs, not just the value itself.

NumPy provides:

* `argmin()`
* `argmax()`

These functions return the index position of the minimum or maximum value.

---

### Argmin

Returns the index of the smallest value.

#### Function

```python
np.argmin()
```

#### Example

```python
import numpy as np

arr = np.array([10, 5, 8, 3, 12])
print(np.argmin(arr))
```

**Output**

```
3
```

**Explanation**

```
Index 3 contains value 3 (smallest element).
```

---

### Argmax

Returns the index of the largest value.

#### Function

```python
np.argmax()
```

#### Example

```python
import numpy as np

arr = np.array([10, 5, 8, 3, 12])
print(np.argmax(arr))
```

**Output**

```
4
```

**Explanation**

```
Index 4 contains value 12 (largest element).
```

---

### Argmax in 2D Arrays

```python
arr = np.array([
    [1,7,3],
    [4,2,9]
])

print(np.argmax(arr))
```

**Output**

```
5
```

**Explanation**

```
NumPy flattens the array before computing the index.

Flattened array:
[1,7,3,4,2,9]

Index of maximum value 9 = 5.
```

---

## Practical Example: Complete Aggregation Analysis

```python
import numpy as np

data = np.array([2,4,6,8,10])

print("Sum:", np.sum(data))
print("Product:", np.prod(data))
print("Cumulative Sum:", np.cumsum(data))
print("Cumulative Product:", np.cumprod(data))
print("Minimum:", np.min(data))
print("Maximum:", np.max(data))
print("Argmin:", np.argmin(data))
print("Argmax:", np.argmax(data))
```

**Output**

```
Sum: 30
Product: 3840
Cumulative Sum: [ 2  6 12 20 30]
Cumulative Product: [   2    8   48  384 3840]
Minimum: 2
Maximum: 10
Argmin: 0
Argmax: 4
```

---

## Summary

Aggregation functions allow you to summarize large datasets efficiently.

### Key Aggregation Operations

| Operation          | Function     |
| ------------------ | ------------ |
| Sum                | np.sum()     |
| Product            | np.prod()    |
| Cumulative sum     | np.cumsum()  |
| Cumulative product | np.cumprod() |
| Minimum value      | np.min()     |
| Maximum value      | np.max()     |
| Index of minimum   | np.argmin()  |
| Index of maximum   | np.argmax()  |

---

## Why Aggregation is Important

Aggregation operations help:

* Summarize datasets
* Perform statistical analysis
* Compute totals and cumulative values
* Identify extreme values
* Analyze large numerical datasets efficiently

These functions are heavily used in real-world data analysis workflows.


