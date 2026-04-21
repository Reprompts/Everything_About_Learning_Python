
# Pandas Hands-On Tutorial

## Chapter 20: Data Visualization 

Data visualization is one of the most important parts of data analysis and data science. Raw numbers and tables are often difficult to interpret, but visual representations help reveal:
- patterns  
- trends  
- distributions  
- relationships between variables  
- anomalies or outliers  

Pandas includes built-in plotting capabilities that integrate with Matplotlib, one of Python's most powerful visualization libraries. This means that with very little code, Pandas can generate many types of charts directly from a DataFrame.

Data visualization in Pandas is commonly used in:
- exploratory data analysis (EDA)  
- business reporting dashboards  
- financial trend analysis  
- machine learning feature understanding  
- academic research  

This article explains in extremely detailed practical depth:
- Built-in plotting features  
- Line plots  
- Bar charts  
- Histograms  
- Scatter plots  
- Box plots  

---

## Built-in Plotting Features in Pandas

Pandas provides a convenient plotting interface through the `plot()` method.

Example:

```python
import pandas as pd
import matplotlib.pyplot as plt

data = {
    "Year": [2019, 2020, 2021, 2022],
    "Sales": [200, 250, 300, 350]
}

df = pd.DataFrame(data)

df.plot(x="Year", y="Sales")
plt.show()
````

This automatically generates a line chart.

---

## Plotting Syntax

Basic syntax:

```python
df.plot(kind="chart_type")
```

Common chart types supported:

| Chart Type | Description                    |
| ---------- | ------------------------------ |
| Line       | Trend over time                |
| Bar        | Categorical comparison         |
| Barh       | Horizontal bar chart           |
| Hist       | Distribution                   |
| Scatter    | Relationship between variables |
| Box        | Distribution and outliers      |
| Area       | Cumulative trends              |
| Pie        | Proportion of categories       |

---

## Plotting Multiple Columns

Example dataset:

| Year | Sales | Profit |
| ---- | ----- | ------ |
| 2019 | 200   | 50     |
| 2020 | 250   | 60     |
| 2021 | 300   | 75     |
| 2022 | 350   | 90     |

Plot both columns:

```python
df.plot(x="Year", y=["Sales", "Profit"])
plt.show()
```

This generates multiple lines in the same graph.

---

## Line Plots

Line plots are used to visualize trends over continuous data, especially time series.

Common uses:

* stock price trends
* sales growth over years
* temperature changes
* website traffic over time

Example:

```python
data = {
    "Month": ["Jan", "Feb", "Mar", "Apr", "May"],
    "Sales": [100, 150, 200, 250, 300]
}

df = pd.DataFrame(data)

df.plot(x="Month", y="Sales", kind="line")
plt.show()
```

---

## Multiple Line Plot

```python
data = {
    "Month": ["Jan", "Feb", "Mar", "Apr"],
    "ProductA": [100, 120, 150, 180],
    "ProductB": [80, 110, 130, 170]
}

df = pd.DataFrame(data)

df.plot(x="Month")
plt.show()
```

This displays multiple trend lines.

---

## Customizing Line Plots

```python
df.plot(x="Month", y="Sales")
plt.title("Monthly Sales Trend")
plt.xlabel("Month")
plt.ylabel("Sales")
plt.show()
```

---

## Bar Charts

Bar charts compare values across categories.

Examples:

* product sales comparison
* department budgets
* population by country

Example:

```python
data = {
    "Product": ["Laptop", "Phone", "Tablet"],
    "Sales": [500, 700, 300]
}

df = pd.DataFrame(data)

df.plot(x="Product", y="Sales", kind="bar")
plt.show()
```

---

## Horizontal Bar Chart

```python
df.plot(x="Product", y="Sales", kind="barh")
plt.show()
```

Useful when category labels are long.

---

## Grouped Bar Chart

Example dataset:

| Product | 2023 | 2024 |
| ------- | ---- | ---- |
| Laptop  | 500  | 600  |
| Phone   | 700  | 800  |
| Tablet  | 300  | 350  |

```python
df.plot(x="Product", kind="bar")
plt.show()
```

---

## Histograms

Histograms show the distribution of numerical data.

They help answer:

* how frequently values occur
* whether data is normally distributed
* presence of skewness

Example:

```python
import numpy as np

data = {
    "Scores": np.random.randint(50, 100, 100)
}

df = pd.DataFrame(data)

df["Scores"].plot(kind="hist")
plt.show()
```

---

### Adjust Number of Bins

```python
df["Scores"].plot(kind="hist", bins=20)
plt.show()
```

---

### Multiple Histograms

```python
df.plot(kind="hist", alpha=0.5)
plt.show()
```

---

## Scatter Plots

Scatter plots show relationships between two numerical variables.

Common uses:

* correlation analysis
* regression analysis
* identifying clusters
* detecting outliers

Example:

```python
data = {
    "HoursStudied": [1, 2, 3, 4, 5],
    "Score": [50, 55, 65, 70, 80]
}

df = pd.DataFrame(data)

df.plot(x="HoursStudied", y="Score", kind="scatter")
plt.show()
```

---

### Interpreting Scatter Plots

| Pattern        | Meaning              |
| -------------- | -------------------- |
| Upward trend   | Positive correlation |
| Downward trend | Negative correlation |
| Random scatter | No correlation       |

---

## Box Plots

Box plots visualize statistical distribution:

* median
* quartiles
* spread
* outliers

Example:

```python
data = {
    "Scores": [50, 60, 70, 75, 80, 90, 95, 100]
}

df = pd.DataFrame(data)

df.plot(kind="box")
plt.show()
```

---

### Box Plot Structure

* Minimum
* Q1 (First quartile)
* Median
* Q3 (Third quartile)
* Maximum
* Outliers

---

## Box Plot for Multiple Columns

```python
data = {
    "Math": [70, 80, 90, 85],
    "Science": [60, 75, 88, 92],
    "English": [65, 78, 85, 87]
}

df = pd.DataFrame(data)

df.plot(kind="box")
plt.show()
```

---

## Real-World Example: Sales Visualization

### Dataset:

| Month | Sales |
| ----- | ----- |
| Jan   | 200   |
| Feb   | 250   |
| Mar   | 300   |
| Apr   | 280   |
| May   | 350   |

### Line Plot (trend)

```python
df.plot(x="Month", y="Sales", kind="line")
```

### Bar Chart (comparison)

```python
df.plot(x="Month", y="Sales", kind="bar")
```

### Histogram (distribution)

```python
df["Sales"].plot(kind="hist")
```

---

## Customizing Visualizations

### Title

```python
plt.title("Monthly Sales")
```

### Axis Labels

```python
plt.xlabel("Month")
plt.ylabel("Revenue")
```

### Grid

```python
plt.grid(True)
```

---

## Performance Considerations

Large datasets may produce slow or cluttered visualizations.

Solutions:

* sample data before plotting
* aggregate data
* use summarized statistics

Example:

```python
df.sample(1000).plot(kind="scatter")
```

---

## Summary

Pandas provides powerful built-in visualization tools for quick data exploration.

### Line Plots

Used for:

* time trends
* growth analysis

```python
df.plot(kind="line")
```

### Bar Charts

Used for:

* category comparison

```python
df.plot(kind="bar")
```

### Histograms

Used for:

* distribution analysis

```python
df.plot(kind="hist")
```

### Scatter Plots

Used for:

* relationships between variables

```python
df.plot(kind="scatter")
```

### Box Plots

Used for:

* distribution summary
* outlier detection

```python
df.plot(kind="box")
```

Pandas visualization tools make exploratory data analysis fast and effective, turning raw data into meaningful insights with minimal code.


