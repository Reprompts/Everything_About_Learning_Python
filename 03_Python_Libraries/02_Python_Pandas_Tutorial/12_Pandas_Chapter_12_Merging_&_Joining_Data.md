# Pandas Hands-On Tutorial

## Chapter 12: Merging & Joining Data

In real-world data analysis, data rarely exists in a single dataset. Instead, information is often spread across multiple tables or files such as:

- customer data  
- sales transactions  
- product catalogs  
- employee records  
- financial reports  

To perform meaningful analysis, these datasets must be combined into a unified structure.

Pandas provides several powerful tools for combining datasets:

- Concatenation  
- Merging  
- Joining  

These operations are conceptually similar to SQL joins and are fundamental for working with relational data.

---

## This Article Covers

- Combining multiple datasets  
- Concatenating datasets  
- Merge operations  
- Join operations  
- Inner join  
- Outer join  
- Left join  
- Right join  
- Merging using multiple keys  
- Handling duplicate keys during merge  

---

## Combining Multiple Datasets

Combining datasets means bringing together information from different tables into a single DataFrame.

### Example scenario

**Customers table**

| CustomerID | Name    |
|------------|---------|
| 1          | Alice   |
| 2          | Bob     |
| 3          | Charlie |

**Orders table**

| CustomerID | OrderAmount |
|------------|-------------|
| 1          | 500         |
| 2          | 700         |
| 2          | 300         |

To analyze customer spending, we must combine these tables.

---

## Concatenating Datasets

Concatenation means stacking datasets together.

Pandas provides the `concat()` function.

Concatenation can happen in two ways:
- Vertical concatenation (rows)
- Horizontal concatenation (columns)

---

## Vertical Concatenation

Rows from multiple DataFrames are appended.

```python
import pandas as pd

df1 = pd.DataFrame({
    "Name": ["Alice", "Bob"],
    "Age": [25, 30]
})

df2 = pd.DataFrame({
    "Name": ["Charlie", "David"],
    "Age": [35, 40]
})
````

### Concatenate rows

```python
pd.concat([df1, df2])
```

### Result

| Name    | Age |
| ------- | --- |
| Alice   | 25  |
| Bob     | 30  |
| Charlie | 35  |
| David   | 40  |

---

### Resetting index

```python
pd.concat([df1, df2], ignore_index=True)
```

---

## Horizontal Concatenation

Columns from multiple DataFrames are combined.

```python
df1 = pd.DataFrame({
    "Name": ["Alice", "Bob"]
})

df2 = pd.DataFrame({
    "Salary": [50000, 60000]
})
```

### Concatenate columns

```python
pd.concat([df1, df2], axis=1)
```

### Result

| Name  | Salary |
| ----- | ------ |
| Alice | 50000  |
| Bob   | 60000  |

---

## Merge Operations

Merging is used to combine datasets based on common columns (keys).

The Pandas `merge()` function works similarly to SQL joins.

### Syntax

```python
pd.merge(left_df, right_df, on="key")
```

---

### Example datasets

**Employees table**

| EmployeeID | Name    |
| ---------- | ------- |
| 1          | Alice   |
| 2          | Bob     |
| 3          | Charlie |

**Salary table**

| EmployeeID | Salary |
| ---------- | ------ |
| 1          | 50000  |
| 2          | 60000  |
| 3          | 55000  |

---

### Merge datasets

```python
pd.merge(employees, salary, on="EmployeeID")
```

### Result

| EmployeeID | Name    | Salary |
| ---------- | ------- | ------ |
| 1          | Alice   | 50000  |
| 2          | Bob     | 60000  |
| 3          | Charlie | 55000  |

---

## Join Operations

Joining is conceptually similar to merging but often uses indexes instead of columns.

```python
df1.join(df2)
```

---

## Inner Join

An inner join returns only rows where matching keys exist in both tables.

```python
pd.merge(customers, orders, on="CustomerID", how="inner")
```

### Result

| CustomerID | Name  | OrderAmount |
| ---------- | ----- | ----------- |
| 1          | Alice | 500         |
| 2          | Bob   | 700         |

Customer 3 is removed because no matching order exists.

---

## Outer Join

An outer join returns all rows from both tables, filling missing values with NaN.

```python
pd.merge(customers, orders, on="CustomerID", how="outer")
```

### Result

| CustomerID | Name    | OrderAmount |
| ---------- | ------- | ----------- |
| 1          | Alice   | 500         |
| 2          | Bob     | 700         |
| 3          | Charlie | NaN         |

---

## Left Join

Returns all rows from the left DataFrame and matching rows from the right.

```python
pd.merge(customers, orders, on="CustomerID", how="left")
```

### Result

| CustomerID | Name    | OrderAmount |
| ---------- | ------- | ----------- |
| 1          | Alice   | 500         |
| 2          | Bob     | 700         |
| 3          | Charlie | NaN         |

---

## Right Join

Returns all rows from the right DataFrame.

```python
pd.merge(customers, orders, on="CustomerID", how="right")
```

### Result

| CustomerID | Name  | OrderAmount |
| ---------- | ----- | ----------- |
| 1          | Alice | 500         |
| 2          | Bob   | 700         |

---

## Merging Using Multiple Keys

Some datasets require multiple columns to uniquely identify records.

### Example datasets

**Sales table**

| Region | Product | Sales |
| ------ | ------- | ----- |
| US     | A       | 500   |
| US     | B       | 700   |
| EU     | A       | 600   |

**Pricing table**

| Region | Product | Price |
| ------ | ------- | ----- |
| US     | A       | 10    |
| US     | B       | 15    |
| EU     | A       | 12    |

---

### Merge using multiple keys

```python
pd.merge(sales, pricing, on=["Region", "Product"])
```

---

### Result

| Region | Product | Sales | Price |
| ------ | ------- | ----- | ----- |
| US     | A       | 500   | 10    |
| US     | B       | 700   | 15    |
| EU     | A       | 600   | 12    |

---

## Handling Duplicate Keys During Merge

Duplicate keys occur when multiple rows share the same key.

### Example

**Customers**

| CustomerID | Name  |
| ---------- | ----- |
| 1          | Alice |
| 2          | Bob   |

**Orders**

| CustomerID | OrderAmount |
| ---------- | ----------- |
| 1          | 500         |
| 1          | 700         |
| 2          | 300         |

---

### Merge

```python
pd.merge(customers, orders, on="CustomerID")
```

### Result

| CustomerID | Name  | OrderAmount |
| ---------- | ----- | ----------- |
| 1          | Alice | 500         |
| 1          | Alice | 700         |
| 2          | Bob   | 300         |

Alice appears twice due to multiple orders.

---

## Many-to-Many Merges

### Example

**Table A**

| Key | Value |
| --- | ----- |
| 1   | A     |
| 1   | B     |

**Table B**

| Key | Value |
| --- | ----- |
| 1   | X     |
| 1   | Y     |

### Result

| Key | Value | Value |
| --- | ----- | ----- |
| 1   | A     | X     |
| 1   | A     | Y     |
| 1   | B     | X     |
| 1   | B     | Y     |

---

## Merge Indicators

```python
pd.merge(df1, df2, on="key", how="outer", indicator=True)
```

### `_merge` column values:

* left_only
* right_only
* both

---

## Real-World Example

### E-commerce analytics

**Products table**

| ProductID | ProductName |
| --------- | ----------- |
| 101       | Laptop      |
| 102       | Phone       |

**Sales table**

| ProductID | Quantity |
| --------- | -------- |
| 101       | 5        |
| 102       | 10       |

---

### Merge

```python
sales_data = pd.merge(products, sales, on="ProductID")
```

### Compute revenue

```python
sales_data["Revenue"] = sales_data["Quantity"] * price
```

---

## Performance Considerations

Merging large datasets can be computationally expensive.

### Best practices:

* Merge only required columns
* Ensure keys are indexed
* Avoid unnecessary duplicate merges

Example:

* Dataset size: 10 million rows
* Use indexed keys for better performance

---

## Summary

Combining datasets is a fundamental skill in data analysis and data engineering.

### Pandas provides:

#### Concatenation

* Stacks datasets (rows or columns)
* `pd.concat()`

#### Merging

* Combines based on keys
* `pd.merge()`

#### Joining

* Combines using indexes
* `df.join()`

---

### Join types:

* Inner Join → matching rows only
* Outer Join → all rows from both tables
* Left Join → all rows from left table
* Right Join → all rows from right table

---

### Advanced techniques:

* Multiple key merges
* Handling duplicate keys
* Merge indicators

---

### Applications:

* Data engineering pipelines
* Business intelligence dashboards
* Financial reporting systems
* Machine learning data preparation

---

Mastering merges and joins enables analysts to integrate complex datasets and extract deeper insights.

