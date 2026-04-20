
# Pandas Hands-On Tutorial

## Chapter 2: Core Pandas Data Structures

Pandas is built around three fundamental data structures:

- **Series** — One-dimensional labeled array  
- **DataFrame** — Two-dimensional labeled table  
- **Index** — Immutable labels used to identify rows and columns  

These structures allow Pandas to represent and manipulate structured data efficiently.

Think of them as layers:

```

NumPy Arrays → Pandas Series → Pandas DataFrame
↓
Index

```

Understanding these structures is essential because almost every operation in Pandas involves them.

---

## Series

### What is a Series

A Series is a one-dimensional labeled array capable of holding any data type.

It consists of two main components:

- **Index** → labels  
- **Values** → actual data  

### Example Representation

```

Index   Value
0       10
1       20
2       30

````

### Example in Python

```python
import pandas as pd

s = pd.Series([10, 20, 30])
print(s)
````

**Output:**

```
0    10
1    20
2    30
dtype: int64
```

### Structure Visualization

```
Series
│
├── Index: [0, 1, 2]
└── Values: [10, 20, 30]
```

Series are similar to:

* Python dictionaries (key-value pairs)
* NumPy arrays (ordered values)
* Database columns

---

## Creating Series

Common sources:

* Python lists
* Python dictionaries
* NumPy arrays
* Scalar values
* Existing Pandas structures

---

### Series from Lists

```python
import pandas as pd

numbers = [10, 20, 30, 40]
s = pd.Series(numbers)
print(s)
```

**Custom index:**

```python
s = pd.Series(numbers, index=["a", "b", "c", "d"])
print(s)
```

---

### Series from Dictionaries

```python
data = {
    "Alice": 25,
    "Bob": 30,
    "Charlie": 35
}

s = pd.Series(data)
print(s)
```

**Subset using index:**

```python
s = pd.Series(data, index=["Alice", "Charlie"])
print(s)
```

> Missing keys produce `NaN`.

---

### Series from NumPy Arrays

```python
import numpy as np
import pandas as pd

arr = np.array([100, 200, 300])
s = pd.Series(arr)
print(s)
```

**Custom index:**

```python
s = pd.Series(arr, index=["x", "y", "z"])
```

---

## Series Indexing

### Label-Based Indexing

```python
s["x"]
```

**Example:**

```python
s = pd.Series([10, 20, 30], index=["a", "b", "c"])
print(s["b"])
```

---

### Position-Based Indexing

```python
s[1]
```

For clarity:

```python
s.loc["a"]   # label-based
s.iloc[1]    # position-based
```

---

## Series Slicing

```python
s = pd.Series([10, 20, 30, 40, 50])
print(s[1:4])
```

**Label slicing:**

```python
s = pd.Series([10, 20, 30], index=["a", "b", "c"])
print(s["a":"b"])
```

---

## Series Attributes

| Attribute | Description        |
| --------- | ------------------ |
| index     | labels             |
| values    | underlying array   |
| dtype     | data type          |
| shape     | dimensions         |
| size      | number of elements |
| name      | name of the series |

### Example

```python
s = pd.Series([10, 20, 30], name="scores")

print(s.index)
print(s.values)
print(s.dtype)
print(s.shape)
print(s.size)
print(s.name)
```

---

## Series Methods

### Statistical Methods

```python
s.mean()
s.sum()
s.min()
s.max()
```

---

### Value Operations

```python
s.unique()
s.value_counts()
```

**Example:**

```python
s = pd.Series([1, 2, 2, 3, 3, 3])
print(s.value_counts())
```

---

### Missing Data Handling

```python
s.isnull()
s.dropna()
s.fillna()
```

---

## Series Arithmetic Operations

```python
s = pd.Series([10, 20, 30])
print(s + 5)
```

### Operations with Another Series

```python
s1 = pd.Series([1, 2, 3])
s2 = pd.Series([4, 5, 6])

print(s1 + s2)
```

### Alignment by Index

```python
s1 = pd.Series([1, 2], index=["a", "b"])
s2 = pd.Series([3, 4], index=["b", "c"])

print(s1 + s2)
```

---

## DataFrame

### What is a DataFrame

A DataFrame is a two-dimensional labeled data structure.

It consists of:

* Rows → Index
* Columns → Column labels
* Values → Data

### Visualization

```
Name   Age   City
0      Alice  25    NY
1      Bob    30    LA
2      Tom    28    SF
```

### Example

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Tom"],
    "Age": [25, 30, 28]
}

df = pd.DataFrame(data)
print(df)
```

---

## Creating DataFrames

### DataFrame from Lists

```python
data = [
    ["Alice", 25],
    ["Bob", 30],
    ["Tom", 28]
]

df = pd.DataFrame(data, columns=["Name", "Age"])
print(df)
```

---

### DataFrame from Dictionaries

```python
data = {
    "Name": ["Alice", "Bob", "Tom"],
    "Age": [25, 30, 28]
}

df = pd.DataFrame(data)
```

---

### DataFrame from NumPy Arrays

```python
import numpy as np

arr = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

df = pd.DataFrame(arr, columns=["A", "B", "C"])
```

---

### DataFrame from Series

```python
s1 = pd.Series([1, 2, 3])
s2 = pd.Series([4, 5, 6])

df = pd.DataFrame({
    "col1": s1,
    "col2": s2
})

print(df)
```

---

## DataFrame Attributes

| Attribute | Description            |
| --------- | ---------------------- |
| shape     | rows × columns         |
| columns   | column labels          |
| index     | row labels             |
| dtypes    | data types             |
| size      | number of elements     |
| values    | underlying NumPy array |

---

## DataFrame Methods

| Method        | Purpose               |
| ------------- | --------------------- |
| head()        | first rows            |
| tail()        | last rows             |
| describe()    | statistics            |
| info()        | structure             |
| sort_values() | sorting               |
| drop()        | removing rows/columns |

### Example

```python
df.describe()
```

---

## Viewing Data

### Head

```python
df.head()
```

### Tail

```python
df.tail()
```

### Sample

```python
df.sample(3)
```

---

## Inspecting Structure

### Info

```python
df.info()
```

---

### Describe

```python
df.describe()
```

---

## Index

The Index defines how rows and columns are labeled.

### Example

```
Index: [0, 1, 2, 3]
```

### Custom Index

```python
df.index = ["a", "b", "c"]
```

---

## Index Objects

```python
df.index
df.columns
```

---

## Index Properties

| Property     | Meaning           |
| ------------ | ----------------- |
| values       | underlying labels |
| dtype        | data type name    |
| index        | name              |
| is_unique    | uniqueness        |
| is_monotonic | sorted order      |

---

## Index Immutability

❌ Invalid:

```python
df.index[0] = "new"
```

✔ Correct:

```python
df.index = ["a", "b", "c"]
```

---

## Setting Index

```python
df.set_index("Name", inplace=True)
```

---

## Resetting Index

```python
df.reset_index()
```

---

## MultiIndex Basics

### Example Structure

```
Region   Year
Asia     2020
Asia     2021
Europe   2020
```

### Example Creation

```python
arrays = [
    ["Asia", "Asia", "Europe", "Europe"],
    [2020, 2021, 2020, 2021]
]

index = pd.MultiIndex.from_arrays(arrays)

df = pd.DataFrame(
    {"Sales": [100, 120, 90, 95]},
    index=index
)

print(df)
```

---

## Summary

### Series

* One-dimensional labeled array
* Supports indexing, slicing, and vectorized operations
* Created from lists, dictionaries, and NumPy arrays

### DataFrame

* Two-dimensional table
* Rows and columns with labels
* Built from many data sources
* Supports powerful analysis methods

### Index

* Labels that identify rows and columns
* Immutable
* Enables alignment and fast lookups
* Supports hierarchical indexing (MultiIndex)

---

Together, these structures make Pandas a powerful system for structured data manipulation.

