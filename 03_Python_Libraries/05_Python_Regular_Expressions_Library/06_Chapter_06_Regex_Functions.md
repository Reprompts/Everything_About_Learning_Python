# Regular Expressions Hands-On Tutorial 
## Python Regular Expressions Library: Regex Functions in Python (`re` Module)

Python provides powerful support for regular expressions through the `re` module. This module contains several functions that allow you to search, extract, split, and modify text using regex patterns.

Understanding these functions is essential because each function behaves slightly differently depending on how and where it searches for patterns.

This article explains the most important regex functions in Python with clear explanations and practical examples.

---

# 1. Overview of the `re` Module

The `re` module provides functions that allow programs to:

- Search text for patterns
- Extract specific pieces of data
- Split text based on patterns
- Replace text patterns

To use regex in Python:

```python
import re
```

Once imported, we can use functions like:

- `re.match()`
- `re.search()`
- `re.findall()`
- `re.finditer()`
- `re.split()`
- `re.sub()`
- `re.subn()`
- `re.compile()`

Each function serves a different purpose.

---

# 2. `re.match()`

## Purpose

`re.match()` checks for a match only at the beginning of a string.

### Syntax

```python
re.match(pattern, string)
```

It returns:

- A Match object if the pattern matches at the start
- `None` if no match occurs

### Example

```python
import re

text = "Python is powerful"

match = re.match(r"Python", text)

print(match)
```

**Output**

```python
<re.Match object>
```

### Extracting the Matched Text

```python
print(match.group())
```

**Output**

```python
Python
```

### When It Fails

```python
text = "I love Python"

match = re.match(r"Python", text)

print(match)
```

**Output**

```python
None
```

Because `re.match()` only checks the start of the string.

---

# 3. `re.search()`

## Purpose

`re.search()` finds the first occurrence of a pattern anywhere in the string.

### Syntax

```python
re.search(pattern, string)
```

### Example

```python
import re

text = "I love Python"

match = re.search(r"Python", text)

print(match.group())
```

**Output**

```python
Python
```

Unlike `re.match()`, this works because the pattern appears anywhere in the text.

### Example: Finding Numbers

```python
import re

text = "Order number: 45821"

match = re.search(r"\d+", text)

print(match.group())
```

**Output**

```python
45821
```

### Key Difference

| Function | Behavior |
|-----------|-----------|
| `re.match()` | Start of string only |
| `re.search()` | Anywhere in string |

---

# 4. `re.findall()`

## Purpose

`re.findall()` returns all matches of a pattern in a list.

### Syntax

```python
re.findall(pattern, string)
```

### Example

```python
import re

text = "apple banana mango"

matches = re.findall(r"a\w+", text)

print(matches)
```

**Output**

```python
['apple', 'anana', 'ango']
```

### Extract Numbers

```python
import re

text = "Prices: 10, 20, 30"

numbers = re.findall(r"\d+", text)

print(numbers)
```

**Output**

```python
['10', '20', '30']
```

`findall()` is useful when you need every match in the text.

---

# 5. `re.finditer()`

## Purpose

`re.finditer()` returns an iterator of match objects instead of a list.

### Syntax

```python
re.finditer(pattern, string)
```

This allows you to access:

- Matched text
- Position in the string
- Start and end indices

### Example

```python
import re

text = "Price: 100, 200, 300"

matches = re.finditer(r"\d+", text)

for match in matches:
    print(match.group(), match.start())
```

**Output**

```python
100 7
200 12
300 17
```

### Advantages of `finditer()`

It provides additional information:

| Method | Purpose |
|----------|---------|
| `group()` | Matched text |
| `start()` | Start index |
| `end()` | End index |

---

# 6. `re.split()`

## Purpose

`re.split()` splits a string based on a regex pattern.

### Syntax

```python
re.split(pattern, string)
```

### Example

Split by spaces:

```python
import re

text = "apple banana mango"

words = re.split(r"\s+", text)

print(words)
```

**Output**

```python
['apple', 'banana', 'mango']
```

### Example: Split by Multiple Separators

Text:

```text
apple,banana;grape orange
```

Regex:

```regex
[,; ]
```

Python:

```python
import re

text = "apple,banana;grape orange"

result = re.split(r"[,; ]", text)

print(result)
```

**Output**

```python
['apple', 'banana', 'grape', 'orange']
```

---

# 7. `re.sub()`

## Purpose

`re.sub()` replaces matches with another string.

### Syntax

```python
re.sub(pattern, replacement, string)
```

### Example

Replace digits:

```python
import re

text = "Price: 100"

result = re.sub(r"\d+", "XXX", text)

print(result)
```

**Output**

```python
Price: XXX
```

### Example: Remove Special Characters

```python
import re

text = "Hello@World#2024!"

clean = re.sub(r"\W", "", text)

print(clean)
```

**Output**

```python
HelloWorld2024
```

---

# 8. `re.subn()`

## Purpose

`re.subn()` works like `re.sub()` but also returns the number of replacements.

### Syntax

```python
re.subn(pattern, replacement, string)
```

Returns:

```python
(new_string, number_of_replacements)
```

### Example

```python
import re

text = "apple banana apple"

result = re.subn(r"apple", "orange", text)

print(result)
```

**Output**

```python
('orange banana orange', 2)
```

This tells us:

- Replacement happened 2 times

---

# 9. `re.compile()`

## Purpose

`re.compile()` creates a compiled regex object.

### Syntax

```python
pattern = re.compile(regex)
```

Compiled patterns can be reused multiple times.

### Example

```python
import re

pattern = re.compile(r"\d+")

text = "Price 100 and 200"

matches = pattern.findall(text)

print(matches)
```

**Output**

```python
['100', '200']
```

---

# 10. Why Use Compiled Patterns?

Compiled patterns improve:

- Performance
- Code readability
- Reusability

Instead of writing:

```python
re.findall(r"\d+", text)
re.search(r"\d+", text)
```

You compile once:

```python
pattern = re.compile(r"\d+")
```

Then reuse:

```python
pattern.search()
pattern.findall()
pattern.sub()
```

---

# 11. Using Methods on Compiled Patterns

Compiled patterns support methods similar to `re` functions.

### Example

```python
import re

pattern = re.compile(r"\d+")

text = "Numbers: 10 20 30"

print(pattern.findall(text))
```

**Output**

```python
['10', '20', '30']
```

---

# 12. Example: Extract Emails Using Compiled Regex

```python
import re

pattern = re.compile(r"\w+@\w+\.\w+")

text = """
Contact:
admin@test.com
user@gmail.com
"""

emails = pattern.findall(text)

print(emails)
```

**Output**

```python
['admin@test.com', 'user@gmail.com']
```

Compiled regex is efficient for large datasets.

---

# 13. Summary Table

| Function | Purpose |
|-----------|----------|
| `re.match()` | Match at the beginning of the string |
| `re.search()` | Find the first match anywhere |
| `re.findall()` | Return all matches |
| `re.finditer()` | Return an iterator of match objects |
| `re.split()` | Split string using regex |
| `re.sub()` | Replace matches |
| `re.subn()` | Replace matches and return count |
| `re.compile()` | Create a reusable regex object |

---

# 14. Example: Putting Everything Together

```python
import re

text = "Emails: test1@gmail.com test2@yahoo.com"

pattern = r"\w+@\w+\.\w+"

print("Search:", re.search(pattern, text).group())

print("Findall:", re.findall(pattern, text))

for match in re.finditer(pattern, text):
    print("Found:", match.group(), "at", match.start())

print("Replace:", re.sub(pattern, "[EMAIL]", text))
```

**Output**

```python
Search: test1@gmail.com

Findall: ['test1@gmail.com', 'test2@yahoo.com']

Found: test1@gmail.com at 8

Found: test2@yahoo.com at 24

Replace: Emails: [EMAIL] [EMAIL]
```

---

# Conclusion

The `re` module functions provide powerful ways to interact with text using regular expressions.

## Key Capabilities

- Matching patterns
- Extracting multiple values
- Splitting text
- Replacing patterns
- Reusing compiled regex

Understanding these functions allows Python programs to perform advanced tasks such as:

- Log file analysis
- Data cleaning
- Web scraping
- Text parsing
- Form validation

Together, these tools make regex an essential part of modern Python text processing.
