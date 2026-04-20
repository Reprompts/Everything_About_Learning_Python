
# Pandas Hands-On Tutorial

## Chapter 5: Data Types & Conversion

Data types are one of the most fundamental concepts in Pandas. Every column in a dataset stores values of a specific type, and these types determine:

- How data is stored in memory  
- How operations are performed  
- How calculations behave  
- How efficiently the dataset can be processed  

In real-world datasets, data types are often incorrect or inconsistent, making inspection and conversion essential steps in data preprocessing.

---

## Topics Covered

- Understanding Pandas data types  
- Type inference  
- Converting data types  
- Using `astype()`  
- Numeric conversion  
- String conversion  
- Datetime conversion  
- Category data type  

---

## Understanding Pandas Data Types

Every column in a Pandas DataFrame has a `dtype` (data type).

### Example Dataset

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 35],
    "Salary": [50000.0, 60000.5, 75000.0],
    "JoinDate": ["2022-01-10", "2021-06-15", "2020-09-20"]
}

df = pd.DataFrame(data)
````

### Inspect Data Types

```python
df.dtypes
```

**Output:**

```
Name         object
Age           int64
Salary      float64
JoinDate     object
```

---

## Common Pandas Data Types

| Data Type  | Description             |
| ---------- | ----------------------- |
| int64      | integer numbers         |
| float64    | decimal numbers         |
| object     | strings or mixed values |
| bool       | True/False              |
| datetime64 | date and time values    |
| category   | categorical data        |
| timedelta  | time differences        |

> Example:

* Name → object
* Age → int64
* Salary → float64
* JoinDate → object (should be datetime)

---

## Type Inference

Pandas automatically infers data types when loading data.

```python
df = pd.read_csv("employees.csv")
```

### Inference Examples

* Numbers → int or float
* Text → object
* Boolean → bool

> Note: Type inference is not always accurate.

### Problem Example

```
Salary
"50000"
"60000"
"70000"
```

Pandas interprets this as:

```
object
```

Which prevents numerical calculations.

---

## Converting Data Types

Conversion is needed when:

* Numbers are stored as text
* Dates are stored as strings
* Categorical data is stored as text
* Precision or format changes are required

### Conversion Methods

* `astype()`
* `to_numeric()`
* `to_datetime()`
* `astype("string")`
* `astype("category")`

---

## Using `astype()`

### Syntax

```python
df["column"].astype(new_type)
```

### Examples

**Convert integer to float**

```python
df["Age"] = df["Age"].astype("float")
```

**Convert float to integer**

```python
df["Salary"] = df["Salary"].astype("int")
```

**Convert multiple columns**

```python
df = df.astype({"Age": "float", "Salary": "int"})
```

### Important Notes

* Conversion fails if incompatible values exist
* Example: `"abc"` → cannot convert to int

---

## Numeric Conversion

### Example Dataset

```
Age
"25"
"30"
"35"
```

### Convert to Numeric

```python
df["Age"] = pd.to_numeric(df["Age"])
```

---

### Handling Invalid Values

```python
pd.to_numeric(df["Age"], errors="coerce")
```

Invalid values become:

```
NaN
```

---

### Downcasting

```python
pd.to_numeric(df["Age"], downcast="integer")
```

Reduces memory usage (e.g., int64 → int32).

---

## String Conversion

### Convert to String

```python
df["Age"].astype(str)
```

---

### Using Pandas String Type

```python
df["Name"] = df["Name"].astype("string")
```

### Benefits

* Consistent handling
* Better missing value support

---

### String Operations

```python
df["Name"].str.upper()
df["Name"].str.lower()
df["Name"].str.replace("a", "@")
```

---

## Datetime Conversion

### Example

```
"2022-01-10"
"2021-06-15"
```

### Convert to Datetime

```python
df["JoinDate"] = pd.to_datetime(df["JoinDate"])
```

**Result dtype:**

```
datetime64[ns]
```

---

### Benefits

```python
df["JoinDate"].dt.year
df["JoinDate"].dt.month
df["JoinDate"].max() - df["JoinDate"].min()
```

---

### Handling Invalid Dates

```python
pd.to_datetime(df["JoinDate"], errors="coerce")
```

Invalid values become:

```
NaT  (Not a Time)
```

---

## Category Data Type

### Example

```
Department
HR
IT
Finance
IT
```

### Convert to Category

```python
df["Department"] = df["Department"].astype("category")
```

---

### Advantages

* Memory efficient
* Faster operations
* Better semantic meaning

---

### Inspect Categories

```python
df["Department"].cat.categories
```

---

### Rename Categories

```python
df["Department"].cat.rename_categories({
    "HR": "Human Resources"
})
```

---

### Ordered Categories

```python
df["Department"].cat.as_ordered()
```

---

## Real-World Example: Data Type Cleaning

### Raw Data

```
Name      Age    Salary    JoinDate
Alice     "25"   "50000"   "2022-01-10"
Bob       "30"   "60000"   "2021-06-15"
Charlie   "35"   "75000"   "2020-09-20"
```

### Cleaning Workflow

```python
df["Age"] = pd.to_numeric(df["Age"])
df["Salary"] = pd.to_numeric(df["Salary"])
df["JoinDate"] = pd.to_datetime(df["JoinDate"])
df["Department"] = df["Department"].astype("category")
```

### Result

* Age → int64
* Salary → int64
* JoinDate → datetime64
* Department → category

---

## Performance and Memory Considerations

Proper data types improve performance.

### Example

* Object column → 10 MB
* Category column → 1 MB

### Memory Usage

```python
df.memory_usage()
```

---

## Summary

Understanding and managing data types is essential in Pandas.

### Key Points

* Pandas automatically infers data types
* Real-world data often contains incorrect types
* `astype()` is used for manual conversion
* `to_numeric()` converts strings to numbers
* `to_datetime()` converts strings to dates
* String dtype improves text handling
* Category dtype optimizes memory and performance

### Benefits of Correct Data Types

* Accurate calculations
* Efficient memory usage
* Faster operations
* Reliable analysis results

