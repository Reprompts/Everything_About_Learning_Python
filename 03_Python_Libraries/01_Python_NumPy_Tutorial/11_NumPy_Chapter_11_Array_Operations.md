
# Python Numpy: Part 11 (Array Operations)

NumPy arrays are designed to perform mathematical operations efficiently on entire datasets. Unlike Python lists, NumPy allows **vectorized operations**, meaning operations are applied to all elements of an array simultaneously.

This capability is essential for:
- Scientific computing  
- Data analysis  
- Machine learning  
- Image processing  
- Numerical simulations  

NumPy performs operations using optimized C implementations, making computations significantly faster than traditional Python loops.

This article covers:
- Element-wise operations  
- Array arithmetic operations  
- Addition  
- Subtraction  
- Multiplication  
- Division  
- Exponentiation  
- Modulus operations  
- Broadcasted operations  

---

## 1. Element-wise Operations

In NumPy, most operations are performed element by element.

Example:
```python
import numpy as np
a = np.array([1,2,3])
b = np.array([4,5,6])
result = a + b
print(result)
````

**Output**

```
[5 7 9]
```

**Explanation**

* 1 + 4 = 5
* 2 + 5 = 7
* 3 + 6 = 9

---

## 2. Array Arithmetic Operations

NumPy supports all basic arithmetic operations:

| Operation      | Operator |
| -------------- | -------- |
| Addition       | +        |
| Subtraction    | -        |
| Multiplication | *        |
| Division       | /        |
| Exponentiation | **       |
| Modulus        | %        |

---

## 3. Addition

### Array + Array

```python
a = np.array([10,20,30])
b = np.array([1,2,3])
print(a + b)
```

**Output**

```
[11 22 33]
```

### Array + Scalar

```python
a = np.array([5,10,15])
print(a + 10)
```

**Output**

```
[15 20 25]
```

---

## 4. Subtraction

```python
a = np.array([20,30,40])
b = np.array([5,10,15])
print(a - b)
```

**Output**

```
[15 20 25]
```

### Scalar Subtraction

```python
a = np.array([50,60,70])
print(a - 10)
```

---

## 5. Multiplication

```python
a = np.array([2,3,4])
b = np.array([5,6,7])
print(a * b)
```

**Output**

```
[10 18 28]
```

> ⚠️ `*` is element-wise multiplication, not matrix multiplication.

### Scalar Multiplication

```python
a = np.array([1,2,3])
print(a * 5)
```

---

## 6. Division

```python
a = np.array([10,20,30])
b = np.array([2,4,5])
print(a / b)
```

**Output**

```
[5. 5. 6.]
```

### Scalar Division

```python
a = np.array([100,200,300])
print(a / 10)
```

---

## 7. Exponentiation

```python
a = np.array([2,3,4])
print(a ** 2)
```

**Output**

```
[ 4  9 16]
```

### Array Exponentiation

```python
a = np.array([2,3,4])
b = np.array([3,2,1])
print(a ** b)
```

---

## 8. Modulus Operations

```python
a = np.array([10,20,30])
b = np.array([3,7,4])
print(a % b)
```

**Output**

```
[1 6 2]
```

### Scalar Modulus

```python
a = np.array([10,15,20,25])
print(a % 2)
```

---

## 9. Broadcasted Operations

Broadcasting allows operations between arrays of different shapes.

### Scalar Broadcasting

```python
a = np.array([1,2,3])
print(a + 10)
```

### Vector Broadcasting

```python
a = np.array([
    [1,2,3],
    [4,5,6]
])
b = np.array([10,20,30])
print(a + b)
```

**Output**

```
[[11 22 33]
 [14 25 36]]
```

---

## 10. Practical Example

```python
prices = np.array([100,200,300])
tax_rate = 0.18
final_price = prices + prices * tax_rate
print(final_price)
```

**Output**

```
[118. 236. 354.]
```

---

## Summary

| Concept          | Description                                  |
| ---------------- | -------------------------------------------- |
| Element-wise ops | Operations applied to corresponding elements |
| Addition         | Combine arrays or scalars                    |
| Subtraction      | Compute differences                          |
| Multiplication   | Element-wise product                         |
| Division         | Element-wise division                        |
| Exponentiation   | Power operations                             |
| Modulus          | Remainder calculation                        |
| Broadcasting     | Automatic shape alignment                    |

---

NumPy operations are:

* Fast
* Efficient
* Vectorized

They form the foundation for:

* Machine learning
* Data science
* Scientific computing

```
```
