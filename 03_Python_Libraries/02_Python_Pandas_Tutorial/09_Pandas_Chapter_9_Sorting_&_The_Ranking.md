
# Pandas Hands-On Tutorial

## Chapter 9: Sorting & Ranking

Sorting and ranking are essential operations in data analysis and data exploration. In real-world datasets, analysts frequently need to organize data in a meaningful order to answer questions such as:

- Who are the top-performing employees?  
- Which products generated the highest revenue?  
- What are the lowest sales regions?  
- Which entries appear first or last chronologically?  

Sorting helps organize data, while ranking assigns positions to values based on importance.

---

## Topics Covered

- Sorting by index  
- Sorting by column values  
- Sorting by multiple columns  
- Ranking values  
- Sorting with custom rules  

---

## Sorting by Index

### Example Dataset

```python
import pandas as pd

data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Salary": [50000, 60000, 55000]
}

df = pd.DataFrame(data, index=[2, 0, 1])
print(df)
````

---

### Sort by Index

```python
df.sort_index()
```

---

### Descending Order

```python
df.sort_index(ascending=False)
```

---

### Sort Columns by Index

```python
df.sort_index(axis=1)
```

---

## Sorting by Column Values

### Example Dataset

| Name    | Age | Salary |
| ------- | --- | ------ |
| Alice   | 25  | 50000  |
| Bob     | 30  | 60000  |
| Charlie | 35  | 55000  |

---

### Sort by One Column

```python
df.sort_values(by="Salary")
```

---

### Descending Order

```python
df.sort_values(by="Salary", ascending=False)
```

---

### Modify Original Data

```python
df.sort_values(by="Salary", inplace=True)
```

---

## Sorting by Multiple Columns

### Example Dataset

| Name    | Department | Salary |
| ------- | ---------- | ------ |
| Alice   | HR         | 50000  |
| Bob     | IT         | 60000  |
| Charlie | HR         | 55000  |
| David   | IT         | 52000  |

---

### Multi-Level Sorting

```python
df.sort_values(by=["Department", "Salary"])
```

---

### Different Sort Orders

```python
df.sort_values(
    by=["Department", "Salary"],
    ascending=[True, False]
)
```

---

## Ranking Values

### Example Dataset

| Employee | Sales |
| -------- | ----- |
| A        | 100   |
| B        | 200   |
| C        | 150   |

---

### Basic Ranking

```python
df["Rank"] = df["Sales"].rank()
```

---

### Descending Ranking

```python
df["Rank"] = df["Sales"].rank(ascending=False)
```

---

## Handling Ties

### Average (default)

```python
df["Sales"].rank(method="average")
```

---

### Minimum Rank

```python
df["Sales"].rank(method="min")
```

---

### Maximum Rank

```python
df["Sales"].rank(method="max")
```

---

### First Appearance

```python
df["Sales"].rank(method="first")
```

---

## Ranking Within Groups

### Example

```python
df["Rank"] = df.groupby("Department")["Salary"].rank(ascending=False)
```

---

## Sorting with Custom Rules

### Example Dataset

| Priority |
| -------- |
| Low      |
| Medium   |
| High     |

---

### Custom Sorting Order

```python
priority_order = {
    "High": 1,
    "Medium": 2,
    "Low": 3
}

df.sort_values(
    by="Priority",
    key=lambda x: x.map(priority_order)
)
```

---

## Sorting Text Data

```python
df.sort_values(by="Name")
```

---

## Sorting Dates

```python
df.sort_values(by="Date")
```

---

## Real-World Example

### Example Dataset

| Product | Region | Revenue |
| ------- | ------ | ------- |
| A       | US     | 5000    |
| B       | EU     | 8000    |
| C       | US     | 6000    |
| D       | EU     | 4000    |

---

### Sort by Region and Revenue

```python
df.sort_values(by=["Region", "Revenue"], ascending=[True, False])
```

---

### Rank by Revenue

```python
df["Rank"] = df["Revenue"].rank(ascending=False)
```

---

## Performance Considerations

Sorting can be expensive for large datasets.

### Best Practices

* Sort only when necessary
* Use indexing for faster lookups
* Avoid repeated sorting operations

---

## Summary

Sorting and ranking are fundamental operations for organizing and analyzing datasets.

### Key Techniques

* Sort rows by index
* Sort by column values
* Sort using multiple columns
* Assign rank positions
* Apply custom sorting rules

### Applications

* Identify top performers
* Organize reports
* Prioritize business metrics
* Compare values effectively

Sorting and ranking are widely used in:

* Business intelligence dashboards
* Financial analysis
* Ranking systems
* Recommendation systems
* Performance analytics

Mastering these techniques enables efficient data exploration and better decision-making.


