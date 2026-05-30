# Regular Expressions Hands-On Tutorial
## Python Regular Expressions Library: Advanced Regex Usage, Applications, and Best Practices in Python

Once you understand regex syntax, groups, lookarounds, and match objects, the next step is learning how to use regex effectively in real-world programs.

Advanced regex usage focuses on:

- Text substitution
- Splitting complex strings
- Handling large text blocks
- Optimizing performance
- Building reliable validation patterns
- Writing maintainable regex

This article explores these topics in practical depth with Python examples.

---

# 1. Substitution and Splitting

Regex is widely used to modify text and restructure data.

Python's `re` module provides:

- `re.sub()`
- `re.subn()`
- `re.split()`

These functions enable powerful text transformations.

---

# 2. Replacing Text with `re.sub()`

`re.sub()` replaces occurrences of a pattern.

## Syntax

```python
re.sub(pattern, replacement, string)
```

## Example: Replace Digits with `#`

```python
import re

text = "User123 logged in at 10:45"
result = re.sub(r"\d", "#", text)

print(result)
```

### Output

```text
User### logged in at ##:##
```

---

# 3. Limiting Replacements

You can limit how many replacements occur using the `count` argument.

## Syntax

```python
re.sub(pattern, replacement, string, count)
```

## Example

```python
import re

text = "apple apple apple"

result = re.sub(
    r"apple",
    "orange",
    text,
    count=2
)

print(result)
```

### Output

```text
orange orange apple
```

Only the first two matches are replaced.

---

# 4. Using Functions for Replacement

Instead of a fixed replacement string, `re.sub()` can call a function for each match.

This allows dynamic transformations.

## Example: Double Every Number

```python
import re

def double_number(match):
    number = int(match.group())
    return str(number * 2)

text = "Prices: 10 20 30"

result = re.sub(
    r"\d+",
    double_number,
    text
)

print(result)
```

### Output

```text
Prices: 20 40 60
```

---

# 5. Group References in Replacements

Captured groups can be referenced in replacement strings.

## Syntax

```text
\1 \2 \3
```

## Example: Swap First and Last Names

```python
import re

text = "John Smith"

result = re.sub(
    r"(\w+) (\w+)",
    r"\2 \1",
    text
)

print(result)
```

### Output

```text
Smith John
```

## Using Named Groups

```python
re.sub(
    r"(?P<first>\w+) (?P<last>\w+)",
    r"\g<last> \g<first>",
    text
)
```

---

# 6. Splitting Strings with Regex

`re.split()` splits strings using regex patterns.

## Syntax

```python
re.split(pattern, string)
```

## Example

```python
import re

text = "apple,banana;orange grape"

result = re.split(
    r"[ ,;]",
    text
)

print(result)
```

### Output

```python
['apple', 'banana', 'orange', 'grape']
```

---

# 7. Multiple Delimiters

Regex allows splitting using multiple separators.

Examples:

- Comma
- Semicolon
- Space
- Tab

### Regex

```regex
[,\s;]+
```

### Python Example

```python
import re

text = "apple, banana; orange   grape"

result = re.split(
    r"[,\s;]+",
    text
)

print(result)
```

### Output

```python
['apple', 'banana', 'orange', 'grape']
```

---

# Escaping and Handling

Regex uses many special characters, which sometimes must be escaped.

---

# 8. Escaping Regex Characters

Special regex characters include:

```text
. ^ $ * + ? { } [ ] \ | ( )
```

To match them literally, use a backslash.

## Example: Match a Literal Dot

```regex
\.
```

### Python Example

```python
import re

text = "file.txt"

match = re.search(r"\.", text)

print(match.group())
```

### Output

```text
.
```

---

# 9. Using `re.escape()`

When patterns contain user input, escaping manually is unsafe.

Python provides:

```python
re.escape()
```

## Example

```python
import re

text = "price is $100"

pattern = re.escape("$100")

match = re.search(pattern, text)

print(match.group())
```

### Output

```text
$100
```

`re.escape()` converts special characters into literal matches.

---

# Multiline and Complex Matching

Real-world data often spans multiple lines.

Regex flags allow matching across lines.

---

# 10. Multiline Mode

### Flag

```python
re.MULTILINE
```

### Effect

`^` and `$` match the start and end of each line.

## Example

```python
import re

text = """apple
banana
orange"""

matches = re.findall(
    r"^\w+",
    text,
    re.MULTILINE
)

print(matches)
```

### Output

```python
['apple', 'banana', 'orange']
```

---

# 11. Matching Across Lines

Normally:

```regex
.
```

does not match newline characters.

Use:

```python
re.DOTALL
```

## Example

```python
import re

text = "start\nmiddle\nend"

match = re.search(
    r"start.*end",
    text,
    re.DOTALL
)

print(match.group())
```

### Output

```text
start
middle
end
```

---

# 12. Handling Large Text Blocks

When processing:

- Log files
- HTML documents
- Configuration files
- Reports

Use:

- `re.DOTALL`
- `re.MULTILINE`
- Compiled patterns

### Example Use Case

Extracting sections from large documents using compiled regex patterns.

---

# Regex Performance

Bad regex patterns can become extremely slow.

Understanding performance is crucial.

---

# 13. Optimizing Regex

## Tips

- Avoid unnecessary backtracking
- Use specific patterns instead of `.*`
- Anchor patterns when possible
- Use compiled regex

### Example Improvement

#### Bad Pattern

```regex
.*error.*
```

#### Better Pattern

```regex
error
```

---

# 14. Avoiding Catastrophic Backtracking

### Example Problematic Regex

```regex
(a+)+
```

Applied to:

```text
aaaaaaaaaaaaaaaaaaaa!
```

This can cause exponential backtracking and severe performance issues.

### Solution

Use more precise patterns whenever possible.

---

# 15. Efficient Pattern Design

## Guidelines

Prefer:

```regex
\d+
```

Instead of:

```regex
[0-9]+
```

Use anchors whenever possible:

```regex
^pattern$
```

Avoid excessive wildcards:

```regex
.*.*.*
```

---

# 16. Using Compiled Patterns

Compiled patterns improve performance.

## Example

```python
import re

pattern = re.compile(r"\d+")

for line in dataset:
    pattern.search(line)
```

This avoids recompiling the regex on every iteration.

---

# Practical Applications

Regex is heavily used in data processing pipelines.

---

# 17. Extracting Emails

### Regex

```regex
[\w\.-]+@[\w\.-]+\.\w+
```

### Python Example

```python
import re

text = """
Contact us:
support@gmail.com
admin@yahoo.com
"""

emails = re.findall(
    r"[\w\.-]+@[\w\.-]+\.\w+",
    text
)

print(emails)
```

### Output

```python
['support@gmail.com', 'admin@yahoo.com']
```

---

# 18. Extracting Phone Numbers

### Example Format

```text
9876543210
```

### Regex

```regex
\d{10}
```

### Python Example

```python
phones = re.findall(r"\d{10}", text)
```

---

# 19. Validating Passwords

### Requirements

- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one digit

### Regex

```regex
^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$
```

### Python

```python
bool(re.match(pattern, password))
```

---

# 20. Validating Usernames

### Requirements

- Letters
- Digits
- Underscores
- Length between 3 and 20 characters

### Regex

```regex
^[a-zA-Z0-9_]{3,20}$
```

---

# 21. Log Parsing

### Example Log Entry

```text
ERROR 2024-01-01 Disk failure
```

### Regex

```regex
(\w+) (\d{4}-\d{2}-\d{2}) (.+)
```

### Extracted Fields

- Log level
- Date
- Message

---

# 22. Structured Text Parsing

### Example

```text
Name: Rahul
Age: 25
```

### Regex

```regex
(\w+): (.+)
```

Used for:

- Configuration files
- Metadata extraction
- Key-value parsing

---

# 23. Data Cleaning

Regex helps clean messy data.

## Remove Extra Spaces

```python
re.sub(r"\s+", " ", text)
```

## Remove HTML Tags

```python
re.sub(r"<.*?>", "", html)
```

---

# Debugging and Best Practices

Complex regex patterns can become difficult to read and maintain.

Good practices help keep regex manageable.

---

# 24. Testing Patterns

Always test regex patterns using:

- Sample inputs
- Edge cases
- Invalid inputs

### Useful Tools

- regex101
- Python test scripts
- Unit tests

---

# 25. Using Verbose Mode

### Flag

```python
re.VERBOSE
```

Allows multi-line readable regex definitions.

## Example

```python
import re

pattern = re.compile(
    r"""
    \d{4}   # year
    -
    \d{2}   # month
    -
    \d{2}   # day
    """,
    re.VERBOSE
)
```

Benefits:

- Readability
- Easier maintenance
- Built-in documentation

---

# 26. Breaking Complex Regex

Instead of creating one huge regex, break patterns into logical parts.

### Example Components

```python
email_user
email_domain
email_extension
```

Combine them later into a complete pattern.

This improves readability and maintainability.

---

# 27. Writing Readable Patterns

Prefer:

```regex
(?P<year>\d{4})
```

Instead of:

```regex
(\d{4})
```

Named groups clearly describe what each captured value represents.

---

# 28. Documenting Regex

Complex regex should always include comments.

## Example

```python
# Extract email addresses
pattern = r"[\w\.-]+@[\w\.-]+\.\w+"
```

Documentation prevents confusion and simplifies maintenance.

---

# Final Summary

Advanced regex techniques enable powerful text processing.

## Text Processing

- `re.sub()` for replacements
- `re.split()` for splitting strings
- Group references in replacements

## Handling Patterns

- Escaping special characters
- `re.escape()` for safe patterns

## Multiline Processing

- `re.MULTILINE`
- `re.DOTALL`

## Performance

- Compiled regex
- Avoiding catastrophic backtracking
- Efficient pattern design

## Real Applications

- Email extraction
- Phone number detection
- Password validation
- Log parsing
- Data cleaning

## Best Practices

- Test regex patterns thoroughly
- Use verbose mode for complex regex
- Write readable regex
- Document complex patterns

Regex is one of the most powerful tools in Python for text processing, enabling developers to build systems for:

- Data extraction
- Text transformation
- Validation
- Log analysis
- Data cleaning pipelines
