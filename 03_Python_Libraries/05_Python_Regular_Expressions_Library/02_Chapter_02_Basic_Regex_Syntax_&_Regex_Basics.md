# Regular Expressions Hands-On Tutorial 
## Python Regular Expressions Library: Basic Regex Syntax and Pattern Basics (Python)

Regular Expressions are built from small building blocks called **patterns**.

Understanding these basics is essential before using advanced regex features like:

* groups
* quantifiers
* lookaheads

This article explains the fundamental syntax of regex and how Python interprets patterns using the `re` module, with many practical examples.

---

# 1. Literal Characters

The simplest regex pattern is a **literal character**, which matches the exact same character in text.

## Example

Pattern:

```regex id="fkl2s1"
cat
```

Text:

```text id="s1n3dx"
I have a cat
```

Regex will match:

```text id="7s2pfm"
cat
```

---

## Python Example

```python id="1qklzd"
import re

text = "I have a cat"

pattern = "cat"

match = re.search(pattern, text)

print(match.group())
```

Output:

```text id="n5t0qg"
cat
```

---

## Multiple Matches

```python id="7hvgdr"
import re

text = "cat dog cat tiger"

matches = re.findall("cat", text)

print(matches)
```

Output:

```python id="r19az9"
['cat', 'cat']
```

Literal characters work exactly like normal text search.

---

## Case Sensitivity

Regex is case-sensitive by default.

Pattern:

```regex id="xumxj3"
cat
```

Text:

```text id="5z2c6o"
Cat
cat
```

Matches:

```text id="myx72s"
cat
```

But not:

```text id="s4v5jj"
Cat
```

To ignore case:

```python id="c2mk1f"
re.search("cat", text, re.IGNORECASE)
```

---

# 2. Special Characters (Metacharacters)

Regex becomes powerful because of special characters, called **metacharacters**.

These characters do not represent themselves, but instead represent patterns.

## Common Regex Special Characters

```text id="5qbl5y"
. ^ $ * + ? { } [ ] \ | ( )
```

Each has a special meaning.

---

## Example

Pattern:

```regex id="x0ljlwm"
c.t
```

Meaning:

* `c`
* any character
* `t`

Matches:

```text id="p7qv1e"
cat
cot
cut
c9t
```

---

## Python Example

```python id="7qjgyt"
import re

text = "cat cot cut"

matches = re.findall("c.t", text)

print(matches)
```

Output:

```python id="j0ijsm"
['cat', 'cot', 'cut']
```

---

# 3. Escaping Special Characters

Sometimes you want to match the actual symbol, not its regex meaning.

For example:

```text id="x1a3m4"
.
```

Normally means:

```text id="7gxduq"
any character
```

But if you want to match a real period, you must escape it.

---

## Escape Using Backslash (`\`)

```regex id="p0yqk9"
\.
```

---

## Example

Text:

```text id="vlaj2o"
file.txt
```

Regex:

```regex id="5e0c9v"
\.txt
```

---

## Python Example

```python id="gcfhzv"
import re

text = "file.txt"

match = re.search("\.txt", text)

print(match.group())
```

Output:

```text id="w99rvx"
.txt
```

---

## Common Escaped Characters

| Character | Escaped Version | Meaning               |
| --------- | --------------- | --------------------- |
| `.`       | `\.`            | literal dot           |
| `*`       | `\*`            | literal star          |
| `+`       | `\+`            | literal plus          |
| `?`       | `\?`            | literal question mark |
| `$`       | `\$`            | literal dollar        |
| `^`       | `\^`            | literal caret         |
| `( )`     | `\(` `\)`       | literal parentheses   |

---

# 4. Using Raw Strings for Regex Patterns

Python strings also treat backslash as an escape character.

Examples:

```python id="eftrwq"
\n
\t
\\
```

This creates problems when writing regex.

Example:

```regex id="4d8e6s"
\d
```

Means digit in regex.

But Python interprets `\d` as an escape.

To avoid this, we use raw strings.

---

## Raw String Syntax

```python id="0m2k4r"
r"pattern"
```

Example:

```python id="dqs1ea"
pattern = r"\d"
```

This tells Python:

> Do not interpret backslashes. Send them directly to the regex engine.

---

## Example Without Raw String

```python id="j3xzcv"
pattern = "\\d"
```

Hard to read.

---

## Example With Raw String

```python id="0vj3sl"
pattern = r"\d"
```

Much cleaner.

---

## Best Practice

Always write regex like this:

```python id="v8r0ow"
r"pattern"
```

Example:

```python id="jlwmpt"
import re

text = "My number is 123"

pattern = r"\d+"

print(re.findall(pattern, text))
```

Output:

```python id="sx1l7i"
['123']
```

---

# 5. Whitespace Characters

Whitespace characters represent spaces or spacing characters.

## Common Whitespace Characters

| Character | Meaning         |
| --------- | --------------- |
| `(space)` | normal space    |
| `\t`      | tab             |
| `\n`      | newline         |
| `\r`      | carriage return |

Regex provides a shortcut:

```regex id="h1hjlwm"
\s
```

Meaning:

```text id="ic6v5k"
any whitespace
```

Includes:

* space
* tab
* newline

---

## Example

Text:

```text id="wx2j6h"
Hello    World
```

Regex:

```regex id="twu5h4"
\s+
```

Matches:

```text id="0efrxq"
multiple spaces
```

---

## Python Example

```python id="63lr6t"
import re

text = "Hello    World"

matches = re.findall(r"\s+", text)

print(matches)
```

Output:

```python id="j66otj"
['    ']
```

---

## Removing Extra Spaces

```python id="z98fpi"
import re

text = "Hello    World"

clean = re.sub(r"\s+", " ", text)

print(clean)
```

Output:

```text id="c32t7k"
Hello World
```

---

# 6. Digit Characters

Regex provides a shortcut for digits:

```regex id="4s6j0x"
\d
```

Meaning:

```text id="eckx7k"
any digit
```

Equivalent to:

```regex id="aq1v5x"
[0-9]
```

---

## Example

Text:

```text id="9i91jg"
Order ID: 54821
```

Regex:

```regex id="mjlwm8"
\d
```

Matches:

```text id="kkjlwm"
5 4 8 2 1
```

---

## Extract Full Numbers

Use `+`:

```regex id="gztlq2"
\d+
```

---

## Python Example

```python id="9y0c1z"
import re

text = "Order ID: 54821"

numbers = re.findall(r"\d+", text)

print(numbers)
```

Output:

```python id="0l1lf9"
['54821']
```

---

## Matching Phone Numbers

Example text:

```text id="5bw1ik"
Call me at 9876543210
```

Regex:

```regex id="sp4egq"
\d{10}
```

Python:

```python id="12h1r0"
import re

text = "Call me at 9876543210"

match = re.search(r"\d{10}", text)

print(match.group())
```

Output:

```text id="7jlwm8"
9876543210
```

---

# 7. Word Characters

Regex shortcut:

```regex id="fswjlwm"
\w
```

Matches:

* letters
* digits
* underscore

Equivalent to:

```regex id="m7n8b5"
[a-zA-Z0-9_]
```

---

## Example

Text:

```text id="o9exjlwm"
Python_3
```

Regex:

```regex id="3qk61d"
\w+
```

Matches:

```text id="tjlwm0"
Python_3
```

---

## Python Example

```python id="1tjlwm"
import re

text = "Python_3 is great"

matches = re.findall(r"\w+", text)

print(matches)
```

Output:

```python id="z4m6hc"
['Python_3', 'is', 'great']
```

---

## Extract Words

Example:

```text id="jlwm2a"
Hello world 123
```

Regex:

```regex id="jlwm3b"
\w+
```

Matches:

```text id="jlwm4c"
Hello
world
123
```

---

# 8. Non-Word Characters

Regex also defines the opposite of word characters.

```regex id="jlwm5d"
\W
```

Matches anything NOT a word character.

Meaning:

* not `a-z`
* not `A-Z`
* not `0-9`
* not `_`

---

## Example

Text:

```text id="jlwm6e"
Hello, world!
```

Regex:

```regex id="jlwm7f"
\W
```

Matches:

* `,`
* `!`
* space

---

## Python Example

```python id="jlwm8g"
import re

text = "Hello, world!"

matches = re.findall(r"\W", text)

print(matches)
```

Output:

```python id="jlwm9h"
[',', ' ', '!']
```

---

## Removing Special Characters

Example:

```text id="jlwm0i"
Hello@World#2024!
```

Regex:

```regex id="jlwm1j"
\W
```

Replace with empty:

```python id="jlwm2k"
import re

text = "Hello@World#2024!"

clean = re.sub(r"\W", "", text)

print(clean)
```

Output:

```text id="jlwm3l"
HelloWorld2024
```

---

# 9. Summary of Basic Regex Character Classes

| Regex | Meaning                        | Equivalent              |
| ----- | ------------------------------ | ----------------------- |
| `.`   | any character (except newline) | —                       |
| `\d`  | digit                          | `[0-9]`                 |
| `\D`  | non-digit                      | `[^0-9]`                |
| `\w`  | word character                 | `[a-zA-Z0-9_]`          |
| `\W`  | non-word character             | `[^a-zA-Z0-9_]`         |
| `\s`  | whitespace                     | space, tab, newline     |
| `\S`  | non-whitespace                 | everything except space |

---

# 10. Practical Example (Combining Everything)

Example text:

```text id="jlwm4m"
User: Rahul
Email: rahul123@gmail.com
Age: 25
```

Python program extracting useful data:

```python id="jlwm5n"
import re

text = """
User: Rahul
Email: rahul123@gmail.com
Age: 25
"""

name = re.search(r"\w+", text)

email = re.search(r"\w+@\w+\.\w+", text)

age = re.search(r"\d+", text)

print("Name:", name.group())
print("Email:", email.group())
print("Age:", age.group())
```

Output:

```text id="jlwm6o"
Name: User
Email: rahul123@gmail.com
Age: 25
```

Regex quickly extracts structured information from text.

---

# Conclusion

Basic regex syntax forms the foundation of pattern matching.

Key concepts include:

* Literal characters → exact matches
* Special characters → pattern logic
* Escaping characters → match literal symbols
* Raw strings (`r""`) → avoid escape conflicts in Python
* Whitespace characters (`\s`)
* Digit characters (`\d`)
* Word characters (`\w`)
* Non-word characters (`\W`)

These fundamental building blocks allow regex to identify patterns in text efficiently.

Once these basics are understood, the next important topics are:

* Character classes `[ ]`
* Quantifiers (`* + ? {}`)
* Anchors (`^ $`)
* Groups and capturing
* Lookaheads and lookbehinds

These will allow you to create extremely powerful regex patterns for real-world text processing.
