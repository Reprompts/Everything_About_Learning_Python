
# Pandas Hands-On Tutorial

## Chapter 10: Data Aggregation

Data aggregation is a fundamental concept in data analysis and business intelligence. Aggregation refers to summarizing large datasets into meaningful numerical metrics.

Instead of examining individual records, analysts compute:

- Total sales  
- Average salary  
- Minimum and maximum values  
- Count of observations  
- Variability of data  

These statistics transform raw data into actionable insights.

---

## Topics Covered

- Aggregation functions  
- Sum operations  
- Mean calculations  
- Median calculations  
- Counting elements  
- Minimum values  
- Maximum values  
- Standard deviation  
- Variance  
- Custom aggregation functions  

---

## Aggregation Functions

### Example Dataset

```python
import pandas as pd

data = {
    "Employee": ["Alice", "Bob", "Charlie", "David", "Emma"],
    "Salary": [50000, 60000, 55000, 65000, 70000],
    "Age": [25, 30, 28, 35, 32]
}

df = pd.DataFrame(data)
````

---

### Common Functions

| Function   | Purpose            |
| ---------- | ------------------ |
| `sum()`    | total value        |
| `mean()`   | average value      |
| `median()` | middle value       |
| `count()`  | number of values   |
| `min()`    | smallest value     |
| `max()`    | largest value      |
| `std()`    | standard deviation |
| `var()`    | variance           |

---

## Sum Operations

### Column Sum

```python
df["Salary"].sum()
```

---

### Multiple Columns

```python
df.sum()
```

---

### Row-wise Sum

```python
df[["Salary", "Age"]].sum(axis=1)
```

---

## Mean Calculations

### Example

```python
df["Salary"].mean()
```

---

### Mean Across Columns

```python
df.mean(numeric_only=True)
```

---

## Median Calculations

### Example

```python
df["Salary"].median()
```

---

### Why Median Matters

* Resistant to outliers
* More accurate for skewed data

---

## Counting Elements

### Count Non-Missing Values

```python
df["Salary"].count()
```

---

### Unique Count

```python
df["Salary"].nunique()
```

---

## Minimum Values

```python
df["Salary"].min()
```

---

## Maximum Values

```python
df["Salary"].max()
```

---

## Standard Deviation

Measures spread of data.

```python
df["Salary"].std()
```

---

## Variance

Measures variability.

```python
df["Salary"].var()
```

---

### Relationship

```
std = sqrt(variance)
```

---

## Custom Aggregation Functions

### Example Function

```python
def salary_range(series):
    return series.max() - series.min()
```

---

### Apply Custom Function

```python
salary_range(df["Salary"])
```

---

### Using `agg()`

```python
df["Salary"].agg(salary_range)
```

---

## Multiple Aggregations

```python
df["Salary"].agg(["min", "max", "mean", "median"])
```

---

## Aggregating Multiple Columns

```python
df.agg({
    "Salary": ["min", "max", "mean"],
    "Age": ["mean", "median"]
})
```

---

## Real-World Example

```python
df["Revenue"].agg([
    "sum",
    "mean",
    "min",
    "max",
    "std"
])
```

---

## Business Use Cases

### Retail

* Total sales per month
* Average order value
* Maximum purchase amount

### Finance

* Average portfolio return
* Maximum loss
* Variance of stock prices

### HR Analytics

* Average salary
* Salary distribution
* Employee count

---

## Performance Best Practices

* Use built-in aggregation functions
* Avoid manual loops
* Combine operations using `agg()`

---

## Summary

Data aggregation transforms raw data into meaningful summaries.

### Key Operations

* `sum()` → totals
* `mean()` → averages
* `median()` → central value
* `count()` → count values
* `min()` / `max()` → extremes
* `std()` → dispersion
* `var()` → variance
* custom functions → advanced logic

### Benefits

* Summarize large datasets
* Detect patterns
* Measure variability
* Generate insights

### Applications

* Business intelligence dashboards
* Financial analysis
* Statistical modeling
* Machine learning pipelines

