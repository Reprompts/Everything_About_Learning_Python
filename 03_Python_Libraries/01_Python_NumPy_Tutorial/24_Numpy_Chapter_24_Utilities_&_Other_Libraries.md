
# NumPy Hands-On Tutorial

## Chapter 24: Utilities & Other Libraries

NumPy is not only a powerful numerical library but also the foundation of the Python scientific ecosystem. Many libraries such as Pandas, SciPy, Matplotlib, and machine learning frameworks depend heavily on NumPy arrays.

This chapter covers:

- NumPy Utility Operations  
- NumPy Integration with Other Libraries  

---

## NumPy Utilities

NumPy provides utility functions for:

- Array comparison  
- Set operations  
- Counting and frequency analysis  

Used in:

- Data preprocessing  
- Feature engineering  
- Data validation  
- Statistical analysis  
- Machine learning pipelines  

---

## Array Comparison Operations

These operations compare elements and return Boolean arrays.

### Example
```python
import numpy as np

arr = np.array([10,20,30,40])
result = arr > 25
print(result)
````

**Output**

```
[False False True True]
```

---

### Common Comparison Operators

| Operator | Meaning               |
| -------- | --------------------- |
| ==       | Equal                 |
| !=       | Not equal             |
| >        | Greater than          |
| <        | Less than             |
| >=       | Greater than or equal |
| <=       | Less than or equal    |

---

### Example

```python
import numpy as np

a = np.array([1,2,3,4])
b = np.array([1,3,2,4])

print(a == b)
```

**Output**

```
[ True False False  True]
```

---

## Checking Array Equality

### Using `np.array_equal()`

Checks if arrays have the same shape and elements.

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([1,2,3])

print(np.array_equal(a,b))
```

**Output**

```
True
```

---

### Using `np.allclose()`

Used for floating-point comparison with tolerance.

```python
import numpy as np

a = np.array([0.1 + 0.2])
b = np.array([0.3])

print(np.allclose(a,b))
```

**Output**

```
True
```

---

## Finding Array Intersections

```python
import numpy as np

a = np.array([1,2,3,4])
b = np.array([3,4,5,6])

result = np.intersect1d(a,b)
print(result)
```

**Output**

```
[3 4]
```

---

## Array Set Operations

### Union

```python
np.union1d(a,b)
```

### Example

```python
print(np.union1d(a,b))
```

**Output**

```
[1 2 3 4 5]
```

---

### Difference

```python
np.setdiff1d(a,b)
```

**Output**

```
[1 2]
```

---

### Symmetric Difference

```python
np.setxor1d(a,b)
```

**Output**

```
[1 2 4 5]
```

---

## Counting Occurrences

### Using `np.unique()`

```python
import numpy as np

arr = np.array([1,2,2,3,3,3,4])
values, counts = np.unique(arr, return_counts=True)

print(values)
print(counts)
```

**Output**

```
[1 2 3 4]
[1 2 3 1]
```

---

### Using `np.bincount()`

```python
import numpy as np

arr = np.array([1,2,2,3,3,3])
print(np.bincount(arr))
```

**Output**

```
[0 1 2 3]
```

---

## NumPy with Other Libraries

NumPy is the core numerical engine for the Python ecosystem.

### Major Libraries

* Pandas
* Matplotlib
* SciPy
* Machine learning frameworks

---

## NumPy with Pandas

Pandas uses NumPy internally.

### NumPy → DataFrame

```python
import numpy as np
import pandas as pd

arr = np.array([
    [1,2,3],
    [4,5,6]
])

df = pd.DataFrame(arr, columns=["A","B","C"])
print(df)
```

---

### DataFrame → NumPy

```python
array = df.values
print(array)
```

---

## NumPy with Matplotlib

Used for visualization.

### Example: Line Plot

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0,10,100)
y = np.sin(x)

plt.plot(x,y)
plt.show()
```

---

### Example: Histogram

```python
data = np.random.randn(1000)
plt.hist(data)
plt.show()
```

---

## NumPy with SciPy

SciPy extends NumPy with advanced algorithms.

### Example

```python
from scipy import linalg
import numpy as np

A = np.array([[1,2],[3,4]])
det = linalg.det(A)

print(det)
```

---

## NumPy in Machine Learning Workflows

NumPy is fundamental in ML pipelines.

### Typical Workflow

#### Step 1: Load Dataset

```python
data = np.loadtxt("dataset.csv", delimiter=",")
```

#### Step 2: Split Features and Labels

```python
X = data[:, :-1]
y = data[:, -1]
```

#### Step 3: Feature Scaling

```python
X = (X - np.mean(X, axis=0)) / np.std(X, axis=0)
```

#### Step 4: Matrix Operations

```python
prediction = X @ weights
```

#### Step 5: Model Training

```python
predictions = X @ weights
error = predictions - y
```

---

## Why NumPy is Essential

NumPy provides:

* Fast numerical computation
* Efficient memory usage
* Vectorized operations
* Integration with scientific libraries

---

## Summary

### NumPy Utility Operations

| Operation           | Function           | Example Usage                      |
| ------------------- | ------------------ | ---------------------------------- |
| Array equality      | `np.array_equal()` | `np.array_equal(A, B)`             |
| Floating comparison | `np.allclose()`    | `np.allclose(A, B)`                |
| Intersection        | `np.intersect1d()` | `np.intersect1d(A, B)`             |
| Union               | `np.union1d()`     | `np.union1d(A, B)`                 |
| Difference          | `np.setdiff1d()`   | `np.setdiff1d(A, B)`               |
| Symmetric diff      | `np.setxor1d()`    | `np.setxor1d(A, B)`                |
| Counting            | `np.unique()`      | `np.unique(A, return_counts=True)` |

---

### NumPy Ecosystem Integration

| Library      | Purpose              |
| ------------ | -------------------- |
| Pandas       | Data analysis        |
| Matplotlib   | Visualization        |
| SciPy        | Scientific computing |
| ML Libraries | Machine learning     |


