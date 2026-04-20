
# Pandas Hands-On Tutorial

## Chapter 4: Data Inspection & Exploration

Before performing any analysis, machine learning, or transformation on a dataset, the first and most critical step is **data inspection and exploration**.

This process helps analysts and developers understand:

- The structure of the dataset  
- The types of data present  
- The distribution of values  
- The presence of missing or incorrect data  
- The statistical properties of the dataset  

In data science workflows, this phase is often called:

> **Exploratory Data Analysis (EDA)**

Pandas provides many powerful functions for quickly inspecting datasets.

---

## Example Dataset Used

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie", "David", "Eva"],
    "Age": [25, 30, 35, 28, 22],
    "Department": ["HR", "IT", "Finance", "IT", "Marketing"],
    "Salary": [50000, 70000, 80000, 65000, 45000]
}

df = pd.DataFrame(data)
````

**Dataset Structure:**

```
      Name  Age Department  Salary
0    Alice   25         HR   50000
1      Bob   30         IT   70000
2  Charlie   35    Finance   80000
3    David   28         IT   65000
4      Eva   22  Marketing   45000
```

---

## Viewing First Rows

```python
df.head(n)
```

* `n` = number of rows (default = 5)

### Example

```python
df.head()
```

```python
df.head(3)
```

### Why `head()` is Important

* Verify dataset loading
* Inspect column names
* Check formatting
* Identify obvious issues

---

## Viewing Last Rows

```python
df.tail(n)
```

### Example

```python
df.tail()
```

```python
df.tail(2)
```

### Why `tail()` is Useful

* Detect incomplete records
* Identify truncated data
* Check formatting issues at dataset end

---

## Displaying Dataset Structure

```python
df.info()
```

### Example Output

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5 entries, 0 to 4
Data columns (total 4 columns):
 #   Column      Non-Null Count  Dtype
---  ------      --------------  -----
 0   Name        5 non-null      object
 1   Age         5 non-null      int64
 2   Department  5 non-null      object
 3   Salary      5 non-null      int64
```

### Information Provided

* Number of rows
* Number of columns
* Column names
* Data types
* Non-null values
* Memory usage

---

## Checking Shape of Dataset

```python
df.shape
```

**Output:**

```
(5, 4)
```

* 5 rows
* 4 columns

### Why Shape Matters

* Dataset size
* Dimensionality
* Complexity of analysis

---

## Understanding Data Types

| Data Type | Meaning         |
| --------- | --------------- |
| int64     | integers        |
| float64   | decimal numbers |
| object    | strings         |
| bool      | boolean values  |
| datetime  | date/time       |

### Checking Column Data Types

```python
df.dtypes
```

### Importance of Correct Data Types

* Accurate calculations
* Proper processing
* Efficient memory usage

---

## Summary Statistics

```python
df.describe()
```

### Example Output

```
             Age       Salary
count   5.000000      5.000000
mean   28.000000  62000.000000
std     4.690416  13638.181696
min    22.000000  45000.000000
25%    25.000000  50000.000000
50%    28.000000  65000.000000
75%    30.000000  70000.000000
max    35.000000  80000.000000
```

### Explanation of Metrics

| Metric | Meaning            |
| ------ | ------------------ |
| count  | number of values   |
| mean   | average            |
| std    | standard deviation |
| min    | minimum value      |
| 25%    | first quartile     |
| 50%    | median             |
| 75%    | third quartile     |
| max    | maximum            |

---

## Descriptive Statistics

```python
df["Salary"].mean()
df["Salary"].median()
df["Salary"].std()
df["Salary"].min()
df["Salary"].max()
```

---

## Counting Unique Values

```python
df["Department"].value_counts()
```

```python
df["Department"].nunique()
```

```python
df["Department"].unique()
```

### Helps Identify

* Categories
* Duplicates
* Class imbalance

---

## Checking Missing Values

```python
df.isnull()
```

```python
df.isnull().sum()
```

```python
df.isnull().any()
```

### Missing Value Types

* `NaN`
* `None`
* `NaT`

---

## Understanding Dataset Distribution

### Frequency Distribution

```python
df["Department"].value_counts()
```

### Numeric Distribution

```python
df["Salary"].describe()
```

### Histogram

```python
df["Salary"].hist()
```

### Common Distribution Types

* Normal distribution
* Skewed distribution
* Bimodal distribution

---

## Real-World Data Exploration Workflow

```
Load dataset
      ↓
View first rows (head)
      ↓
View last rows (tail)
      ↓
Inspect structure (info)
      ↓
Check shape
      ↓
Check data types
      ↓
Summary statistics
      ↓
Analyze categories
      ↓
Detect missing values
      ↓
Understand distributions
```

### Example

```python
df = pd.read_csv("employees.csv")

df.head()
df.info()
df.shape
df.describe()
df["Department"].value_counts()
df.isnull().sum()
```

---

## Importance of Data Inspection

Helps detect:

* Missing values
* Incorrect data types
* Duplicate records
* Inconsistent categories
* Extreme outliers

Without inspection, analysis results may be incorrect.

---

## Summary

Data inspection and exploration are the first steps in any data analysis project.

### Key Tools in Pandas

* `head()` → view first rows
* `tail()` → view last rows
* `info()` → dataset structure
* `shape` → dimensions
* `dtypes` → column types
* `describe()` → statistics
* `value_counts()` → categorical counts
* `isnull()` → missing data detection

These tools help quickly understand and evaluate datasets before deeper analysis.

