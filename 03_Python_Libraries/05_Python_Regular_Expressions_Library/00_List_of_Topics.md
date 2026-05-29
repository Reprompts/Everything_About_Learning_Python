# Python Regular Expressions Library

---

# 1. Introduction to Regular Expressions

## What are Regular Expressions (Regex)

Regular Expressions (Regex) are special sequences of characters used for pattern matching in text.

Regex allows programmers to:

* Search text
* Extract information
* Validate formats
* Replace content
* Split strings

Regex acts like a mini language for describing text patterns.

Example:

```python
import re

text = "My number is 9876543210"

match = re.search(r"\d+", text)

print(match.group())
```

Output:

```python
9876543210
```

---

## Purpose of Regex

Regex is used to:

* Find patterns in text
* Validate user input
* Process large text data
* Automate text manipulation
* Extract structured information

---

## Common Use Cases

### Pattern Matching

Finding words, numbers, or symbols inside text.

Example:

```python
import re

text = "Python is powerful"

result = re.search(r"powerful", text)

print(result.group())
```

---

### Data Extraction

Extracting emails, phone numbers, dates, etc.

Example:

```python
import re

text = "Contact: admin@example.com"

email = re.search(r"\S+@\S+", text)

print(email.group())
```

---

### Validation

Checking whether input follows a specific format.

Example:

```python
import re

password = "Abc12345"

pattern = r"^[A-Za-z0-9]{8,}$"

print(bool(re.match(pattern, password)))
```

---

### Text Manipulation

Replacing or modifying text.

Example:

```python
import re

text = "I love Java"

updated = re.sub(r"Java", "Python", text)

print(updated)
```

---

## Regex in Python

Python provides regex support through the built-in `re` module.

---

## Importing the re Module

```python
import re
```

---

## Basic Regex Workflow

1. Create a regex pattern
2. Apply it using `re` functions
3. Process match results

Example:

```python
import re

text = "Order ID: 12345"

pattern = r"\d+"

match = re.search(pattern, text)

print(match.group())
```

---

## Regex vs Normal String Operations

### Normal String Operations

Good for simple tasks.

```python
text = "hello world"

print("world" in text)
```

---

### Regex

Better for complex pattern matching.

```python
import re

text = "abc123xyz"

print(bool(re.search(r"\d+", text)))
```

---

## Understanding Pattern Matching

Regex patterns describe:

* Characters
* Character groups
* Repetition
* Position
* Conditions

Example:

```python
r"\d{3}"
```

Meaning:

* `\d` → digit
* `{3}` → exactly 3 times

Matches:

```python
123
456
999
```

---

# 2. Basic Regex Syntax and Pattern Basics

## Literal Characters

Literal characters match themselves.

Example:

```python
import re

print(bool(re.search(r"cat", "black cat")))
```

---

## Special Characters

Regex special characters have special meanings.

| Character | Meaning         |
| --------- | --------------- |
| .         | Any character   |
| ^         | Start           |
| $         | End             |
| *         | Zero or more    |
| +         | One or more     |
| ?         | Optional        |
| []        | Character class |
| ()        | Grouping        |

---

## Escaping Special Characters

Use `\` to treat special characters literally.

Example:

```python
import re

text = "Price is $100"

print(bool(re.search(r"\$", text)))
```

---

## Using Raw Strings (`r""`) for Patterns

Raw strings prevent escape issues.

Recommended:

```python
pattern = r"\d+"
```

Instead of:

```python
pattern = "\\d+"
```

---

## Whitespace Characters

| Pattern | Meaning    |
| ------- | ---------- |
| `\s`    | Whitespace |
| `\t`    | Tab        |
| `\n`    | Newline    |

Example:

```python
import re

text = "Hello World"

print(re.findall(r"\s", text))
```

---

## Digit Characters

```python
\d
```

Matches digits.

Example:

```python
import re

print(re.findall(r"\d", "A1B2C3"))
```

---

## Word Characters

```python
\w
```

Matches:

* letters
* digits
* underscore

Example:

```python
import re

print(re.findall(r"\w", "hello_123"))
```

---

## Non-Word Characters

```python
\W
```

Matches symbols and spaces.

Example:

```python
import re

print(re.findall(r"\W", "hello@world!"))
```

---

# 3. Character Classes and Predefined Classes

## Character Classes

Character classes use square brackets.

---

## Square Brackets `[ ]`

Example:

```python
[abc]
```

Matches:

* a
* b
* c

---

## Matching Specific Characters

```python
import re

print(re.findall(r"[aeiou]", "programming"))
```

---

## Matching Character Ranges

```python
[a-z]
[A-Z]
[0-9]
```

Example:

```python
import re

print(re.findall(r"[A-Z]", "Python Regex"))
```

---

## Matching Digits and Letters

```python
[a-zA-Z0-9]
```

---

## Uppercase and Lowercase Matching

```python
import re

text = "PyThOn"

print(re.findall(r"[A-Z]", text))
print(re.findall(r"[a-z]", text))
```

---

## Negated Character Classes `[^ ]`

Matches everything except specified characters.

Example:

```python
import re

print(re.findall(r"[^0-9]", "abc123"))
```

---

## Combining Characters in Classes

```python
[a-zA-Z_]
```

---

## Predefined Classes

| Pattern | Meaning             |
| ------- | ------------------- |
| `\d`    | Digits              |
| `\D`    | Non-digits          |
| `\w`    | Word characters     |
| `\W`    | Non-word characters |
| `\s`    | Whitespace          |
| `\S`    | Non-whitespace      |

---

# 4. Anchors and Boundaries

## Start of String Anchor `^`

Example:

```python
import re

print(bool(re.search(r"^Hello", "Hello World")))
```

---

## End of String Anchor `$`

Example:

```python
import re

print(bool(re.search(r"World$", "Hello World")))
```

---

## Word Boundary `\b`

Example:

```python
import re

print(bool(re.search(r"\bcat\b", "black cat")))
```

---

## Non-Word Boundary `\B`

Example:

```python
import re

print(bool(re.search(r"\Bcat\B", "concatenate")))
```

---

## Using Anchors in Multiline Text

```python
import re

text = """Hello
World"""

print(re.findall(r"^World", text, re.MULTILINE))
```

---

## Controlling Where Matches Occur

Anchors help:

* validate input
* match exact positions
* avoid partial matches

---

# 5. Quantifiers

## Exact Repetition `{n}`

Example:

```python
import re

print(re.findall(r"\d{4}", "Year 2025"))
```

---

## Minimum Repetition `{n,}`

Example:

```python
import re

print(re.findall(r"\d{2,}", "1 12 123"))
```

---

## Range Repetition `{n,m}`

Example:

```python
import re

print(re.findall(r"\d{2,4}", "1 12 123 12345"))
```

---

## Zero or More `*`

Example:

```python
import re

print(re.findall(r"ab*", "a ab abb abbb"))
```

---

## One or More `+`

Example:

```python
import re

print(re.findall(r"ab+", "a ab abb"))
```

---

## Optional `?`

Example:

```python
import re

print(re.findall(r"colou?r", "color colour"))
```

---

## Greedy Quantifiers

Greedy quantifiers match as much as possible.

Example:

```python
import re

text = "<h1>Title</h1>"

print(re.findall(r"<.*>", text))
```

---

## Non-Greedy (Lazy) Quantifiers

Example:

```python
import re

text = "<h1>Title</h1>"

print(re.findall(r"<.*?>", text))
```

---

# 6. Regex Functions in Python re Module

## `re.match()`

Matches from the beginning.

```python
import re

print(re.match(r"Python", "Python Regex"))
```

---

## `re.search()`

Finds first match anywhere.

```python
import re

print(re.search(r"Regex", "Python Regex"))
```

---

## `re.findall()`

Returns all matches.

```python
import re

print(re.findall(r"\d+", "1 22 333"))
```

---

## `re.finditer()`

Returns iterator of match objects.

```python
import re

for match in re.finditer(r"\d+", "1 22 333"):
    print(match.group())
```

---

## `re.split()`

Splits text using regex.

```python
import re

print(re.split(r",\s*", "apple, banana, mango"))
```

---

## `re.sub()`

Replaces text.

```python
import re

print(re.sub(r"cat", "dog", "cat bat cat"))
```

---

## `re.subn()`

Returns replaced text and count.

```python
import re

print(re.subn(r"cat", "dog", "cat bat cat"))
```

---

## `re.compile()`

Compiles regex pattern.

```python
import re

pattern = re.compile(r"\d+")

print(pattern.findall("123 abc 456"))
```

---

## Understanding Compiled Patterns

Compiled patterns:

* improve performance
* reuse regex efficiently

---

# 7. Grouping, Named Groups, and Backreferences

## Grouping

Groups use parentheses.

```python
(pattern)
```

---

## Capturing Groups `( )`

Example:

```python
import re

match = re.search(r"(\d+)-(\d+)", "123-456")

print(match.group(1))
print(match.group(2))
```

---

## Accessing Group Results

```python
group()
groups()
```

---

## Multiple and Nested Groups

Example:

```python
import re

match = re.search(r"((\d{2})-(\d{2}))", "12-34")

print(match.groups())
```

---

## Numbered Groups

Groups are numbered from left to right.

---

## Using Groups in Substitutions

Example:

```python
import re

text = "John Doe"

print(re.sub(r"(\w+) (\w+)", r"\2 \1", text))
```

---

## Named Groups

### Defining Named Groups `(?P<name>)`

Example:

```python
import re

match = re.search(r"(?P<year>\d{4})", "Year 2025")

print(match.group("year"))
```

---

## Accessing Named Groups

```python
group("name")
```

---

## Named Group Matches

```python
groups()
groupdict()
```

---

## Using Named Groups in Replacements

Example:

```python
import re

text = "2025-05-29"

result = re.sub(
    r"(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})",
    r"\g<d>/\g<m>/\g<y>",
    text
)

print(result)
```

---

## Backreferences

## Referencing Previous Groups `\1`

Example:

```python
import re

text = "hello hello"

print(bool(re.search(r"(\w+)\s+\1", text)))
```

---

## Matching Repeated Patterns

Useful for:

* duplicate words
* repeated sequences
* validation

---

## Using Backreferences in Substitutions

Example:

```python
import re

text = "abc abc"

print(re.sub(r"(\w+)\s+\1", r"\1", text))
```

---

# 8. Lookaround Assertions

## Positive Lookahead `(?=...)`

Example:

```python
import re

text = "100USD"

print(re.findall(r"\d+(?=USD)", text))
```

---

## Negative Lookahead `(?!...)`

Example:

```python
import re

text = "100USD 200INR"

print(re.findall(r"\d+(?!USD)", text))
```

---

## Positive Lookbehind `(?<=...)`

Example:

```python
import re

text = "$100"

print(re.findall(r"(?<=\$)\d+", text))
```

---

## Negative Lookbehind `(?<!...)`

Example:

```python
import re

text = "$100 200"

print(re.findall(r"(?<!\$)\d+", text))
```

---

## Using Lookarounds in Complex Patterns

Lookarounds:

* do not consume characters
* only check conditions

Useful for:

* validation
* filtering
* advanced extraction

---

# 9. Regex Objects and Match Handling

## Regex Objects

Compiled regex creates pattern objects.

```python
import re

pattern = re.compile(r"\d+")
```

---

## Compiled Pattern Objects

Example:

```python
matches = pattern.findall("123 abc 456")
```

---

## Pattern Methods

| Method     | Purpose     |
| ---------- | ----------- |
| match()    | Match start |
| search()   | Find first  |
| findall()  | Find all    |
| finditer() | Iterator    |

---

## Match Object Methods

---

## `group()`

```python
import re

match = re.search(r"\d+", "abc123")

print(match.group())
```

---

## `groups()`

```python
import re

match = re.search(r"(\d+)-(\d+)", "12-34")

print(match.groups())
```

---

## `groupdict()`

```python
import re

match = re.search(r"(?P<x>\d+)", "123")

print(match.groupdict())
```

---

## `start()`

```python
print(match.start())
```

---

## `end()`

```python
print(match.end())
```

---

## `span()`

```python
print(match.span())
```

---

## Extracting Matched Substrings

```python
substring = text[match.start():match.end()]
```

---

## Iterating Matches

## Using `finditer()`

```python
import re

for match in re.finditer(r"\d+", "1 22 333"):
    print(match.group())
```

---

## Looping Through Matches

```python
import re

text = "A1 B22 C333"

for match in re.finditer(r"\d+", text):
    print(match.group(), match.span())
```

---

## Accessing Match Details

```python
match.group()
match.start()
match.end()
match.span()
```

---

# 10. Advanced Usage, Applications, and Best Practices

# Substitution and Splitting

## Replacing Text with `re.sub()`

```python
import re

text = "I love Java"

print(re.sub(r"Java", "Python", text))
```

---

## Limiting Replacements

```python
import re

text = "cat cat cat"

print(re.sub(r"cat", "dog", text, count=2))
```

---

## Using Functions for Replacement

```python
import re

def double(match):
    return str(int(match.group()) * 2)

print(re.sub(r"\d+", double, "10 20 30"))
```

---

## Group References in Replacements

```python
import re

text = "John Doe"

print(re.sub(r"(\w+) (\w+)", r"\2, \1", text))
```

---

## Splitting Strings with Regex

```python
import re

text = "apple,banana;orange mango"

print(re.split(r"[;, ]+", text))
```

---

## Multiple Delimiters

Useful for:

* CSV-like parsing
* cleaning inconsistent separators

---

# Escaping and Handling

## Escaping Regex Characters

```python
import re

print(re.search(r"\.", "file.txt"))
```

---

## Using `re.escape()`

```python
import re

text = "price $100"

escaped = re.escape(text)

print(escaped)
```

---

# Multiline and Complex Matching

## Multiline Mode

```python
import re

text = """Hello
World"""

print(re.findall(r"^World", text, re.MULTILINE))
```

---

## Matching Across Lines

```python
import re

text = "Start\nMiddle\nEnd"

print(re.findall(r"Start.*End", text, re.DOTALL))
```

---

## Handling Large Text Blocks

Tips:

* use compiled regex
* avoid unnecessary backtracking
* prefer specific patterns

---

# Performance

## Optimizing Regex

Good regex:

* specific
* minimal
* efficient

Bad regex:

* overly broad
* deeply nested
* ambiguous

---

## Avoiding Catastrophic Backtracking

Avoid patterns like:

```python
(a+)+
```

Use more precise patterns instead.

---

## Efficient Pattern Design

Prefer:

```python
[a-z]+
```

Instead of:

```python
.*
```

---

## Using Compiled Patterns

```python
import re

pattern = re.compile(r"\d+")

for line in lines:
    pattern.search(line)
```

---

# Practical Applications

## Extracting Emails

```python
import re

text = "admin@example.com"

print(re.findall(r"\S+@\S+", text))
```

---

## Extracting Phone Numbers

```python
import re

text = "9876543210"

print(re.findall(r"\d{10}", text))
```

---

## Validating Passwords

```python
import re

password = "Abc12345"

pattern = r"^(?=.*[A-Z])(?=.*\d).{8,}$"

print(bool(re.match(pattern, password)))
```

---

## Validating Usernames

```python
import re

username = "python_user"

pattern = r"^[a-zA-Z0-9_]{3,20}$"

print(bool(re.match(pattern, username)))
```

---

## Log Parsing

```python
import re

log = "[ERROR] Database failed"

print(re.findall(r"\[(.*?)\]", log))
```

---

## Structured Text Parsing

Useful for:

* CSV
* HTML snippets
* configuration files
* logs

---

## Data Cleaning

Regex helps:

* remove unwanted spaces
* clean symbols
* standardize text

Example:

```python
import re

text = "Hello!!!"

print(re.sub(r"[!]", "", text))
```

---

# Debugging and Best Practices

## Testing Patterns

Always test regex with:

* valid inputs
* invalid inputs
* edge cases

---

## Using Verbose Mode

```python
import re

pattern = re.compile(r"""
\d+     # digits
-       # dash
\d+     # digits
""", re.VERBOSE)
```

---

## Breaking Complex Regex

Instead of writing huge unreadable regex:

* split logic
* use groups
* use comments

---

## Writing Readable Patterns

Prefer readability over compactness.

Good:

```python
r"[A-Za-z0-9_]+"
```

Bad:

```python
r"\w+"
```

(when clarity matters)

---

## Documenting Regex

Always explain:

* what the regex does
* expected input
* assumptions

Example:

```python
# Match 10-digit phone number
pattern = r"\d{10}"
```

---

# Conclusion

Python's `re` module provides powerful tools for:

* searching text
* validating input
* extracting structured data
* transforming strings
* automating text processing

Mastering regex helps developers:

* process large datasets
* clean messy input
* validate user forms
* parse logs and files
* build efficient automation systems

Regex becomes extremely powerful when combined with:

* Python string handling
* file processing
* data cleaning
* web scraping
* automation scripts
