# Pandas Hands-On Tutorial

## Chapter 21: Advanced Features 

Pandas provides far more than basic data manipulation. For advanced analytics and high-performance data processing, Pandas includes several powerful mechanisms that operate efficiently on large datasets.

These advanced features allow users to:
- perform custom aggregations  
- leverage vectorized computations  
- use efficient indexing  
- apply broadcasting operations  
- automatically align data structures  
- work safely with labeled data  

These capabilities are essential in data science pipelines, financial analysis systems, and large-scale analytics workflows.

This article explores the following advanced Pandas features in extremely detailed practical depth:
- Custom aggregation functions  
- Vectorized computations  
- Efficient indexing strategies  
- Broadcasting operations  
- Alignment of data structures  
- Automatic label alignment  

---

## Custom Aggregation Functions

Aggregation means combining multiple values into a single summary value.

Common built-in aggregations include:
- sum()
- mean()
- max()
- min()
- count()

However, in real-world analytics we often need custom aggregation logic. Pandas allows users to define custom aggregation functions that operate on grouped or entire datasets.

---

### Example Dataset

```python
import pandas as pd

data = {
    "Department": ["Sales", "Sales", "IT", "IT", "HR"],
    "Salary": [50000, 60000, 70000, 80000, 45000]
}

df = pd.DataFrame(data)
````

Dataset:

| Department | Salary |
| ---------- | ------ |
| Sales      | 50000  |
| Sales      | 60000  |
| IT         | 70000  |
| IT         | 80000  |
| HR         | 45000  |

---

### Using Built-in Aggregation

```python
df.groupby("Department")["Salary"].mean()
```

---

### Creating Custom Aggregation Function

Example: salary range

```python
def salary_range(series):
    return series.max() - series.min()

df.groupby("Department")["Salary"].agg(salary_range)
```

---

### Multiple Custom Aggregations

```python
df.groupby("Department")["Salary"].agg(
    ["mean", "max", "min", salary_range]
)
```

---

### Lambda Aggregation

```python
df.groupby("Department")["Salary"].agg(
    lambda x: x.max() - x.min()
)
```

---

## Vectorized Computations

Vectorization means performing operations on entire arrays at once instead of using loops.

It is extremely fast because it uses optimized C-level implementations.

---

### Example Dataset

```python
data = {
    "Price": [10, 20, 30],
    "Quantity": [2, 3, 4]
}

df = pd.DataFrame(data)
```

| Price | Quantity |
| ----- | -------- |
| 10    | 2        |
| 20    | 3        |
| 30    | 4        |

---

### Non-Vectorized Approach (Slow)

```python
total = []
for i in range(len(df)):
    total.append(df["Price"][i] * df["Quantity"][i])

df["Total"] = total
```

---

### Vectorized Approach (Fast)

```python
df["Total"] = df["Price"] * df["Quantity"]
```

---

### Result

| Price | Quantity | Total |
| ----- | -------- | ----- |
| 10    | 2        | 20    |
| 20    | 3        | 60    |
| 30    | 4        | 120   |

Vectorization is often 100× faster than loops.

---

### Vectorized Conditional Operations

```python
df["Discounted"] = df["Price"] * 0.9
df["HighPrice"] = df["Price"] > 15
```

---

### Vectorized String Operations

```python
df["Name"].str.upper()
```

---

### Vectorized Datetime Operations

```python
df["Date"].dt.year
```

---

## Efficient Indexing Strategies

Indexes allow fast data lookup and selection.

---

### Default Index

0, 1, 2, 3 → RangeIndex

---

### Setting a Column as Index

```python
df.set_index("EmployeeID", inplace=True)
```

---

### Example Dataset

| EmployeeID | Name  | Salary |
| ---------- | ----- | ------ |
| 101        | Alice | 50000  |
| 102        | Bob   | 60000  |

Now:

```python
df.loc[101]
```

---

### Sorting Index

```python
df.sort_index(inplace=True)
```

---

### MultiIndex Example

```python
df.set_index(["Region", "Year"], inplace=True)
```

---

## Broadcasting Operations

Broadcasting allows operations between arrays of different shapes.

---

### Example Dataset

```python
df + 5
```

Result: scalar is applied to all values.

---

### Broadcasting with Series

```python
df + series
```

Series aligns by column labels.

---

## Alignment of Data Structures

Pandas automatically aligns data during operations based on labels.

---

### Example

DataFrame A:

| Index | Value |
| ----- | ----- |
| 0     | 10    |
| 1     | 20    |
| 2     | 30    |

DataFrame B:

| Index | Value |
| ----- | ----- |
| 1     | 5     |
| 2     | 15    |
| 3     | 25    |

```python
df1 + df2
```

Result:

| Index | Value |
| ----- | ----- |
| 0     | NaN   |
| 1     | 25    |
| 2     | 45    |
| 3     | NaN   |

---

### Handling Missing Alignment

```python
df1.add(df2, fill_value=0)
```

---

## Automatic Label Alignment

Pandas aligns by:

* rows
* columns
* indexes

Example:

```python
df["New"] = series
```

Values are placed based on matching index labels.

---

## Real-World Example: Sales Data Processing

### Dataset

| Product | Price | Quantity |
| ------- | ----- | -------- |
| A       | 10    | 5        |
| B       | 20    | 3        |
| C       | 15    | 8        |

---

### Vectorized Computation

```python
df["Revenue"] = df["Price"] * df["Quantity"]
```

---

### Aggregation

```python
df["Revenue"].sum()
```

---

### Custom Aggregation

```python
df["Revenue"].agg(lambda x: x.max() - x.min())
```

---

### Broadcasting

```python
df["RevenueWithTax"] = df["Revenue"] * 1.18
```

---

## Performance Advantages

| Feature            | Benefit                    |
| ------------------ | -------------------------- |
| Vectorization      | extremely fast computation |
| Custom aggregation | flexible analytics         |
| Indexing           | fast data retrieval        |
| Broadcasting       | concise operations         |
| Alignment          | safe calculations          |

---

## Summary

Advanced Pandas features enable powerful data processing workflows.

---

### Custom Aggregation

* agg()
* lambda functions
* user-defined functions

---

### Vectorized Computations

Operate on entire columns:

```python
df["A"] * df["B"]
```

---

### Efficient Indexing

* set_index()
* loc[]
* MultiIndex

---

### Broadcasting

```python
df + scalar
df + series
```

---

### Alignment of Data Structures

Ensures safe operations between matching labels.

---

### Automatic Label Alignment

Pandas automatically aligns:

* rows
* columns
* indexes

---

These advanced features make Pandas extremely powerful for:

* large-scale data analysis
* machine learning preprocessing
* financial modeling
* scientific computing

Mastering these capabilities allows analysts to build efficient, scalable data processing pipelines.


