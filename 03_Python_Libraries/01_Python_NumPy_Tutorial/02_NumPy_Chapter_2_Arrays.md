

---

# NumPy Hands-On Tutorial
# NumPy Chapter 2: Arrays

The NumPy array (`ndarray`) is the core data structure of the NumPy library. Almost everything in NumPy revolves around creating, manipulating, and performing computations on arrays.

In real-world applications like data science, machine learning, and scientific computing, data is almost always represented using NumPy arrays. This is because they are:

* Fast
* Memory efficient
* Optimized for mathematical operations
* Capable of handling multi-dimensional data

This article takes a detailed look at NumPy arrays, covering how they are created, their attributes, memory structure, and how to inspect them in practical scenarios.

---

## 1. What is an ndarray

An **ndarray (N-Dimensional Array)** is a homogeneous container of elements arranged in a grid-like structure.

### Key Characteristics

* Stores elements of the same data type
* Supports multiple dimensions
* Stored in contiguous memory
* Enables vectorized operations

---

### Conceptual View

#### Example 1D Array

```
[10, 20, 30, 40]
```

#### Example 2D Array (Matrix)

```
[
 [1, 2, 3],
 [4, 5, 6]
]
```

#### Example 3D Array

```
[
 [[1,2],[3,4]],
 [[5,6],[7,8]]
]
```

---

### Creating a Simple ndarray

```python
import numpy as np

arr = np.array([10, 20, 30, 40])
print(arr)
print(type(arr))
```

**Output**

```
[10 20 30 40]
<class 'numpy.ndarray'>
```

This object is the fundamental building block of NumPy.

---

## 2. Creating Arrays

NumPy provides multiple ways to create arrays depending on the situation.

---

### 2.1 Creating Array from Python List

The most common method is using `np.array()`.

```python
import numpy as np

data = [1, 2, 3, 4]
arr = np.array(data)

print(arr)
```

**Output**

```
[1 2 3 4]
```

---

### 2.2 Creating 2D Arrays

```python
matrix = np.array([
    [1,2,3],
    [4,5,6]
])

print(matrix)
```

**Output**

```
[[1 2 3]
 [4 5 6]]
```

---

### 2.3 Creating Arrays with Built-in Functions

NumPy includes several built-in functions for generating arrays quickly.

#### Zeros Array

```python
arr = np.zeros(5)
print(arr)
```

**Output**

```
[0. 0. 0. 0. 0.]
```

---

#### Ones Array

```python
arr = np.ones(4)
print(arr)
```

**Output**

```
[1. 1. 1. 1.]
```

---

#### Identity Matrix

```python
arr = np.eye(3)
print(arr)
```

**Output**

```
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
```

---

#### Range Array

```python
arr = np.arange(0, 10)
print(arr)
```

**Output**

```
[0 1 2 3 4 5 6 7 8 9]
```

---

#### Linearly Spaced Numbers

```python
arr = np.linspace(0, 1, 5)
print(arr)
```

**Output**

```
[0.   0.25 0.5  0.75 1.  ]
```

---

#### Random Arrays

```python
arr = np.random.rand(3,3)
print(arr)
```

**Example Output**

```
[[0.45 0.73 0.11]
 [0.92 0.61 0.30]
 [0.28 0.54 0.88]]
```

---

## 3. Array Attributes

NumPy arrays carry useful metadata that describe their structure and memory usage.

### Important Attributes

| Attribute | Meaning                     |
| --------- | --------------------------- |
| ndim      | Number of dimensions        |
| shape     | Structure of array          |
| size      | Total number of elements    |
| dtype     | Data type of elements       |
| itemsize  | Memory used by each element |
| nbytes    | Total memory consumed       |

---

## 4. Array Properties

Array properties help us understand how an ndarray is structured.

### Example

```python
import numpy as np

arr = np.array([[1,2,3],[4,5,6]])

print(arr.ndim)
print(arr.shape)
print(arr.size)
print(arr.dtype)
```

**Output**

```
2
(2, 3)
6
int64
```

### Explanation

* 2 dimensions
* Shape: 2 rows and 3 columns
* Total elements: 6
* Data type: integer

---

## 5. Array Dimensions

The dimension (`ndim`) represents the number of axes in an array.

| Dimension | Description |
| --------- | ----------- |
| 0D        | Scalar      |
| 1D        | Vector      |
| 2D        | Matrix      |
| 3D        | Tensor      |

---

### Example

```python
scalar = np.array(5)
vector = np.array([1,2,3])
matrix = np.array([[1,2],[3,4]])
tensor = np.array([
    [[1,2],[3,4]],
    [[5,6],[7,8]]
])
```

```python
print(vector.ndim)
print(matrix.ndim)
print(tensor.ndim)
```

**Output**

```
1
2
3
```

---

## 6. Array Shape

The shape defines the structure of the array and is returned as a tuple.

### Example

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr.shape)
```

**Output**

```
(2, 3)
```

**Meaning:**

* 2 rows
* 3 columns

---

### 3D Example

```python
arr = np.array([
    [[1,2],[3,4]],
    [[5,6],[7,8]]
])

print(arr.shape)
```

**Output**

```
(2, 2, 2)
```

**Meaning**

* 2 matrices
* 2 rows
* 2 columns

---

## 7. Array Size

The `size` property gives the total number of elements.

### Formula

```
size = product of shape dimensions
```

### Example

```
shape = (2,3)
size = 2 × 3 = 6
```

### Practical Example

```python
arr = np.array([
    [10,20,30],
    [40,50,60]
])

print(arr.size)
```

**Output**

```
6
```

---

## 8. Array Data Types (dtype)

NumPy arrays store elements of a single data type.

### Common Data Types

| Type    | Description     |
| ------- | --------------- |
| int32   | 32-bit integer  |
| int64   | 64-bit integer  |
| float32 | 32-bit floating |
| float64 | 64-bit floating |
| bool    | Boolean         |
| complex | Complex numbers |

---

### Checking Data Type

```python
arr = np.array([1,2,3])
print(arr.dtype)
```

**Output**

```
int64
```

---

### Specifying Data Type

```python
arr = np.array([1,2,3], dtype=float)

print(arr)
print(arr.dtype)
```

**Output**

```
[1. 2. 3.]
float64
```

---

### Mixed Data Conversion

```python
arr = np.array([1, 2.5, 3])

print(arr)
print(arr.dtype)
```

**Output**

```
[1.  2.5 3. ]
float64
```

NumPy automatically promotes data types to maintain consistency.

---

## 9. Array Memory Layout

NumPy arrays are stored in contiguous memory blocks, which is a major reason for their speed.

---

### Python List Memory

```
Pointer → Object
Pointer → Object
Pointer → Object
```

Each element is stored separately.

---

### NumPy Memory Layout

```
|10|20|30|40|50|
```

All elements are stored together in a single continuous block.

---

### Advantages

* Faster access
* Better CPU cache usage
* Efficient vectorized computation

---

### Memory Properties

```python
arr = np.array([10,20,30,40])

print(arr.itemsize)
print(arr.nbytes)
```

**Output**

```
8
32
```

**Explanation**

* `itemsize` = 8 bytes per element
* `nbytes` = total memory used (8 × 4)

---

## 10. Viewing Array Information

NumPy provides several ways to inspect arrays.

---

### 10.1 Using Built-in Attributes

```python
import numpy as np

arr = np.array([[1,2,3],[4,5,6]])

print("Array:")
print(arr)

print("Dimensions:", arr.ndim)
print("Shape:", arr.shape)
print("Size:", arr.size)
print("Data type:", arr.dtype)
print("Item size:", arr.itemsize)
print("Total bytes:", arr.nbytes)
```

---

### Output Example

```
Array:
[[1 2 3]
 [4 5 6]]
Dimensions: 2
Shape: (2, 3)
Size: 6
Data type: int64
Item size: 8
Total bytes: 48
```

---

### 10.2 Using np.info()

```python
np.info(np.ndarray)
```

Displays documentation for ndarray.

---

### 10.3 Using dir()

```python
dir(arr)
```

Lists all available attributes and methods.

---

### 10.4 Using type()

```python
type(arr)
```

**Output**

```
<class 'numpy.ndarray'>
```

---

### 10.5 Checking Memory Flags

```python
arr.flags
```

**Example Output**

```
C_CONTIGUOUS : True
F_CONTIGUOUS : False
OWNDATA : True
WRITEABLE : True
```

These flags describe how the array is stored and managed in memory.

---

## Practical Example: Complete Array Inspection

```python
import numpy as np

data = np.array([
    [10,20,30],
    [40,50,60]
])

print("Array:")
print(data)

print("\nArray Info")
print("Dimensions:", data.ndim)
print("Shape:", data.shape)
print("Size:", data.size)
print("Data Type:", data.dtype)
print("Bytes per Element:", data.itemsize)
print("Total Memory:", data.nbytes)
```

---

### Output

```
Array:
[[10 20 30]
 [40 50 60]]

Array Info
Dimensions: 2
Shape: (2,3)
Size: 6
Data Type: int64
Bytes per Element: 8
Total Memory: 48
```

---

## Summary

NumPy arrays (`ndarray`) are the core structure for numerical computation in Python.

### Key Concepts

* `ndarray` represents multi-dimensional homogeneous data
* Arrays can be created from lists, ranges, or generators
* Important attributes include:

  * shape
  * size
  * dtype
  * ndim
* Arrays are stored in contiguous memory
* NumPy enables fast, vectorized computations

A solid understanding of ndarray is essential, because every advanced NumPy operation builds on this foundation.

---

## Practice Examples

1. Create a NumPy array with values `[10, 20, 30, 40, 50]`. Print the array and its data type.

2. Create two NumPy arrays `[1,2,3]` and `[4,5,6]`. Perform element-wise addition and print the result.

3. Create a NumPy array from 1 to 10 using a NumPy function (not a Python list). Print the array.

4. Multiply all elements of a NumPy array `[2,4,6,8]` by 5 and print the output.

5. Create a 2D NumPy array:

   ```
   [[1,2,3],
    [4,5,6]]
   ```

   Print:

   * The array
   * Its shape

6. Create a NumPy array `[5,10,15,20]` and calculate:

   * Sum
   * Mean

7. Create a NumPy array `[1,2,3,4]` and add 10 to each element using broadcasting.

8. Create a NumPy array of size 5 with all elements as 0, then change it to all 1s.

9. Create a NumPy array `[1,2,3,4,5]` and compute:

   * Square of each element
   * Square root of each element

10. Create two arrays:

```
a = [1,2,3]
b = [10,20,30]
```

Perform element-wise multiplication and print the result.

11. Create:

* A scalar (0D) with value 100
* A 1D array `[1, 2, 3]`
* A 2D array `[[1,2],[3,4]]`

Print the number of dimensions (`ndim`) for each.

12. Create a 2D NumPy array:

```
[[10, 20, 30],
 [40, 50, 60]]
```

Print:

* Shape
* Total size

Verify:

```
size = rows × columns
```

13. Create an array using `np.arange()` from 5 to 15:

* Print the array
* Print its dtype
* Convert it to float and print again

14. Create a NumPy array using `np.linspace()` from 0 to 5 with 6 elements:

* Print the array
* Print its shape and ndim

15. Create a 3×3 matrix of zeros
    Then:

* Convert it into a matrix of ones
* Print both arrays

16. Generate a 2×2 random array:

* Print the array
* Print dtype, itemsize, and nbytes

17. Create an array:

```
[1, 2, 3, 4]
```

Print:

* itemsize
* nbytes

Verify:

```
nbytes = itemsize × number of elements
```

18. Create an array:

```
[1, 2.5, 3]
```

* Print the array
* Print dtype
* Explain dtype choice

19. Create a 3D array:

```
[
 [[1,2],[3,4]],
 [[5,6],[7,8]]
]
```

Print:

* ndim
* shape
* size

20. Create any 2D NumPy array and perform complete inspection:
    Print:

* Array
* ndim
* shape
* size
* dtype
* itemsize
* nbytes
* flags

---

Thanks for reading. For more such articles, follow RePromptsQuest.
