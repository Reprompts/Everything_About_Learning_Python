# Pandas Hands-On Tutorial

## Chapter 11: GroupBy Operations

The GroupBy operation is one of the most powerful and widely used features in Pandas. It allows analysts to split data into groups, perform operations on each group, and combine the results into meaningful summaries.

GroupBy is heavily used in:
- Business analytics
- Financial analysis
- Data science workflows
- Reporting systems
- Data engineering pipelines

It enables answering questions like:
- What is the total sales per region?
- What is the average salary per department?
- Which department has the highest employee count?
- What is the average order value per customer?

---

## The GroupBy Process: Split → Apply → Combine

- **Split** → divide data into groups  
- **Apply** → perform calculations on each group  
- **Combine** → merge results into a final output  

---

## Understanding GroupBy Concept

GroupBy allows operations to be performed separately on subsets of data defined by group keys.

### Example dataset

```python
import pandas as pd

data = {
    "Department": ["HR", "IT", "HR", "IT", "Finance"],
    "Employee": ["Alice", "Bob", "Charlie", "David", "Emma"],
    "Salary": [50000, 60000, 55000, 65000, 70000]
}

df = pd.DataFrame(data)
````

### Dataset

| Department | Employee | Salary |
| ---------- | -------- | ------ |
| HR         | Alice    | 50000  |
| IT         | Bob      | 60000  |
| HR         | Charlie  | 55000  |
| IT         | David    | 65000  |
| Finance    | Emma     | 70000  |

---

## Splitting Data into Groups

The first stage of GroupBy is splitting the dataset into groups based on a column.

```python
grouped = df.groupby("Department")
```

Conceptually:

* HR Group → Alice, Charlie
* IT Group → Bob, David
* Finance Group → Emma

### Viewing groups

```python
grouped.groups
```

Output:

```python
{
 'HR': [0, 2],
 'IT': [1, 3],
 'Finance': [4]
}
```

---

## Applying Operations to Groups

Example: calculate average salary per department

```python
df.groupby("Department")["Salary"].mean()
```

Output:

```
Department
Finance    70000
HR         52500
IT         62500
```

---

## Combining Results

Pandas automatically combines results into a structured output such as a Series or DataFrame.

---

## GroupBy Aggregation

Aggregation summarizes grouped data into statistical values.

### Single aggregation

```python
df.groupby("Department")["Salary"].sum()
```

Output:

```
Finance → 70000
HR → 105000
IT → 125000
```

---

### Multiple aggregations

```python
df.groupby("Department")["Salary"].agg(
    ["sum", "mean", "min", "max"]
)
```

Output:

| Department | Sum    | Mean  | Min   | Max   |
| ---------- | ------ | ----- | ----- | ----- |
| Finance    | 70000  | 70000 | 70000 | 70000 |
| HR         | 105000 | 52500 | 50000 | 55000 |
| IT         | 125000 | 62500 | 60000 | 65000 |

---

### Aggregating multiple columns

```python
df.groupby("Department").agg({
    "Salary": ["mean", "max"],
    "Employee": "count"
})
```

---

## GroupBy Transformation

Transformation returns output with the same shape as the original data.

### Example

```python
df["Dept_Avg_Salary"] = df.groupby("Department")["Salary"].transform("mean")
```

### Result

| Department | Salary | Dept_Avg_Salary |
| ---------- | ------ | --------------- |
| HR         | 50000  | 52500           |
| IT         | 60000  | 62500           |
| HR         | 55000  | 52500           |
| IT         | 65000  | 62500           |
| Finance    | 70000  | 70000           |

---

## GroupBy Filtering

Filtering removes groups that do not meet conditions.

```python
df.groupby("Department").filter(
    lambda x: x["Salary"].mean() > 60000
)
```

---

## Grouping by Multiple Columns

```python
df.groupby(["Department", "Gender"])["Salary"].mean()
```

---

### Example dataset

| Department | Gender | Salary |
| ---------- | ------ | ------ |
| HR         | Female | 50000  |
| IT         | Male   | 60000  |
| HR         | Male   | 55000  |
| IT         | Male   | 65000  |
| Finance    | Female | 70000  |

---

### Output

| Department | Gender | Avg Salary |
| ---------- | ------ | ---------- |
| Finance    | Female | 70000      |
| HR         | Female | 50000      |
| HR         | Male   | 55000      |
| IT         | Male   | 62500      |

---

## Hierarchical Grouping

Grouping by multiple columns creates a MultiIndex structure.

Example:

```
Department   Gender
Finance      Female
HR           Female
             Male
IT           Male
```

---

### Resetting index

```python
df.groupby(["Department", "Gender"])["Salary"].mean().reset_index()
```

---

## Real-World Business Example

### Sales dataset

| Region | Product | Sales |
| ------ | ------- | ----- |
| US     | A       | 5000  |
| US     | B       | 7000  |
| EU     | A       | 6000  |
| EU     | B       | 4000  |

---

### Total sales per region

```python
df.groupby("Region")["Sales"].sum()
```

---

### Average sales per product per region

```python
df.groupby(["Region", "Product"])["Sales"].mean()
```

---

### Sales share within region

```python
df["Regional_Total"] = df.groupby("Region")["Sales"].transform("sum")
df["Share"] = df["Sales"] / df["Regional_Total"]
```

---

## Advanced GroupBy Operations

### Selecting specific columns

```python
df.groupby("Department")[["Salary"]].mean()
```

---

### Iterating over groups

```python
for dept, group in df.groupby("Department"):
    print(dept)
    print(group)
```

---

### Getting a specific group

```python
df.groupby("Department").get_group("HR")
```

---

## Performance Considerations

GroupBy operations are optimized but can be expensive on large datasets.

### Best practices:

* Group only necessary columns
* Avoid repeated group operations
* Use vectorized aggregation functions

Example:

* Dataset size: 10 million rows
* GroupBy remains efficient due to C-level optimizations

---

## Summary

GroupBy is one of the most powerful analytical tools in Pandas.

It allows analysts to:

* Split datasets into logical groups
* Perform statistical operations per group
* Combine results into meaningful summaries

### Key techniques:

* `groupby()` splitting
* Aggregations (`sum`, `mean`, `count`)
* `transform()` operations
* Filtering groups
* Multi-column grouping
* Hierarchical indexing

### Common use cases:

* Business intelligence dashboards
* Financial analytics
* Marketing analysis
* Operational reporting
* Machine learning feature engineering

Mastering GroupBy enables powerful data summarization and advanced analytical workflows.
