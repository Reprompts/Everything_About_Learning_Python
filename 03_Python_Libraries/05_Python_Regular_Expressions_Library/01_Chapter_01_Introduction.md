# Regular Expressions Hands-On Tutorial 
## Python Regular Expressions Library: Introduction to Regular Expressions (Regex)

Regular Expressions (commonly called **regex** or **regexp**) are one of the most powerful tools for text processing, pattern detection, and data extraction. They allow programs to search, analyze, and manipulate text based on patterns instead of exact strings.

Regex is widely used in:

* programming languages
* command-line tools
* databases
* editors
* data processing systems

In Python, regex is implemented through the built-in `re` module.

This article explains regex from the ground up, including its purpose, workflow, and how Python uses it.

---

# 1. What are Regular Expressions (Regex)

A **Regular Expression** is a pattern used to match text.

Instead of searching for exact words, regex allows you to define rules describing what the text should look like.

For example:

| Pattern | Meaning                                | Matches             |
| ------- | -------------------------------------- | ------------------- |
| `cat`   | Exact word                             | `cat`               |
| `c.t`   | `c` followed by any character then `t` | `cat`, `cot`, `cut` |
| `\d`    | Any digit                              | `1`, `5`, `9`       |
| `\w+`   | One or more word characters            | `hello`, `Python`   |

Example text:

```text
Contact us at support@example.com or admin@test.org
```

Regex pattern for email:

```regex
\w+@\w+\.\w+
```

Matches:

```text
support@example.com
admin@test.org
```

Instead of writing complex manual logic, regex allows powerful pattern matching in one expression.

---

# 2. Purpose of Regular Expressions

Regex exists because text data is messy and variable. Programs often need to identify patterns instead of exact values.

The main purposes of regex are:

---

## 1. Pattern Matching

Find occurrences of specific text patterns.

Example:

Find all words starting with `"Py"`.

Pattern:

```regex
Py\w*
```

Matches:

```text
Python
PyTorch
Pyramid
```

---

## 2. Data Extraction

Extract useful information from large text.

Example text:

```text
Order ID: 45821
Customer: John
Total: $98
```

Regex can extract:

* numbers
* names
* emails
* dates
* URLs

Without manually parsing every character.

---

## 3. Validation

Regex is commonly used to validate input data.

| Validation Type | Regex Purpose              |
| --------------- | -------------------------- |
| Email address   | Check email format         |
| Phone number    | Ensure correct digits      |
| Password        | Ensure complexity rules    |
| Date            | Ensure correct date format |

Example email validation:

```regex
^[\w\.-]+@[\w\.-]+\.\w+$
```

---

## 4. Text Manipulation

Regex can replace or modify text patterns.

Example:

Replace multiple spaces with one.

Original text:

```text
Hello     World
```

Regex replace:

```regex
\s+
```

Result:

```text
Hello World
```

---

# 3. Common Use Cases of Regex

Regex is heavily used in real-world programming and data processing systems.

---

## Data Cleaning

Cleaning messy datasets.

Example:

Removing unwanted characters.

```text
Price: $100!!!
```

Regex extracts:

```text
100
```

---

## Log Analysis

Server logs contain structured text.

Example log entry:

```text
192.168.1.10 - - [01/Jan/2025] "GET /index.html"
```

Regex extracts:

* IP address
* date
* request method

---

## Web Scraping

When scraping websites, regex extracts:

* links
* phone numbers
* product prices
* email addresses

---

## Form Validation

Web applications validate user input using regex.

| Input    | Validation           |
| -------- | -------------------- |
| Email    | correct email format |
| Password | minimum characters   |
| Username | allowed characters   |

---

## Search Engines

Search engines use pattern matching internally for query parsing.

---

# 4. Pattern Matching Concept

The core idea of regex is **pattern matching**.

Instead of searching for fixed text:

```text
Hello
```

Regex allows searching for patterns of characters.

Example text:

```text
apple
apply
apt
banana
```

Regex:

```regex
ap\w+
```

Matches:

```text
apple
apply
apt
```

The regex engine reads the pattern and compares it against the text.

---

# 5. Data Extraction with Regex

Regex can extract specific structured information from text.

Example text:

```text
User: Rahul
Email: rahul@gmail.com
Phone: 9876543210
```

Regex patterns:

Email:

```regex
\w+@\w+\.\w+
```

Phone:

```regex
\d{10}
```

Extracted values:

```text
rahul@gmail.com
9876543210
```

This is extremely useful in:

* log processing
* document parsing
* data mining
* web scraping

---

# 6. Validation Using Regex

Validation ensures input follows a specific format.

Example: Password rules

Requirements:

* Minimum 8 characters
* At least 1 digit
* At least 1 uppercase letter

Regex example:

```regex
^(?=.*[A-Z])(?=.*\d).{8,}$
```

This ensures the password meets complexity rules.

---

# 7. Text Manipulation with Regex

Regex can modify text automatically.

Common tasks include:

| Operation | Description                |
| --------- | -------------------------- |
| Search    | find pattern               |
| Replace   | replace pattern            |
| Split     | divide text                |
| Remove    | delete unwanted characters |

Example removing punctuation.

Original:

```text
Hello!!! How are you???
```

Regex:

```regex
[!?.]+
```

Result:

```text
Hello How are you
```

---

# 8. Regex in Python

Python provides regex functionality through the `re` module.

The `re` module implements a regular expression engine that can:

* search text
* extract data
* validate strings
* replace patterns

To use regex in Python:

```python
import re
```

This module contains many functions for pattern matching.

---

# 9. Importing the re Module

The `re` module is part of Python's standard library, so no installation is required.

Example:

```python
import re
```

Now regex functions can be used.

Example:

```python
import re

text = "Python is powerful"

match = re.search("Python", text)

if match:
    print("Match found")
```

Output:

```text
Match found
```

---

# 10. Basic Regex Workflow in Python

The regex workflow usually follows this structure:

```text
Text → Regex Pattern → Regex Engine → Match Result
```

Step-by-step process:

---

## Step 1: Define Text

```text
"Contact me at test@email.com"
```

---

## Step 2: Define Regex Pattern

```regex
\w+@\w+\.\w+
```

---

## Step 3: Apply Regex Function

* `re.search()`
* `re.findall()`
* `re.match()`

---

## Step 4: Process Results

Example:

```python
import re

text = "Contact me at test@email.com"

pattern = r"\w+@\w+\.\w+"

result = re.search(pattern, text)

print(result.group())
```

Output:

```text
test@email.com
```

---

# 11. Regex vs Normal String Operations

Python already provides string methods:

* `find()`
* `replace()`
* `split()`
* `startswith()`
* `endswith()`

But regex is far more powerful.

---

## Example Using String Method

```python
text.find("cat")
```

Only finds exact text.

---

## Example Using Regex

```regex
c.t
```

Matches:

```text
cat
cot
cut
```

---

## Comparison Table

| Feature          | String Methods | Regex     |
| ---------------- | -------------- | --------- |
| Simple search    | Yes            | Yes       |
| Pattern matching | No             | Yes       |
| Validation       | Limited        | Excellent |
| Data extraction  | Difficult      | Easy      |
| Complex matching | Impossible     | Powerful  |

---

# 12. Understanding Pattern Matching

Regex works through a pattern engine.

The engine reads the pattern and tries to match it against the text.

Example:

Text:

```text
My number is 98765
```

Regex:

```regex
\d+
```

Meaning:

```text
\d  -> digit
+   -> one or more
```

Matches:

```text
98765
```

---

## Visual Representation

```text
Text:   My number is 98765
Regex:  \d+

Engine scanning:

M → not digit
y → not digit
space
n → not digit
...
9 → digit → start match
8 → digit
7 → digit
6 → digit
5 → digit

Match result:
98765
```

---

# 13. Why Regex is Powerful

Regex allows very complex pattern detection using small expressions.

Example:

Find dates like:

```text
12/05/2024
```

Regex:

```regex
\d{2}/\d{2}/\d{4}
```

---

Find URLs:

```text
https://example.com
```

Regex:

```regex
https?://\w+\.\w+
```

---

# 14. Limitations of Regex

Although powerful, regex has some drawbacks.

---

## Readability

Complex regex can be hard to understand.

Example:

```regex
^(?=.*[A-Z])(?=.*\d).{8,}$
```

---

## Maintenance

Large regex patterns can become difficult to maintain.

---

## Not Suitable for All Parsing

Regex cannot parse complex nested structures like:

* full HTML
* programming languages

---

# 15. Real-World Example (Putting It All Together)

Example program extracting emails.

```python
import re

text = """
Contact:
admin@example.com
support@test.org
hello@gmail.com
"""

pattern = r"\w+@\w+\.\w+"

emails = re.findall(pattern, text)

print(emails)
```

Output:

```python
['admin@example.com', 'support@test.org', 'hello@gmail.com']
```

Regex quickly extracts all email addresses from the text.

---

# Conclusion

Regular Expressions are an essential tool for text processing and pattern recognition.

They allow programs to:

* search text using patterns
* extract useful data
* validate input
* clean and manipulate text

In Python, the `re` module provides a complete regex engine for performing these tasks efficiently.

Understanding regex is extremely valuable in many areas of programming, including:

* data analysis
* web scraping
* log processing
* automation
* form validation
* natural language processing

Regex turns complex text operations into compact, powerful expressions, making it one of the most important skills in modern programming.
