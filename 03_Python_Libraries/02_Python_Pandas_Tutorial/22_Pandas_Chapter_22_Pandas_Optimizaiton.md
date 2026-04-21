# Pandas Hands-On Tutorial

## Chapter 22: Pandas Optimization 

When working with datasets containing millions of rows, performance becomes critical. Inefficient operations can slow down analysis dramatically or even cause memory crashes. Pandas provides several mechanisms to ensure high-performance data processing.

Key performance optimization strategies include:
- using vectorized operations  
- avoiding Python loops  
- applying efficient indexing  
- optimizing data types  
- monitoring memory usage  

These techniques help transform Pandas from a simple data manipulation library into a high-performance analytical engine.

---

## Using Vectorization for Speed

Vectorization means performing operations on entire arrays of data at once instead of processing individual elements sequentially.

Pandas is built on NumPy, which provides highly optimized C-level implementations for vectorized operations. Because of this, vectorized operations are often **10×–100× faster than loops**.

---

### Example Dataset

```python
import pandas as pd

data = {
    "Price": [100, 200, 150, 120],
    "Quantity": [2, 3, 4, 5]
}

df = pd.DataFrame(data)
````

| Price | Quantity |
| ----- | -------- |
| 100   | 2        |
| 200   | 3        |
| 150   | 4        |
| 120   | 5        |

---

### Loop-Based Calculation (Slow)

```python
total = []
for i in range(len(df)):
    total.append(df["Price"][i] * df["Quantity"][i])

df["Total"] = total
```

---

### Vectorized Calculation (Fast)

```python
df["Total"] = df["Price"] * df["Quantity"]
```

---

### Result

| Price | Quantity | Total |
| ----- | -------- | ----- |
| 100   | 2        | 200   |
| 200   | 3        | 600   |
| 150   | 4        | 600   |
| 120   | 5        | 600   |

The entire column is processed in a single optimized operation.

---

## Vectorized Conditional Operations

```python
df["HighSales"] = df["Total"] > 500
```

---

## Avoiding Loops in Pandas

### Common Mistake (Slow)

```python
for index, row in df.iterrows():
    df.loc[index, "Total"] = row["Price"] * row["Quantity"]
```

Problems:

* extremely slow
* inefficient memory usage
* breaks vectorized optimization

---

### Better Alternative

```python
df["Total"] = df["Price"] * df["Quantity"]
```

---

## Using apply() Instead of Loops

```python
df["Total"] = df.apply(
    lambda row: row["Price"] * row["Quantity"],
    axis=1
)
```

However, `apply()` is still slower than pure vectorization.

---

## Efficient Indexing Practices

Indexes improve data retrieval performance. A well-designed index allows faster filtering and grouping operations.

---

### Setting an Index

```python
df.set_index("EmployeeID", inplace=True)
```

Now access becomes:

```python
df.loc[101]
```

---

### Sorting Index

```python
df.sort_index(inplace=True)
```

---

### Using MultiIndex

```python
df.set_index(["Region", "Year"], inplace=True)
```

This enables hierarchical queries.

---

## Using Categorical Data Types

String columns often consume large memory.

Instead of storing repeated strings, Pandas can store categories.

---

### Convert to Category

```python
df["City"] = df["City"].astype("category")
```

Internally:

* codes → integers
* categories → unique values

Example:

Codes:

```
[0, 1, 2, 1, 0]
```

Categories:

```
["New York", "London", "Paris"]
```

---

### Memory Reduction Example

| Data Type | Memory |
| --------- | ------ |
| object    | 10 MB  |
| category  | 1 MB   |

Up to **90% memory reduction**.

---

### Faster Grouping

Categorical data improves:

* groupby
* sorting
* filtering

---

## Monitoring Memory Usage

---

### Checking Memory Usage

```python
df.memory_usage()
```

---

### Detailed Memory Analysis

```python
df.memory_usage(deep=True)
```

---

### Total Memory Usage

```python
df.memory_usage(deep=True).sum()
```

---

### Inspect Data Types

```python
df.dtypes
```

---

## Pandas Options and Settings

Pandas provides configurable global settings for display and formatting.

---

## Display Options

```python
pd.set_option("display.max_rows", 100)
pd.set_option("display.max_columns", 50)
```

---

### Column Width

```python
pd.set_option("display.max_colwidth", 100)
```

---

## Formatting Options

```python
pd.set_option("display.float_format", "{:.2f}".format)
```

Example:

```
3.14159 → 3.14
```

---

## Configuration Settings

View all options:

```python
pd.describe_option()
```

Search options:

```python
pd.describe_option("display")
```

---

## Reset Options

```python
pd.reset_option("display.max_rows")
pd.reset_option("all")
```

---

## Setting Global Options

```python
pd.set_option("display.width", 120)
```

---

## Exporting and Sharing Data

After processing, data is exported in formats like:

* CSV
* Excel
* JSON
* SQL
* HTML

---

## Export CSV

```python
df.to_csv("processed_data.csv", index=False)
```

---

## Export Excel

```python
df.to_excel("report.xlsx", index=False)
```

---

## Export Multiple Sheets

```python
with pd.ExcelWriter("analysis.xlsx") as writer:
    df1.to_excel(writer, sheet_name="Sales")
    df2.to_excel(writer, sheet_name="Customers")
```

---

## Export JSON

```python
df.to_json("data.json")
```

---

## Export SQL

```python
df.to_sql("sales_table", connection)
```

---

## Export HTML

```python
df.to_html("table.html")
```

---

## Best Practices in Pandas

---

### Clean Workflow

Typical pipeline:

* Load data
* Clean data
* Transform data
* Analyze data
* Export results

---

### Example Pipeline

```python
df = pd.read_csv("sales.csv")

df.dropna(inplace=True)

df["Revenue"] = df["Price"] * df["Quantity"]

summary = df.groupby("Category").sum()
```

---

## Efficient Transformations

```python
df = (
    df.dropna()
      .assign(Revenue=df["Price"] * df["Quantity"])
      .groupby("Category")
      .sum()
)
```

---

## Reproducible Pipelines

Best practices:

* avoid manual edits
* store transformations in scripts
* document steps

---

## Writing Readable Code

```python
df["total_sales"] = df["price"] * df["quantity"]
```

---

## Improving Performance

Key techniques:

* vectorization
* efficient indexing
* memory optimization
* categorical data
* avoiding loops

---

## Summary

Advanced Pandas workflows involve optimizing performance, configuring environment settings, exporting results, and following best practices.

---

### Performance Optimization

* vectorization
* efficient indexing
* categorical data
* memory monitoring

---

### Pandas Options

* display settings
* formatting options
* configuration controls

---

### Exporting Data

* CSV
* Excel
* JSON
* SQL
* HTML

---

### Best Practices

* clean workflows
* efficient transformations
* reproducible pipelines
* readable code
* performance optimization

---

Mastering these techniques enables analysts and data scientists to build robust, efficient, and scalable data analysis systems using Pandas.


