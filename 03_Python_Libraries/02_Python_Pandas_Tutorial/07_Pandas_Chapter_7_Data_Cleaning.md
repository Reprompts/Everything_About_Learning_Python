
# Pandas Hands-On Tutorial

## Chapter 7: Data Cleaning

Data cleaning is one of the most important steps in the data analysis pipeline. In real-world datasets, raw data is rarely perfect. It often contains:

- Duplicate records  
- Inconsistent values  
- Formatting errors  
- Typographical mistakes  
- Extra spaces or unwanted characters  
- Incorrect data formats  

Before performing analysis, visualization, or machine learning, the data must be cleaned and standardized.

Pandas provides powerful tools to detect, correct, and standardize messy datasets efficiently.

---

## Topics Covered

- Removing duplicate rows  
- Renaming columns  
- Replacing incorrect values  
- Trimming whitespace  
- Handling inconsistent values  
- Correcting data formats  
- Standardizing data values  

---

## Removing Duplicate Rows

### Example Dataset

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Alice", "Charlie"],
    "Age": [25, 30, 25, 35],
    "City": ["New York", "London", "New York", "Paris"]
}

df = pd.DataFrame(data)
print(df)
````

### Detecting Duplicates

```python
df.duplicated()
```

---

### Removing Duplicates

```python
df.drop_duplicates()
```

---

### Based on Specific Columns

```python
df.drop_duplicates(subset=["Name"])
```

---

### Keep Last Occurrence

```python
df.drop_duplicates(keep="last")
```

---

### Remove All Duplicates

```python
df.drop_duplicates(keep=False)
```

---

## Renaming Columns

### Rename Specific Columns

```python
df.rename(columns={
    "Employee Name": "name",
    "Employee Age": "age"
})
```

---

### Rename All Columns

```python
df.columns = ["name", "age", "salary"]
```

---

### Standardizing Column Names

```python
df.columns = df.columns.str.lower().str.replace(" ", "_")
```

---

## Replacing Incorrect Values

### Example

```python
df["Gender"] = df["Gender"].replace({
    "M": "Male",
    "F": "Female",
    "male": "Male",
    "female": "Female"
})
```

---

### Replace Multiple Values

```python
df["Status"] = df["Status"].replace({
    "Y": "Yes",
    "N": "No"
})
```

---

## Trimming Whitespace

### Remove Leading and Trailing Spaces

```python
df["Name"] = df["Name"].str.strip()
```

---

### Remove Left Spaces

```python
df["Name"].str.lstrip()
```

---

### Remove Right Spaces

```python
df["Name"].str.rstrip()
```

---

### Apply to Entire Dataset

```python
df = df.apply(lambda x: x.str.strip() if x.dtype == "object" else x)
```

---

## Handling Inconsistent Values

### Standardizing Values

```python
df["Country"] = df["Country"].replace({
    "U.S.A": "USA",
    "United States": "USA",
    "us": "USA"
})
```

---

### Converting Case

```python
df["Country"] = df["Country"].str.lower()
```

```python
df["Country"] = df["Country"].str.upper()
```

---

## Correcting Data Formats

### Removing Currency Symbols

```python
df["Salary"] = df["Salary"].str.replace("$", "")
```

---

### Convert to Numeric

```python
df["Salary"] = pd.to_numeric(df["Salary"])
```

---

### Fixing Date Formats

```python
df["JoinDate"] = pd.to_datetime(df["JoinDate"])
```

---

### Formatting Dates

```python
df["JoinDate"].dt.strftime("%Y-%m-%d")
```

---

## Standardizing Data Values

### Example

```python
df["Department"] = df["Department"].replace({
    "Human Resources": "HR",
    "hr": "HR"
})
```

---

### Standardizing Case

```python
df["Department"] = df["Department"].str.upper()
```

---

### Standardizing Boolean Values

```python
df["Active"] = df["Active"].replace({
    "Yes": True,
    "No": False
})
```

---

## Real-World Data Cleaning Workflow

### Step 1: Load Dataset

```python
df = pd.read_csv("dataset.csv")
```

---

### Step 2: Inspect Data

```python
df.head()
df.info()
df.describe()
```

---

### Step 3: Remove Duplicates

```python
df.drop_duplicates(inplace=True)
```

---

### Step 4: Fix Column Names

```python
df.columns = df.columns.str.lower().str.replace(" ", "_")
```

---

### Step 5: Handle Whitespace

```python
df = df.apply(lambda x: x.str.strip() if x.dtype == "object" else x)
```

---

### Step 6: Standardize Values

```python
df["country"] = df["country"].str.upper()
```

---

### Step 7: Fix Data Types

```python
df["salary"] = pd.to_numeric(df["salary"])
df["join_date"] = pd.to_datetime(df["join_date"])
```

---

## Common Data Cleaning Problems

| Problem             | Example          | Solution                           |
| ------------------- | ---------------- | ---------------------------------- |
| duplicate rows      | repeated data    | `drop_duplicates()`                |
| whitespace          | " Alice "        | `str.strip()`                      |
| inconsistent values | "USA", "U.S.A"   | `replace()`                        |
| wrong formats       | "$5000"          | remove symbol + convert to numeric |
| mixed cases         | "India", "INDIA" | `lower()` / `upper()`              |

---

## Summary

Data cleaning is an essential step before performing any serious data analysis.

### Key Tasks

* Remove duplicate rows
* Rename columns for clarity
* Replace incorrect values
* Trim whitespace
* Handle inconsistent values
* Correct data formats
* Standardize values

### Benefits

* Accurate analytics
* Reliable machine learning models
* Consistent data storage
* Trustworthy insights

