
# Pandas Hands-On Tutorial

## Chapter 18: File Handling 

In real-world data analysis, data rarely originates directly inside a Python program. Instead, it typically comes from external storage systems such as:

- CSV datasets  
- Excel spreadsheets  
- JSON APIs  
- SQL databases  
- HTML tables from web pages  
- text log files  
- columnar storage formats like Parquet  
- serialized binary formats like Pickle  

Pandas provides a comprehensive file input/output (I/O) system that allows analysts to efficiently load, process, and save data across many different formats.

File handling with Pandas is essential in:
- data science pipelines  
- ETL (Extract–Transform–Load) systems  
- financial reporting  
- web data scraping  
- machine learning workflows  
- data warehousing systems  

---

## This article covers:

- Reading files  
- Writing files  
- Working with multiple data formats  

---

## File Input/Output in Pandas

Pandas includes high-level functions to read and write structured data.

### General pattern:

```python
pd.read_format()
df.to_format()
````

### Examples:

```python
pd.read_csv()
pd.read_excel()
df.to_csv()
df.to_excel()
```

These functions automatically convert external data into Pandas DataFrames.

---

# Reading Files

Reading files is the first step in most data analysis workflows.

---

## Reading CSV Files

CSV (Comma-Separated Values) is the most widely used format.

### Example CSV:

```
name,age,city
Alice,25,New York
Bob,30,London
Charlie,28,Sydney
```

### Reading CSV:

```python
import pandas as pd

df = pd.read_csv("data.csv")
```

### Result:

| name    | age | city     |
| ------- | --- | -------- |
| Alice   | 25  | New York |
| Bob     | 30  | London   |
| Charlie | 28  | Sydney   |

---

## Important Parameters

### Custom delimiter

```python
df = pd.read_csv("data.csv", sep="|")
```

### Reading specific columns

```python
df = pd.read_csv("data.csv", usecols=["name", "age"])
```

### Handling missing values

```python
df = pd.read_csv("data.csv", na_values=["NA", "missing"])
```

### Skipping rows

```python
df = pd.read_csv("data.csv", skiprows=2)
```

### Reading large CSV in chunks

```python
chunks = pd.read_csv("large_file.csv", chunksize=1000)

for chunk in chunks:
    print(chunk.head())
```

---

## Reading Excel Files

Excel files are widely used in business reporting.

### Read Excel:

```python
df = pd.read_excel("sales.xlsx")
```

### Specific sheet:

```python
df = pd.read_excel("sales.xlsx", sheet_name="January")
```

### Multiple sheets:

```python
data = pd.read_excel("sales.xlsx", sheet_name=None)
```

Returns a dictionary of DataFrames.

---

## Reading JSON Files

JSON is commonly used in APIs.

### Example JSON:

```json
[
 {"name": "Alice", "age": 25},
 {"name": "Bob", "age": 30}
]
```

### Read JSON:

```python
df = pd.read_json("data.json")
```

---

## Nested JSON

```python
from pandas import json_normalize

df = json_normalize(data)
```

---

## Reading HTML Tables

Pandas can extract tables from web pages.

```python
tables = pd.read_html("https://example.com")
df = tables[0]
```

Used in web scraping.

---

## Reading SQL Databases

```python
import sqlite3

conn = sqlite3.connect("database.db")
df = pd.read_sql("SELECT * FROM users", conn)
```

### Read table directly:

```python
df = pd.read_sql_table("users", conn)
```

---

## Reading Text Files

```python
df = pd.read_csv("data.txt", sep=" ")
```

---

## Fixed-width files

```python
df = pd.read_fwf("data.txt")
```

---

## Reading Parquet Files

Parquet is optimized for big data.

```python
df = pd.read_parquet("data.parquet")
```

### Advantages:

* smaller size
* faster queries
* column compression

Used in:

* Spark pipelines
* data lakes
* cloud analytics

---

## Reading Pickle Files

```python
df = pd.read_pickle("data.pkl")
```

Preserves Python objects exactly.

---

# Writing Files

---

## Writing CSV

```python
df.to_csv("output.csv")
```

### Without index:

```python
df.to_csv("output.csv", index=False)
```

### Custom delimiter:

```python
df.to_csv("output.csv", sep="|")
```

---

## Writing Excel

```python
df.to_excel("output.xlsx")
```

### Multiple sheets:

```python
with pd.ExcelWriter("report.xlsx") as writer:
    df1.to_excel(writer, sheet_name="Sales")
    df2.to_excel(writer, sheet_name="Customers")
```

---

## Writing JSON

```python
df.to_json("data.json")
```

### Format:

```python
df.to_json("data.json", orient="records")
```

---

## Writing HTML

```python
df.to_html("table.html")
```

---

## Writing SQL

```python
df.to_sql("users", conn)
```

### Replace table:

```python
df.to_sql("users", conn, if_exists="replace")
```

Options:

* fail
* replace
* append

---

## Writing Parquet

```python
df.to_parquet("data.parquet")
```

Used in big data systems.

---

## Writing Pickle

```python
df.to_pickle("data.pkl")
```

Preserves:

* datatypes
* indexes
* complex structures

---

# Real-World Example: Data Pipeline

### Step 1 - Load CSV

```python
df = pd.read_csv("sales.csv")
```

### Step 2 - Clean data

```python
df.dropna(inplace=True)
```

### Step 3 - Transform

```python
df["Total"] = df["Price"] * df["Quantity"]
```

### Step 4 - Save results

```python
df.to_csv("clean_sales.csv", index=False)
df.to_excel("sales_report.xlsx")
df.to_sql("sales", conn)
```

---

# Performance Considerations

Large datasets require efficient formats.

| Format  | Use Case        |
| ------- | --------------- |
| CSV     | simple exchange |
| Excel   | reports         |
| JSON    | APIs            |
| SQL     | databases       |
| Parquet | big data        |
| Pickle  | Python objects  |

---

### Key insight:

Parquet is often **10–20× faster than CSV** for large datasets.

---

# Summary

File handling is a core capability of Pandas that connects Python to external data systems.

---

## Reading functions:

* `pd.read_csv()`
* `pd.read_excel()`
* `pd.read_json()`
* `pd.read_html()`
* `pd.read_sql()`
* `pd.read_fwf()`
* `pd.read_parquet()`
* `pd.read_pickle()`

---

## Writing functions:

* `df.to_csv()`
* `df.to_excel()`
* `df.to_json()`
* `df.to_html()`
* `df.to_sql()`
* `df.to_parquet()`
* `df.to_pickle()`

---

Pandas acts as a central hub connecting:

* databases
* APIs
* cloud storage
* big data systems
* machine learning pipelines

---

## Final takeaway

Mastering file handling allows analysts to efficiently load, process, and store data across diverse systems in real-world workflows.


