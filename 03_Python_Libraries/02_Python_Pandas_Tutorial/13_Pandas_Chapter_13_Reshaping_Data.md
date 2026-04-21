# Pandas Hands-On Tutorial

## Chapter 13: Reshaping Data

Data reshaping is the process of changing the structure or layout of a dataset without changing the actual data values.

In real-world analytics and data engineering workflows, datasets often need to be reorganized to perform calculations, build dashboards, or train machine learning models.

Reshaping operations help convert data between different formats such as:
- wide format → long format  
- long format → wide format  
- row-based summaries → column-based summaries  

These transformations are fundamental in:
- business intelligence reporting  
- data visualization  
- statistical analysis  
- machine learning preprocessing  

Pandas provides powerful reshaping tools such as:
- `pivot()`
- `pivot_table()`
- `melt()`
- `stack()`
- `unstack()`

---

## This Article Explains

- Understanding data reshaping  
- Pivot tables  
- Using `pivot()`  
- Using `pivot_table()`  
- Melting datasets  
- Stacking data  
- Unstacking data  
- Wide vs long data formats  

---

## Understanding Data Reshaping

Data reshaping changes the organizational structure of a dataset.

### Example dataset (long format)

| Student | Subject | Marks |
|---------|---------|-------|
| Alice   | Math    | 90    |
| Alice   | Physics | 85    |
| Bob     | Math    | 80    |
| Bob     | Physics | 88    |

This format is called **long format**.

But many analyses require **wide format**:

| Student | Math | Physics |
|---------|------|---------|
| Alice   | 90   | 85      |
| Bob     | 80   | 88      |

Reshaping allows converting between these formats.

---

## Pivot Tables

A pivot table summarizes and restructures data by transforming rows into columns.

Pivot tables help answer questions like:
- sales by region and product  
- average salary by department  
- revenue by month and category  

### Example dataset

| Date | Product | Sales |
|------|---------|-------|
| Jan  | Laptop  | 5000  |
| Jan  | Phone   | 3000  |
| Feb  | Laptop  | 4000  |
| Feb  | Phone   | 3500  |

### Pivot result

| Date | Laptop | Phone |
|------|--------|-------|
| Jan  | 5000   | 3000  |
| Feb  | 4000   | 3500  |

---

## Using `pivot()`

The `pivot()` function reshapes data without aggregation.

### Syntax

```python
df.pivot(index, columns, values)
````

* index → row labels
* columns → column categories
* values → data values

---

### Example dataset

```python
import pandas as pd

data = {
    "Student": ["Alice", "Alice", "Bob", "Bob"],
    "Subject": ["Math", "Physics", "Math", "Physics"],
    "Marks": [90, 85, 80, 88]
}

df = pd.DataFrame(data)
```

### Pivot operation

```python
df.pivot(index="Student", columns="Subject", values="Marks")
```

### Result

| Subject | Math | Physics |
| ------- | ---- | ------- |
| Student |      |         |
| Alice   | 90   | 85      |
| Bob     | 80   | 88      |

---

## Pivot Limitation

`pivot()` fails if duplicate values exist.

Example:

| Student | Subject | Marks |
| ------- | ------- | ----- |
| Alice   | Math    | 90    |
| Alice   | Math    | 95    |

Error:

```
ValueError: Index contains duplicate entries
```

---

## Using `pivot_table()`

`pivot_table()` is an enhanced version of `pivot()` that supports aggregation.

### Syntax

```python
pd.pivot_table(
    df,
    values,
    index,
    columns,
    aggfunc
)
```

### Common aggregation functions

* mean
* sum
* count
* min
* max

---

### Example dataset

| Region | Product | Sales |
| ------ | ------- | ----- |
| US     | Laptop  | 5000  |
| US     | Phone   | 3000  |
| EU     | Laptop  | 4000  |
| EU     | Phone   | 3500  |

---

### Pivot table

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Product",
    aggfunc="sum"
)
```

### Result

| Region | Laptop | Phone |
| ------ | ------ | ----- |
| EU     | 4000   | 3500  |
| US     | 5000   | 3000  |

---

### Multiple aggregations

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Product",
    aggfunc=["sum", "mean"]
)
```

---

### Adding totals

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Product",
    aggfunc="sum",
    margins=True
)
```

---

## Melting Datasets

Melting converts wide format → long format.

Used in:

* visualization (Matplotlib, Seaborn)
* machine learning preprocessing
* statistical modeling

---

### Example wide dataset

| Student | Math | Physics |
| ------- | ---- | ------- |
| Alice   | 90   | 85      |
| Bob     | 80   | 88      |

---

### Using `melt()`

```python
pd.melt(
    df,
    id_vars="Student",
    var_name="Subject",
    value_name="Marks"
)
```

### Result

| Student | Subject | Marks |
| ------- | ------- | ----- |
| Alice   | Math    | 90    |
| Alice   | Physics | 85    |
| Bob     | Math    | 80    |
| Bob     | Physics | 88    |

---

### Melt parameters

* id_vars → fixed columns
* value_vars → columns to melt
* var_name → new column name
* value_name → values column

---

## Stacking Data

Stacking converts columns into rows using hierarchical indexing.

### Example

| Student | Math | Physics |
| ------- | ---- | ------- |
| Alice   | 90   | 85      |
| Bob     | 80   | 88      |

### Stack operation

```python
df.set_index("Student").stack()
```

### Result

```
Alice  Math       90
       Physics    85
Bob    Math       80
       Physics    88
```

This produces a MultiIndex Series.

---

## Unstacking Data

Unstacking is the reverse of stacking.

```python
stacked.unstack()
```

### Result

| Subject | Math | Physics |
| ------- | ---- | ------- |
| Student |      |         |
| Alice   | 90   | 85      |
| Bob     | 80   | 88      |

---

## Wide vs Long Data Formats

### Wide Format

| Student | Math | Physics |
| ------- | ---- | ------- |
| Alice   | 90   | 85      |
| Bob     | 80   | 88      |

**Advantages:**

* easy to read
* compact structure

---

### Long Format

| Student | Subject | Marks |
| ------- | ------- | ----- |
| Alice   | Math    | 90    |
| Alice   | Physics | 85    |
| Bob     | Math    | 80    |
| Bob     | Physics | 88    |

**Advantages:**

* easier grouping
* better visualization
* better ML compatibility

---

## Real-World Business Example

### Sales dataset

| Month | Product | Revenue |
| ----- | ------- | ------- |
| Jan   | Laptop  | 5000    |
| Jan   | Phone   | 3000    |
| Feb   | Laptop  | 4500    |
| Feb   | Phone   | 3200    |

---

### Pivot for dashboard

```python
df.pivot(index="Month", columns="Product", values="Revenue")
```

---

### Melt for visualization

```python
pd.melt(df, id_vars="Month")
```

---

## Performance Considerations

Reshaping operations are optimized but can be expensive for large datasets.

### Best practices:

* avoid unnecessary reshaping
* use `pivot_table()` for aggregation
* ensure proper indexing for stack/unstack

Example:

* Dataset size: 10 million rows
* Pandas handles reshaping efficiently

---

## Summary

Data reshaping transforms datasets into structures suitable for analysis, visualization, and modeling.

### Key techniques:

#### Pivot

```python
df.pivot()
```

Reshapes data without aggregation.

---

#### Pivot Table

```python
pd.pivot_table()
```

Creates aggregated summaries.

---

#### Melt

```python
pd.melt()
```

Converts wide → long format.

---

#### Stack

```python
df.stack()
```

Moves columns into hierarchical rows.

---

#### Unstack

```python
df.unstack()
```

Moves index levels into columns.

---

## Final Note

Understanding wide vs long formats is essential because many analytical tools depend on correct data structure.

These techniques are widely used in:

* financial reporting
* business intelligence dashboards
* statistical analysis
* machine learning pipelines
* exploratory data analysis

Mastering data reshaping allows analysts to transform complex datasets into flexible analytical structures.

