

---

# NumPy Hands-On Tutorial
# Chapter 1: Introduction

NumPy is one of the most important libraries in the Python scientific computing ecosystem. It forms the backbone of numerical computation in Python and is widely used for working with arrays, mathematical operations, and large datasets.

Almost every major data science and scientific library in Python—such as Pandas, SciPy, TensorFlow, PyTorch, and scikit-learn—relies on NumPy internally. Understanding NumPy is therefore a fundamental step for anyone getting into data science, machine learning, or scientific computing.

---

## 1. What is NumPy?

NumPy is a core Python library designed specifically for numerical computing. It provides efficient tools and data structures that make working with numbers fast and convenient.

It offers:

* A powerful N-dimensional array object
* Fast mathematical operations
* Linear algebra functions
* Random number generation
* Statistical operations
* Broadcasting for efficient computation

### Central Feature: ndarray

At the heart of NumPy is the **NumPy Array (`ndarray`)**, which is a highly optimized data structure for storing and manipulating numerical data.

Unlike regular Python lists, NumPy arrays are:

* Typed (all elements share the same data type)
* Memory efficient
* Vectorized (operations are applied to entire arrays at once)

### Example

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
print(arr)
print(type(arr))
```

**Output:**

```
[1 2 3 4 5]
<class 'numpy.ndarray'>
```

This array object is the foundation of almost all numerical operations in Python.

---

## 2. Why NumPy is Used

NumPy was created to address the limitations of Python lists when dealing with large-scale numerical data. While lists are flexible, they are not optimized for performance-heavy mathematical computations.

### Key Advantages

| Feature            | Description                   |
| ------------------ | ----------------------------- |
| Speed              | Runs in optimized C code      |
| Memory Efficiency  | Uses contiguous memory        |
| Vectorization      | No need for explicit loops    |
| Mathematical Tools | Built-in algebra & statistics |
| Multi-dimensional  | Supports 1D, 2D, 3D arrays    |

---

### Example: List vs NumPy

#### Python List

```python
list1 = [1,2,3]
list2 = [4,5,6]

result = []
for i in range(len(list1)):
    result.append(list1[i] + list2[i])

print(result)
```

**Output:**

```
[5, 7, 9]
```

Here, we manually loop through elements, which becomes inefficient as data grows.

---

#### NumPy Array

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

result = a + b
print(result)
```

**Output:**

```
[5 7 9]
```

No loop is required here. NumPy automatically applies the operation element-wise.

✔ This is known as **vectorization**, and it is one of the main reasons NumPy is so fast.

---

## 3. NumPy in the Python Scientific Ecosystem

NumPy sits at the core of the scientific Python ecosystem and acts as the foundation for many higher-level libraries.

```
Python
   ↓
NumPy
   ↓
SciPy
   ↓
Pandas
   ↓
Matplotlib / Seaborn
   ↓
Machine Learning Libraries
```

### Why Everything Uses NumPy

Libraries are built on top of NumPy because it provides:

* Efficient numerical storage
* Fast mathematical operations
* Memory-optimized data structures

### Example

```python
import pandas as pd
import numpy as np

data = np.array([10,20,30])
series = pd.Series(data)

print(series)
```

Here, the NumPy array is used internally by Pandas to store data efficiently.

---

## 4. Python Lists vs NumPy Arrays

Although Python lists and NumPy arrays may look similar at first glance, they behave very differently under the hood.

### Python Lists

* Can store mixed data types
* Slower for numerical computations
* Stores references to objects rather than raw data

```python
my_list = [1, "hello", 3.14, True]
```

---

### NumPy Arrays

* Store a single data type
* Much faster for mathematical operations
* Stored in contiguous memory blocks

```python
import numpy as np

arr = np.array([1,2,3,4])
```

---

### Memory Layout

**Python List:**

```
Pointer → Object
Pointer → Object
Pointer → Object
```

**NumPy Array:**

```
[1][2][3][4][5]
```

This contiguous memory layout is a key reason why NumPy performs so efficiently.

---

### Performance Example

```python
import numpy as np
import time

size = 1000000

list_data = list(range(size))
array_data = np.arange(size)

start = time.time()
result = [x * 2 for x in list_data]
print("List time:", time.time() - start)

start = time.time()
result = array_data * 2
print("NumPy time:", time.time() - start)
```

In most cases, NumPy will be **10–100 times faster** than standard Python lists.

---

## 5. Relationship with Pandas and SciPy

NumPy acts as the foundation for many other libraries, especially Pandas and SciPy.

---

### NumPy + Pandas

Pandas is mainly used for:

* Tabular data handling
* Data cleaning
* Data analysis

However, internally, Pandas relies heavily on NumPy arrays.

```python
import pandas as pd
import numpy as np

data = np.array([10,20,30])
series = pd.Series(data)
```

---

### NumPy + SciPy

SciPy builds on NumPy and provides advanced scientific algorithms such as:

* Optimization
* Signal processing
* Integration
* Linear algebra
* Statistics

```python
from scipy import linalg
import numpy as np

A = np.array([[1,2],[3,4]])
inverse = linalg.inv(A)

print(inverse)
```

---

### Ecosystem Summary

| Library      | Role                 |
| ------------ | -------------------- |
| NumPy        | Numerical arrays     |
| Pandas       | Data analysis        |
| SciPy        | Scientific computing |
| Matplotlib   | Visualization        |
| Scikit-learn | Machine learning     |

NumPy sits right at the center of this ecosystem.

---

## 6. Advantages of NumPy Arrays

### 1. High Performance

NumPy operations are executed in optimized C code, making them significantly faster than Python loops.

```python
import numpy as np

arr = np.array([1,2,3,4])
print(arr * 2)
```

**Output:**

```
[2 4 6 8]
```

---

### 2. Vectorization

Operations are applied to the entire array at once.

```python
arr = np.array([1,2,3,4])
print(arr + 10)
```

**Output:**

```
[11 12 13 14]
```

---

### 3. Multi-Dimensional Arrays

NumPy supports multiple dimensions:

* 1D → vectors
* 2D → matrices
* 3D+ → tensors

```python
matrix = np.array([
    [1,2,3],
    [4,5,6]
])

print(matrix)
```

---

### 4. Broadcasting

Broadcasting allows operations between arrays of different shapes without explicit loops.

```python
arr = np.array([1,2,3])
print(arr + 10)
```

---

### 5. Mathematical Functions

NumPy provides a wide range of built-in mathematical functions:

```python
np.sum()
np.mean()
np.std()
np.sqrt()
np.exp()
np.log()
np.dot()
```

---

## 7. Installing NumPy

### Using pip

```bash
pip install numpy
```

### Specific Version

```bash
pip install numpy==1.26.4
```

### Using Conda

```bash
conda install numpy
```

---

### Verify Installation

```python
import numpy
```

If no error appears, NumPy has been installed successfully.

---

## 8. Importing NumPy

```python
import numpy as np
```

Using `np` as an alias is a standard convention followed across the Python community, making code shorter and easier to read.

---

## 9. Checking NumPy Version

### Method 1

```python
import numpy as np
print(np.__version__)
```

### Method 2 (Terminal)

```bash
pip show numpy
```

---

## 10. Setting Up the Environment

### Option 1: Python Interpreter

```bash
python
```

```python
import numpy as np
```

---

### Option 2: Jupyter Notebook (Recommended)

```bash
pip install notebook
jupyter notebook
```

```python
import numpy as np
arr = np.array([1,2,3])
arr
```

---

### Option 3: VS Code

* Install Python extension
* Install Jupyter extension
* Create a `.py` file

```python
import numpy as np

data = np.arange(10)
print(data)
```

---

### Option 4: Virtual Environment (Best Practice)

```bash
python -m venv numpy_env
```

#### Activate

**Windows**

```bash
numpy_env\Scripts\activate
```

**Linux/Mac**

```bash
source numpy_env/bin/activate
```

#### Install NumPy

```bash
pip install numpy
```

---

## Practical Example

```python
import numpy as np

print("NumPy Version:", np.__version__)

data = np.array([10,20,30,40])

print("Array:", data)
print("Sum:", np.sum(data))
print("Mean:", np.mean(data))
```

**Output:**

```
NumPy Version: 1.26.4
Array: [10 20 30 40]
Sum: 100
Mean: 25
```

---

## Summary

NumPy is the foundation of numerical computing in Python and a must-know library for anyone working with data.

### Key Takeaways

* Provides fast multidimensional arrays
* Enables vectorized operations
* Forms the base of Pandas, SciPy, and ML libraries
* Much faster than Python lists for numerical work
* Easy to install and widely used
* Essential for Data Science, AI, and Scientific Computing

---

**Thanks for reading! **
