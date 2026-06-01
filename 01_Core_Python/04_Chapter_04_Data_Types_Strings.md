# Python Programming: Chapter 4 Python Data Types: Strings

## 1. Introduction to Strings in Python

A string in Python is a sequence of characters used to represent textual data. Strings are one of the most frequently used data types in programming because they allow programs to store, process, and manipulate text.

In Python, strings are:

- Immutable sequences of Unicode characters
- Ordered collections of characters
- Indexed and sliceable
- Rich in built-in operations and methods

### Examples of Strings

```python
"Hello"
"Python Programming"
"12345"
"Data Science"
```

Internally, Python treats strings as objects of the `str` class.

```python
name = "Python"
```

Here:

- `"Python"` is a string object
- `name` is a reference to that object

---

# 2. String Creation

Strings can be created in several ways in Python.

## 2.1 Using Single Quotes

```python
text = 'Hello'
```

Single quotes are useful when the string contains double quotes.

```python
sentence = 'She said "Hello"'
```

## 2.2 Using Double Quotes

```python
text = "Hello"
```

Double quotes are useful when the string contains single quotes.

```python
sentence = "It's a beautiful day"
```

## 2.3 Using Triple Quotes

Triple quotes allow multi-line strings.

```python
paragraph = """Python is a programming language
that supports multiple paradigms
and is widely used in many domains."""
```

Triple quotes are also commonly used for documentation strings (docstrings).

## 2.4 Using the `str()` Constructor

Strings can also be created using the `str()` function.

```python
num = str(100)
```

This converts the integer into a string.

---

# 3. String Immutability

Strings in Python are immutable, meaning their contents cannot be changed after creation.

```python
text = "hello"
text[0] = "H"
```

This produces an error because strings cannot be modified.

Instead, a new string must be created.

```python
text = "hello"
text = "H" + text[1:]
```

This creates a new string object.

## 3.1 Why Immutability Matters

Immutability provides several advantages:

- Memory optimization
- Thread safety
- Predictable behavior
- Hashability for dictionary keys

---

# 4. String Indexing

Strings are ordered sequences, so each character has a position called an index.

```python
word = "Python"
```

| Character | P | y | t | h | o | n |
|------------|---|---|---|---|---|---|
| Index | 0 | 1 | 2 | 3 | 4 | 5 |

## 4.1 Accessing Characters

```python
word[0]
```

**Output**

```python
'P'
```

## 4.2 Negative Indexing

Python also supports negative indexing.

| Character | P | y | t | h | o | n |
|------------|---|---|---|---|---|---|
| Index | -6 | -5 | -4 | -3 | -2 | -1 |

```python
word[-1]
```

**Output**

```python
'n'
```

---

# 5. String Slicing

Slicing allows extracting a portion of a string.

## Syntax

```python
string[start:stop:step]
```

### Example

```python
text = "Python"
print(text[0:3])
```

**Output**

```python
Pyt
```

## 5.1 Omitting Parameters

```python
text[:3]     # start from beginning
text[3:]     # go to end
text[:]      # entire string
```

## 5.2 Using Step

```python
text = "Python"
print(text[::2])
```

**Output**

```python
Pto
```

## 5.3 Reversing a String

```python
text = "Python"
print(text[::-1])
```

**Output**

```python
nohtyP
```

---

# 6. String Concatenation

Concatenation means joining two or more strings.

```python
first = "Hello"
second = "World"

result = first + " " + second
```

**Output**

```python
Hello World
```

## 6.1 Concatenation Using `join()`

```python
words = ["Python", "is", "powerful"]

sentence = " ".join(words)
```

**Output**

```python
Python is powerful
```

`join()` is more efficient than repeated `+` operations.

---

# 7. String Repetition

Strings can be repeated using the `*` operator.

```python
text = "Hi "
print(text * 3)
```

**Output**

```python
Hi Hi Hi
```

---

# 8. String Formatting

String formatting inserts values into strings.

## 8.1 Using `%` Formatting (Older Style)

```python
name = "Alice"
age = 25

print("Name: %s Age: %d" % (name, age))
```

## 8.2 Using `format()` Method

```python
name = "Alice"
age = 25

print("Name: {} Age: {}".format(name, age))
```

## 8.3 Positional Formatting

```python
print("{0} is {1} years old".format("Alice", 25))
```

## 8.4 Keyword Formatting

```python
print("{name} is {age} years old".format(name="Alice", age=25))
```

---

# 9. f-Strings (Formatted String Literals)

Introduced in Python 3.6, f-strings are the most modern and efficient formatting method.

```python
name = "Alice"
age = 25

print(f"{name} is {age} years old")
```

## 9.1 Expression Evaluation

```python
x = 10
y = 20

print(f"Sum = {x + y}")
```

**Output**

```python
Sum = 30
```

---

# 10. String Methods

## 10.1 Changing Case

### `upper()`

```python
text = "python"
print(text.upper())
```

**Output**

```python
PYTHON
```

### `lower()`

```python
text = "PYTHON"
print(text.lower())
```

**Output**

```python
python
```

### `title()`

```python
text = "python programming language"
print(text.title())
```

**Output**

```python
Python Programming Language
```

### `capitalize()`

```python
text = "python programming"
print(text.capitalize())
```

**Output**

```python
Python programming
```

### `swapcase()`

```python
text = "Python"
print(text.swapcase())
```

**Output**

```python
pYTHON
```

### `casefold()`

```python
text = "PYTHON"
print(text.casefold())
```

**Output**

```python
python
```

---

## 10.2 Removing Whitespace

### `strip()`

```python
text = "  Python  "
print(text.strip())
```

**Output**

```python
Python
```

### `lstrip()`

```python
text = "   Python"
print(text.lstrip())
```

### `rstrip()`

```python
text = "Python   "
print(text.rstrip())
```

---

## 10.3 Searching Methods

### `find()`

```python
text = "Python programming"
print(text.find("pro"))
```

**Output**

```python
7
```

### `rfind()`

```python
text = "apple apple"
print(text.rfind("apple"))
```

**Output**

```python
6
```

### `index()`

```python
text = "Python"
print(text.index("th"))
```

**Output**

```python
2
```

### `rindex()`

```python
text = "apple apple"
print(text.rindex("apple"))
```

**Output**

```python
6
```

### `count()`

```python
text = "apple apple apple"
print(text.count("apple"))
```

**Output**

```python
3
```

### `startswith()`

```python
text = "Python Programming"
print(text.startswith("Python"))
```

**Output**

```python
True
```

### `endswith()`

```python
text = "Python Programming"
print(text.endswith("Programming"))
```

**Output**

```python
True
```

---

## 10.4 Replacing

### `replace()`

```python
text = "I like Python"
print(text.replace("Python", "Java"))
```

**Output**

```python
I like Java
```

---

## 10.5 Splitting Strings

### `split()`

```python
sentence = "Python is powerful"
words = sentence.split()

print(words)
```

**Output**

```python
['Python', 'is', 'powerful']
```

### `rsplit()`

```python
text = "a,b,c,d"
print(text.rsplit(",", 1))
```

**Output**

```python
['a,b,c', 'd']
```

### `splitlines()`

```python
text = "Hello\nWorld"
print(text.splitlines())
```

**Output**

```python
['Hello', 'World']
```

---

## 10.6 Joining Strings

### `join()`

```python
words = ["Python", "is", "great"]

sentence = " ".join(words)
print(sentence)
```

**Output**

```python
Python is great
```

---

## 10.7 Checking String Content

| Method | Description |
|----------|-------------|
| `isalpha()` | Alphabetic only |
| `isalnum()` | Letters and digits |
| `isdigit()` | Digits only |
| `isnumeric()` | Numeric characters |
| `islower()` | Lowercase |
| `isupper()` | Uppercase |
| `istitle()` | Title case |
| `isspace()` | Whitespace only |

Example:

```python
"Python".isalpha()
"Python123".isalnum()
"12345".isdigit()
```

---

## 10.8 Alignment Methods

```python
text.center(20)
text.ljust(20)
text.rjust(20)
```

---

## 10.9 Padding

### `zfill()`

```python
num = "42"
print(num.zfill(5))
```

**Output**

```python
00042
```

---

## 10.10 Partition Methods

### `partition()`

```python
text = "Python-is-fun"
print(text.partition("-"))
```

**Output**

```python
('Python', '-', 'is-fun')
```

### `rpartition()`

```python
text = "Python-is-fun"
print(text.rpartition("-"))
```

**Output**

```python
('Python-is', '-', 'fun')
```

---

## 10.11 Formatting Methods

### `format()`

```python
name = "Alice"
print("Hello {}".format(name))
```

### `format_map()`

```python
data = {'name': 'Alice'}

print("Hello {name}".format_map(data))
```

---

## 10.12 Encoding

### `encode()`

```python
text = "Python"
print(text.encode())
```

**Output**

```python
b'Python'
```

---

## 10.13 Tab Expansion

### `expandtabs()`

```python
text = "Hello\tWorld"
print(text.expandtabs(4))
```

---

## 10.14 Translation

### `maketrans()` and `translate()`

```python
table = str.maketrans("ae", "12")

text = "apple"
print(text.translate(table))
```

**Output**

```python
1ppl2
```

---

## Complete List of Python String Methods

```text
capitalize()
casefold()
center()
count()
encode()
endswith()
expandtabs()
find()
format()
format_map()
index()
isalnum()
isalpha()
isascii()
isdigit()
isidentifier()
islower()
isnumeric()
isprintable()
isspace()
istitle()
isupper()
join()
ljust()
lower()
lstrip()
maketrans()
partition()
replace()
rfind()
rindex()
rjust()
rpartition()
rsplit()
rstrip()
split()
splitlines()
startswith()
strip()
swapcase()
title()
translate()
upper()
zfill()
```

---

# 11. Unicode Strings

Python strings are Unicode by default.

Unicode allows representation of characters from virtually all languages.

```python
text = "こんにちは"
```

## 11.1 Unicode Example

```python
print(ord("A"))
print(chr(65))
```

**Output**

```python
65
A
```

---

# 12. Raw Strings

Raw strings treat backslashes literally.

```python
path = r"C:\Users\Name\Documents"
```

Without raw strings, backslashes act as escape characters.

## 12.1 Escape Sequences

| Escape Sequence | Meaning |
|----------------|----------|
| `\n` | Newline |
| `\t` | Tab |
| `\\` | Backslash |

Raw strings prevent interpretation of these escapes.

---

# 13. Byte Strings

Byte strings represent raw binary data.

```python
data = b"Hello"
```

Unlike regular strings:

- Byte strings store bytes
- Used for file I/O and network communication

## 13.1 Converting Between Strings and Bytes

### Encoding

```python
text = "Hello"
data = text.encode("utf-8")
```

### Decoding

```python
data.decode("utf-8")
```

---

# 14. Summary

Python strings provide a powerful and flexible system for text processing.

### Key Characteristics

- Immutable sequences of characters
- Unicode support by default
- Extensive built-in methods
- Efficient formatting techniques
- Support for raw strings and byte strings

### Common Applications

- File processing
- Web development
- Data analysis
- Natural Language Processing (NLP)

A deep understanding of string behavior is essential for writing efficient and reliable Python programs.
