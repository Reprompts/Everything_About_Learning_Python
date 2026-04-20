

# NumPy Hands-On Tutorial  

## Chapter 14: Mathematical Functions

NumPy provides a large collection of high-performance mathematical functions designed to operate on NumPy arrays efficiently. These functions are implemented internally using vectorized operations, meaning they apply the mathematical operation to every element in the array automatically, without requiring loops.

### Mathematical functions are widely used in:
- Scientific computing  
- Data analysis  
- Machine learning  
- Engineering simulations  
- Statistical modeling  
- Signal processing  

In NumPy, mathematical functions belong to the **Universal Functions (ufuncs)** family, meaning they perform fast element-wise operations on arrays.

---

## 1. Basic Arithmetic Functions

NumPy provides several functions to perform basic arithmetic operations on arrays.

These operations can be performed using:
- Mathematical operators (`+`, `-`, `*`, `/`)
- NumPy arithmetic functions

### Common Arithmetic Functions

| Function        | Operation       |
|----------------|----------------|
| `np.add()`      | Addition       |
| `np.subtract()` | Subtraction    |
| `np.multiply()` | Multiplication |
| `np.divide()`   | Division       |
| `np.mod()`      | Modulus        |
| `np.remainder()`| Remainder      |

### Example: Addition
```python
import numpy as np
a = np.array([10, 20, 30])
b = np.array([1, 2, 3])
result = np.add(a, b)
print(result)
````

**Output**

```
[11 22 33]
```

### Explanation

* 10 + 1 = 11
* 20 + 2 = 22
* 30 + 3 = 33

### Example: Multiplication

```python
a = np.array([2,4,6])
b = np.array([3,5,7])
print(np.multiply(a,b))
```

**Output**

```
[ 6 20 42]
```

### Example: Division

```python
a = np.array([10,20,30])
print(np.divide(a,5))
```

**Output**

```
[2. 4. 6.]
```

---

## 2. Power Operations

Power functions raise numbers to a given exponent.

### Common Power Functions

| Function           | Description             |
| ------------------ | ----------------------- |
| `np.power()`       | Raise values to a power |
| `np.square()`      | Square of elements      |
| `np.cbrt()`        | Cube root               |
| `np.float_power()` | Floating power          |

### Example: Power

```python
import numpy as np
arr = np.array([2,3,4])
print(np.power(arr,2))
```

**Output**

```
[ 4  9 16]
```

### Example: Square

```python
arr = np.array([5,6,7])
print(np.square(arr))
```

**Output**

```
[25 36 49]
```

---

## 3. Square Root

Square root functions compute the root of each element.

### Function

* `np.sqrt()`

### Example

```python
import numpy as np
arr = np.array([1,4,9,16])
print(np.sqrt(arr))
```

**Output**

```
[1. 2. 3. 4.]
```

### Explanation

* √1 = 1
* √4 = 2
* √9 = 3
* √16 = 4

---

## 4. Absolute Values

Absolute value functions return the non-negative magnitude of numbers.

### Functions

| Function        | Description    |
| --------------- | -------------- |
| `np.abs()`      | Absolute value |
| `np.absolute()` | Same as abs    |

### Example

```python
import numpy as np
arr = np.array([-10,-5,3,8])
print(np.abs(arr))
```

**Output**

```
[10 5 3 8]
```

### Explanation

* |-10| = 10
* |-5| = 5
* |3| = 3
* |8| = 8

---

## 5. Rounding Operations

Rounding functions adjust numbers to the nearest integers or decimals.

### Common Rounding Functions

| Function     | Description            |
| ------------ | ---------------------- |
| `np.round()` | Round to nearest value |
| `np.floor()` | Round down             |
| `np.ceil()`  | Round up               |
| `np.trunc()` | Remove decimal part    |

### Example

```python
import numpy as np
arr = np.array([1.4, 2.7, 3.5])
print(np.round(arr))
```

**Output**

```
[1. 3. 4.]
```

### Example: Floor

```python
arr = np.array([1.9,2.1,3.8])
print(np.floor(arr))
```

**Output**

```
[1. 2. 3.]
```

### Example: Ceiling

```python
print(np.ceil(arr))
```

**Output**

```
[2. 3. 4.]
```

---

## 6. Trigonometric Operations

NumPy provides functions for trigonometric calculations.

### Used in:

* Physics
* Engineering
* Signal processing
* Graphics
* Robotics

**Important:** NumPy trigonometric functions use radians, not degrees.

### Common Trigonometric Functions

| Function      | Description     |
| ------------- | --------------- |
| `np.sin()`    | Sine            |
| `np.cos()`    | Cosine          |
| `np.tan()`    | Tangent         |
| `np.arcsin()` | Inverse sine    |
| `np.arccos()` | Inverse cosine  |
| `np.arctan()` | Inverse tangent |

### Example

```python
import numpy as np
angles = np.array([0, np.pi/2, np.pi])
print(np.sin(angles))
```

**Output**

```
[0. 1. 0.]
```

### Converting Degrees to Radians

```python
angles = np.array([0,30,60,90])
radians = np.radians(angles)
print(np.sin(radians))
```

**Output**

```
[0.        0.5       0.8660254 1.       ]
```

---

## 7. Hyperbolic Functions

### Used in:

* Physics
* Differential equations
* Neural networks
* Relativity calculations

### Common Hyperbolic Functions

| Function       | Description                |
| -------------- | -------------------------- |
| `np.sinh()`    | Hyperbolic sine            |
| `np.cosh()`    | Hyperbolic cosine          |
| `np.tanh()`    | Hyperbolic tangent         |
| `np.arcsinh()` | Inverse hyperbolic sine    |
| `np.arccosh()` | Inverse hyperbolic cosine  |
| `np.arctanh()` | Inverse hyperbolic tangent |

### Example

```python
import numpy as np
arr = np.array([0,1,2])
print(np.sinh(arr))
```

**Output**

```
[0.         1.17520119 3.62686041]
```

---

## 8. Logarithmic Functions

### Used in:

* Data science
* Information theory
* Statistics
* Machine learning

### Common Logarithmic Functions

| Function     | Description |
| ------------ | ----------- |
| `np.log()`   | Natural log |
| `np.log10()` | Base 10 log |
| `np.log2()`  | Base 2 log  |
| `np.log1p()` | log(1+x)    |

### Example

```python
import numpy as np
arr = np.array([1,10,100])
print(np.log10(arr))
```

**Output**

```
[0. 1. 2.]
```

### Explanation

* log10(1) = 0
* log10(10) = 1
* log10(100) = 2

---

## 9. Exponential Functions

### Used in:

* Growth models
* Neural networks
* Financial modeling
* Probability distributions

### Common Exponential Functions

| Function     | Description |
| ------------ | ----------- |
| `np.exp()`   | e^x         |
| `np.exp2()`  | 2^x         |
| `np.expm1()` | e^x − 1     |

### Example

```python
import numpy as np
arr = np.array([1,2,3])
print(np.exp(arr))
```

**Output**

```
[ 2.71828183  7.3890561  20.08553692]
```

### Explanation

* e¹ ≈ 2.718
* e² ≈ 7.389
* e³ ≈ 20.085

---

## Practical Example Combining Multiple Functions

```python
import numpy as np
data = np.array([4,9,16,25])
sqrt_values = np.sqrt(data)
log_values = np.log(data)
exp_values = np.exp(data)

print("Square Root:", sqrt_values)
print("Log:", log_values)
print("Exponential:", exp_values)
```

**Output**

```
Square Root: [2. 3. 4. 5.]
Log: [1.38629436 2.19722458 2.77258872 3.21887582]
Exponential: [5.45981500e+01 ...]
```

---

## Summary

NumPy provides a powerful set of mathematical functions that operate efficiently on arrays.

### Major Categories

| Category         | Examples                |
| ---------------- | ----------------------- |
| Basic arithmetic | add, subtract, multiply |
| Power functions  | power, square           |
| Roots            | sqrt                    |
| Absolute values  | abs                     |
| Rounding         | round, floor, ceil      |
| Trigonometric    | sin, cos, tan           |
| Hyperbolic       | sinh, cosh, tanh        |
| Logarithmic      | log, log10, log2        |
| Exponential      | exp, exp2               |

### Key Advantages

* Fast vectorized computations
* Efficient memory usage
* Clean and readable code
* Ideal for large datasets

---

These mathematical functions form the foundation of numerical computing in NumPy.

