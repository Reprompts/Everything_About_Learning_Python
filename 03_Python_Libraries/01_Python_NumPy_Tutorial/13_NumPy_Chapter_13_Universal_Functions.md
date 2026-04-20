
# NumPy Hands-On Tutorial

## Chapter 13: Universal Functions

Universal functions, commonly called **ufuncs**, are one of the core features of NumPy. They are specialized functions designed to perform fast, element-wise operations on NumPy arrays.

Unlike normal Python functions that operate on individual values, ufuncs operate on entire arrays at once. They are implemented in highly optimized C code, making them extremely fast and efficient.

Universal functions are heavily used in:
- Scientific computing  
- Data analysis  
- Machine learning  
- Engineering simulations  
- Statistical modeling  

---

## 1. What are Universal Functions

A universal function (ufunc) is a NumPy function that performs element-wise operations on arrays.

Example:
```python
import numpy as np
arr = np.array([1,2,3,4])
result = np.square(arr)
print(result)
````

**Output**

```
[ 1  4  9 16]
```

**Explanation**

* 1² = 1
* 2² = 4
* 3² = 9
* 4² = 16

---

## 2. Vectorized Operations

Vectorization means applying operations to entire arrays without explicit loops.

### Using Python loops

```python
numbers = [1,2,3,4]
result = []
for x in numbers:
    result.append(x*x)
print(result)
```

### NumPy vectorized version

```python
import numpy as np
arr = np.array([1,2,3,4])
result = arr * arr
print(result)
```

**Output**

```
[ 1  4  9 16]
```

### Benefits

* Faster execution
* Less code
* Better memory usage

---

## 3. Mathematical Ufuncs

| Function      | Description    |
| ------------- | -------------- |
| np.add()      | Addition       |
| np.subtract() | Subtraction    |
| np.multiply() | Multiplication |
| np.divide()   | Division       |
| np.power()    | Exponentiation |
| np.sqrt()     | Square root    |
| np.abs()      | Absolute value |

### Example

```python
arr = np.array([1,4,9,16])
print(np.sqrt(arr))
```

**Output**

```
[1. 2. 3. 4.]
```

### Absolute Value

```python
arr = np.array([-5,-3,2,4])
print(np.abs(arr))
```

---

## 4. Trigonometric Functions

| Function    | Description     |
| ----------- | --------------- |
| np.sin()    | Sine            |
| np.cos()    | Cosine          |
| np.tan()    | Tangent         |
| np.arcsin() | Inverse sine    |
| np.arccos() | Inverse cosine  |
| np.arctan() | Inverse tangent |

> ⚠️ NumPy uses **radians**, not degrees.

### Example

```python
angles = np.array([0, np.pi/2, np.pi])
print(np.sin(angles))
```

**Output**

```
[0. 1. 0.]
```

### Degrees to Radians

```python
angles = np.array([0,30,60,90])
radians = np.radians(angles)
print(np.sin(radians))
```

---

## 5. Exponential Functions

| Function   | Description |
| ---------- | ----------- |
| np.exp()   | e^x         |
| np.exp2()  | 2^x         |
| np.power() | x^y         |

### Example

```python
arr = np.array([1,2,3])
print(np.exp(arr))
```

---

## 6. Logarithmic Functions

| Function   | Description |
| ---------- | ----------- |
| np.log()   | Natural log |
| np.log10() | Base 10 log |
| np.log2()  | Base 2 log  |

### Example

```python
arr = np.array([1,10,100])
print(np.log10(arr))
```

**Output**

```
[0. 1. 2.]
```

---

## 7. Comparison Functions

| Function           | Description      |
| ------------------ | ---------------- |
| np.equal()         | Equality         |
| np.not_equal()     | Not equal        |
| np.greater()       | Greater than     |
| np.less()          | Less than        |
| np.greater_equal() | Greater or equal |
| np.less_equal()    | Less or equal    |

### Example

```python
arr = np.array([10,20,30])
print(np.greater(arr, 15))
```

---

## 8. Logical Functions

| Function         | Description |
| ---------------- | ----------- |
| np.logical_and() | AND         |
| np.logical_or()  | OR          |
| np.logical_not() | NOT         |
| np.logical_xor() | XOR         |

### Example

```python
a = np.array([True, False, True])
b = np.array([True, True, False])
print(np.logical_and(a,b))
```

---

## 9. Custom Universal Functions

You can create custom ufuncs using `np.vectorize()`.

### Example

```python
def square(x):
    return x*x

vectorized_square = np.vectorize(square)
arr = np.array([1,2,3,4])
print(vectorized_square(arr))
```

### Temperature Conversion

```python
def celsius_to_fahrenheit(c):
    return (c * 9/5) + 32

convert = np.vectorize(celsius_to_fahrenheit)
temps = np.array([0,20,30])
print(convert(temps))
```

---

## 10. Advantages of Universal Functions

| Benefit           | Explanation                          |
| ----------------- | ------------------------------------ |
| Speed             | Optimized C implementation           |
| Vectorization     | Operate on arrays directly           |
| Memory efficiency | Avoid unnecessary copies             |
| Simplicity        | Replace loops with single operations |
| Scalability       | Handle large datasets efficiently    |

---

## Summary

| Concept               | Description                         |
| --------------------- | ----------------------------------- |
| Universal functions   | Element-wise array operations       |
| Vectorized operations | Replace loops with array operations |
| Mathematical ufuncs   | sqrt, abs, power                    |
| Trigonometric         | sin, cos, tan                       |
| Exponential           | exp, exp2                           |
| Logarithmic           | log, log10, log2                    |
| Comparison            | greater, equal, less                |
| Logical               | logical_and, logical_or             |
| Custom ufuncs         | Create vectorized functions         |

---

Universal functions are essential for:

* Numerical computations
* Scientific simulations
* Machine learning
* Large-scale data processing

They are one of the key reasons NumPy is fast and widely used in data science.


