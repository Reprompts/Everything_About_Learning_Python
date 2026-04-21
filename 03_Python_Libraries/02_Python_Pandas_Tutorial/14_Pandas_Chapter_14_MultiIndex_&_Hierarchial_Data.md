# Pandas Hands-On Tutorial

## MultiIndex & Hierarchial Data 

In many real-world datasets, data naturally exists in multiple levels of hierarchy. For example:

- Country → State → City  
- Company → Department → Employee  
- Year → Month → Day  
- Product Category → Subcategory → Product  

A normal DataFrame uses a single index level, but Pandas provides **MultiIndex (Hierarchical Indexing)** to represent data with multiple dimensions within a single DataFrame.

MultiIndex enables:
- storing higher-dimensional data in 2D structures  
- performing complex grouped analysis  
- representing hierarchical relationships  
- performing powerful slicing and aggregation  

---

## This Article Explains

- Creating MultiIndex  
- Accessing hierarchical data  
- MultiIndex slicing  
- MultiIndex reshaping  
- Aggregating hierarchical data  

---

## Understanding MultiIndex

A MultiIndex allows multiple levels of indexing on rows or columns.

### Example dataset

| Region | Product | Sales |
|--------|---------|-------|
| US     | Laptop  | 5000  |
| US     | Phone   | 3000  |
| EU     | Laptop  | 4500  |
| EU     | Phone   | 2800  |

After applying MultiIndex:

```

Region  Product
US      Laptop    5000
Phone     3000
EU      Laptop    4500
Phone     2800

````

- Level 1 → Region  
- Level 2 → Product  

---

## Creating MultiIndex

There are several ways to create a MultiIndex in Pandas.

---

## Method 1: Using `set_index()`

```python
import pandas as pd

data = {
    "Region": ["US", "US", "EU", "EU"],
    "Product": ["Laptop", "Phone", "Laptop", "Phone"],
    "Sales": [5000, 3000, 4500, 2800]
}

df = pd.DataFrame(data)
df_multi = df.set_index(["Region", "Product"])
````

### Result

| Region | Product | Sales |
| ------ | ------- | ----- |
| US     | Laptop  | 5000  |
|        | Phone   | 3000  |
| EU     | Laptop  | 4500  |
|        | Phone   | 2800  |

---

## Method 2: MultiIndex from Arrays

```python
arrays = [
    ["US", "US", "EU", "EU"],
    ["Laptop", "Phone", "Laptop", "Phone"]
]

index = pd.MultiIndex.from_arrays(
    arrays,
    names=["Region", "Product"]
)

df = pd.DataFrame(
    {"Sales": [5000, 3000, 4500, 2800]},
    index=index
)
```

---

## Method 3: MultiIndex from Tuples

```python
tuples = [
    ("US", "Laptop"),
    ("US", "Phone"),
    ("EU", "Laptop"),
    ("EU", "Phone")
]

index = pd.MultiIndex.from_tuples(
    tuples,
    names=["Region", "Product"]
)
```

---

## Accessing Hierarchical Data

MultiIndex allows accessing data at different levels.

---

## Access First Level

```python
df_multi.loc["US"]
```

### Output

| Product | Sales |
| ------- | ----- |
| Laptop  | 5000  |
| Phone   | 3000  |

---

## Access Specific Value

```python
df_multi.loc[("US", "Laptop")]
```

### Output

```
Sales → 5000
```

---

## Using `.xs()` (Cross Section)

```python
df_multi.xs("Laptop", level="Product")
```

### Output

| Region | Sales |
| ------ | ----- |
| US     | 5000  |
| EU     | 4500  |

---

## MultiIndex Slicing

---

## Basic Slicing

```python
df_multi.loc["US":"EU"]
```

---

## Using `slice()`

```python
df_multi.loc[(slice(None), "Laptop"), :]
```

Explanation:

* `slice(None)` → all regions
* `"Laptop"` → filter product

---

## Partial Indexing

```python
df_multi.loc["EU"]
```

### Output

| Product | Sales |
| ------- | ----- |
| Laptop  | 4500  |
| Phone   | 2800  |

---

## MultiIndex Reshaping

MultiIndex can be reshaped using:

* `stack()`
* `unstack()`

---

## Unstacking Data

```python
df_multi.unstack()
```

### Result

| Region | Laptop | Phone |
| ------ | ------ | ----- |
| US     | 5000   | 3000  |
| EU     | 4500   | 2800  |

---

## Unstack Specific Level

```python
df_multi.unstack(level="Product")
```

---

## Stacking Data

```python
df_unstacked.stack()
```

### Result

```
Region  Product
US      Laptop    5000
        Phone     3000
EU      Laptop    4500
        Phone     2800
```

---

## MultiIndex Columns

MultiIndex can also exist in columns.

### Example

```python
data = {
    ("Sales", "Laptop"): [5000, 4500],
    ("Sales", "Phone"): [3000, 2800],
    ("Profit", "Laptop"): [1200, 1000],
    ("Profit", "Phone"): [800, 700]
}

df = pd.DataFrame(data, index=["US", "EU"])
```

### Result

|    | Sales  | Sales | Profit | Profit |
| -- | ------ | ----- | ------ | ------ |
|    | Laptop | Phone | Laptop | Phone  |
| US | 5000   | 3000  | 1200   | 800    |
| EU | 4500   | 2800  | 1000   | 700    |

---

## Aggregating Hierarchical Data

---

## Aggregation by Level

```python
df_multi.groupby(level="Region").sum()
```

### Result

| Region | Sales |
| ------ | ----- |
| US     | 8000  |
| EU     | 7300  |

---

## Aggregating Multiple Levels

```python
df_multi.groupby(level=["Region", "Product"]).sum()
```

---

## Using `sum(level=...)`

```python
df_multi.sum(level="Region")
```

---

## Aggregating Columns

```python
df.sum(level=0, axis=1)
```

---

## Real-World Business Example

### Dataset

| Region | Product | Sales |
| ------ | ------- | ----- |
| US     | Laptop  | 5000  |
| US     | Phone   | 3000  |
| EU     | Laptop  | 4500  |
| EU     | Phone   | 2800  |

---

### Create MultiIndex

```python
df_multi = df.set_index(["Region", "Product"])
```

---

## Total Sales per Region

```python
df_multi.groupby(level="Region").sum()
```

---

## Compare Product Sales

```python
df_multi.unstack()
```

---

## Analyze Product Performance

```python
df_multi.xs("Laptop", level="Product")
```

---

## Performance Considerations

MultiIndex operations are efficient but can become expensive with deep hierarchies or large datasets.

### Best practices:

* avoid unnecessary levels
* use meaningful index names
* reset index when needed

Example:

* Dataset size: 5 million rows
* MultiIndex remains efficient due to optimized indexing

---

## Summary

MultiIndex allows Pandas to represent hierarchical datasets within a single DataFrame.

### Key capabilities:

---

### Creating MultiIndex

* `set_index()`
* `MultiIndex.from_arrays()`
* `MultiIndex.from_tuples()`

---

### Accessing Hierarchical Data

* `loc[]`
* `xs()`

---

### Slicing

* `slice()`
* partial indexing

---

### Reshaping

* `stack()`
* `unstack()`

---

### Aggregation

* `groupby(level=...)`
* `sum(level=...)`

---

## Final Note

MultiIndex structures are widely used in:

* financial time series analysis
* business intelligence reporting
* multi-dimensional analytics
* pivot-style data analysis

Mastering MultiIndex enables analysts to work with complex hierarchical datasets efficiently.


