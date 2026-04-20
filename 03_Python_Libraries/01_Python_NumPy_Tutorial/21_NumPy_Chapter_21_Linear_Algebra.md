
# NumPy Hands-On Tutorial

## Chapter 21: Linear Algebra

Linear algebra is one of the most important areas of numerical computing. It forms the mathematical foundation of many fields including:

- Machine Learning  
- Data Science  
- Computer Graphics  
- Physics simulations  
- Engineering computations  
- Optimization algorithms  

NumPy provides powerful support for linear algebra operations through the module:

```

numpy.linalg

```

This module includes functions for matrix multiplication, determinants, solving linear systems, eigenvalues, and more.

---

## Matrix Creation

A matrix is a 2-dimensional array consisting of rows and columns.

In NumPy, matrices are represented using 2D arrays (`ndarray`).

### Example Structure
```

| a11  a12  a13 |
| a21  a22  a23 |
| a31  a32  a33 |

````

---

### Creating a Matrix from Lists
```python
import numpy as np

A = np.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])

print(A)
````

**Output**

```
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```

---

## Creating Special Matrices

### Identity Matrix

```python
I = np.eye(3)
print(I)
```

**Output**

```
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
```

---

### Zero Matrix

```python
Z = np.zeros((2,3))
print(Z)
```

---

### Ones Matrix

```python
O = np.ones((3,3))
print(O)
```

---

## Matrix Multiplication

Matrix multiplication is one of the most fundamental operations in linear algebra.

### Rule

If:

```
A shape = (m × n)
B shape = (n × p)
```

Then:

```
A × B → (m × p)
```

---

### Using `np.matmul()`

```python
import numpy as np

A = np.array([
    [1,2],
    [3,4]
])

B = np.array([
    [5,6],
    [7,8]
])

C = np.matmul(A,B)
print(C)
```

**Output**

```
[[19 22]
 [43 50]]
```

---

### Using `@` Operator

```python
C = A @ B
print(C)
```

---

## Dot Product

The dot product is widely used in machine learning and vector mathematics.

### Formula

```
A · B = Σ(Ai × Bi)
```

### Example

```
A = [1,2,3]
B = [4,5,6]

Dot product = 1×4 + 2×5 + 3×6 = 32
```

---

### Using `np.dot()`

```python
import numpy as np

A = np.array([1,2,3])
B = np.array([4,5,6])

result = np.dot(A,B)
print(result)
```

**Output**

```
32
```

---

### Dot Product for Matrices

```python
A = np.array([[1,2],[3,4]])
B = np.array([[5,6],[7,8]])

print(np.dot(A,B))
```

**Output**

```
[[19 22]
 [43 50]]
```

---

## Inner Product

The inner product is similar to the dot product but follows a slightly different definition for higher dimensions.

### Function

```python
np.inner()
```

### Example

```python
import numpy as np

A = np.array([1,2,3])
B = np.array([4,5,6])

print(np.inner(A,B))
```

**Output**

```
32
```

---

### Inner Product with Matrices

```python
A = np.array([[1,2],[3,4]])
B = np.array([[5,6],[7,8]])

print(np.inner(A,B))
```

---

## Outer Product

The outer product produces a matrix from two vectors.

### Formula

```
A ⊗ B
```

If:

```
A = [a,b]
B = [c,d]
```

Then:

```
|a*c  a*d|
|b*c  b*d|
```

---

### Using `np.outer()`

```python
import numpy as np

A = np.array([1,2,3])
B = np.array([4,5])

print(np.outer(A,B))
```

**Output**

```
[[ 4  5]
 [ 8 10]
 [12 15]]
```

---

## Determinant Calculation

The determinant is a scalar value computed from a square matrix.

It helps determine:

* Whether a matrix is invertible
* Volume scaling in linear transformations
* Linear independence

### Formula (2×2)

```
|a  b|
|c  d|

det = ad − bc
```

---

### Using `np.linalg.det()`

```python
import numpy as np

A = np.array([
    [1,2],
    [3,4]
])

det = np.linalg.det(A)
print(det)
```

**Output**

```
-2.0
```

---

## Matrix Transpose

Transpose swaps rows and columns.

If:

```
A shape = (m,n)
```

Then:

```
Aᵀ shape = (n,m)
```

---

### Using `.T`

```python
import numpy as np

A = np.array([
    [1,2,3],
    [4,5,6]
])

print(A.T)
```

**Output**

```
[[1 4]
 [2 5]
 [3 6]]
```

---

## Matrix Inverse

The inverse of a matrix is similar to division in linear algebra.

If:

```
A × A⁻¹ = I
```

Where:

```
I = Identity matrix
```

Condition:

```
det(A) ≠ 0
```

---

### Using `np.linalg.inv()`

```python
import numpy as np

A = np.array([
    [4,7],
    [2,6]
])

inv = np.linalg.inv(A)
print(inv)
```

**Output**

```
[[ 0.6 -0.7]
 [-0.2  0.4]]
```

---

## Solving Linear Equations

Linear equations can be written as:

```
Ax = b
```

Where:

* A = coefficient matrix
* x = unknown variables
* b = result vector

---

### Example System

```
2x + y = 5
x + 3y = 6
```

---

### Using `np.linalg.solve()`

```python
import numpy as np

A = np.array([
    [2,1],
    [1,3]
])

B = np.array([5,6])

x = np.linalg.solve(A,B)
print(x)
```

**Output**

```
[1.8 1.4]
```

---

## Eigenvalues and Eigenvectors

Eigenvalues and eigenvectors describe how a matrix transforms space.

### Definition

```
A v = λ v
```

Where:

* A = matrix
* v = eigenvector
* λ = eigenvalue

---

### Applications

* Machine learning (PCA)
* Quantum physics
* Vibrational analysis
* Stability analysis

---

### Using `np.linalg.eig()`

```python
import numpy as np

A = np.array([
    [2,1],
    [1,2]
])

values, vectors = np.linalg.eig(A)

print("Eigenvalues:")
print(values)

print("Eigenvectors:")
print(vectors)
```

---

## Practical Example: Linear Algebra Workflow

```python
import numpy as np

A = np.array([
    [3,1],
    [2,4]
])

B = np.array([
    [1,2],
    [3,4]
])

print("Matrix multiplication:")
print(A @ B)

print("Determinant:")
print(np.linalg.det(A))

print("Transpose:")
print(A.T)

print("Inverse:")
print(np.linalg.inv(A))

print("Eigenvalues and Eigenvectors:")
print(np.linalg.eig(A))
```

---

## Summary

NumPy provides powerful linear algebra tools through `numpy.linalg`.

### Core Operations

| Operation             | Function             | Example Usage        |
| --------------------- | -------------------- | -------------------- |
| Matrix multiplication | `@` or `np.matmul()` | `C = A @ B`          |
| Dot product           | `np.dot()`           | `d = np.dot(a, b)`   |
| Inner product         | `np.inner()`         | `i = np.inner(a, b)` |
| Outer product         | `np.outer()`         | `o = np.outer(a, b)` |

---

### Matrix Properties

| Operation   | Function          | Example Usage              |
| ----------- | ----------------- | -------------------------- |
| Determinant | `np.linalg.det()` | `d = np.linalg.det(A)`     |
| Transpose   | `.T`              | `A_T = A.T`                |
| Inverse     | `np.linalg.inv()` | `A_inv = np.linalg.inv(A)` |

---

### Solving Problems

| Operation                | Function            | Example Usage               |
| ------------------------ | ------------------- | --------------------------- |
| Solve linear equations   | `np.linalg.solve()` | `x = np.linalg.solve(A, b)` |
| Eigenvalues/Eigenvectors | `np.linalg.eig()`   | `w, v = np.linalg.eig(A)`   |

---

## Why Linear Algebra in NumPy is Important

These operations power many modern technologies including:

* Machine learning algorithms
* Neural networks
* Computer graphics transformations
* Recommendation systems
* Scientific simulations

NumPy provides highly optimized linear algebra operations backed by BLAS and LAPACK libraries, making them extremely fast.


