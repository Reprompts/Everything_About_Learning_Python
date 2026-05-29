# Regular Expressions Hands-On Tutorial 
## Python Regular Expressions Library: Character Classes and Predefined Classes in Regex (Python)

Regular expressions become significantly more powerful when we use **character classes**. Character classes allow us to define a set of characters that can match a single position in text.

Instead of matching only one specific character, a character class can match any character from a defined group.

In Python regex (using the `re` module), character classes are widely used for:

* flexible pattern matching
* data validation
* text parsing
* input filtering

This article explains character classes and predefined classes in detail with practical Python examples.

---

# 1. Character Classes

A character class defines a set of characters that can match one position in a string.

Character classes are written using square brackets:

```regex id="a1b2c3"
[ ]
```

Anything inside the brackets becomes a set of allowed characters.

---

## Basic Example

Pattern:

```regex id="b2c3d4"
[abc]
```

Meaning:

Match either:

* `a`
* `b`
* `c`

---

## Example Text

```text id="c3d4e5"
apple banana cat dog
```

Regex:

```regex id="d4e5f6"
[abc]
```

Matches:

```text id="e5f6g7"
a
b
a
a
c
a
```

---

## Python Example

```python id="f6g7h8"
import re

text = "apple banana cat dog"

matches = re.findall(r"[abc]", text)

print(matches)
```

Output:

```python id="g7h8i9"
['a', 'b', 'a', 'a', 'c', 'a']
```

The regex engine checks each character and matches if it belongs to the defined class.

---

# 2. Square Brackets `[ ]`

Square brackets create a character set.

Inside brackets:

* each character is treated individually
* the regex matches any one character from the set

---

## Example

```regex id="h8i9j0"
[xyz]
```

Matches:

* `x`
* `y`
* `z`

But only one character at a time.

---

## Example

Text:

```text id="i9j0k1"
xyz zebra
```

Regex:

```regex id="j0k1l2"
[zx]
```

Matches:

```text id="k1l2m3"
x
z
z
```

---

## Python Example

```python id="l2m3n4"
import re

text = "xyz zebra"

matches = re.findall(r"[zx]", text)

print(matches)
```

Output:

```python id="m3n4o5"
['x', 'z', 'z']
```

---

# 3. Matching Specific Characters

Character classes allow matching specific characters only.

Example pattern:

```regex id="n4o5p6"
[aeiou]
```

Meaning:

Match any vowel.

---

## Example

Text:

```text id="o5p6q7"
hello world
```

Regex:

```regex id="p6q7r8"
[aeiou]
```

Matches:

```text id="q7r8s9"
e
o
o
```

---

## Python Example

```python id="r8s9t0"
import re

text = "hello world"

matches = re.findall(r"[aeiou]", text)

print(matches)
```

Output:

```python id="s9t0u1"
['e', 'o', 'o']
```

---

# 4. Matching Character Ranges

Instead of listing characters individually, we can define ranges using `-`.

Example:

```regex id="t0u1v2"
[a-z]
```

Meaning:

Any lowercase letter.

---

## Common Ranges

| Range      | Meaning           |
| ---------- | ----------------- |
| `[a-z]`    | lowercase letters |
| `[A-Z]`    | uppercase letters |
| `[0-9]`    | digits            |
| `[a-zA-Z]` | all letters       |

---

## Example

Text:

```text id="u1v2w3"
Python3
```

Regex:

```regex id="v2w3x4"
[a-z]
```

Matches:

```text id="w3x4y5"
y
t
h
o
n
```

---

## Python Example

```python id="x4y5z6"
import re

text = "Python3"

matches = re.findall(r"[a-z]", text)

print(matches)
```

Output:

```python id="y5z6a7"
['y', 't', 'h', 'o', 'n']
```

---

# 5. Matching Digits and Letters

Ranges can also represent numbers.

Pattern:

```regex id="z6a7b8"
[0-9]
```

Meaning:

Any digit.

---

## Example

Text:

```text id="a7b8c9"
Room 405
```

Regex:

```regex id="b8c9d0"
[0-9]
```

Matches:

```text id="c9d0e1"
4
0
5
```

---

## Python Example

```python id="d0e1f2"
import re

text = "Room 405"

matches = re.findall(r"[0-9]", text)

print(matches)
```

Output:

```python id="e1f2g3"
['4', '0', '5']
```

---

## Extract Full Number

Use quantifier:

```regex id="f2g3h4"
[0-9]+
```

Example:

```python id="g3h4i5"
import re

text = "Room 405"

number = re.search(r"[0-9]+", text)

print(number.group())
```

Output:

```text id="h4i5j6"
405
```

---

# 6. Uppercase and Lowercase Matching

Regex treats uppercase and lowercase separately unless using flags.

Example:

```regex id="i5j6k7"
[A-Z]
```

Matches:

* uppercase letters

---

## Example

Text:

```text id="j6k7l8"
Python Programming
```

Regex:

```regex id="k7l8m9"
[A-Z]
```

Matches:

```text id="l8m9n0"
P
P
```

---

## Python Example

```python id="m9n0o1"
import re

text = "Python Programming"

matches = re.findall(r"[A-Z]", text)

print(matches)
```

Output:

```python id="n0o1p2"
['P', 'P']
```

---

## Matching All Letters

Pattern:

```regex id="o1p2q3"
[a-zA-Z]
```

Meaning:

Any alphabet letter.

---

# 7. Negated Character Classes `[^ ]`

A negated character class matches any character **NOT** in the set.

Syntax:

```regex id="p2q3r4"
[^characters]
```

The caret `^` inside brackets means **NOT**.

---

## Example

Pattern:

```regex id="q3r4s5"
[^abc]
```

Meaning:

Any character except:

* `a`
* `b`
* `c`

---

## Example Text

```text id="r4s5t6"
abcde
```

Regex:

```regex id="s5t6u7"
[^abc]
```

Matches:

```text id="t6u7v8"
d
e
```

---

## Python Example

```python id="u7v8w9"
import re

text = "abcde"

matches = re.findall(r"[^abc]", text)

print(matches)
```

Output:

```python id="v8w9x0"
['d', 'e']
```

---

## Example: Removing Letters

Text:

```text id="w9x0y1"
A1B2C3
```

Regex:

```regex id="x0y1z2"
[^0-9]
```

Matches:

```text id="y1z2a3"
A
B
C
```

---

## Python Example

```python id="z2a3b4"
import re

text = "A1B2C3"

matches = re.findall(r"[^0-9]", text)

print(matches)
```

Output:

```python id="a3b4c5"
['A', 'B', 'C']
```

---

# 8. Combining Characters in Classes

Character classes can combine multiple patterns.

Example:

```regex id="b4c5d6"
[a-zA-Z0-9]
```

Meaning:

Any letter or digit.

---

## Example

Text:

```text id="c5d6e7"
User_123!
```

Regex:

```regex id="d6e7f8"
[a-zA-Z0-9]
```

Matches:

```text id="e7f8g9"
U s e r 1 2 3
```

---

## Python Example

```python id="f8g9h0"
import re

text = "User_123!"

matches = re.findall(r"[a-zA-Z0-9]", text)

print(matches)
```

Output:

```python id="g9h0i1"
['U', 's', 'e', 'r', '1', '2', '3']
```

---

# 9. Predefined Character Classes

Regex provides built-in shortcuts for common character sets.

These are called **predefined classes**.

---

# 10. `\d` → Digits

Pattern:

```regex id="h0i1j2"
\d
```

Matches:

Any digit (`0–9`)

Equivalent to:

```regex id="i1j2k3"
[0-9]
```

---

## Example

Text:

```text id="j2k3l4"
Order 4567
```

Regex:

```regex id="k3l4m5"
\d
```

Matches:

```text id="l4m5n6"
4
5
6
7
```

---

## Python Example

```python id="m5n6o7"
import re

text = "Order 4567"

matches = re.findall(r"\d", text)

print(matches)
```

Output:

```python id="n6o7p8"
['4', '5', '6', '7']
```

---

# 11. `\D` → Non-Digits

Pattern:

```regex id="o7p8q9"
\D
```

Matches anything except digits.

Equivalent:

```regex id="p8q9r0"
[^0-9]
```

---

## Example

```python id="q9r0s1"
import re

text = "Room 405"

matches = re.findall(r"\D", text)

print(matches)
```

Output:

```python id="r0s1t2"
['R', 'o', 'o', 'm', ' ']
```

---

# 12. `\w` → Word Characters

Pattern:

```regex id="s1t2u3"
\w
```

Matches:

* letters
* digits
* underscore

Equivalent:

```regex id="t2u3v4"
[a-zA-Z0-9_]
```

---

## Example

```python id="u3v4w5"
import re

text = "user_123"

matches = re.findall(r"\w", text)

print(matches)
```

Output:

```python id="v4w5x6"
['u', 's', 'e', 'r', '_', '1', '2', '3']
```

---

# 13. `\W` → Non-Word Characters

Pattern:

```regex id="w5x6y7"
\W
```

Matches anything not a word character.

Equivalent:

```regex id="x6y7z8"
[^a-zA-Z0-9_]
```

---

## Example

```python id="y7z8a9"
import re

text = "hello@world!"

matches = re.findall(r"\W", text)

print(matches)
```

Output:

```python id="z8a9b0"
['@', '!']
```

---

# 14. `\s` → Whitespace

Matches:

* space
* tab
* newline

---

## Example

```python id="a9b0c1"
import re

text = "Hello   World"

matches = re.findall(r"\s", text)

print(matches)
```

Output:

```python id="b0c1d2"
[' ', ' ', ' ']
```

---

# 15. `\S` → Non-Whitespace

Matches any character except whitespace.

---

## Example

```python id="c1d2e3"
import re

text = "Hello World"

matches = re.findall(r"\S", text)

print(matches)
```

Output:

```python id="d2e3f4"
['H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd']
```

---

# 16. Practical Example (Extracting Data)

Text:

```text id="e3f4g5"
Name: Rahul
Email: rahul123@gmail.com
Age: 25
```

---

## Python Program

```python id="f4g5h6"
import re

text = """
Name: Rahul
Email: rahul123@gmail.com
Age: 25
"""

name = re.search(r"[A-Z][a-z]+", text)

email = re.search(r"\w+@\w+\.\w+", text)

age = re.search(r"\d+", text)

print(name.group())
print(email.group())
print(age.group())
```

Output:

```text id="g5h6i7"
Rahul
rahul123@gmail.com
25
```

Character classes make pattern extraction much easier.

---

# 17. Summary Table

| Pattern  | Meaning                |
| -------- | ---------------------- |
| `[abc]`  | match `a`, `b`, or `c` |
| `[a-z]`  | lowercase letters      |
| `[A-Z]`  | uppercase letters      |
| `[0-9]`  | digits                 |
| `[^abc]` | not `a`, `b`, or `c`   |
| `\d`     | digit                  |
| `\D`     | non-digit              |
| `\w`     | word character         |
| `\W`     | non-word character     |
| `\s`     | whitespace             |
| `\S`     | non-whitespace         |

---

# Conclusion

Character classes are one of the most important building blocks of regex.

They allow flexible pattern matching by defining sets of characters.

## Key Ideas

* Square brackets `[ ]` create character sets
* Ranges (`a-z`) match groups of characters
* Negation (`[^ ]`) excludes characters
* Predefined classes (`\d`, `\w`, `\s`) simplify common patterns

Using character classes effectively allows you to build powerful regex patterns for:

* searching
* extracting
* validating
* transforming text
