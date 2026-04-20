
# NumPy Hands-On Tutorial

## Chapter 22: File Handling

When working with numerical data, we often need to store arrays to disk and load them later. File handling in NumPy allows you to:

- Save large datasets efficiently  
- Reload arrays without recomputation  
- Share datasets between programs  
- Store intermediate results in scientific workflows  

NumPy provides multiple functions for file handling, supporting binary formats and text formats.

- Binary formats are faster and compact  
- Text formats are human-readable and easier to share  

### Main Functions

- `np.save()`  
- `np.load()`  
- `np.savez()`  
- `np.savetxt()`  
- `np.loadtxt()`  

---

## Saving Arrays to Files

Saving arrays allows you to store computed results for later use.

### Example Scenario

- A machine learning model generates large matrices  
- You save them to disk  
- Later reload them for further analysis  

---

## Loading Arrays from Files

Loading arrays allows you to retrieve stored data quickly.

Useful when working with:

- Large datasets  
- Preprocessed data  
- Simulation results  
- Model parameters  

NumPy loads arrays directly into memory as NumPy arrays.

---

## Using `np.save()`

Stores an array in NumPy binary format (`.npy`).

### Advantages

- Fast reading/writing  
- Compact storage  
- Preserves data type and shape  

### Syntax
```python
np.save(filename, array)
````

---

### Example: Saving an Array

```python
import numpy as np

arr = np.array([10,20,30,40,50])
np.save("data.npy", arr)
```

File created:

```
data.npy
```

---

### Loading Saved Array

```python
import numpy as np

data = np.load("data.npy")
print(data)
```

**Output**

```
[10 20 30 40 50]
```

---

## Using `np.load()`

Reads arrays from NumPy binary files.

### Syntax

```python
np.load(filename)
```

---

### Example

```python
import numpy as np

arr = np.array([[1,2,3],[4,5,6]])
np.save("matrix.npy", arr)

loaded = np.load("matrix.npy")
print(loaded)
```

**Output**

```
[[1 2 3]
 [4 5 6]]
```

---

## Using `np.savez()`

Stores multiple arrays in one compressed file (`.npz`).

### Syntax

```python
np.savez(filename, array1=value1, array2=value2)
```

---

### Example

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

np.savez("arrays.npz", first=a, second=b)
```

File created:

```
arrays.npz
```

---

### Loading Multiple Arrays

```python
data = np.load("arrays.npz")

print(data["first"])
print(data["second"])
```

**Output**

```
[1 2 3]
[4 5 6]
```

---

## Using `np.savetxt()`

Saves arrays as text files.

### Advantages

* Human-readable
* Easy to edit
* Compatible with many tools

### Disadvantages

* Larger file size
* Slower than binary formats

---

### Syntax

```python
np.savetxt(filename, array)
```

---

### Example

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6]
])

np.savetxt("data.txt", arr)
```

---

### Formatting Output

```python
np.savetxt("data.txt", arr, fmt="%d")
```

Output file:

```
1 2 3
4 5 6
```

---

## Using `np.loadtxt()`

Reads arrays from text files.

### Syntax

```python
np.loadtxt(filename)
```

---

### Example

```python
import numpy as np

data = np.loadtxt("data.txt")
print(data)
```

**Output**

```
[[1. 2. 3.]
 [4. 5. 6.]]
```

---

## Reading CSV Files

CSV = Comma Separated Values

### Example CSV

```
10,20,30
40,50,60
70,80,90
```

---

### Example

```python
import numpy as np

data = np.loadtxt("data.csv", delimiter=",")
print(data)
```

---

## Writing CSV Files

```python
import numpy as np

arr = np.array([
    [10,20,30],
    [40,50,60]
])

np.savetxt("data.csv", arr, delimiter=",", fmt="%d")
```

---

## Binary File Storage

Binary storage is the most efficient way to store large arrays.

### Advantages

* Faster read/write
* Smaller file size
* Exact data preservation
* No precision loss

### Formats

* `.npy`
* `.npz`

---

## Binary vs Text Storage

| Feature        | Binary (.npy) | Text (.txt/.csv)   |
| -------------- | ------------- | ------------------ |
| Speed          | Very fast     | Slower             |
| File size      | Small         | Larger             |
| Human readable | No            | Yes                |
| Precision      | Exact         | May lose precision |

---

## Practical Example: Saving and Loading Data

```python
import numpy as np

# Create sample dataset
data = np.random.rand(5,3)

# Save dataset
np.save("dataset.npy", data)

# Load dataset
loaded_data = np.load("dataset.npy")

print("Original Data")
print(data)

print("Loaded Data")
print(loaded_data)
```

---

## Real-World Example: Dataset Storage

```python
import numpy as np

scores = np.array([
    [85,90,88],
    [78,82,80],
    [92,95,94]
])

# Save to CSV
np.savetxt("scores.csv", scores, delimiter=",", fmt="%d")

# Load from CSV
data = np.loadtxt("scores.csv", delimiter=",")

print(data)
```

---

## Summary

NumPy provides powerful functions for saving and loading arrays.

### Binary Storage Functions

| Function   | Purpose                   |
| ---------- | ------------------------- |
| np.save()  | Save array to `.npy` file |
| np.load()  | Load array from `.npy`    |
| np.savez() | Save multiple arrays      |

---

### Text File Functions

| Function     | Purpose              |
| ------------ | -------------------- |
| np.savetxt() | Save array as text   |
| np.loadtxt() | Load array from text |

---

### CSV Handling

| Task      | Function                         | Example Usage                               |
| --------- | -------------------------------- | ------------------------------------------- |
| Write CSV | `np.savetxt(..., delimiter=",")` | `np.savetxt("data.csv", A, delimiter=",")`  |
| Read CSV  | `np.loadtxt(..., delimiter=",")` | `A = np.loadtxt("data.csv", delimiter=",")` |

---

## Key Takeaways

* Binary files (`.npy`) are fastest and most efficient
* `.npz` files allow storing multiple arrays
* Text files (`.txt`, `.csv`) are portable and readable
* NumPy file handling is widely used in data pipelines and scientific computing

