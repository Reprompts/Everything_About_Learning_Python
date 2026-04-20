# NumPy Hands-On Tutorial

## Chapter 8: Boolean Indexing & Conditional Selection

In real-world data analysis, we rarely want to process every element in an array. Instead, we often need to:

* Filter values
* Extract elements that meet conditions
* Remove invalid values
* Apply operations only to specific data

Boolean indexing allows us to perform these tasks efficiently.

It is widely used in:

* Data science
* Machine learning
* Data cleaning
* Statistical analysis

---

## 1. Boolean Arrays

A boolean array contains only `True` or `False` values.

### Example

```python
import numpy as np
arr = np.array([10, 20, 30, 40, 50])
condition = arr > 25
print(condition)
```

### Output

```
[False False  True  True  True]
```

---

## 2. Filtering Arrays with Conditions

### Example

```python
arr = np.array([10,20,30,40,50])
filtered = arr[arr > 25]
print(filtered)
```

### Output

```
[30 40 50]
```

### Explanation

* `arr > 25` → `[False False True True True]`
* Only `True` values are selected

---

## 3. Selecting Elements Using Conditions

### Supported Operators

| Operator | Meaning          |
| -------- | ---------------- |
| >        | Greater than     |
| <        | Less than        |
| >=       | Greater or equal |
| <=       | Less or equal    |
| ==       | Equal            |
| !=       | Not equal        |

### Example: Even Numbers

```python
arr = np.array([1,2,3,4,5,6])
even_numbers = arr[arr % 2 == 0]
print(even_numbers)
```

### Output

```
[2 4 6]
```

### Example: Values > 50

```python
arr = np.array([10,55,32,70,90,25])
result = arr[arr > 50]
print(result)
```

### Output

```
[55 70 90]
```

---

## 4. Boolean Indexing with 2D Arrays

### Example

```python
matrix = np.array([
    [10,20,30],
    [40,50,60],
    [70,80,90]
])

result = matrix[matrix > 50]
print(result)
```

### Output

```
[60 70 80 90]
```

---

## 5. Combining Multiple Conditions

Use bitwise operators:

* `&` → AND
* `|` → OR
* `~` → NOT

### Example: AND

```python
arr = np.array([10,20,30,40,50,60])
result = arr[(arr > 20) & (arr < 50)]
print(result)
```

### Output

```
[30 40]
```

### Example: OR

```python
result = arr[(arr < 20) | (arr > 50)]
print(result)
```

### Output

```
[10 60]
```

### Example: NOT

```python
result = arr[~(arr > 30)]
print(result)
```

### Output

```
[10 20 30]
```

---

## 6. Using Logical Functions

### logical_and()

```python
arr = np.array([10,20,30,40,50])
condition = np.logical_and(arr > 20, arr < 50)
print(arr[condition])
```

### Output

```
[30 40]
```

### logical_or()

```python
condition = np.logical_or(arr < 20, arr > 40)
print(arr[condition])
```

### Output

```
[10 50]
```

### logical_not()

```python
condition = np.logical_not(arr > 30)
print(arr[condition])
```

### Output

```
[10 20 30]
```

---

## 7. Updating Values Using Boolean Indexing

### Example: Replace Values > 50

```python
arr = np.array([10,40,60,80])
arr[arr > 50] = 100
print(arr)
```

### Output

```
[ 10  40 100 100]
```

### Example: Replace Negative Values

```python
arr = np.array([-3,5,-1,7,-8])
arr[arr < 0] = 0
print(arr)
```

### Output

```
[0 5 0 7 0]
```

---

## 8. Practical Data Filtering Example

```python
marks = np.array([35,60,75,40,90,55])
```

### Passed Students

```python
passed = marks[marks >= 40]
print(passed)
```

### Output

```
[60 75 40 90 55]
```

### Distinction Students

```python
distinction = marks[marks >= 75]
print(distinction)
```

### Output

```
[75 90]
```

### Marks Between 50 and 80

```python
result = marks[(marks >= 50) & (marks <= 80)]
print(result)
```

### Output

```
[60 75 55]
```

---

## 9. Performance Advantage

Boolean indexing is much faster than loops.

Instead of:

```python
for value in array:
    if condition:
        # append
```

NumPy uses vectorized operations internally.

### Benefits

* Faster execution
* Cleaner code
* Efficient memory usage

---

## Summary

| Concept                   | Description                       |
| ------------------------- | --------------------------------- |
| Boolean arrays            | Arrays of True/False values       |
| Conditional filtering     | Extract elements meeting criteria |
| Element selection         | Use comparison operators          |
| Multi-condition filtering | Combine conditions with &, |      |
| Logical operations        | Use NumPy logical functions       |
| Data modification         | Update values based on conditions |

---

## Conclusion

Boolean indexing is essential for:

* Data cleaning
* Feature selection
* Machine learning preprocessing
* Statistical analysis

Mastering it enables efficient filtering and manipulation of large datasets.

---
