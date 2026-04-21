
# Pandas Hands-On Tutorial

## Chapter 19: Working with Large Datasets

Modern data analysis often involves very large datasets, sometimes containing:

- millions of rows  
- hundreds of columns  
- gigabytes of storage  

### Examples include:
- web analytics logs  
- financial transactions  
- IoT sensor streams  
- social media datasets  
- machine learning training data  

Although Pandas is extremely powerful, it runs in-memory, meaning the entire dataset must typically fit into RAM.

Therefore, working efficiently with large datasets requires understanding:

- memory usage  
- data type optimization  
- chunk processing  
- efficient operations  

---

## Understanding Memory Usage

Before optimizing large datasets, it is important to understand memory consumption.

Each column in a DataFrame stores values using specific data types.

### Example dataset:

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 28],
    "Salary": [50000, 60000, 55000]
}

df = pd.DataFrame(data)
````

---

## Checking Memory Usage

```python
df.memory_usage()
```

### Example output:

```
Index     128
Name      192
Age        24
Salary     24
```

Values are in bytes.

---

## Detailed Memory Usage

```python
df.memory_usage(deep=True)
```

This includes memory used by strings and objects.

---

## Total Memory Usage

```python
df.memory_usage(deep=True).sum()
```

Example:

```
368 bytes
```

---

## Data Type Inspection

```python
df.dtypes
```

Output:

```
Name      object
Age       int64
Salary    int64
```

👉 `object` (strings) consume the most memory.

---

# Optimizing Memory Consumption

Large datasets may exceed RAM if not optimized.

---

## 1. Reducing Numeric Precision

Default:

* int64
* float64

But often unnecessary.

### Example:

```python
df["Age"] = df["Age"].astype("int8")
```

### Memory comparison:

| Type  | Memory  |
| ----- | ------- |
| int64 | 8 bytes |
| int32 | 4 bytes |
| int16 | 2 bytes |
| int8  | 1 byte  |

👉 Can reduce memory up to 8×.

---

## Downcasting Numeric Types

```python
pd.to_numeric(df["Salary"], downcast="integer")
```

Options:

* integer
* float
* unsigned

---

## 2. Converting Strings to Categorical

Example:

```
City
New York
London
New York
London
Paris
```

### Convert:

```python
df["City"] = df["City"].astype("category")
```

### Memory benefit:

| Type     | Memory |
| -------- | ------ |
| object   | high   |
| category | low    |

👉 Up to 10× reduction.

---

### How category works:

Instead of storing strings:

```
[New York, London, New York]
```

It stores:

```
[1, 0, 1]
```

---

## 3. Removing Unnecessary Columns

```python
df = df.drop(columns=["email"])
```

Removing unused columns reduces memory significantly.

---

# Reading Data in Chunks

Large datasets may not fit into memory.

## Chunk processing:

```python
chunks = pd.read_csv("large_sales.csv", chunksize=10000)
```

---

## Processing each chunk:

```python
for chunk in chunks:
    print(chunk.head())
```

---

## Aggregating results:

```python
total = 0

for chunk in pd.read_csv("large_sales.csv", chunksize=10000):
    total += chunk["Sales"].sum()

print(total)
```

---

## Filtering while reading:

```python
filtered_data = []

for chunk in pd.read_csv("data.csv", chunksize=5000):
    filtered = chunk[chunk["Sales"] > 1000]
    filtered_data.append(filtered)

result = pd.concat(filtered_data)
```

---

# Using Efficient Data Types

---

## Boolean type

```python
df["IsActive"] = df["IsActive"].astype("bool")
```

---

## Datetime type

```python
df["Date"] = pd.to_datetime(df["Date"])
```

---

## Nullable integers

```python
df["Age"] = df["Age"].astype("Int64")
```

Used when missing values exist.

---

# Performance Improvements

---

## Avoid Python loops ❌

```python
for i in range(len(df)):
    df["Total"][i] = df["Price"][i] * df["Quantity"][i]
```

Very slow.

---

## Use vectorization ✅

```python
df["Total"] = df["Price"] * df["Quantity"]
```

👉 100× faster.

---

## Efficient filtering

```python
df[df["Sales"] > 1000]
```

---

## Avoid unnecessary copies

```python
df.copy()
```

Consumes memory.

---

## Efficient indexing

```python
df.set_index("Date", inplace=True)
```

Improves lookup performance.

---

## Using query()

```python
df.query("Sales > 1000")
```

Often faster than filtering.

---

# Efficient File Formats

| Format  | Advantage        |
| ------- | ---------------- |
| CSV     | simple           |
| Parquet | compressed, fast |
| Feather | very fast        |
| HDF5    | large datasets   |

---

### Parquet example:

```python
df.to_parquet("data.parquet")
```

👉 Reduces file size by 70–90%.

---

# Real-World Example: Large Dataset

Dataset:

* sales_2024.csv
* 50 million rows

---

## Step 1: Read in chunks

```python
chunks = pd.read_csv("sales_2024.csv", chunksize=100000)
```

---

## Step 2: Process data

```python
total_sales = 0

for chunk in chunks:
    total_sales += chunk["Sales"].sum()
```

---

## Step 3: Result

```
Total Sales = $2,450,000
```

---

# Memory Optimization Workflow

### Step 1: Inspect memory

```python
df.memory_usage(deep=True)
```

---

### Step 2: Optimize types

* int64 → int32
* float64 → float32
* object → category

---

### Step 3: Remove unused columns

```python
df.drop()
```

---

### Step 4: Use chunk processing

```python
pd.read_csv(chunksize=)
```

---

### Step 5: Use vectorized operations

Avoid loops.

---

# Summary

Working with large datasets requires careful optimization.

---

## Memory Analysis Tools

* `df.memory_usage()`
* `df.memory_usage(deep=True)`
* `df.dtypes`

---

## Memory Optimization Techniques

* downcasting numeric types
* using category dtype
* removing unused columns

---

## Chunk Processing

```python
pd.read_csv(chunksize=)
```

Enables processing datasets larger than RAM.

---

## Efficient Data Types

* int8 instead of int64
* float32 instead of float64
* category instead of object
* datetime64 for dates
* bool for binary values

---

## Performance Best Practices

* vectorized operations
* avoid Python loops
* efficient indexing
* query() usage
* Parquet format

---

## Final Insight

These techniques allow Pandas to efficiently handle:

* millions of rows
* large-scale datasets
* real-world production systems

making it suitable for modern data engineering and analytics workflows.


