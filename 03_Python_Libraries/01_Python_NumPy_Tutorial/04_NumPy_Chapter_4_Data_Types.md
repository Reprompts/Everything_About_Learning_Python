# NumPy Hands-On Tutorial

## Chapter 4: Data Types

One of the most important features of NumPy arrays is that they are **homogeneous**, meaning all elements in a NumPy array must have the same data type. This design enables fast numerical computation, efficient memory usage, and optimized CPU processing.

NumPy provides a rich system of data types (`dtype`) that represent different kinds of numbers and objects.

Understanding NumPy data types is essential because they influence:

* Memory consumption
* Performance
* Numerical precision
* Compatibility with scientific algorithms

---

## 1. Understanding NumPy Data Types

In NumPy, the data type of array elements is represented using the `dtype` attribute.

`dtype` determines:

* Type of data stored
* Memory used per element
* Range of values allowed
* Precision of numbers

### Example

```python
import numpy as np
arr = np.array([1, 2, 3, 4])
print(arr.dtype)
```

### Output

```
int64
```

This means each element is stored as a 64-bit integer.

### Why NumPy Uses Fixed Data Types

**Python lists allow mixed types:**

```python
data = [1, 3.5, "hello", True]
```

**NumPy arrays require consistent data types:**

```python
arr = np.array([1, 2, 3, 4])
```

### Advantages

* Faster computation
* Efficient memory layout
* Vectorized mathematical operations

---

## 2. Specifying Data Types When Creating Arrays

You can explicitly specify the data type using the `dtype` parameter.

### Syntax

```python
np.array(data, dtype=type)
```

### Example: Integer Array

```python
arr = np.array([1,2,3,4], dtype=np.int32)
print(arr)
print(arr.dtype)
```

### Output

```
[1 2 3 4]
int32
```

### Example: Float Array

```python
arr = np.array([1,2,3], dtype=float)
print(arr)
print(arr.dtype)
```

### Output

```
[1. 2. 3.]
float64
```

### Why Specify Data Types?

It helps control:

* Memory usage
* Numerical precision
* Performance

**Example:**

* int8 → 1 byte
* int64 → 8 bytes

---

## 3. Integer Data Types

NumPy provides several integer types with different memory sizes.

| Data Type | Size    | Range             |
| --------- | ------- | ----------------- |
| int8      | 1 byte  | -128 to 127       |
| int16     | 2 bytes | -32,768 to 32,767 |
| int32     | 4 bytes | Large range       |
| int64     | 8 bytes | Very large range  |

### Example

```python
arr = np.array([1,2,3,4], dtype=np.int16)
print(arr)
print(arr.dtype)
```

### Checking Memory Usage

```python
arr = np.array([1,2,3], dtype=np.int16)
print(arr.itemsize)
```

### Output

```
2
```

---

## 4. Floating Point Data Types

Floating point types store decimal numbers.

| Data Type | Size    | Precision |
| --------- | ------- | --------- |
| float16   | 2 bytes | Low       |
| float32   | 4 bytes | Medium    |
| float64   | 8 bytes | High      |

### Example

```python
arr = np.array([1.5, 2.3, 3.8], dtype=np.float32)
print(arr)
print(arr.dtype)
```

### Default Float Type

```python
arr = np.array([1.5, 2.7])
print(arr.dtype)
```

---

## 5. Boolean Data Types

Boolean arrays store `True` / `False` values.

### Example

```python
arr = np.array([True, False, True])
print(arr)
print(arr.dtype)
```

### Boolean from Conditions

```python
arr = np.array([10,20,30,40])
result = arr > 25
print(result)
```

### Practical Use

* Filtering data
* Masking elements
* Conditional operations

```python
arr[arr > 25]
```

---

## 6. Complex Numbers

NumPy supports complex number data types.

A complex number:

```
real + imaginary part
```

### Example

```python
arr = np.array([1+2j, 3+4j, 5+6j])
print(arr)
print(arr.dtype)
```

### Access Parts

```python
arr = np.array([1+2j, 3+4j])
print(arr.real)
print(arr.imag)
```

### Use Cases

* Signal processing
* Electrical engineering
* Quantum physics
* Fourier transforms

---

## 7. Object Data Types

NumPy can store Python objects using `dtype=object`.

### Example

```python
arr = np.array([1, "hello", 3.5], dtype=object)
print(arr)
print(arr.dtype)
```

### When to Use

* Strings
* Lists
* Custom Python objects

### Note

Object arrays lose most NumPy performance benefits.

---

## 8. Changing Data Types with `astype()`

### Syntax

```python
array.astype(new_type)
```

### Convert Integer to Float

```python
arr = np.array([1,2,3,4])
new_arr = arr.astype(float)
print(new_arr)
print(new_arr.dtype)
```

### Convert Float to Integer

```python
arr = np.array([1.7, 2.8, 3.9])
new_arr = arr.astype(int)
print(new_arr)
```

**Note:** Values are truncated, not rounded.

### Convert to Boolean

```python
arr = np.array([0,1,2,0,3])
bool_arr = arr.astype(bool)
print(bool_arr)
```

### Rule

* 0 → False
* Non-zero → True

### Convert to String

```python
arr = np.array([1,2,3])
str_arr = arr.astype(str)
print(str_arr)
print(str_arr.dtype)
```

---

## Practical Example

```python
data = np.array([1,2,3,4])
print("Original dtype:", data.dtype)

float_data = data.astype(float)
print("Float array:", float_data)
print("New dtype:", float_data.dtype)
```

---

## Summary

NumPy data types control how data is stored and processed in arrays.

| Data Type | Purpose           |
| --------- | ----------------- |
| Integer   | Whole numbers     |
| Float     | Decimal numbers   |
| Boolean   | True/False values |
| Complex   | Complex numbers   |
| Object    | Python objects    |

### Key Operations

* Check type using `dtype`
* Specify type using `dtype=`
* Convert type using `astype()`

---

## Conclusion

Choosing the correct data type helps optimize:

* Memory usage
* Computation speed
* Numerical precision

---
