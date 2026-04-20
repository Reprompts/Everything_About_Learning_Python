

---

# NumPy (Numerical Python) — Complete Guide

NumPy is one of the most important libraries in the Python scientific computing ecosystem. It provides powerful tools for numerical computation, array manipulation, and mathematical operations.

Almost every major scientific or data science library in Python such as Pandas, SciPy, TensorFlow, PyTorch, and scikit-learn is built on top of NumPy.

---

## 1. What is NumPy?

NumPy is a core Python library for numerical computing that provides:

* A powerful N-dimensional array object
* Fast mathematical operations
* Linear algebra functions
* Random number generation
* Statistical operations
* Broadcasting for efficient computation

### Central Feature: ndarray

The core of NumPy is the **NumPy Array (`ndarray`)**.

Unlike Python lists, NumPy arrays are:

* Typed
* Memory efficient
* Vectorized (fast operations)

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

---

## 2. Why NumPy is Used

NumPy was created to overcome limitations of Python lists for large numerical data.

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

✔ No loops needed → **Vectorization**

---

## 3. NumPy in the Python Scientific Ecosystem

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

* Efficient data storage
* Fast operations
* Memory optimization

### Example

```python
import pandas as pd
import numpy as np

data = np.array([10,20,30])
series = pd.Series(data)

print(series)
```

✔ Pandas internally uses NumPy arrays

---

## 4. Python Lists vs NumPy Arrays

### Python Lists

* Can store mixed data types
* Slower for math operations
* Stores references (objects)

```python
my_list = [1, "hello", 3.14, True]
```

### NumPy Arrays

* Single data type
* Faster computation
* Stored in contiguous memory

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

✔ NumPy is typically **10–100x faster**

---

## 5. Relationship with Pandas and SciPy

### NumPy + Pandas

* Used for data analysis
* Built on NumPy arrays

```python
import pandas as pd
import numpy as np

data = np.array([10,20,30])
series = pd.Series(data)
```

---

### NumPy + SciPy

SciPy provides:

* Optimization
* Signal processing
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

---

## 6. Advantages of NumPy Arrays

### 1. High Performance

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

```python
matrix = np.array([
    [1,2,3],
    [4,5,6]
])

print(matrix)
```

---

### 4. Broadcasting

```python
arr = np.array([1,2,3])
print(arr + 10)
```

---

### 5. Mathematical Functions

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

✔ No error = Installed successfully

---

## 8. Importing NumPy

```python
import numpy as np
```

✔ `np` is the standard alias

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
* Create `.py` file

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

NumPy is the foundation of numerical computing in Python.

### Key Takeaways

* Provides fast multidimensional arrays
* Enables vectorized operations
* Backbone of Pandas, SciPy, and ML libraries
* Much faster than Python lists
* Easy to install and use
* Essential for Data Science, AI, and Scientific Computing

---

**Thanks for reading!**
