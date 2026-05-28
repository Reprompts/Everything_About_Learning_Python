# Date & Time Hands-On Tutorial
## Python Date & Time: Datetime Formatting, Parsing, and Conversions in Python

Working with date and time in real-world applications often requires converting between different representations.

For example:

* Converting a `datetime` object to a readable string
* Parsing a date string from user input
* Converting timestamps to `datetime`
* Extracting specific components like year or hour

Python's `datetime` module provides powerful tools for formatting, parsing, and converting date and time values.

The most important methods for these operations are:

* `strftime()` → Formatting datetime objects into strings
* `strptime()` → Parsing strings into datetime objects

This article explores these concepts in detail.

---

# 1. Formatting Date and Time Using strftime()

The `strftime()` method converts a `date`, `time`, or `datetime` object into a formatted string.

## Syntax

```python id="jv0c4t"
datetime_object.strftime(format_string)
```

## Example

```python id="2yv3bh"
from datetime import datetime

now = datetime.now()

formatted = now.strftime("%Y-%m-%d %H:%M:%S")

print(formatted)
```

### Example Output

```text id="mjlwm7"
2026-04-08 21:45:30
```

This method is widely used when displaying date/time values in:

* User interfaces
* Reports
* Log files
* Dashboards
* APIs

---

# 2. Common Datetime Formatting Codes

Formatting codes define how the date and time should appear in the output.

| Code | Meaning            | Example     |
| ---- | ------------------ | ----------- |
| `%Y` | 4-digit year       | `2026`      |
| `%y` | 2-digit year       | `26`        |
| `%m` | Month number       | `04`        |
| `%B` | Full month name    | `April`     |
| `%b` | Short month name   | `Apr`       |
| `%d` | Day of month       | `08`        |
| `%A` | Full weekday name  | `Wednesday` |
| `%a` | Short weekday name | `Wed`       |
| `%H` | Hour (24-hour)     | `21`        |
| `%I` | Hour (12-hour)     | `09`        |
| `%M` | Minutes            | `45`        |
| `%S` | Seconds            | `30`        |
| `%p` | AM / PM            | `PM`        |
| `%f` | Microseconds       | `123456`    |
| `%j` | Day of year        | `098`       |
| `%U` | Week number        | `14`        |

---

## Example

```python id="cxajgj"
from datetime import datetime

now = datetime.now()

print(now.strftime("%A, %B %d, %Y"))
```

### Output

```text id="wdt3lc"
Wednesday, April 08, 2026
```

---

# 3. Custom Date Formats

Using formatting codes, you can design custom date formats.

---

## Example 1 — Simple Format

```python id="1xgb2v"
print(now.strftime("%d-%m-%Y"))
```

### Output

```text id="evqjew"
08-04-2026
```

---

## Example 2 — Time Format

```python id="c3g6kq"
print(now.strftime("%H:%M:%S"))
```

### Output

```text id="q7m1z7"
21:45:30
```

---

## Example 3 — Human Friendly Format

```python id="0djlwm"
print(now.strftime("%A %d %B %Y"))
```

### Output

```text id="g0j5rm"
Wednesday 08 April 2026
```

---

## Example 4 — 12 Hour Clock

```python id="rfmr7o"
print(now.strftime("%I:%M %p"))
```

### Output

```text id="0cok40"
09:45 PM
```

---

# 4. Parsing Date Strings Using strptime()

While `strftime()` converts:

```text id="z1c8dx"
datetime → string
```

The `strptime()` method performs the opposite conversion.

It converts:

```text id="6g0vwm"
string → datetime object
```

---

## Syntax

```python id="40j2u2"
datetime.strptime(date_string, format)
```

---

## Example

```python id="4rw0zt"
from datetime import datetime

date_str = "2026-04-08"

dt = datetime.strptime(date_str, "%Y-%m-%d")

print(dt)
```

### Output

```text id="w6dy2n"
2026-04-08 00:00:00
```

This method is useful when:

* Reading dates from user input
* Parsing log files
* Processing CSV or JSON data
* Handling API responses

---

# 5. Handling Different Date Formats

Real-world data often contains many different date formats.

## Example Formats

```text id="hm8mnd"
2026-04-08
08-04-2026
04/08/2026
April 8, 2026
```

Each format must be parsed with the correct pattern.

---

## Format: 08-04-2026

```python id="9d5d91"
from datetime import datetime

dt = datetime.strptime("08-04-2026", "%d-%m-%Y")

print(dt)
```

---

## Format: 04/08/2026

```python id="5bx9zi"
dt = datetime.strptime("04/08/2026", "%m/%d/%Y")
```

---

## Format: April 8, 2026

```python id="w75x1o"
dt = datetime.strptime("April 8, 2026", "%B %d, %Y")
```

---

# 6. Parsing Timestamps

A timestamp represents time as the number of seconds since January 1, 1970 (Unix epoch).

## Example Timestamp

```text id="r4k0ja"
1712600000
```

Python can convert timestamps to `datetime`.

---

## Example

```python id="5w4sbo"
from datetime import datetime

timestamp = 1712600000

dt = datetime.fromtimestamp(timestamp)

print(dt)
```

### Example Output

```text id="m28gj7"
2024-04-08 20:26:40
```

This is commonly used in:

* Databases
* Logging systems
* APIs
* Blockchain systems

---

# 7. Datetime Conversion Operations

The `datetime` class allows conversions between different time-related objects.

## Conversions Include

* `datetime → date`
* `datetime → time`
* `date + time → datetime`

---

# Converting Datetime to Date

The `date()` method extracts the date portion.

## Example

```python id="k9yw1h"
from datetime import datetime

dt = datetime.now()

print(dt.date())
```

### Output

```text id="nhm1a2"
2026-04-08
```

---

# Converting Datetime to Time

The `time()` method extracts the time portion.

## Example

```python id="w2hlc1"
print(dt.time())
```

### Output

```text id="0uq0g8"
21:45:30.234567
```

---

# Combining Date and Time into Datetime

Sometimes you have a separate `date` and `time` object and need to combine them.

## Example

```python id="4ofz7k"
from datetime import date, time, datetime

d = date(2026, 4, 8)
t = time(21, 45)

combined = datetime.combine(d, t)

print(combined)
```

### Output

```text id="0sp2pv"
2026-04-08 21:45:00
```

---

# Extracting Components from Datetime

Datetime objects allow direct access to individual components.

## Example

```python id="l7clhl"
from datetime import datetime

dt = datetime.now()

print(dt.year)
print(dt.month)
print(dt.day)
print(dt.hour)
print(dt.minute)
print(dt.second)
```

### Example Output

```text id="5ljlwm"
2026
4
8
21
45
30
```

---

## Attributes Available

| Attribute     | Meaning            |
| ------------- | ------------------ |
| `year`        | Year value         |
| `month`       | Month number       |
| `day`         | Day                |
| `hour`        | Hour               |
| `minute`      | Minute             |
| `second`      | Second             |
| `microsecond` | Fraction of second |

---

# 8. Formatting and Localization

Different countries use different date formats.

| Region       | Format       |
| ------------ | ------------ |
| USA          | `MM/DD/YYYY` |
| Europe       | `DD/MM/YYYY` |
| ISO Standard | `YYYY-MM-DD` |

Python formatting codes allow adapting to these formats.

---

# International Date Formats

---

## US Format

```python id="c6vq1f"
print(now.strftime("%m/%d/%Y"))
```

### Output

```text id="l20g6d"
04/08/2026
```

---

## European Format

```python id="bsf4t6"
print(now.strftime("%d/%m/%Y"))
```

### Output

```text id="tn5vuw"
08/04/2026
```

---

## ISO Standard Format

```python id="1vtdou"
print(now.strftime("%Y-%m-%d"))
```

### Output

```text id="i3fr4g"
2026-04-08
```

ISO format is widely used in:

* Databases
* APIs
* Data exchange formats

---

# 9. Formatting Timestamps for Display

Applications often store timestamps but display human-readable dates.

## Example

```python id="yc32ml"
from datetime import datetime

timestamp = 1712600000

dt = datetime.fromtimestamp(timestamp)

formatted = dt.strftime("%A, %d %B %Y %H:%M:%S")

print(formatted)
```

### Example Output

```text id="j3dlga"
Monday, 08 April 2024 20:26:40
```

This approach is used in:

* Dashboards
* Log viewers
* Social media posts
* Analytics reports

---

# Example Program: Formatting and Parsing

```python id="u7bpc5"
from datetime import datetime

# Current datetime
now = datetime.now()

# Format datetime
formatted = now.strftime("%Y-%m-%d %H:%M:%S")

print("Formatted:", formatted)

# Parse string to datetime
date_string = "2026-04-08 21:45:30"

parsed = datetime.strptime(date_string, "%Y-%m-%d %H:%M:%S")

print("Parsed:", parsed)
```

---

## Output

```text id="hnj2ly"
Formatted: 2026-04-08 21:45:30
Parsed: 2026-04-08 21:45:30
```

---

# Summary

Python provides powerful tools for formatting, parsing, and converting `datetime` objects.

## Key Methods Include

| Method            | Purpose                    |
| ----------------- | -------------------------- |
| `strftime()`      | Convert datetime → string  |
| `strptime()`      | Convert string → datetime  |
| `date()`          | Extract date from datetime |
| `time()`          | Extract time from datetime |
| `combine()`       | Merge date and time        |
| `fromtimestamp()` | Convert Unix timestamp     |

---

## These Tools Allow Developers To

* Display dates in user-friendly formats
* Parse date strings from user input
* Convert timestamps into readable dates
* Adapt date formats for different countries
* Extract individual time components

Mastering these operations is essential for building robust applications involving logging, scheduling, data analysis, APIs, and international systems.
