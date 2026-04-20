
# Pandas Hands-On Tutorial

## Chapter 8: Data Transformation

Data transformation is the process of changing the structure, values, or representation of data to make it suitable for analysis, modeling, or reporting.

In real-world data pipelines, raw datasets are rarely ready for analysis. Analysts and data engineers often need to:

- Convert values  
- Create derived columns  
- Apply functions across data  
- Transform rows and columns  
- Perform mathematical operations  
- Standardize values for modeling  

Pandas provides powerful tools for performing these transformations efficiently.

---

## Topics Covered

- Applying functions to data  
- Using `apply()`  
- Using `map()`  
- Using `applymap()`  
- Vectorized operations  
- Column transformations  
- Row transformations  
- Creating new columns  
- Modifying existing columns  

---

## Applying Functions to Data

### Example Dataset

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 35],
    "Salary": [50000, 60000, 70000]
}

df = pd.DataFrame(data)
````

---

## Using `apply()`

### Syntax

```python
df.apply(function, axis)
```

* `axis=0` → apply to columns
* `axis=1` → apply to rows

---

### Apply Function to a Column

```python
df["Salary"] = df["Salary"].apply(lambda x: x * 1.10)
```

---

### Using Custom Function

```python
def categorize_salary(salary):
    if salary > 65000:
        return "High"
    else:
        return "Normal"

df["Salary_Category"] = df["Salary"].apply(categorize_salary)
```

---

### Apply Function Across Rows

```python
def total_compensation(row):
    return row["Salary"] + 5000

df["Total_Compensation"] = df.apply(total_compensation, axis=1)
```

---

## Using `map()`

### Syntax

```python
Series.map(mapping_or_function)
```

---

### Mapping Values

```python
department_map = {
    1: "HR",
    2: "Finance",
    3: "IT"
}

df["Department"] = df["Department_Code"].map(department_map)
```

---

### Mapping with Function

```python
df["Age"].map(lambda x: x**2)
```

---

## `map()` vs `replace()`

| Feature  | map()               | replace()                  |
| -------- | ------------------- | -------------------------- |
| Purpose  | Transform values    | Replace specific values    |
| Use Case | Mapping categories  | Cleaning/correcting values |
| Input    | function or mapping | old → new values           |
| Output   | transformed values  | updated dataset            |

---

## Using `applymap()`

Applies a function to every element in a DataFrame.

```python
df.applymap(lambda x: x * 2)
```

---

### Example: Convert Text to Uppercase

```python
df.applymap(lambda x: x.upper() if isinstance(x, str) else x)
```

---

## Vectorized Operations

Vectorization allows operations on entire columns.

```python
df["Salary"] = df["Salary"] * 1.10
```

---

### Example: Bonus Column

```python
df["Bonus"] = df["Salary"] * 0.20
```

---

## Column Transformations

### Example

```python
df["Age_Months"] = df["Age"] * 12
```

---

### String Transformations

```python
df["Name"] = df["Name"].str.upper()
df["Name"] = df["Name"].str.replace("a", "@")
```

Other methods:

* `str.lower()`
* `str.strip()`
* `str.split()`

---

## Row Transformations

### Example

```python
df["Total"] = df["Price"] * df["Quantity"]
```

---

### Using `apply()` for Rows

```python
df["Total"] = df.apply(
    lambda row: row["Price"] * row["Quantity"],
    axis=1
)
```

---

## Creating New Columns

### Example

```python
df["Tax"] = df["Salary"] * 0.15
```

---

### Conditional Columns

```python
df["High_Earner"] = df["Salary"] > 60000
```

---

### Using `np.where()`

```python
import numpy as np

df["Category"] = np.where(
    df["Salary"] > 60000,
    "High",
    "Normal"
)
```

---

## Modifying Existing Columns

### Example

```python
df["Salary"] = df["Salary"] * 1.05
```

---

### Conditional Update

```python
df.loc[df["Salary"] > 60000, "Salary"] *= 1.10
```

---

### String Modification

```python
df["Name"] = df["Name"].str.title()
```

---

## Real-World Data Transformation Workflow

### Step 1: Load Data

```python
df = pd.read_csv("sales_data.csv")
```

---

### Step 2: Clean Dataset

* Remove duplicates
* Handle missing values
* Fix formats

---

### Step 3: Transform Columns

```python
df["Revenue"] = df["Price"] * df["Quantity"]
```

---

### Step 4: Create Derived Features

```python
df["Profit"] = df["Revenue"] - df["Cost"]
```

---

### Step 5: Categorize Values

```python
df["Revenue_Level"] = np.where(
    df["Revenue"] > 10000,
    "High",
    "Low"
)
```

---

## Performance Best Practices

| Method                | Best Use                     |
| --------------------- | ---------------------------- |
| vectorized operations | fastest numerical operations |
| map()                 | mapping values               |
| apply()               | complex logic                |
| applymap()            | element-wise operations      |

Avoid unnecessary loops whenever possible.

---

## Summary

Data transformation enables analysts to reshape and prepare data for analysis.

### Key Techniques

* Applying functions using `apply()`
* Mapping values using `map()`
* Transforming DataFrames using `applymap()`
* Using vectorized operations
* Transforming columns and rows
* Creating new columns
* Modifying existing columns

### Applications

* Feature engineering
* Business analytics
* Machine learning pipelines
* Financial modeling
* Large-scale data processing

Mastering data transformation in Pandas allows you to convert raw data into meaningful insights.

