# Regular Expressions Hands-On Tutorial 
## Python Regular Expressions Library: Quantifiers in Regular Expressions (Python)

Quantifiers are one of the most powerful features of regular expressions.

They control how many times a pattern can repeat.

Without quantifiers, regex would only match single characters or fixed patterns.

With quantifiers, regex can match variable-length patterns, which is essential for real-world text processing.

## Examples

* Matching numbers of any length
* Extracting words of unknown size
* Validating password rules
* Parsing HTML-like structures

In Python, quantifiers are used through the `re` module.

---

# 1. What Are Quantifiers?

A quantifier specifies how many times the preceding element can appear.

## Example

```regex
a+
```

Meaning:

```text
one or more 'a'
```

Matches:

```text
a
aa
aaa
aaaa
```

But not:

```text
(empty)
```

---

## Example in Python

```python
import re

text = "aaa aa a"

matches = re.findall(r"a+", text)

print(matches)
```

Output:

```python
['aaa', 'aa', 'a']
```

The regex engine groups continuous repetitions.

---

# 2. Exact Repetition `{n}`

The quantifier `{n}` means:

```text
match exactly n occurrences
```

## Syntax

```regex
pattern{n}
```

---

## Example

Pattern:

```regex
a{3}
```

Meaning:

```text
exactly three 'a'
```

Matches:

```text
aaa
```

But not:

```text
aa
aaaa
```

---

## Python Example

```python
import re

text = "aaa aa aaaa"

matches = re.findall(r"a{3}", text)

print(matches)
```

Output:

```python
['aaa', 'aaa']
```

Because:

* `aaa`
* `aaaa` → first three `a`

---

## Real-World Example: PIN Code

Pattern:

```regex
\d{4}
```

Meaning:

```text
exactly 4 digits
```

Example:

```python
import re

text = "My PIN is 1234"

match = re.search(r"\d{4}", text)

print(match.group())
```

Output:

```text
1234
```

---

# 3. Minimum Repetition `{n,}`

The quantifier `{n,}` means:

```text
match at least n occurrences
```

## Syntax

```regex
pattern{n,}
```

---

## Example

Pattern:

```regex
a{2,}
```

Meaning:

```text
two or more 'a'
```

Matches:

```text
aa
aaa
aaaa
```

---

## Python Example

```python
import re

text = "a aa aaa aaaa"

matches = re.findall(r"a{2,}", text)

print(matches)
```

Output:

```python
['aa', 'aaa', 'aaaa']
```

---

## Real-World Example: Password Length

Minimum 8 characters:

```regex
.{8,}
```

Meaning:

```text
any character repeated at least 8 times
```

---

# 4. Range Repetition `{n,m}`

The quantifier `{n,m}` means:

```text
match between n and m occurrences
```

## Syntax

```regex
pattern{n,m}
```

---

## Example

Pattern:

```regex
a{2,4}
```

Meaning:

```text
between 2 and 4 'a'
```

Matches:

```text
aa
aaa
aaaa
```

But not:

```text
a
aaaaa
```

---

## Python Example

```python
import re

text = "a aa aaa aaaa aaaaa"

matches = re.findall(r"a{2,4}", text)

print(matches)
```

Output:

```python
['aa', 'aaa', 'aaaa', 'aaaa']
```

---

## Real-World Example: Year Format

Pattern:

```regex
\d{2,4}
```

Matches:

```text
21
2024
1999
```

---

# 5. Zero or More `*`

The `*` quantifier means:

```text
match zero or more occurrences
```

Equivalent to:

```regex
{0,}
```

---

## Example

Pattern:

```regex
ab*
```

Meaning:

```text
'a' followed by zero or more 'b'
```

Matches:

```text
a
ab
abb
abbb
```

---

## Python Example

```python
import re

text = "a ab abb abbb"

matches = re.findall(r"ab*", text)

print(matches)
```

Output:

```python
['a', 'ab', 'abb', 'abbb']
```

---

## Example Explanation

```text
b* → zero or more b
```

So:

```text
a
```

is valid.

---

# 6. One or More `+`

The `+` quantifier means:

```text
match one or more occurrences
```

Equivalent to:

```regex
{1,}
```

---

## Example

Pattern:

```regex
ab+
```

Meaning:

```text
'a' followed by at least one 'b'
```

Matches:

```text
ab
abb
abbb
```

But not:

```text
a
```

---

## Python Example

```python
import re

text = "a ab abb abbb"

matches = re.findall(r"ab+", text)

print(matches)
```

Output:

```python
['ab', 'abb', 'abbb']
```

---

## Real-World Example: Extract Numbers

Pattern:

```regex
\d+
```

Meaning:

```text
one or more digits
```

Example:

```python
import re

text = "Order 12345"

number = re.search(r"\d+", text)

print(number.group())
```

Output:

```text
12345
```

---

# 7. Optional `?`

The `?` quantifier means:

```text
zero or one occurrence
```

Equivalent to:

```regex
{0,1}
```

---

## Example

Pattern:

```regex
colou?r
```

Matches:

```text
color
colour
```

---

## Python Example

```python
import re

text = "color colour"

matches = re.findall(r"colou?r", text)

print(matches)
```

Output:

```python
['color', 'colour']
```

---

## Real-World Example: HTTP vs HTTPS

Pattern:

```regex
https?
```

Matches:

```text
http
https
```

---

# 8. Greedy Quantifiers

By default, regex quantifiers are **greedy**.

Greedy means:

```text
match as much text as possible
```

---

## Example

Text:

```html
<tag>hello</tag>
```

Regex:

```regex
<.*>
```

Matches:

```html
<tag>hello</tag>
```

Because:

```regex
.* → any character repeated as much as possible
```

---

## Python Example

```python
import re

text = "<tag>hello</tag>"

match = re.search(r"<.*>", text)

print(match.group())
```

Output:

```html
<tag>hello</tag>
```

---

# 9. Non-Greedy (Lazy) Quantifiers

A lazy quantifier matches as little as possible.

## Syntax

```regex
*?
+?
??
{n,m}?
```

Adding `?` makes it lazy.

---

## Example

Text:

```html
<tag>hello</tag>
```

Regex:

```regex
<.*?>
```

Matches:

```html
<tag>
```

---

## Python Example

```python
import re

text = "<tag>hello</tag>"

match = re.search(r"<.*?>", text)

print(match.group())
```

Output:

```html
<tag>
```

Now regex stops at the first possible match.

---

# 10. Greedy vs Lazy Comparison

Text:

```html
<div>One</div><div>Two</div>
```

---

## Greedy

Regex:

```regex
<.*>
```

Match:

```html
<div>One</div><div>Two</div>
```

---

## Lazy

Regex:

```regex
<.*?>
```

Matches:

```html
<div>
<div>
```

---

## Python Example

```python
import re

text = "<div>One</div><div>Two</div>"

print(re.findall(r"<.*>", text))

print(re.findall(r"<.*?>", text))
```

Output:

```python
['<div>One</div><div>Two</div>']
['<div>', '<div>']
```

---

# 11. Practical Example: Extract Emails

Text:

```text
Contact:
admin@test.com
hello@gmail.com
```

Regex:

```regex
\w+@\w+\.\w+
```

---

## Python Example

```python
import re

text = """
admin@test.com
hello@gmail.com
"""

emails = re.findall(r"\w+@\w+\.\w+", text)

print(emails)
```

Output:

```python
['admin@test.com', 'hello@gmail.com']
```

Quantifiers allow:

```regex
\w+
```

to match usernames of any length.

---

# 12. Quantifier Summary Table

| Quantifier | Meaning           |
| ---------- | ----------------- |
| `{n}`      | exactly n         |
| `{n,}`     | at least n        |
| `{n,m}`    | between n and m   |
| `*`        | zero or more      |
| `+`        | one or more       |
| `?`        | optional          |
| `*?`       | lazy zero or more |
| `+?`       | lazy one or more  |

---

# Conclusion

Quantifiers control how many times a pattern repeats, making regex flexible and powerful.

## Important Quantifiers

* `{n}` → exact repetition
* `{n,}` → minimum repetition
* `{n,m}` → range repetition
* `*` → zero or more
* `+` → one or more
* `?` → optional

Understanding greedy vs lazy quantifiers is also critical for controlling how much text is matched.

Quantifiers enable regex to handle real-world patterns such as:

* numbers of unknown length
* usernames and emails
* HTML-like structures
* variable-length text

They are one of the core mechanisms that make regex extremely powerful.
