# Regular Expressions Hands-On Tutorial
## Python Regular Expressions Library: Grouping, Named Groups, and Backreferences in Python Regex

Regular expressions become extremely powerful when we start using groups.

Grouping allows us to:

- Capture parts of a match
- Organize complex patterns
- Extract structured data
- Reuse previous matches
- Perform intelligent substitutions

Python's `re` module provides strong support for capturing groups, named groups, and backreferences, which are essential for advanced text processing.

This article explains these concepts in deep practical detail.

---

# 1. Grouping in Regex

Grouping allows us to treat multiple characters as a single unit.

Groups are created using parentheses:

```regex
(pattern)
```

Parentheses tell the regex engine:

- Treat the pattern inside as a single logical unit
- Capture the matched text

## Example

Pattern:

```regex
(cat)
```

Text:

```text
cat dog cat
```

Python example:

```python
import re

text = "cat dog cat"

match = re.search(r"(cat)", text)

print(match.group())
```

**Output**

```text
cat
```

---

# 2. Capturing Groups `( )`

A capturing group stores the matched text so it can be retrieved later.

Each group receives a number starting from `1`.

## Example Pattern

```regex
(\w+) (\w+)
```

Meaning:

- First word
- Space
- Second word

## Example

Text:

```text
John Smith
```

Python:

```python
import re

text = "John Smith"

match = re.search(r"(\w+) (\w+)", text)

print(match.group(0))
print(match.group(1))
print(match.group(2))
```

**Output**

```text
John Smith
John
Smith
```

### Explanation

| Group | Value |
|---------|---------|
| `group(0)` | Entire match |
| `group(1)` | First word |
| `group(2)` | Second word |

---

# 3. Accessing Group Results

The Match object provides methods to access group results.

## Methods

| Method | Purpose |
|----------|-----------|
| `group()` | Full match |
| `group(n)` | Specific group |
| `groups()` | Tuple of all groups |

## Example

```python
import re

text = "John Smith"

match = re.search(r"(\w+) (\w+)", text)

print(match.groups())
```

**Output**

```python
('John', 'Smith')
```

---

# 4. Multiple Groups

You can define multiple groups in one regex.

## Example Pattern

```regex
(\w+)@(\w+)\.(\w+)
```

Meaning:

- Username
- `@`
- Domain
- `.`
- Extension

## Example

Text:

```text
admin@gmail.com
```

Python:

```python
import re

text = "admin@gmail.com"

match = re.search(r"(\w+)@(\w+)\.(\w+)", text)

print(match.group(1))
print(match.group(2))
print(match.group(3))
```

**Output**

```text
admin
gmail
com
```

---

# 5. Nested Groups

Groups can be nested inside other groups.

## Example Pattern

```regex
((\d{2})-(\d{2})-(\d{4}))
```

Matching date format:

```text
DD-MM-YYYY
```

## Example

Text:

```text
12-05-2024
```

Python:

```python
import re

text = "12-05-2024"

match = re.search(r"((\d{2})-(\d{2})-(\d{4}))", text)

print(match.group(0))
print(match.group(1))
print(match.group(2))
print(match.group(3))
print(match.group(4))
```

**Output**

```text
12-05-2024
12-05-2024
12
05
2024
```

Nested groups allow extracting subcomponents of complex patterns.

---

# 6. Numbered Groups

Groups are numbered from left to right based on opening parentheses.

## Example Pattern

```regex
(\w+) (\d+)
```

### Groups

| Group | Meaning |
|---------|---------|
| 1 | Word |
| 2 | Number |

## Example

Text:

```text
Item 100
```

Python:

```python
import re

text = "Item 100"

match = re.search(r"(\w+) (\d+)", text)

print(match.group(1))
print(match.group(2))
```

**Output**

```text
Item
100
```

---

# 7. Using Groups in Substitutions

Captured groups can be reused during replacements.

## Syntax

```regex
\1 \2 \3
```

## Example: Swap Names

Text:

```text
John Smith
```

Goal:

```text
Smith John
```

Python:

```python
import re

text = "John Smith"

result = re.sub(r"(\w+) (\w+)", r"\2 \1", text)

print(result)
```

**Output**

```text
Smith John
```

### Explanation

| Group | Value |
|---------|---------|
| `\1` | John |
| `\2` | Smith |

Replacement uses:

```regex
\2 \1
```

---

# Named Groups

Numbered groups can become confusing in complex regex patterns.

Python supports named groups, which provide clearer access.

---

# 8. Defining Named Groups `(?P<name>)`

Named groups are defined using:

```regex
(?P<name>pattern)
```

## Example

```regex
(?P<username>\w+)
```

### Pattern

```regex
(?P<user>\w+)@(?P<domain>\w+)\.(?P<ext>\w+)
```

Text:

```text
admin@gmail.com
```

Python:

```python
import re

text = "admin@gmail.com"

match = re.search(
    r"(?P<user>\w+)@(?P<domain>\w+)\.(?P<ext>\w+)",
    text
)

print(match.group("user"))
print(match.group("domain"))
print(match.group("ext"))
```

**Output**

```text
admin
gmail
com
```

---

# 9. Accessing Named Groups

Named groups can be accessed using:

```python
match.group("name")
```

or:

```python
match.groupdict()
```

## Example

```python
import re

text = "admin@gmail.com"

match = re.search(
    r"(?P<user>\w+)@(?P<domain>\w+)\.(?P<ext>\w+)",
    text
)

print(match.groupdict())
```

**Output**

```python
{'user': 'admin', 'domain': 'gmail', 'ext': 'com'}
```

---

# 10. Named Group Matches

Named groups make complex regex patterns easier to understand.

## Example: Extracting Date Parts

Pattern:

```regex
(?P<day>\d{2})-(?P<month>\d{2})-(?P<year>\d{4})
```

Python:

```python
import re

text = "12-05-2024"

match = re.search(
    r"(?P<day>\d{2})-(?P<month>\d{2})-(?P<year>\d{4})",
    text
)

print(match.group("day"))
print(match.group("month"))
print(match.group("year"))
```

**Output**

```text
12
05
2024
```

---

# 11. Named Groups in Substitutions

Named groups can also be used in replacements.

## Syntax

```regex
\g<name>
```

## Example

Text:

```text
John Smith
```

Goal:

```text
Smith John
```

Python:

```python
import re

text = "John Smith"

result = re.sub(
    r"(?P<first>\w+) (?P<last>\w+)",
    r"\g<last> \g<first>",
    text
)

print(result)
```

**Output**

```text
Smith John
```

---

# Backreferences

Backreferences allow regex to refer to previously captured groups.

This enables matching repeated patterns.

---

# 12. Referencing Previous Groups `\1`

## Syntax

```regex
\1
```

Meaning:

```text
same text matched by group 1
```

## Example

Text:

```text
hello hello
```

Regex:

```regex
(\w+) \1
```

Meaning:

```text
same word repeated
```

Python:

```python
import re

text = "hello hello"

match = re.search(r"(\w+) \1", text)

print(match.group())
```

**Output**

```text
hello hello
```

---

# 13. Matching Repeated Patterns

## Example: Detecting Duplicate Words

Text:

```text
This is is a test
```

Regex:

```regex
(\w+)\s+\1
```

Python:

```python
import re

text = "This is is a test"

match = re.search(r"(\w+)\s+\1", text)

print(match.group())
```

**Output**

```text
is is
```

Backreferences are very useful for:

- Detecting duplicate words
- Validating repeated structures
- Ensuring consistency in patterns

---

# 14. Backreferences in Substitutions

Backreferences can also be used during replacements.

## Example: Removing Duplicate Words

Text:

```text
This is is a test
```

Python:

```python
import re

text = "This is is a test"

result = re.sub(r"(\w+)\s+\1", r"\1", text)

print(result)
```

**Output**

```text
This is a test
```

---

# 15. Practical Example (Extracting Structured Data)

Text:

```text
Name: Rahul
Email: rahul@gmail.com
Phone: 9876543210
```

Python Program:

```python
import re

pattern = (
    r"Name: (?P<name>\w+).*"
    r"Email: (?P<email>\S+).*"
    r"Phone: (?P<phone>\d+)"
)

text = """
Name: Rahul
Email: rahul@gmail.com
Phone: 9876543210
"""

match = re.search(pattern, text, re.DOTALL)

print(match.group("name"))
print(match.group("email"))
print(match.group("phone"))
```

**Output**

```text
Rahul
rahul@gmail.com
9876543210
```

---

# 16. Summary Table

| Feature | Syntax | Purpose |
|----------|----------|----------|
| Grouping | `(pattern)` | Capture text |
| Access group | `group(n)` | Retrieve match |
| Named group | `(?P<name>pattern)` | Label group |
| Named access | `group("name")` | Retrieve named match |
| Backreference | `\1` | Refer to previous group |
| Named reference | `\g<name>` | Refer in replacement |

---

# Conclusion

Grouping, named groups, and backreferences are among the most powerful capabilities of regular expressions.

They allow regex to:

- Extract structured information
- Organize complex patterns
- Reuse matched data
- Detect repeated patterns
- Perform intelligent substitutions

## Key Takeaways

- Groups `( )` capture parts of matches
- Named groups improve readability
- Backreferences enforce pattern consistency

These features transform regex from simple pattern matching into a powerful text parsing and transformation system.
```
