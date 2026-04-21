# Pandas Hands-On Tutorial

## Chapter 15: String Operations 

In real-world datasets, text data is extremely common. Examples include:

- customer names  
- product descriptions  
- email addresses  
- phone numbers  
- transaction notes  
- addresses  

However, text data is often messy, inconsistent, and unstructured. To analyze it effectively, we must perform string manipulation and text processing.

Pandas provides powerful vectorized string operations that allow performing text operations on entire columns efficiently. These operations are accessed using the `.str` accessor.

---

## This Article Explains

- Using the string accessor  
- String methods in Pandas  
- Pattern matching  
- Using regular expressions  
- Splitting strings  
- Replacing patterns in text  
- Extracting data from strings  

---

## Understanding String Operations in Pandas

Pandas allows applying string methods to entire columns.

### Example dataset

```python
import pandas as pd

data = {
    "Name": ["Alice Smith", "Bob Johnson", "Charlie Brown"],
    "Email": ["alice@gmail.com", "bob@yahoo.com", "charlie@outlook.com"]
}

df = pd.DataFrame(data)
````

### Dataset

| Name          | Email                                             |
| ------------- | ------------------------------------------------- |
| Alice Smith   | [alice@gmail.com](mailto:alice@gmail.com)         |
| Bob Johnson   | [bob@yahoo.com](mailto:bob@yahoo.com)             |
| Charlie Brown | [charlie@outlook.com](mailto:charlie@outlook.com) |

---

## Using the String Accessor

The `.str` accessor allows applying string operations to Series objects.

```python
df["Name"].str.lower()
```

### Output

```
alice smith
bob johnson
charlie brown
```

`.str` ensures operations are vectorized and efficient.

---

## String Methods in Pandas

| Method            | Purpose                   |
| ----------------- | ------------------------- |
| `.str.lower()`    | Convert text to lowercase |
| `.str.upper()`    | Convert text to uppercase |
| `.str.title()`    | Convert to title case     |
| `.str.len()`      | Compute string length     |
| `.str.strip()`    | Remove whitespace         |
| `.str.contains()` | Search for pattern        |
| `.str.replace()`  | Replace text              |
| `.str.split()`    | Split strings             |
| `.str.extract()`  | Extract patterns          |

---

## Converting Text Case

### Lowercase

```python
df["Name"].str.lower()
```

### Output

```
alice smith
bob johnson
charlie brown
```

---

### Uppercase

```python
df["Name"].str.upper()
```

---

### Title case

```python
df["Name"].str.title()
```

---

## Removing Whitespace

Real datasets often contain extra spaces.

```python
df["Name"].str.strip()
```

### Variants:

* `str.lstrip()` → remove left spaces
* `str.rstrip()` → remove right spaces

---

## Calculating String Length

```python
df["Name"].str.len()
```

### Output

* Alice Smith → 11
* Bob Johnson → 11
* Charlie Brown → 13

Useful for validation.

---

## Pattern Matching

Use `.str.contains()` to find patterns.

### Example dataset

| Email                                         |
| --------------------------------------------- |
| [alice@gmail.com](mailto:alice@gmail.com)     |
| [bob@yahoo.com](mailto:bob@yahoo.com)         |
| [charlie@gmail.com](mailto:charlie@gmail.com) |

### Find Gmail users

```python
df["Email"].str.contains("gmail")
```

### Filter rows

```python
df[df["Email"].str.contains("gmail")]
```

---

## Using Regular Expressions

Regex enables advanced pattern matching.

### Example

```python
df["Email"].str.contains(r"@gmail")
```

### Use cases:

* phone numbers
* email validation
* postal codes
* product codes

---

## Splitting Strings

### Example

```python
df["Name"].str.split(" ")
```

### Output

```
["Alice", "Smith"]
["Bob", "Johnson"]
```

---

## Splitting into Columns

```python
df[["FirstName", "LastName"]] = df["Name"].str.split(" ", expand=True)
```

### Result

| FirstName | LastName |
| --------- | -------- |
| Alice     | Smith    |
| Bob       | Johnson  |
| Charlie   | Brown    |

---

## Replacing Patterns in Text

```python
df["Email"].str.replace("gmail.com", "company.com")
```

---

### Regex replacement

Remove numbers:

```python
df["Product"].str.replace(r"\d+", "", regex=True)
```

---

## Extracting Data from Strings

### Example dataset

| Email                                             |
| ------------------------------------------------- |
| [alice@gmail.com](mailto:alice@gmail.com)         |
| [bob@yahoo.com](mailto:bob@yahoo.com)             |
| [charlie@outlook.com](mailto:charlie@outlook.com) |

---

### Extract domain

```python
df["Domain"] = df["Email"].str.extract(r"@(\w+)")
```

### Result

| Email                                             | Domain  |
| ------------------------------------------------- | ------- |
| [alice@gmail.com](mailto:alice@gmail.com)         | gmail   |
| [bob@yahoo.com](mailto:bob@yahoo.com)             | yahoo   |
| [charlie@outlook.com](mailto:charlie@outlook.com) | outlook |

---

### Regex explanation

```
@(\w+)
```

* `@` → matches symbol
* `\w+` → word characters
* `()` → captures value

---

## Extracting Multiple Values

### Example

```python
df[["Prefix", "Number"]] = df["ProductCode"].str.extract(r"([A-Z]+)-(\d+)")
```

### Result

| Prefix | Number |
| ------ | ------ |
| PRD    | 1001   |
| PRD    | 1002   |
| PRD    | 1003   |

---

## Chaining String Operations

```python
df["Name"].str.strip().str.lower().str.replace(" ", "_")
```

### Output

```
alice_smith
```

---

## Real-World Example: Cleaning Customer Data

```python
df["Customer"] = (
    df["Customer"]
    .str.strip()
    .str.title()
)
```

### Result

* Alice Smith
* Bob Johnson
* Charlie Brown

---

## Real-World Example: Extracting Phone Numbers

### Dataset

| Contact               |
| --------------------- |
| Call me at 9876543210 |
| Phone: 9123456789     |

### Extract numbers

```python
df["Phone"] = df["Contact"].str.extract(r"(\d{10})")
```

### Regex

```
\d{10}
```

Matches 10-digit numbers.

---

## Performance Considerations

String operations in Pandas are vectorized and efficient.

### Best practices:

* use `.str` methods instead of loops
* chain operations when possible
* use regex only when needed

Example:

* Dataset size: 5 million rows
* Vectorized operations remain efficient

---

## Summary

String operations are essential for cleaning and processing textual data.

### Key capabilities:

---

### String manipulation

* `.str.lower()`
* `.str.upper()`
* `.str.strip()`
* `.str.len()`

---

### Pattern matching

* `.str.contains()`

---

### Regular expressions

* regex-based searching and extraction

---

### Splitting strings

* `.str.split()`

---

### Replacing text

* `.str.replace()`

---

### Extracting patterns

* `.str.extract()`

---

## Final Note

These techniques are widely used in:

* data cleaning pipelines
* natural language preprocessing
* customer data management
* log file analysis
* email and contact processing

Mastering string operations allows analysts to transform messy text data into structured, analyzable information.


