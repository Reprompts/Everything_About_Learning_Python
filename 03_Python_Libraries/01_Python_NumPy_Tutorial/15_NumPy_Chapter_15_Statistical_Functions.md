
# NumPy Hands-On Tutorial  

## Chapter 15: Statistical Functions

Statistical analysis is one of the most common tasks in data science, machine learning, engineering, finance, and scientific computing. NumPy provides a powerful set of statistical functions that operate efficiently on arrays.

These functions allow you to compute:

- Central tendency (mean, median, mode concept)
- Data spread (variance, standard deviation)
- Extremes (minimum and maximum values)
- Distribution metrics (percentiles, quantiles)
- Relationships between variables (correlation)

Because NumPy arrays are vectorized and optimized in C, statistical calculations on large datasets are much faster than traditional Python loops.

---

## Mean

The mean (also called the average) is the sum of all values divided by the number of values.  
It represents the central value of the dataset.

### Mathematical Formula
```

Mean = (x₁ + x₂ + x₃ + … + xₙ) / n

````

### NumPy Function
```python
np.mean()
````

### Example

```python
import numpy as np

data = np.array([10, 20, 30, 40, 50])
result = np.mean(data)
print(result)
```

**Output**

```
30.0
```

**Explanation**

```
(10 + 20 + 30 + 40 + 50) / 5 = 30
```

### Mean Along Axis

```python
data = np.array([
    [10,20,30],
    [40,50,60]
])

print(np.mean(data, axis=0))
```

**Output**

```
[25. 35. 45.]
```

---

## Median

The median is the middle value of a sorted dataset.
It is useful when datasets contain outliers.

### Function

```python
np.median()
```

### Example

```python
data = np.array([3,7,2,9,5])
print(np.median(data))
```

**Output**

```
5.0
```

**Sorted Data**

```
[2,3,5,7,9]
```

---

### Median with Even Number of Values

```python
data = np.array([1,2,3,4])
print(np.median(data))
```

**Output**

```
2.5
```

---

## Mode (Concept)

The mode is the value that appears most frequently in a dataset.

NumPy does not have a direct `mode()` function.

### Using NumPy

```python
data = np.array([1,2,2,3,3,3,4])

values, counts = np.unique(data, return_counts=True)
mode = values[np.argmax(counts)]

print(mode)
```

**Output**

```
3
```

---

## Standard Deviation

Measures how much the data varies from the mean.

* Small → values close to mean
* Large → values spread out

### Formula

```
σ = sqrt( (1/N) * Σ (xᵢ - μ)² )
```

### Function

```python
np.std()
```

### Example

```python
data = np.array([10,12,14,16,18])
print(np.std(data))
```

**Output**

```
2.828
```

---

## Variance

Variance measures spread of data.

### Formula

```
Variance = (1/N) * Σ (xᵢ - μ)²
```

### Function

```python
np.var()
```

### Example

```python
data = np.array([2,4,6,8])
print(np.var(data))
```

**Output**

```
5.0
```

---

## Minimum Values

### Function

```python
np.min()
```

### Example

```python
data = np.array([15, 22, 8, 34, 19])
print(np.min(data))
```

**Output**

```
8
```

---

## Maximum Values

### Function

```python
np.max()
```

### Example

```python
data = np.array([10,25,40,5])
print(np.max(data))
```

**Output**

```
40
```

---

## Percentiles

Percentiles divide a dataset into 100 equal parts.

* 25th → lower quartile
* 50th → median
* 75th → upper quartile

### Function

```python
np.percentile()
```

### Example

```python
data = np.array([10,20,30,40,50])
print(np.percentile(data, 50))
```

**Output**

```
30
```

### Multiple Percentiles

```python
print(np.percentile(data, [25,50,75]))
```

**Output**

```
[20. 30. 40.]
```

---

## Quantiles

Quantiles divide data into equal-sized intervals.

| Quantile | Description     |
| -------- | --------------- |
| 0.25     | 25th percentile |
| 0.50     | Median          |
| 0.75     | 75th percentile |

### Function

```python
np.quantile()
```

### Example

```python
data = np.array([10,20,30,40,50])
print(np.quantile(data, 0.5))
```

**Output**

```
30
```

---

## Correlation

Measures relationship between variables.

* 1 → perfect positive
* 0 → no relationship
* -1 → perfect negative

### Function

```python
np.corrcoef()
```

### Positive Correlation

```python
x = np.array([1,2,3,4,5])
y = np.array([2,4,6,8,10])

print(np.corrcoef(x,y))
```

**Output**

```
[[1. 1.]
 [1. 1.]]
```

---

### Negative Correlation

```python
x = np.array([1,2,3,4,5])
y = np.array([10,8,6,4,2])

print(np.corrcoef(x,y))
```

**Output**

```
[[ 1. -1.]
 [-1.  1.]]
```

---

## Practical Example: Full Statistical Analysis

```python
data = np.array([10,20,30,40,50,60])

print("Mean:", np.mean(data))
print("Median:", np.median(data))
print("Standard Deviation:", np.std(data))
print("Variance:", np.var(data))
print("Min:", np.min(data))
print("Max:", np.max(data))
print("25th Percentile:", np.percentile(data, 25))
print("75th Percentile:", np.percentile(data, 75))
```

**Output**

```
Mean: 35
Median: 35
Standard Deviation: 17.078
Variance: 291.66
Min: 10
Max: 60
25th Percentile: 22.5
75th Percentile: 47.5
```

---

## Summary

NumPy statistical functions allow fast and efficient analysis of numerical datasets.

### Key Functions

| Category         | Functions            |
| ---------------- | -------------------- |
| Central tendency | mean, median         |
| Frequency        | mode concept         |
| Spread           | std, var             |
| Extremes         | min, max             |
| Distribution     | percentile, quantile |
| Relationship     | corrcoef             |

---

## Advantages

* Fast numerical computation
* Efficient for large datasets
* Vectorized operations
* Easy integration in scientific workflows

---

## Applications

* Data science
* Machine learning
* Financial analytics
* Scientific simulations

