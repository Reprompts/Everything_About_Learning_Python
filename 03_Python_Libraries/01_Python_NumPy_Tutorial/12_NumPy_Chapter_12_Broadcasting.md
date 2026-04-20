
# Numpy Hands-On Tutorial

## Chapter 12: Broadcasting

Broadcasting is one of the most powerful features in NumPy. It allows NumPy to perform operations on arrays of different shapes automatically without explicitly copying or reshaping data.

Instead of manually expanding arrays, NumPy virtually stretches smaller arrays so they can participate in element-wise operations with larger arrays.

Broadcasting is heavily used in:
- Data science  
- Machine learning  
- Image processing  
- Scientific computing  
- Neural network computations  

It enables cleaner code, faster execution, and lower memory usage.

---

## 1. Understanding Broadcasting

Broadcasting allows NumPy to perform element-wise operations between arrays with different shapes by automatically expanding one of the arrays.

**Example**

Array A:
```

[1 2 3]

```

Scalar:
```

10

````

Operation:
```python
A + 10
````

Result:

```
[11 12 13]
```

Explanation:

The scalar `10` is broadcast to match the shape of the array:

```
[10 10 10]
```

Then element-wise addition happens:

```
[1 2 3]
+
[10 10 10]
=
[11 12 13]
```

> NumPy does not actually create the expanded array in memory — it performs the operation efficiently.

---

## 2. Why Broadcasting is Important

Without broadcasting, we would need to manually reshape arrays.

Example:

```
[1 2 3] + [10 10 10]
```

With broadcasting:

```python
[1 2 3] + 10
```

### Advantages

* Less memory usage
* Faster computations
* Cleaner code
* Automatic alignment of arrays

---

## 3. Broadcasting Rules

NumPy follows specific rules to determine compatibility.

When two arrays are used in an operation:

* Compare shapes from **right to left**
* Dimensions are compatible if:

  * They are equal, or
  * One of them is `1`

### Example

Shapes:

```
(3,4)
(1,4)
```

Comparison:

* 3 vs 1 → compatible
* 4 vs 4 → compatible

✅ Broadcasting works

### Failure Example

Shapes:

```
(3,4)
(2,4)
```

Comparison:

* 3 vs 2 → not compatible

❌ Broadcasting fails

---

## 4. Broadcasting with Scalars

The simplest case.

```python
import numpy as np
arr = np.array([10,20,30])
print(arr + 5)
```

**Output**

```
[15 25 35]
```

### More Examples

```python
arr = np.array([2,4,6])

print(arr * 3)   # [ 6 12 18]
print(arr / 2)   # [1. 2. 3.]
print(arr ** 2)  # [ 4 16 36]
```

---

## 5. Broadcasting with Arrays of Different Shapes

```python
A = np.array([
    [1,2,3],
    [4,5,6]
])

B = np.array([10,20,30])

print(A + B)
```

**Output**

```
[[11 22 33]
 [14 25 36]]
```

### Explanation

Shapes:

* A → (2,3)
* B → (3,)

B is treated as:

```
[[10 20 30],
 [10 20 30]]
```

---

## 6. Broadcasting with Column Vectors

```python
A = np.array([
    [1,2,3],
    [4,5,6]
])

B = np.array([[10],[20]])

print(A + B)
```

**Output**

```
[[11 12 13]
 [24 25 26]]
```

### Explanation

Shapes:

* A → (2,3)
* B → (2,1)

B expands across columns:

```
[[10 10 10]
 [20 20 20]]
```

---

## 7. Multi-Dimensional Broadcasting

```python
A = np.ones((3,1))
B = np.arange(4)

print(A + B)
```

**Output**

```
[[0. 1. 2. 3.]
 [0. 1. 2. 3.]
 [0. 1. 2. 3.]]
```

### Explanation

Shapes:

* A → (3,1)
* B → (4,)

After broadcasting:

* Both → (3,4)

---

## 8. Practical Broadcasting Examples

### Example 1: Adding Bias

```python
data = np.array([
    [1,2,3],
    [4,5,6]
])

bias = np.array([10,10,10])

print(data + bias)
```

**Output**

```
[[11 12 13]
 [14 15 16]]
```

---

### Example 2: Image Brightness Adjustment

```python
image = np.array([
    [50,80],
    [120,150]
])

bright_image = image + 20
print(bright_image)
```

**Output**

```
[[ 70 100]
 [140 170]]
```

---

### Example 3: Data Normalization

```python
data = np.array([
    [10,20,30],
    [40,50,60]
])

mean = np.array([25,35,45])

print(data - mean)
```

**Output**

```
[[-15 -15 -15]
 [ 15  15  15]]
```

---

## 9. Broadcasting vs Manual Expansion

### Without Broadcasting

```
[[10 20 30],
 [10 20 30]]
```

### With Broadcasting

```python
matrix + vector
```

### Benefits

* Less memory usage
* Faster operations
* Simpler code

---

## Summary

| Concept                   | Description                               |
| ------------------------- | ----------------------------------------- |
| Broadcasting              | Automatic expansion of smaller arrays     |
| Broadcasting rules        | Dimensions must match or be 1             |
| Scalar broadcasting       | Scalars applied to every element          |
| Array broadcasting        | Smaller arrays expanded across dimensions |
| Multi-dimensional support | Works with higher-dimensional arrays      |
| Practical uses            | ML, image processing, normalization       |

---

Broadcasting is one of the core features that makes NumPy extremely powerful and efficient for numerical computing.

```
```
