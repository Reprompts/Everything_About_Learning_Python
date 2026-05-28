# Date & Time Hands-On Tutorial 
## Python Date & Time: Date and Time Calculations and Manipulations in Python

In real-world programming, dates and times are rarely used only for display. Most applications must calculate durations, compare timestamps, add or subtract time intervals, or work with ranges of dates.

Python's `datetime` module provides powerful tools to perform these operations accurately and efficiently.

This article explores date and time arithmetic, the `timedelta` class, comparisons, modifications, and practical calculations such as age and durations.

---

# 1. Date and Time Arithmetic

Date and time arithmetic involves performing mathematical operations on date and time values.

## Common Operations Include

* Adding time intervals
* Subtracting dates
* Calculating durations
* Comparing timestamps

Python performs these operations using `timedelta` objects.

---

## Example

```python id="j7n0dy"
from datetime import datetime, timedelta

now = datetime.now()

future = now + timedelta(days=5)

print(future)
```

### Example Output

```text id="07g2f3"
2026-04-13 22:15:30
```

This adds 5 days to the current date.

---

# 2. Adding and Subtracting Time Intervals

The `timedelta` class represents a duration of time.

It can be added to or subtracted from `date` or `datetime` objects.

---

# Adding Time

## Example

```python id="s0o6e2"
from datetime import datetime, timedelta

now = datetime.now()

result = now + timedelta(hours=3)

print(result)
```

### Output Example

```text id="zwvv79"
2026-04-08 01:15:30
```

---

# Subtracting Time

## Example

```python id="mqfmmt"
past = now - timedelta(days=7)

print(past)
```

### Output Example

```text id="o2kw8u"
2026-04-01 22:15:30
```

---

## Common Units

* `days`
* `seconds`
* `microseconds`
* `minutes`
* `hours`
* `weeks`

### Example

```python id="n4zyrw"
timedelta(days=2, hours=5, minutes=30)
```

---

# 3. Calculating Time Differences

One of the most common operations is finding the difference between two dates or datetimes.

## Example

```python id="4zc9x2"
from datetime import datetime

d1 = datetime(2026, 4, 1)
d2 = datetime(2026, 4, 8)

difference = d2 - d1

print(difference)
```

### Output

```text id="ljt9p5"
7 days, 0:00:00
```

The result is a `timedelta` object.

---

# Accessing Difference Components

```python id="5mth7l"
print(difference.days)
print(difference.seconds)
```

### Example Output

```text id="lgt3ib"
7
0
```

---

# 4. Using timedelta in Arithmetic

`timedelta` objects allow mathematical operations.

## Example

```python id="y3o4z6"
from datetime import timedelta

delta1 = timedelta(days=5)
delta2 = timedelta(days=3)

total = delta1 + delta2

print(total)
```

### Output

```text id="1rct4g"
8 days, 0:00:00
```

---

# Multiplying Durations

## Example

```python id="59cyz9"
delta = timedelta(days=2)

print(delta * 3)
```

### Output

```text id="38n9n2"
6 days, 0:00:00
```

---

# 5. The timedelta Class

The `timedelta` class represents a duration or difference between two points in time.

It is commonly used for:

* Date arithmetic
* Calculating durations
* Scheduling tasks
* Measuring execution time

---

## Structure

```python id="h0wct7"
timedelta(
    days=0,
    seconds=0,
    microseconds=0,
    milliseconds=0,
    minutes=0,
    hours=0,
    weeks=0
)
```

---

## Example

```python id="l2w32r"
from datetime import timedelta

duration = timedelta(days=10, hours=5)

print(duration)
```

### Output

```text id="fjlwm0"
10 days, 5:00:00
```

---

# 6. Creating timedelta Objects

## Example Using Multiple Units

```python id="s2sot7"
from datetime import timedelta

td = timedelta(days=3, hours=4, minutes=30)

print(td)
```

### Output

```text id="hch2sz"
3 days, 4:30:00
```

---

## Another Example

```python id="w6hspq"
td = timedelta(weeks=2)

print(td)
```

### Output

```text id="baj2iy"
14 days, 0:00:00
```

---

# 7. Days, Seconds, and Microseconds

Internally, a `timedelta` stores time using three attributes:

| Attribute      | Description         |
| -------------- | ------------------- |
| `days`         | Number of days      |
| `seconds`      | Seconds in the day  |
| `microseconds` | Fraction of seconds |

---

## Example

```python id="sntm8w"
from datetime import timedelta

td = timedelta(days=1, hours=2)

print(td.days)
print(td.seconds)
```

### Output

```text id="m3mddm"
1
7200
```

```text id="pc7f57"
7200 seconds = 2 hours
```

---

# 8. Datetime Comparisons

`datetime` objects support comparison operators.

## Example

```python id="6bnr7d"
from datetime import datetime

d1 = datetime(2026, 4, 1)
d2 = datetime(2026, 4, 8)

print(d1 < d2)
```

### Output

```text id="h0v6c2"
True
```

---

## Comparison Operators Include

| Operator | Meaning          |
| -------- | ---------------- |
| `<`      | Earlier than     |
| `>`      | Later than       |
| `==`     | Equal            |
| `!=`     | Not equal        |
| `<=`     | Less or equal    |
| `>=`     | Greater or equal |

---

## Example

```python id="yrqrzn"
if d1 > d2:
    print("d1 occurs after d2")
```

---

# 9. Datetime Replacement and Modification

The `replace()` method allows modification of specific components.

## Example

```python id="q1jfw8"
from datetime import datetime

now = datetime.now()

new_date = now.replace(year=2030)

print(new_date)
```

### Example Output

```text id="9k1mf9"
2030-04-08 22:30:10
```

---

## Modifying Multiple Components

```python id="cz4ghu"
modified = now.replace(month=12, day=25)

print(modified)
```

### Output

```text id="7cgjm5"
2026-12-25 22:30:10
```

---

# 10. Date and Time Calculations (Age and Durations)

Datetime calculations are often used for age calculations and durations.

---

# Example: Age Calculation

```python id="2h3q0q"
from datetime import date

birth = date(2000, 5, 10)

today = date.today()

age_days = today - birth

print(age_days.days)
```

### Example Output

```text id="13icq5"
9440
```

---

## Approximate Age in Years

```python id="2m0b55"
age_years = age_days.days // 365

print(age_years)
```

---

# Example: Program Execution Time

```python id="tzc8nn"
from datetime import datetime

start = datetime.now()

# some task
for i in range(1000000):
    pass

end = datetime.now()

duration = end - start

print(duration)
```

### Output Example

```text id="klv9an"
0:00:00.150432
```

---

# 11. Calendar and Weekday Operations

Dates can be used to determine the day of the week.

## Example

```python id="wms17z"
from datetime import date

d = date(2026, 4, 8)

print(d.weekday())
```

### Output

```text id="dsvx6l"
2
```

---

## Weekday Mapping

| Number | Day       |
| ------ | --------- |
| `0`    | Monday    |
| `1`    | Tuesday   |
| `2`    | Wednesday |
| `3`    | Thursday  |
| `4`    | Friday    |
| `5`    | Saturday  |
| `6`    | Sunday    |

---

## Example

```python id="g9dhjx"
print(d.strftime("%A"))
```

### Output

```text id="9b6i2u"
Wednesday
```

---

# 12. Working with Date Ranges

Programs often need to iterate through a range of dates.

## Example

```python id="2t9z9v"
from datetime import date, timedelta

start = date(2026, 4, 1)
end = date(2026, 4, 7)

current = start

while current <= end:
    print(current)
    current += timedelta(days=1)
```

### Output

```text id="8jdb9n"
2026-04-01
2026-04-02
2026-04-03
2026-04-04
2026-04-05
2026-04-06
2026-04-07
```

This is useful in:

* Generating reports
* Scheduling tasks
* Analyzing time series data

---

# 13. Working with Time Durations

Durations can represent:

* Program execution time
* Travel time
* Event durations
* Task completion times

---

## Example

```python id="8yfv2l"
from datetime import timedelta

task_duration = timedelta(hours=2, minutes=45)

print(task_duration)
```

### Output

```text id="g7qz6h"
2:45:00
```

---

# Converting Duration to Seconds

```python id="t19yq7"
print(task_duration.total_seconds())
```

### Output

```text id="kk8rpi"
9900.0
```

This method is very useful when performing numeric calculations on time durations.

---

# 14. Example Program: Complete Date Calculation

```python id="z2yxkw"
from datetime import datetime, timedelta

now = datetime.now()

future = now + timedelta(days=10)

difference = future - now

print("Current time:", now)
print("Future time:", future)
print("Difference:", difference)
```

---

## Example Output

```text id="tgc7dx"
Current time: 2026-04-08 22:40:12
Future time: 2026-04-18 22:40:12
Difference: 10 days, 0:00:00
```

---

# Summary

Python's `datetime` module provides robust tools for date and time calculations and manipulations.

## Key Capabilities Include

* Performing date arithmetic
* Adding or subtracting time intervals
* Calculating time differences
* Using `timedelta` objects for durations
* Comparing datetime values
* Modifying dates with `replace()`
* Calculating age and execution durations
* Working with date ranges
* Measuring time durations

---

## These Features Make the datetime Module Essential For Applications Such As

* Scheduling systems
* Financial transactions
* Analytics and reporting
* Logging and monitoring
* Automation scripts

Mastering these calculations enables developers to build accurate, time-aware applications that handle real-world time data reliably.
