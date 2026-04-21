
# Pandas Hands-On Tutorial  
## Chapter 16: Time & Date Operations

Many real-world datasets involve time-based information, such as:

- financial transactions  
- stock prices  
- website traffic logs  
- IoT sensor readings  
- sales data over time  
- weather observations  

Analyzing such datasets requires powerful tools to parse, manipulate, and analyze datetime data.

Pandas provides a robust datetime system built on top of NumPy's time functionality. It allows:

- converting text to datetime objects  
- indexing data by time  
- filtering datasets using time conditions  
- performing date arithmetic  
- extracting date components  
- resampling time series data  
- handling time zones  

Datetime operations are fundamental in time series analysis, financial analytics, and business reporting.

---

## This Article Explains

- Working with datetime data types  
- Converting data to datetime  
- Datetime indexing  
- Filtering data using dates  
- Date arithmetic operations  
- Extracting date components  
- Resampling time series data  
- Handling time zones  

---

## Working with Datetime Data Types

Pandas uses the `datetime64[ns]` data type for representing dates and timestamps.

### Example dataset

```python
import pandas as pd

data = {
    "Date": ["2024-01-01", "2024-01-02", "2024-01-03"],
    "Sales": [500, 700, 650]
}

df = pd.DataFrame(data)
````

### Initial dataset

| Date       | Sales |
| ---------- | ----- |
| 2024-01-01 | 500   |
| 2024-01-02 | 700   |
| 2024-01-03 | 650   |

At this stage, the Date column is stored as a string.

```python
df.dtypes
```

Output:

```
Date     object
Sales    int64
```

---

## Converting Data to Datetime

```python
df["Date"] = pd.to_datetime(df["Date"])
```

Now:

```
Date     datetime64[ns]
Sales    int64
```

---

## Parsing Different Date Formats

Real-world data often contains inconsistent formats:

* 01/02/2024
* 2024-03-01
* March 5, 2024

Pandas can automatically parse them:

```python
pd.to_datetime(df["Date"])
```

### Specifying format

```python
pd.to_datetime(df["Date"], format="%Y-%m-%d")
```

### Format codes

* %Y → year
* %m → month
* %d → day
* %H → hour
* %M → minute
* %S → second

---

## Datetime Indexing

Datetime indexing allows using dates as the DataFrame index.

```python
df.set_index("Date", inplace=True)
```

### Result

| Date       | Sales |
| ---------- | ----- |
| 2024-01-01 | 500   |
| 2024-01-02 | 700   |
| 2024-01-03 | 650   |

```python
type(df.index)
```

Output:

```
pandas.core.indexes.datetimes.DatetimeIndex
```

---

## Filtering Data Using Dates

### Filter specific date

```python
df.loc["2024-01-02"]
```

---

### Filter date range

```python
df.loc["2024-01-01":"2024-01-03"]
```

---

### Filter by month

```python
df.loc["2024-01"]
```

---

## Date Arithmetic Operations

```python
df.index + pd.Timedelta(days=1)
```

Adds one day to each date.

---

## Calculating Date Differences

### Example dataset

| StartDate  | EndDate    |
| ---------- | ---------- |
| 2024-01-01 | 2024-01-10 |

```python
df["Duration"] = df["EndDate"] - df["StartDate"]
```

Result:

```
Duration = 9 days
```

---

## Extracting Date Components

Use `.dt` accessor.

### Example dataset

| Date       |
| ---------- |
| 2024-01-01 |
| 2024-02-10 |
| 2024-03-15 |

---

### Extract year

```python
df["Year"] = df["Date"].dt.year
```

### Extract month

```python
df["Month"] = df["Date"].dt.month
```

### Extract day

```python
df["Day"] = df["Date"].dt.day
```

### Extract weekday

```python
df["Weekday"] = df["Date"].dt.day_name()
```

Output:

```
Monday
Saturday
Friday
```

### Extract quarter

```python
df["Quarter"] = df["Date"].dt.quarter
```

### Extract week number

```python
df["Week"] = df["Date"].dt.isocalendar().week
```

---

## Resampling Time Series Data

Resampling means changing frequency of time series data:

* daily → monthly
* hourly → daily
* minute → hourly

---

### Example dataset

| Date       | Sales |
| ---------- | ----- |
| 2024-01-01 | 500   |
| 2024-01-02 | 700   |
| 2024-01-03 | 650   |
| 2024-01-04 | 800   |

---

### Monthly aggregation

```python
df.resample("M").sum()
```

---

### Weekly aggregation

```python
df.resample("W").mean()
```

---

### Frequency codes

* D → daily
* W → weekly
* M → monthly
* Q → quarterly
* Y → yearly
* H → hourly
* T → minute

---

### Multiple aggregations

```python
df.resample("M").agg(["sum", "mean", "max"])
```

---

## Handling Time Zones

```python
df["Date"] = pd.to_datetime(df["Date"])
df["Date"] = df["Date"].dt.tz_localize("UTC")
```

---

## Converting Time Zones

```python
df["Date"].dt.tz_convert("Asia/Kolkata")
```

---

## Removing Time Zone

```python
df["Date"].dt.tz_localize(None)
```

---

## Real-World Example: Sales Time Analysis

### Dataset

| Date       | Sales |
| ---------- | ----- |
| 2024-01-01 | 500   |
| 2024-01-02 | 700   |
| 2024-02-01 | 650   |

---

### Convert date column

```python
df["Date"] = pd.to_datetime(df["Date"])
```

---

### Monthly sales

```python
df.set_index("Date").resample("M").sum()
```

---

### Sales by weekday

```python
df["Weekday"] = df["Date"].dt.day_name()
df.groupby("Weekday")["Sales"].sum()
```

---

## Performance Considerations

Datetime operations are optimized but can be expensive for large datasets.

### Best practices:

* convert date columns early
* use datetime indexes
* avoid repeated conversions

Example:

* Dataset size: 10 million rows
* Operations remain efficient due to vectorization

---

## Summary

Datetime operations are essential for analyzing time-based datasets.

---

### Key capabilities

#### Converting to datetime

```python
pd.to_datetime()
```

#### Datetime indexing

```python
set_index("Date")
```

#### Filtering by date

```python
df.loc["2024-01"]
df.loc["2024-01-01":"2024-01-10"]
```

#### Date arithmetic

* Timedelta operations
* date differences

#### Extracting components

```python
.dt.year
.dt.month
.dt.day
.dt.day_name()
```

#### Resampling time series

```python
resample("M")
resample("W")
resample("D")
```

#### Time zones

```python
tz_localize()
tz_convert()
```

---

## Final Note

These techniques are widely used in:

* financial market analysis
* sales trend analysis
* website traffic analytics
* IoT sensor monitoring
* forecasting and machine learning

Mastering datetime operations allows analysts to unlock powerful insights from time-based datasets.

