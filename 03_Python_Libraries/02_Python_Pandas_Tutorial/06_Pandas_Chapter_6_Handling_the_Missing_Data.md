
# Pandas Hands-On Tutorial

## Chapter 6: Handling the Missing Data

Missing data is one of the most common and challenging problems in real-world datasets. It is extremely rare to encounter perfectly complete data. Instead, datasets frequently contain:

- Missing entries  
- Null values  
- Incomplete records  
- Corrupted or unavailable data  

Proper handling of missing data is critical because incorrect treatment can lead to:

- Biased analysis  
- Incorrect statistics  
- Poor machine learning models  

Pandas provides powerful tools to detect, analyze, and manage missing data efficiently.

---

## Topics Covered

- Understanding missing data  
- NaN and None values  
- Detecting missing data  
- Dropping missing values  
- Filling missing values  
- Forward fill method  
- Backward fill method  
- Interpolation  
- Replacing missing values  

---

## Understanding Missing Data

Missing data represents the absence of a value in a dataset.

### Example Dataset

```

+---------+-----+--------+
| Name    | Age | Salary |
+---------+-----+--------+
| Alice   | 25  | 50000  |
| Bob     | NaN | 60000  |
| Charlie | 35  | NaN    |
+---------+-----+--------+

````

- Bob's Age is missing  
- Charlie's Salary is missing  

---

## Common Causes of Missing Data

- Data collection errors  
- Incomplete surveys  
- System failures  
- Human input mistakes  
- Data merging issues  
- Unavailable measurements  

---

## NaN and None Values

### NaN (Not a Number)

Most common missing value representation.

```python
import pandas as pd
import numpy as np

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, np.nan, 35],
    "Salary": [50000, 60000, np.nan]
}

df = pd.DataFrame(data)
print(df)
````

**Output:**

```
      Name   Age   Salary
0    Alice  25.0  50000.0
1      Bob   NaN  60000.0
2  Charlie  35.0      NaN
```

**Characteristics:**

* Floating-point value
* Used for numeric missing data

---

### None Values

```python
data = {
    "Name": ["Alice", None, "Charlie"],
    "Age": [25, 30, None]
}

df = pd.DataFrame(data)
```

**Output:**

```
      Name   Age
0    Alice  25.0
1     None  30.0
2  Charlie   NaN
```

> Pandas often converts `None` to `NaN`.

---

## Detecting Missing Data

### `isnull()`

```python
df.isnull()
```

### `notnull()`

```python
df.notnull()
```

---

### Counting Missing Values

```python
df.isnull().sum()
```

---

### Total Missing Values

```python
df.isnull().sum().sum()
```

---

## Dropping Missing Values

### Drop Rows

```python
df.dropna()
```

---

### Drop Columns

```python
df.dropna(axis=1)
```

---

### Drop Based on Specific Columns

```python
df.dropna(subset=["Age"])
```

---

### Drop Rows with All Values Missing

```python
df.dropna(how="all")
```

---

## Filling Missing Values

### Fill with Constant

```python
df.fillna(0)
```

---

### Fill with Mean

```python
df["Age"].fillna(df["Age"].mean())
```

---

### Fill with Median

```python
df["Salary"].fillna(df["Salary"].median())
```

---

### Fill with Mode

```python
df["Department"].fillna(df["Department"].mode()[0])
```

---

## Forward Fill (ffill)

```python
df.fillna(method="ffill")
```

### Example

```
Day   Sales
1     100
2     NaN
3     NaN
4     150
```

**Result:**

```
Day   Sales
1     100
2     100
3     100
4     150
```

---

## Backward Fill (bfill)

```python
df.fillna(method="bfill")
```

### Example

```
Day   Sales
1     100
2     NaN
3     NaN
4     150
```

**Result:**

```
Day   Sales
1     100
2     150
3     150
4     150
```

---

## Interpolation

```python
df.interpolate()
```

### Example

```
Time   Temperature
1      20
2      NaN
3      24
```

**Result:**

```
Time   Temperature
1      20
2      22
3      24
```

---

### Types of Interpolation

```python
df.interpolate(method="linear")
```

Supported methods:

* linear
* time
* polynomial
* spline
* nearest

---

## Replacing Missing Values

### Replace NaN

```python
df.replace(np.nan, 0)
```

---

### Replace with Custom Values

```python
df.replace({
    "Age": np.nan,
    "Salary": np.nan
}, 0)
```

---

### Conditional Replacement

```python
df["Salary"] = df.groupby("Department")["Salary"].transform(
    lambda x: x.fillna(x.mean())
)
```

---

## Real-World Data Cleaning Workflow

```
Load dataset
       ↓
Detect missing values
       ↓
Analyze missing percentage
       ↓
Choose strategy
       ↓
Drop or fill values
       ↓
Validate dataset
```

### Example

```python
df = pd.read_csv("employees.csv")

df.isnull().sum()
df["Age"].fillna(df["Age"].mean(), inplace=True)
df.dropna(subset=["Salary"], inplace=True)
```

---

## Choosing the Right Strategy

| Scenario          | Best Method           |
| ----------------- | --------------------- |
| few missing rows  | drop rows             |
| many missing rows | fill values           |
| time series data  | forward/backward fill |
| numeric trends    | interpolation         |
| categorical data  | mode replacement      |

---

## Summary

Handling missing data is a critical step in data preprocessing.

### Key Concepts

* Missing values are represented as `NaN` or `None`
* Detect using `isnull()` and `notnull()`
* Remove using `dropna()`
* Replace using `fillna()`
* Use `ffill` and `bfill` for sequential data
* Use interpolation for estimation
* Use `replace()` for custom handling

### Benefits of Proper Handling

* Accurate statistical analysis
* Reliable machine learning models
* Clean and usable datasets

