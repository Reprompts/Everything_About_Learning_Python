# Python Date & Time

---

# 1. Fundamentals of Python Date and Time

## Introduction to Date and Time in Python

Date and time handling is an essential part of programming. Many applications depend on accurate time tracking, scheduling, logging, expiration handling, and timestamp processing.

Python provides powerful built-in modules for working with dates and times, primarily through the `datetime` module.

---

## Importance of `datetime` in Programming

Date and time operations are used in almost every real-world application:

* Logging systems
* Banking systems
* Scheduling applications
* Authentication systems
* Analytics dashboards
* Monitoring systems
* Event tracking
* Time-based automation

---

## Use Cases of Datetime in Applications

| Application    | Usage                   |
| -------------- | ----------------------- |
| Banking        | Transaction timestamps  |
| Social Media   | Post creation times     |
| Authentication | OTP expiration          |
| E-commerce     | Order tracking          |
| Analytics      | Time-series data        |
| Logging        | Error and activity logs |
| Automation     | Scheduled tasks         |

---

## Overview of Python Date and Time Modules

Python provides multiple modules for working with date and time:

| Module     | Purpose                                  |
| ---------- | ---------------------------------------- |
| `datetime` | Main module for date and time operations |
| `time`     | Low-level time operations                |
| `calendar` | Calendar-related operations              |
| `zoneinfo` | Time zone support                        |
| `timeit`   | Performance timing                       |

---

## Importing the `datetime` Module

```python
import datetime
```

Import specific classes:

```python
from datetime import date, time, datetime, timedelta
```

---

## Difference Between `datetime`, `date`, `time`, and `timedelta`

| Class       | Purpose                     |
| ----------- | --------------------------- |
| `date`      | Stores year, month, day     |
| `time`      | Stores time only            |
| `datetime`  | Stores both date and time   |
| `timedelta` | Represents time differences |

Example:

```python
from datetime import date, time, datetime, timedelta

d = date(2025, 5, 1)

t = time(10, 30, 45)

dt = datetime(2025, 5, 1, 10, 30, 45)

delta = timedelta(days=5)
```

---

## Understanding Naive and Aware Datetime Objects

### Naive Datetime

A naive datetime does not contain timezone information.

```python
from datetime import datetime

dt = datetime.now()

print(dt)
```

---

### Aware Datetime

An aware datetime includes timezone information.

```python
from datetime import datetime, timezone

dt = datetime.now(timezone.utc)

print(dt)
```

---

# The `datetime` Module Structure

---

## `date` Class

Represents calendar dates.

```python
from datetime import date

today = date.today()

print(today)
```

---

## `time` Class

Represents time independently.

```python
from datetime import time

t = time(14, 30, 15)

print(t)
```

---

## `datetime` Class

Combines both date and time.

```python
from datetime import datetime

dt = datetime.now()

print(dt)
```

---

## `timedelta` Class

Represents duration or difference between dates/times.

```python
from datetime import timedelta

delta = timedelta(days=7)

print(delta)
```

---

## `timezone` Class

Represents timezone information.

```python
from datetime import timezone

utc = timezone.utc
```

---

## Relationships Between These Classes

```text
datetime
 ├── date
 ├── time
 ├── timezone
 └── timedelta
```

---

# 2. Working with Date, Time, and Datetime Objects

---

# Working with the `date` Class

---

## Creating Date Objects

```python
from datetime import date

d = date(2025, 12, 25)

print(d)
```

---

## Getting Current Date

```python
today = date.today()

print(today)
```

---

## Accessing Year, Month, and Day

```python
print(today.year)
print(today.month)
print(today.day)
```

---

## Date Attributes and Methods

| Method        | Description           |
| ------------- | --------------------- |
| `today()`     | Current date          |
| `weekday()`   | Day index             |
| `isoformat()` | ISO format string     |
| `ctime()`     | String representation |

Example:

```python
print(today.weekday())
print(today.isoformat())
```

---

## Date Comparisons

```python
d1 = date(2025, 1, 1)
d2 = date(2025, 12, 31)

print(d1 < d2)
```

---

# Working with the `time` Class

---

## Creating Time Objects

```python
from datetime import time

t = time(10, 45, 30)

print(t)
```

---

## Accessing Hours, Minutes, Seconds

```python
print(t.hour)
print(t.minute)
print(t.second)
```

---

## Microseconds in Time

```python
t = time(10, 30, 15, 500000)

print(t.microsecond)
```

---

## Time Attributes and Methods

| Attribute     | Description  |
| ------------- | ------------ |
| `hour`        | Hour value   |
| `minute`      | Minute value |
| `second`      | Second value |
| `microsecond` | Microseconds |

---

## Time Comparisons

```python
t1 = time(9, 0)
t2 = time(18, 0)

print(t1 < t2)
```

---

# Working with the `datetime` Class

---

## Creating Datetime Objects

```python
from datetime import datetime

dt = datetime(2025, 5, 20, 14, 30, 45)

print(dt)
```

---

## Getting Current Date and Time

```python
now = datetime.now()

print(now)
```

---

## Getting UTC Time

```python
utc_now = datetime.utcnow()

print(utc_now)
```

---

## Accessing Datetime Components

```python
print(now.year)
print(now.month)
print(now.day)
print(now.hour)
print(now.minute)
print(now.second)
```

---

## Datetime Attributes and Methods

| Method        | Purpose                |
| ------------- | ---------------------- |
| `now()`       | Current local datetime |
| `utcnow()`    | Current UTC datetime   |
| `timestamp()` | UNIX timestamp         |
| `strftime()`  | Format datetime        |
| `strptime()`  | Parse datetime         |

---

# 3. Datetime Formatting, Parsing, and Conversions

---

# Formatting Date and Time Using `strftime()`

---

## Basic Formatting

```python
from datetime import datetime

now = datetime.now()

formatted = now.strftime("%Y-%m-%d")

print(formatted)
```

---

## Common Datetime Formatting Codes

| Code | Meaning        |
| ---- | -------------- |
| `%Y` | Full year      |
| `%m` | Month          |
| `%d` | Day            |
| `%H` | Hour (24-hour) |
| `%M` | Minute         |
| `%S` | Second         |

---

## Custom Date Formats

```python
print(now.strftime("%d/%m/%Y"))
print(now.strftime("%I:%M %p"))
```

---

# Parsing Date Strings Using `strptime()`

---

## Basic Parsing

```python
date_str = "2025-05-20"

dt = datetime.strptime(date_str, "%Y-%m-%d")

print(dt)
```

---

## Handling Different Date Formats

```python
date_str = "20/05/2025"

dt = datetime.strptime(date_str, "%d/%m/%Y")

print(dt)
```

---

## Parsing Timestamps

```python
timestamp = "2025-05-20 14:30:00"

dt = datetime.strptime(timestamp, "%Y-%m-%d %H:%M:%S")

print(dt)
```

---

# Datetime Conversion Operations

---

## Converting Datetime to Date

```python
d = now.date()

print(d)
```

---

## Converting Datetime to Time

```python
t = now.time()

print(t)
```

---

## Combining Date and Time into Datetime

```python
from datetime import datetime, date, time

d = date.today()
t = time(10, 30)

dt = datetime.combine(d, t)

print(dt)
```

---

## Extracting Components from Datetime

```python
print(now.year)
print(now.month)
print(now.day)
```

---

# Formatting and Localization

---

## International Date Formats

| Country | Format     |
| ------- | ---------- |
| USA     | MM/DD/YYYY |
| India   | DD/MM/YYYY |
| ISO     | YYYY-MM-DD |

---

## Formatting Timestamps for Display

```python
print(now.strftime("%A, %d %B %Y"))
```

---

# 4. Date and Time Calculations and Manipulations

---

# Date and Time Arithmetic

---

## Adding and Subtracting Time Intervals

```python
from datetime import timedelta

future = now + timedelta(days=7)

print(future)
```

---

## Calculating Time Differences

```python
start = datetime(2025, 1, 1)
end = datetime(2025, 1, 10)

difference = end - start

print(difference)
```

---

## Using `timedelta` in Arithmetic

```python
delta = timedelta(hours=5)

new_time = now + delta

print(new_time)
```

---

# The `timedelta` Class

---

## Creating Timedelta Objects

```python
from datetime import timedelta

delta = timedelta(days=2, hours=5)
```

---

## Days, Seconds, Microseconds

```python
print(delta.days)
print(delta.seconds)
print(delta.microseconds)
```

---

# Datetime Comparisons

---

```python
d1 = datetime(2025, 1, 1)
d2 = datetime(2025, 6, 1)

print(d1 < d2)
```

---

# Datetime Replacement and Modification

---

```python
modified = now.replace(year=2030)

print(modified)
```

---

# Date and Time Calculations

---

## Calculating Age

```python
birth = date(2000, 5, 10)

today = date.today()

age = today.year - birth.year

print(age)
```

---

## Calculating Durations

```python
duration = end - start

print(duration.days)
```

---

# Calendar and Weekday Operations

---

```python
print(now.weekday())
print(now.isoweekday())
```

---

# Working with Date Ranges

---

```python
start = date(2025, 1, 1)

for i in range(7):
    print(start + timedelta(days=i))
```

---

# Working with Time Durations

---

```python
task_time = timedelta(hours=2, minutes=30)

print(task_time)
```

---

# 5. Advanced Usage, Applications, and Best Practices

---

# Working with Timestamps and UNIX Timestamps

---

## Generating UNIX Timestamps

```python
timestamp = datetime.now().timestamp()

print(timestamp)
```

---

## Converting Timestamp to Datetime

```python
dt = datetime.fromtimestamp(timestamp)

print(dt)
```

---

# Time Zone Handling

---

## Using `timezone` Class

```python
from datetime import timezone

utc = timezone.utc
```

---

## Creating Timezone-Aware Datetime

```python
dt = datetime.now(timezone.utc)

print(dt)
```

---

## Converting Between Time Zones

```python
from zoneinfo import ZoneInfo

india = ZoneInfo("Asia/Kolkata")

dt_india = dt.astimezone(india)

print(dt_india)
```

---

## Time Zone Offsets

```python
print(dt_india.utcoffset())
```

---

# Practical Datetime Applications

---

## Logging Timestamps

```python
log_time = datetime.now()

print(f"Log generated at {log_time}")
```

---

## Scheduling Tasks

```python
next_run = datetime.now() + timedelta(hours=1)

print(next_run)
```

---

## Handling Expiration Dates

```python
expiry = datetime.now() + timedelta(days=30)

print(expiry)
```

---

## Tracking Durations

```python
start = datetime.now()

# Task execution

end = datetime.now()

print(end - start)
```

---

# Performance Considerations

---

| Practice                   | Benefit            |
| -------------------------- | ------------------ |
| Use timestamps for storage | Faster processing  |
| Avoid repeated parsing     | Better performance |
| Use timezone-aware objects | Accuracy           |

---

# Common Datetime Pitfalls

---

## Daylight Saving Time Issues

Time calculations may become incorrect during DST transitions.

---

## Incorrect Parsing

Wrong format strings can cause parsing failures.

```python
datetime.strptime("20-05-2025", "%Y/%m/%d")
```

---

# Best Practices for Datetime Handling

---

## Using Timezone-Aware Objects

Preferred:

```python
datetime.now(timezone.utc)
```

Avoid:

```python
datetime.now()
```

---

## Standard Formats for Storage

Use ISO 8601:

```python
2025-05-20T14:30:00Z
```

---

## Consistent Timezone Handling

Store data in UTC and convert to local time only for display.

---

## Testing Datetime Logic

Always test:

* Leap years
* Month boundaries
* Timezone conversions
* Daylight saving transitions
* Invalid dates

---

# Summary

Python provides powerful tools for handling:

* Dates
* Times
* Datetime objects
* Time calculations
* Formatting and parsing
* Timezones
* Scheduling
* Timestamps

The `datetime` module is one of the most important modules for real-world software development because almost every application relies on accurate time handling.
