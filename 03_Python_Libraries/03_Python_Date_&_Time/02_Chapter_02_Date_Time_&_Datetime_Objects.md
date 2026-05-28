# Date & Time Hands-On Tutorial
## Python Date & Time: Working with Date, Time, and Datetime Objects in Python

Python's `datetime` module provides powerful classes that allow developers to represent, manipulate, and compare dates and times easily.

The most commonly used classes in this module are:

* `date`
* `time`
* `datetime`

Each class represents a different concept of time and offers attributes and methods for performing operations on date and time values.

This article explains how to create, access, and work with `date`, `time`, and `datetime` objects in detail.

---

# 1. Working with the date Class

The `date` class represents a calendar date without any time information.

It stores three components:

* Year
* Month
* Day

## Example Date

```text id="fjlwmf"
2026-04-07
```

---

## Creating Date Objects

A `date` object is created by specifying the year, month, and day.

### Syntax

```python id="r2vv2n"
date(year, month, day)
```

### Example

```python id="4fgkk7"
from datetime import date

d = date(2026, 4, 7)

print(d)
```

### Output

```text id="m4dty7"
2026-04-07
```

---

## Rules

* Month must be between `1–12`
* Day must be valid for the month
* Invalid values raise `ValueError`

### Example Error

```python id="zpm9zw"
date(2026, 2, 30)
```

### Output

```text id="u1l4lp"
ValueError: day is out of range for month
```

---

## Getting the Current Date

Python can automatically retrieve the current system date.

### Example

```python id="t7lh2v"
from datetime import date

today = date.today()

print(today)
```

### Example Output

```text id="7ajq7j"
2026-04-07
```

This is commonly used for:

* Logging events
* Recording transactions
* Displaying today's date in applications

---

## Accessing Year, Month, and Day

Date objects store their components as attributes.

### Example

```python id="b3qqj3"
from datetime import date

today = date.today()

print(today.year)
print(today.month)
print(today.day)
```

### Output

```text id="yut9zz"
2026
4
7
```

---

## Attributes Available

| Attribute | Description  |
| --------- | ------------ |
| `year`    | Year value   |
| `month`   | Month number |
| `day`     | Day of month |

---

# Date Attributes and Methods

The `date` class provides several useful attributes and methods.

---

## Important Attributes

| Attribute | Description     |
| --------- | --------------- |
| `year`    | Year component  |
| `month`   | Month component |
| `day`     | Day component   |

---

## Useful Methods

### 1. today()

Returns the current local date.

```python id="icpajm"
date.today()
```

---

### 2. weekday()

Returns the day of the week.

### Values

* `0 = Monday`
* `6 = Sunday`

### Example

```python id="tp0k1x"
from datetime import date

d = date(2026, 4, 7)

print(d.weekday())
```

---

### 3. isoformat()

Returns the date as an ISO string.

```python id="m5wtv1"
print(d.isoformat())
```

### Output

```text id="95yxba"
2026-04-07
```

---

### 4. ctime()

Returns a readable date string.

```python id="jtwxej"
print(d.ctime())
```

### Example Output

```text id="1sh2l2"
Tue Apr 7 00:00:00 2026
```

---

# Date Comparisons

Date objects support comparison operators.

### Example

```python id="ol5miy"
from datetime import date

d1 = date(2026, 4, 7)
d2 = date(2026, 5, 1)

print(d1 < d2)
```

### Output

```text id="4e1xwy"
True
```

---

## Supported Operators

| Operator | Meaning          |
| -------- | ---------------- |
| `<`      | Earlier than     |
| `>`      | Later than       |
| `==`     | Equal            |
| `!=`     | Not equal        |
| `<=`     | Less or equal    |
| `>=`     | Greater or equal |

---

## Example Condition

```python id="x6ry9d"
if d1 < d2:
    print("d1 occurs before d2")
```

---

# 2. Working with the time Class

The `time` class represents a time of day without any date.

Components include:

* Hour
* Minute
* Second
* Microsecond

## Example

```text id="ay1z72"
14:30:45
```

---

## Creating Time Objects

### Syntax

```python id="o3d44n"
time(hour, minute, second, microsecond)
```

### Example

```python id="1qmxn3"
from datetime import time

t = time(14, 30, 45)

print(t)
```

### Output

```text id="4v3itf"
14:30:45
```

---

## Accessing Hours, Minutes, Seconds

Each component can be accessed through attributes.

### Example

```python id="c1k8yq"
from datetime import time

t = time(14, 30, 45)

print(t.hour)
print(t.minute)
print(t.second)
```

### Output

```text id="s5g36g"
14
30
45
```

---

## Attributes Available

| Attribute | Description  |
| --------- | ------------ |
| `hour`    | Hour value   |
| `minute`  | Minute value |
| `second`  | Second value |

---

# Microseconds in Time

The `time` class supports microsecond precision.

```text id="75x8ef"
1 microsecond = 1/1,000,000 second
```

### Example

```python id="c9fcf4"
from datetime import time

t = time(10, 45, 30, 500000)

print(t)
```

### Output

```text id="3z1gij"
10:45:30.500000
```

---

## Accessing Microseconds

```python id="fl9c5c"
print(t.microsecond)
```

---

# Time Attributes and Methods

---

## Important Attributes

| Attribute     | Description        |
| ------------- | ------------------ |
| `hour`        | Hour value         |
| `minute`      | Minute value       |
| `second`      | Seconds            |
| `microsecond` | Fraction of second |

---

## Useful Methods

### isoformat()

Returns time in ISO format.

### Example

```python id="x3x0bn"
print(t.isoformat())
```

### Output

```text id="sj5ob8"
14:30:45
```

---

### strftime()

Formats time as a string.

### Example

```python id="1owrbo"
print(t.strftime("%H:%M:%S"))
```

### Output

```text id="h5c3pm"
14:30:45
```

---

# Time Comparisons

Time objects can also be compared.

### Example

```python id="gnvx1s"
from datetime import time

t1 = time(10, 30)
t2 = time(12, 15)

print(t1 < t2)
```

### Output

```text id="b19vwg"
True
```

---

## Example Condition

```python id="pd0ulw"
if t1 < t2:
    print("t1 occurs earlier")
```

---

# 3. Working with the datetime Class

The `datetime` class combines both date and time information.

## Example

```text id="4sp4rf"
2026-04-07 14:30:45
```

This class is the most commonly used in real-world Python applications.

---

## Creating Datetime Objects

### Syntax

```python id="4u1snp"
datetime(year, month, day, hour, minute, second)
```

### Example

```python id="gd3h6p"
from datetime import datetime

dt = datetime(2026, 4, 7, 14, 30, 45)

print(dt)
```

### Output

```text id="f3w44s"
2026-04-07 14:30:45
```

---

## Getting Current Date and Time

Python can fetch the current system datetime.

### Example

```python id="j3m4aj"
from datetime import datetime

now = datetime.now()

print(now)
```

### Example Output

```text id="xxb2lk"
2026-04-07 11:20:45.123456
```

This includes:

* Date
* Time
* Microseconds

---

# Getting UTC Time

UTC stands for Coordinated Universal Time, the global time standard.

### Example

```python id="7x63zi"
from datetime import datetime

utc_now = datetime.utcnow()

print(utc_now)
```

### Example Output

```text id="v6s5aq"
2026-04-07 05:50:45.123456
```

This is often used in:

* Distributed systems
* Servers
* Global applications

---

# Accessing Datetime Components

The `datetime` object allows access to all date and time components.

### Example

```python id="c6h19m"
from datetime import datetime

now = datetime.now()

print(now.year)
print(now.month)
print(now.day)
print(now.hour)
print(now.minute)
print(now.second)
```

### Output Example

```text id="64rjdo"
2026
4
7
11
20
45
```

---

# Datetime Attributes

| Attribute     | Description        |
| ------------- | ------------------ |
| `year`        | Year               |
| `month`       | Month              |
| `day`         | Day                |
| `hour`        | Hour               |
| `minute`      | Minute             |
| `second`      | Second             |
| `microsecond` | Fraction of second |

---

# Datetime Methods

---

## now()

Returns current local datetime.

```python id="mj6u3o"
datetime.now()
```

---

## today()

Returns the current datetime.

```python id="w5qzb8"
datetime.today()
```

---

## utcnow()

Returns the current UTC time.

```python id="5z60vj"
datetime.utcnow()
```

---

## date()

Extracts the date portion.

### Example

```python id="zhy33r"
print(now.date())
```

### Output

```text id="1juzcv"
2026-04-07
```

---

## time()

Extracts the time portion.

### Example

```python id="06aw0p"
print(now.time())
```

### Output

```text id="x42v3v"
11:20:45.123456
```

---

# Example: Combining Date and Time Operations

## Example Program

```python id="w5phl7"
from datetime import datetime

now = datetime.now()

print("Current datetime:", now)
print("Year:", now.year)
print("Month:", now.month)
print("Day:", now.day)
print("Hour:", now.hour)
print("Minute:", now.minute)
print("Second:", now.second)
```

---

## Example Output

```text id="vcv2px"
Current datetime: 2026-04-07 11:20:45.123456
Year: 2026
Month: 4
Day: 7
Hour: 11
Minute: 20
Second: 45
```

---

# Summary

Python's `datetime` module provides three essential classes for representing time data.

| Class      | Purpose                       |
| ---------- | ----------------------------- |
| `date`     | Represents calendar date      |
| `time`     | Represents time of day        |
| `datetime` | Represents both date and time |

---

## Key Capabilities Include

* Creating date and time objects
* Accessing individual components
* Comparing dates and times
* Retrieving current time
* Handling precise timestamps

---

## These Tools Form the Foundation For

* Date formatting
* Time calculations
* Time zone conversions
* Scheduling systems
* Time-series data analysis

Understanding these objects is essential for building reliable applications that depend on accurate time handling.
