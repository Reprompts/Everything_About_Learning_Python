
# Pandas Hands-On Tutorial

## Chapter 3: Data Selection & Indexing

Data selection and indexing are core operations in Pandas. Once data is loaded into a DataFrame or Series, the next step in almost every data analysis workflow is extracting specific rows, columns, or subsets of the data.

Pandas provides several powerful and flexible tools for accessing data:

- Column selection  
- Row selection  
- Label-based indexing (`loc`)  
- Position-based indexing (`iloc`)  
- Boolean indexing  
- Conditional filtering  
- Multi-column selection  
- Index slicing  
- Querying data  

Understanding these tools allows you to navigate, filter, and manipulate large datasets efficiently.

---

## Example Dataset Used in This Article

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie", "David", "Eva"],
    "Age": [25, 30, 35, 28, 22],
    "Department": ["HR", "IT", "Finance", "IT", "Marketing"],
    "Salary": [50000, 70000, 80000, 65000, 45000]
}

df = pd.DataFrame(data)
print(df)
````

**Output:**

```
      Name  Age Department  Salary
0    Alice   25         HR   50000
1      Bob   30         IT   70000
2  Charlie   35    Finance   80000
3    David   28         IT   65000
4      Eva   22  Marketing   45000
```

---

## Selecting Columns

Columns represent variables or features in the dataset.

### Selecting a Single Column

```python
df["Name"]
```

**Output:**

```
0      Alice
1        Bob
2    Charlie
3      David
4        Eva
Name: Name, dtype: object
```

> This returns a **Series**.

---

### Attribute Style Column Access

```python
df.Age
```

> Bracket notation is recommended because it works with all column names.

---

### Why Columns are Selected First

* Columns → features
* Rows → observations

---

## Selecting Rows

Rows represent records or observations.

```python
df.loc[2]
```

**Output:**

```
Name           Charlie
Age                 35
Department     Finance
Salary           80000
```

---

## Label-Based Indexing (`loc`)

### Syntax

```python
df.loc[row_label, column_label]
```

### Examples

```python
df.loc[1]
```

```python
df.loc[2, "Name"]
```

```python
df.loc[[1, 3]]
```

```python
df.loc[1:3]
```

> `loc` includes both endpoints.

```python
df.loc[1:3, ["Name", "Salary"]]
```

---

## Position-Based Indexing (`iloc`)

### Syntax

```python
df.iloc[row_position, column_position]
```

### Examples

```python
df.iloc[2]
```

```python
df.iloc[2, 1]
```

```python
df.iloc[1:4]
```

> `iloc` excludes the end index.

```python
df.iloc[1:4, 0:2]
```

---

## Boolean Indexing

```python
df["Age"] > 30
```

```python
df[df["Age"] > 30]
```

---

## Conditional Filtering

```python
df[df["Salary"] > 60000]
```

---

### Multiple Conditions

```python
df[(df["Age"] > 25) & (df["Department"] == "IT")]
```

Operators:

* `&` → AND
* `|` → OR
* `~` → NOT

> Each condition must be enclosed in parentheses.

---

## Selecting Multiple Columns

```python
df[["Name", "Salary"]]
```

---

## Combining Filtering and Column Selection

```python
df[df["Department"] == "IT"][["Name", "Salary"]]
```

---

## Selecting Subsets of Data

```python
df.loc[1:3, ["Name", "Age"]]
```

```python
df.iloc[1:4, 0:3]
```

---

## Index Slicing

```python
df[1:4]
```

> Works like Python list slicing.

---

### Slicing with Custom Index

```python
df.set_index("Name", inplace=True)
```

```python
df.loc["Alice":"Charlie"]
```

---

## Querying Data

### Basic Query

```python
df.query("Age > 30")
```

---

### Multiple Conditions

```python
df.query("Age > 25 and Department == 'IT'")
```

---

### Using Variables

```python
min_salary = 60000
df.query("Salary > @min_salary")
```

---

## Comparison of Indexing Methods

| Method           | Type             | Usage                |
| ---------------- | ---------------- | -------------------- |
| df["col"]        | column selection | single column        |
| df.loc           | label-based      | row/column labels    |
| df.iloc          | position-based   | row/column positions |
| boolean indexing | conditional      | filtering rows       |
| query()          | expression-based | readable filtering   |

---

## Real-World Data Analysis Workflow

```
Load dataset
       ↓
Inspect columns
       ↓
Filter rows
       ↓
Select relevant columns
       ↓
Create subset
       ↓
Perform analysis
```

### Example

```python
df = pd.read_csv("employees.csv")

high_salary_it = df[
    (df["Department"] == "IT") &
    (df["Salary"] > 60000)
][["Name", "Salary"]]

print(high_salary_it)
```

---

## Summary

* Columns are selected using bracket notation
* Rows can be selected using `loc` (labels) or `iloc` (positions)
* Boolean indexing enables conditional filtering
* Multiple conditions can be combined using logical operators
* Subsets can be extracted using row and column selection
* Index slicing allows selecting row ranges
* `query()` provides SQL-like filtering syntax

Mastering these techniques allows efficient navigation and analysis of large datasets.


