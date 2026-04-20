
# NumPy Hands-On Tutorial

## Chapter 23: Advanced NumPy

As datasets become larger and computations become more complex, understanding how NumPy handles memory and performance becomes extremely important.

NumPy is designed for high-performance numerical computing, and it achieves this through:

- Efficient memory representation  
- Vectorized operations  
- Broadcasting  
- Advanced array structures  

---

## Topics Covered

- Memory Management in NumPy  
- Performance Optimization  
- Structured Arrays  
- Masked Arrays  

These concepts are widely used in:

- Scientific computing  
- Data science  
- Machine learning pipelines  
- Numerical simulations  

---

## Memory Management in NumPy

NumPy arrays are memory-efficient and computationally fast.

### Python List Memory Model
```

[1,2,3]

Pointer → Object
Pointer → Object
Pointer → Object

```

- Requires multiple memory accesses  

---

### NumPy Array Memory Model
```

[1 2 3]

| 1 | 2 | 3 |

```

- Stored contiguously in memory  
- Enables faster operations  
- Improves CPU caching  
- Supports vectorized computation  

---

## Understanding NumPy Memory Model

Every NumPy array consists of:

### 1. Data Buffer
Stores actual values  
Example:
```

[10,20,30,40]

```

### 2. Metadata

Describes the array:

- Data type (`dtype`)  
- Shape  
- Strides  
- Memory location  

Example:
```

dtype = int64
shape = (4,)
strides = (8,)

````

---

## Views vs Copies

### Copy

Creates a duplicate of the array.

```python
import numpy as np

a = np.array([1,2,3,4])
b = a.copy()

b[0] = 100

print(a)
print(b)
````

**Output**

```
[1 2 3 4]
[100 2 3 4]
```

---

### View

Shares memory with the original array.

```python
a = np.array([1,2,3,4])
b = a.view()

b[0] = 100

print(a)
```

**Output**

```
[100 2 3 4]
```

---

## Memory Sharing Between Arrays

```python
import numpy as np

a = np.arange(10)
b = a[2:6]

print(np.shares_memory(a,b))
```

**Output**

```
True
```

* Slicing creates views

---

## Efficient Memory Usage

Best practices:

* Use smaller data types
* Avoid unnecessary copies
* Prefer views

### Example

```python
import numpy as np

arr = np.array([1,2,3,4], dtype=np.int8)
print(arr.nbytes)
```

---

## Array Strides

Strides define how many bytes to move in memory.

### Example

```
[10 20 30 40]
dtype = int64 → 8 bytes
```

### Checking Strides

```python
import numpy as np

arr = np.array([10,20,30,40])
print(arr.strides)
```

**Output**

```
(8,)
```

---

## Performance Optimization

NumPy is fast because it uses compiled C code.

### Vectorization

Perform operations on entire arrays.

#### Without Vectorization

```python
result = []
for i in range(1000):
    result.append(i * 2)
```

#### With NumPy

```python
import numpy as np

arr = np.arange(1000)
result = arr * 2
```

---

## Avoiding Python Loops

#### Slow Approach

```python
import numpy as np

arr = np.arange(100000)
result = []

for i in arr:
    result.append(i**2)
```

#### Efficient Approach

```python
result = arr ** 2
```

---

## Efficient Array Operations

```python
import numpy as np

arr = np.random.rand(1000000)
mean = np.mean(arr)
```

---

## Broadcasting for Speed

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6]
])

result = arr + 10
print(result)
```

**Output**

```
[[11 12 13]
 [14 15 16]]
```

---

## Memory Efficient Computations

Use in-place operations:

```python
arr = np.arange(10)
arr += 5
```

---

## Structured Arrays

Structured arrays allow storing different data types.

### Example Structure

* Name → string
* Age → integer
* Salary → float

---

### Creating Structured Arrays

```python
import numpy as np

dtype = [
    ('name','U10'),
    ('age','i4'),
    ('salary','f4')
]

data = np.array([
    ('Alice',25,50000),
    ('Bob',30,60000),
    ('Charlie',28,55000)
], dtype=dtype)

print(data)
```

**Output**

```
[('Alice',25,50000.)
 ('Bob',30,60000.)
 ('Charlie',28,55000.)]
```

---

## Accessing Structured Data

```python
print(data['name'])
```

**Output**

```
['Alice' 'Bob' 'Charlie']
```

```python
print(data['salary'])
```

**Output**

```
[50000. 60000. 55000.]
```

---

## Masked Arrays

Masked arrays handle missing or invalid data.

Example:

```
[10, 20, NaN, 40]
```

---

### Creating Masked Arrays

```python
import numpy as np
import numpy.ma as ma

data = np.array([10,20,30,40])
masked = ma.masked_array(data, mask=[0,0,1,0])

print(masked)
```

**Output**

```
[10 20 -- 40]
```

---

## Masking with Conditions

```python
import numpy as np
import numpy.ma as ma

data = np.array([10,20,30,40,50])
masked = ma.masked_greater(data, 30)

print(masked)
```

**Output**

```
[10 20 30 -- --]
```

---

## Working with Masked Values

```python
import numpy as np
import numpy.ma as ma

data = np.array([10,20,30,40])
masked = ma.masked_equal(data, 30)

print(masked.mean())
```

**Output**

```
23.3333
```

---

## Summary

### Memory Management

| Concept      | Description              |
| ------------ | ------------------------ |
| Memory model | Data stored contiguously |
| Views        | Share memory             |
| Copies       | Duplicate data           |
| Strides      | Describe memory steps    |

---

### Performance Optimization

| Technique           | Benefit                |
| ------------------- | ---------------------- |
| Vectorization       | Fast operations        |
| Avoid loops         | Faster execution       |
| Broadcasting        | Efficient memory usage |
| In-place operations | Reduce memory usage    |

---

### Structured Arrays

Used for storing mixed data types.

Common uses:

* Tabular data
* Scientific datasets
* Simulation records

---

### Masked Arrays

Used for handling missing or invalid data.

Benefits:

* Ignore invalid values
* Maintain dataset structure
* Safe computations

---

## Final Insight

Understanding these advanced concepts allows you to:

* Write high-performance numerical code
* Work efficiently with large datasets
* Build optimized scientific workflows

These techniques are widely used in:

* Data science
* Machine learning
* Physics simulations
* Financial modeling
* Large-scale numerical computations


