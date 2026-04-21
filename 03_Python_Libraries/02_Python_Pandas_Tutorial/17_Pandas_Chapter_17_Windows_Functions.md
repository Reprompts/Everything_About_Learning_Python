
# Pandas Hands-On Tutorial  
## Chapter 17: Window Functions

In many real-world analytical problems, calculations are not performed on the entire dataset at once, but rather on subsets of data defined by a window.

### Examples include:
- calculating moving averages of stock prices  
- computing running totals of sales  
- analyzing trends in time series data  
- smoothing noisy sensor data  
- computing cumulative statistics  

These operations are known as **window functions**.

Window functions allow calculations over a set of rows that move across the dataset.

Pandas provides powerful tools for window-based calculations:
- Rolling windows  
- Expanding windows  
- Moving averages  
- Cumulative calculations  

These techniques are widely used in:
- financial analysis  
- time series forecasting  
- signal processing  
- machine learning feature engineering  
- business trend analysis  

---

## Understanding Window Functions

A window function performs calculations across a group of rows related to the current row.

### Example dataset

| Day | Sales |
|-----|-------|
| 1   | 100   |
| 2   | 200   |
| 3   | 300   |
| 4   | 400   |
| 5   | 500   |

Instead of computing statistics across the entire dataset, we compute values within a sliding window.

### Example: 3-day moving average

- Day 3 → average of (1,2,3)  
- Day 4 → average of (2,3,4)  
- Day 5 → average of (3,4,5)  

This is called a **rolling window calculation**.

---

## Rolling Window Calculations

Rolling windows perform calculations over a fixed-size sliding window.

### Syntax

```python
df.rolling(window_size)
````

---

### Example dataset

```python
import pandas as pd

data = {
    "Day": [1, 2, 3, 4, 5],
    "Sales": [100, 200, 300, 400, 500]
}

df = pd.DataFrame(data)
```

---

### Rolling Mean

```python
df["RollingMean"] = df["Sales"].rolling(window=3).mean()
```

| Day | Sales | RollingMean |
| --- | ----- | ----------- |
| 1   | 100   | NaN         |
| 2   | 200   | NaN         |
| 3   | 300   | 200         |
| 4   | 400   | 300         |
| 5   | 500   | 400         |

---

### Rolling Sum

```python
df["RollingSum"] = df["Sales"].rolling(window=3).sum()
```

| Day | Sales | RollingSum |
| --- | ----- | ---------- |
| 1   | 100   | NaN        |
| 2   | 200   | NaN        |
| 3   | 300   | 600        |
| 4   | 400   | 900        |
| 5   | 500   | 1200       |

---

### Rolling Standard Deviation

```python
df["RollingStd"] = df["Sales"].rolling(window=3).std()
```

Used for volatility analysis in finance.

---

## Handling Missing Window Values

First rows may produce `NaN` because the window is not fully filled.

### Solution: `min_periods`

```python
df["RollingMean"] = df["Sales"].rolling(window=3, min_periods=1).mean()
```

| Day | RollingMean |
| --- | ----------- |
| 1   | 100         |
| 2   | 150         |
| 3   | 200         |
| 4   | 300         |
| 5   | 400         |

---

## Expanding Window Operations

Expanding windows compute statistics from the start of the dataset up to the current row.

### Example

* Day 1 → mean(100)
* Day 2 → mean(100,200)
* Day 3 → mean(100,200,300)
* Day 4 → mean(100,200,300,400)

---

### Expanding Mean

```python
df["ExpandingMean"] = df["Sales"].expanding().mean()
```

| Day | Sales | ExpandingMean |
| --- | ----- | ------------- |
| 1   | 100   | 100           |
| 2   | 200   | 150           |
| 3   | 300   | 200           |
| 4   | 400   | 250           |
| 5   | 500   | 300           |

---

### Expanding Sum

```python
df["ExpandingSum"] = df["Sales"].expanding().sum()
```

| Day | Sales | ExpandingSum |
| --- | ----- | ------------ |
| 1   | 100   | 100          |
| 2   | 200   | 300          |
| 3   | 300   | 600          |
| 4   | 400   | 1000         |
| 5   | 500   | 1500         |

---

## Moving Averages

Moving averages smooth fluctuations in data.

### Common uses:

* stock market analysis
* sales trend analysis
* economic forecasting

---

### Simple Moving Average (SMA)

```python
df["SMA"] = df["Sales"].rolling(window=3).mean()
```

---

### Weighted Moving Average (WMA)

```python
import numpy as np

weights = np.array([0.1, 0.3, 0.6])

df["WMA"] = df["Sales"].rolling(3).apply(
    lambda x: (weights * x).sum()
)
```

---

### Exponential Moving Average (EMA)

```python
df["EMA"] = df["Sales"].ewm(span=3).mean()
```

EMA reacts faster to recent changes.

---

## Cumulative Calculations

Cumulative functions compute progressive values across rows.

Used in:

* finance
* accounting
* analytics dashboards

---

### Cumulative Sum

```python
df["CumulativeSales"] = df["Sales"].cumsum()
```

| Day | Sales | CumulativeSales |
| --- | ----- | --------------- |
| 1   | 100   | 100             |
| 2   | 200   | 300             |
| 3   | 300   | 600             |
| 4   | 400   | 1000            |
| 5   | 500   | 1500            |

---

### Cumulative Max

```python
df["CumMax"] = df["Sales"].cummax()
```

### Cumulative Min

```python
df["CumMin"] = df["Sales"].cummin()
```

### Cumulative Product

```python
df["CumProd"] = df["Sales"].cumprod()
```

Used in financial return calculations.

---

## Time-Based Rolling Windows

Rolling windows can also be time-based.

### Example dataset

| Date       | Sales |
| ---------- | ----- |
| 2024-01-01 | 100   |
| 2024-01-02 | 200   |
| 2024-01-03 | 300   |

---

### Convert to datetime index

```python
df["Date"] = pd.to_datetime(df["Date"])
df.set_index("Date", inplace=True)
```

---

### 3-day rolling average

```python
df["RollingAvg"] = df["Sales"].rolling("3D").mean()
```

---

## Real-World Example: Stock Market Analysis

| Date  | Price |
| ----- | ----- |
| Jan 1 | 100   |
| Jan 2 | 105   |
| Jan 3 | 110   |
| Jan 4 | 108   |
| Jan 5 | 115   |

```python
df["MA5"] = df["Price"].rolling(5).mean()
df["MA10"] = df["Price"].rolling(10).mean()
```

Traders use moving average crossovers to detect trends.

---

## Real-World Example: Sales Trend Analysis

| Day | Sales |
| --- | ----- |
| 1   | 100   |
| 2   | 200   |
| 3   | 300   |
| 4   | 250   |
| 5   | 400   |

```python
df["SalesTrend"] = df["Sales"].rolling(3).mean()
```

This smooths fluctuations and reveals trends.

---

## Performance Considerations

Window operations are optimized but can be expensive for very large datasets.

### Best practices:

* avoid unnecessary window sizes
* use vectorized functions
* avoid Python loops

Example:

* Dataset size: 10 million rows
* Pandas remains efficient due to optimized implementation

---

## Summary

Window functions are powerful tools for analyzing trends and patterns in sequential data.

---

### Rolling Windows

```python
rolling()
```

Used for sliding window calculations:

* rolling mean
* rolling sum
* rolling std

---

### Expanding Windows

```python
expanding()
```

Calculations from start of dataset:

* expanding mean
* expanding sum

---

### Moving Averages

* SMA
* WMA
* EMA

Used heavily in time series analysis and finance.

---

### Cumulative Calculations

* `cumsum()`
* `cummax()`
* `cummin()`
* `cumprod()`

Used for running totals and progressive statistics.

---

## Final Note

Window functions are widely used in:

* financial analytics
* sales forecasting
* economic modeling
* sensor data smoothing
* machine learning feature engineering

Mastering these techniques enables analysts to extract trends, smooth noise, and understand temporal patterns in data.


