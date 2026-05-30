# Regular Expressions Hands-On Tutorial
## Python Regular Expressions Library: Regex Objects and Match Handling in Python

When working with regular expressions in Python, we usually interact with two important object types created by the `re` module:

- **Regex (Pattern) Objects** — Compiled regex patterns used for repeated matching
- **Match Objects** — Objects representing a successful match result

Understanding these objects is essential for extracting data, analyzing matches, and building complex text-processing systems.

This article explains these concepts in practical detail with Python examples.

---

# 1. Regex Objects (Compiled Pattern Objects)

A regex object (also called a **pattern object**) is created when a regular expression pattern is compiled.

Instead of repeatedly interpreting the same regex string, Python can compile it once and reuse it, improving performance.

## Creating a Compiled Regex Object

```python
import re

pattern = re.compile(r"\d+")
```

Here:

```python
re.compile()
```

creates a pattern object representing the regex.

---

## Why Compile Regex?

Compiling regex is useful when:

- The same pattern is used many times
- Performance matters
- You want to use pattern-specific methods

### Example

```python
import re

pattern = re.compile(r"\d+")

text1 = "Age: 25"
text2 = "Year: 2024"

print(pattern.search(text1))
print(pattern.search(text2))
```

The pattern is compiled once but reused multiple times.

---

# 2. Pattern Object Methods

Compiled patterns provide methods similar to the `re` module functions.

| Method | Purpose |
|----------|----------|
| `pattern.match()` | Match at beginning |
| `pattern.search()` | Search anywhere |
| `pattern.findall()` | Return all matches |
| `pattern.finditer()` | Return iterator of matches |
| `pattern.sub()` | Substitute matches |
| `pattern.split()` | Split string using regex |

## Example: Pattern Methods

```python
import re

pattern = re.compile(r"\d+")

text = "Order 123, Invoice 456"

print(pattern.findall(text))
```

### Output

```text
['123', '456']
```

### Example Using `search()`

```python
match = pattern.search(text)

print(match.group())
```

### Output

```text
123
```

---

# 3. Match Objects

A match object represents a successful match produced by:

- `re.match()`
- `re.search()`
- `re.finditer()`
- Compiled pattern methods

### Example

```python
import re

text = "Age: 25"

match = re.search(r"\d+", text)

print(match)
```

### Output

```text
<re.Match object>
```

The match object contains detailed information about the match.

---

# 4. Match Object Methods

Match objects provide methods for retrieving match information.

| Method | Purpose |
|----------|----------|
| `group()` | Matched text |
| `groups()` | All captured groups |
| `groupdict()` | Named groups dictionary |
| `start()` | Start index |
| `end()` | End index |
| `span()` | Start and end positions |

---

# Working with Match Objects

Now we explore how to use match object methods.

---

# 5. group()

The `group()` method returns the matched substring.

### Example

```python
import re

text = "Age: 25"

match = re.search(r"\d+", text)

print(match.group())
```

### Output

```text
25
```

---

## Accessing Specific Groups

If capturing groups exist:

```python
import re

text = "John Smith"

match = re.search(r"(\w+) (\w+)", text)

print(match.group(1))
print(match.group(2))
```

### Output

```text
John
Smith
```

---

# 6. groups()

`groups()` returns all captured groups as a tuple.

### Example

```python
import re

text = "John Smith"

match = re.search(r"(\w+) (\w+)", text)

print(match.groups())
```

### Output

```text
('John', 'Smith')
```

This is useful when extracting multiple pieces of data at once.

---

# 7. groupdict()

If named groups are used, `groupdict()` returns a dictionary of matches.

### Example

```python
import re

text = "Email: user@gmail.com"

pattern = r"(?P<username>\w+)@(?P<domain>\w+)\.(?P<ext>\w+)"

match = re.search(pattern, text)

print(match.groupdict())
```

### Output

```text
{
    'username': 'user',
    'domain': 'gmail',
    'ext': 'com'
}
```

Named groups make structured data extraction easier.

---

# 8. start()

`start()` returns the starting index of the match.

### Example

```python
import re

text = "Order number: 12345"

match = re.search(r"\d+", text)

print(match.start())
```

### Output

```text
14
```

Meaning the match begins at position `14`.

---

# 9. end()

`end()` returns the ending index of the match.

### Example

```python
print(match.end())
```

### Output

```text
19
```

---

# 10. span()

`span()` returns both start and end positions.

### Example

```python
print(match.span())
```

### Output

```text
(14, 19)
```

Meaning:

```python
text[14:19]
```

returns:

```text
12345
```

---

# 11. Extracting Matched Substrings

Using `span()`, we can extract the substring manually.

### Example

```python
start, end = match.span()

print(text[start:end])
```

### Output

```text
12345
```

This is helpful when processing multiple matches in large texts.

---

# Iterating Through Matches

Often we need to process multiple matches one by one.

This is where `finditer()` becomes powerful.

---

# 12. Using finditer()

`finditer()` returns an iterator of match objects.

Each match can be examined individually.

### Example

```python
import re

text = "Prices: 100 250 300"

matches = re.finditer(r"\d+", text)

for match in matches:
    print(match.group())
```

### Output

```text
100
250
300
```

---

# 13. Looping Through Matches

Using `finditer()` allows detailed inspection.

### Example

```python
import re

text = "Product A1 costs 100, Product B2 costs 200"

matches = re.finditer(r"\d+", text)

for match in matches:
    print("Match:", match.group())
    print("Start:", match.start())
    print("End:", match.end())
```

### Output

```text
Match: 1
Start: 9
End: 10

Match: 100
Start: 17
End: 20

Match: 2
Start: 30
End: 31

Match: 200
Start: 38
End: 41
```

---

# 14. Accessing Match Details

Using match objects we can extract rich information.

### Example: Extracting Structured Data

#### Text

```text
Name: Rahul
Age: 25
City: Pune
```

### Python Example

```python
import re

text = """
Name: Rahul
Age: 25
City: Pune
"""

pattern = r"(\w+): (\w+)"

matches = re.finditer(pattern, text)

for match in matches:
    key = match.group(1)
    value = match.group(2)

    print(key, "→", value)
```

### Output

```text
Name → Rahul
Age → 25
City → Pune
```

---

# 15. Real-World Example (Log File Parsing)

### Text Log

```text
ERROR 2024-01-01 Disk failure
INFO 2024-01-02 System started
WARNING 2024-01-03 Low memory
```

### Regex Pattern

```regex
(\w+) (\d{4}-\d{2}-\d{2}) (.+)
```

### Python Program

```python
import re

log = """
ERROR 2024-01-01 Disk failure
INFO 2024-01-02 System started
WARNING 2024-01-03 Low memory
"""

pattern = r"(\w+) (\d{4}-\d{2}-\d{2}) (.+)"

for match in re.finditer(pattern, log):
    level = match.group(1)
    date = match.group(2)
    message = match.group(3)

    print(level, date, message)
```

### Output

```text
ERROR 2024-01-01 Disk failure
INFO 2024-01-02 System started
WARNING 2024-01-03 Low memory
```

This is how regex is used for log parsing and data extraction.

---

# 16. Summary Table

## Objects

| Object | Purpose |
|----------|----------|
| Regex Pattern Object | Compiled regex pattern |
| Match Object | Represents a successful match |

---

## Pattern Object Methods

| Method | Description |
|----------|-------------|
| `match()` | Match at start |
| `search()` | Find first match |
| `findall()` | Return all matches |
| `finditer()` | Return match iterator |
| `sub()` | Substitute text |
| `split()` | Split string |

---

## Match Object Methods

| Method | Description |
|----------|-------------|
| `group()` | Matched text |
| `groups()` | Tuple of captured groups |
| `groupdict()` | Dictionary of named groups |
| `start()` | Start position |
| `end()` | End position |
| `span()` | Start and end indexes |

---

# Key Takeaways

Regex objects and match handling allow Python programs to:

- Compile reusable regex patterns
- Inspect match results in detail
- Extract structured information from text
- Iterate through multiple matches
- Analyze positions within strings

These capabilities are widely used in:

- Data extraction
- Log analysis
- Web scraping
- Text processing pipelines
- Input validation systems

Understanding pattern objects and match objects is essential for advanced regex usage because they provide far more information than simple pattern matching. Together, they form the foundation of professional text-processing and data-extraction workflows in Python.
