# Regular Expressions Hands-On Tutorial
## Python Regular Expressions Library: Lookaround Assertions in Regular Expressions (Python)

Lookaround assertions are advanced regex features that allow you to check conditions before or after a match **without including them in the match result**.

In simple terms:

> Lookarounds look ahead or behind in the text to verify a condition, but they do not consume characters.

This makes them extremely useful for:

- Complex validation rules
- Conditional matching
- Extracting data with surrounding constraints
- Preventing unwanted matches

Python's `re` module supports four types of lookarounds.

| Type | Syntax | Direction | Meaning |
|--------|--------|--------|--------|
| Positive Lookahead | `(?=...)` | Forward | Match if pattern exists ahead |
| Negative Lookahead | `(?!...)` | Forward | Match if pattern does **NOT** exist ahead |
| Positive Lookbehind | `(?<=...)` | Backward | Match if pattern exists behind |
| Negative Lookbehind | `(?<!...)` | Backward | Match if pattern does **NOT** exist behind |

---

# 1. Understanding the Idea of Lookarounds

Normal regex consumes characters.

Example:

```text
cat
```

Matching text:

```text
catfish
```

Match result:

```text
cat
```

But sometimes we want to check what comes next without including it in the match.

Example goal:

> Match `"cat"` only if followed by `"fish"`.

Solution:

```regex
cat(?=fish)
```

Here:

```regex
(?=fish)
```

checks the text ahead without consuming it.

---

# 2. Positive Lookahead `(?=...)`

A positive lookahead ensures that a pattern exists ahead of the current position.

## Syntax

```regex
pattern(?=condition)
```

## Meaning

Match `pattern` only if `condition` appears next.

### Example 1 — Match Word Before `@` in Email

#### Text

```text
user@gmail.com
```

#### Regex

```regex
\w+(?=@)
```

#### Explanation

```text
\w+   → word characters
(?=@) → must be followed by "@"
```

### Python Example

```python
import re

text = "user@gmail.com"

match = re.search(r"\w+(?=@)", text)

print(match.group())
```

### Output

```text
user
```

The `@` is not included in the match.

---

### Example 2 — Extract Numbers Before "kg"

#### Text

```text
10kg 25kg 50kg
```

#### Regex

```regex
\d+(?=kg)
```

### Python Example

```python
import re

text = "10kg 25kg 50kg"

matches = re.findall(r"\d+(?=kg)", text)

print(matches)
```

### Output

```text
['10', '25', '50']
```

---

# 3. Negative Lookahead `(?!...)`

A negative lookahead ensures that a pattern does **NOT** appear ahead.

## Syntax

```regex
pattern(?!condition)
```

## Meaning

Match `pattern` only if `condition` does **NOT** follow.

### Example — Match Numbers NOT Followed by "kg"

#### Text

```text
10kg 25 30kg 40
```

#### Regex

```regex
\d+(?!kg)
```

### Python Example

```python
import re

text = "10kg 25 30kg 40"

matches = re.findall(r"\d+(?!kg)", text)

print(matches)
```

### Output

```text
['25', '40']
```

Numbers followed by `"kg"` are excluded.

---

# 4. Positive Lookbehind `(?<=...)`

A positive lookbehind checks whether a specific pattern appears before the match.

## Syntax

```regex
(?<=condition)pattern
```

## Meaning

Match `pattern` only if `condition` exists before it.

### Example — Extract Price After `$`

#### Text

```text
$100 $250 $500
```

#### Regex

```regex
(?<=\$)\d+
```

#### Explanation

```text
(?<=\$) → must be preceded by $
\d+     → digits
```

### Python Example

```python
import re

text = "$100 $250 $500"

matches = re.findall(r"(?<=\$)\d+", text)

print(matches)
```

### Output

```text
['100', '250', '500']
```

The `$` is not included in the result.

---

# 5. Negative Lookbehind `(?<!...)`

A negative lookbehind ensures that a pattern does **NOT** appear before it.

## Syntax

```regex
(?<!condition)pattern
```

## Meaning

Match `pattern` only if `condition` does **NOT** appear before it.

### Example — Match Numbers NOT Preceded by `$`

#### Text

```text
$100 200 $300 400
```

#### Regex

```regex
(?<!\$)\d+
```

### Python Example

```python
import re

text = "$100 200 $300 400"

matches = re.findall(r"(?<!\$)\d+", text)

print(matches)
```

### Output

```text
['200', '400']
```

Numbers preceded by `$` are ignored.

---

# 6. Visual Explanation of Lookarounds

### Example Text

```text
price:$100
```

### Regex

```regex
(?<=\$)\d+
```

### Matching Process

```text
price:$100
       ↑
Look behind for "$"
If yes → match digits
```

### Match Result

```text
100
```

---

# 7. Using Lookarounds in Complex Patterns

Lookarounds become extremely powerful when combined with:

- Quantifiers
- Anchors
- Groups
- Character classes

---

## Example 1 — Password Validation

### Rules

Password must contain:

- At least one digit
- One uppercase letter
- One lowercase letter
- Minimum length of 8 characters

### Regex

```regex
^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$
```

### Explanation

| Part | Meaning |
|--------|--------|
| `^` | Start |
| `(?=.*[A-Z])` | Must contain uppercase |
| `(?=.*[a-z])` | Must contain lowercase |
| `(?=.*\d)` | Must contain digit |
| `.{8,}` | Minimum length 8 |
| `$` | End |

### Python Example

```python
import re

password = "Pass1234"

pattern = r"^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$"

print(bool(re.match(pattern, password)))
```

### Output

```text
True
```

Lookaheads enforce multiple conditions without consuming characters.

---

## Example 2 — Extract Filenames Without Extension

### Text

```text
file1.txt
file2.jpg
file3.txt
```

### Regex

```regex
\w+(?=\.txt)
```

### Python Example

```python
import re

text = "file1.txt file2.jpg file3.txt"

matches = re.findall(r"\w+(?=\.txt)", text)

print(matches)
```

### Output

```text
['file1', 'file3']
```

---

## Example 3 — Match Words Not Followed by Digits

### Text

```text
test1 hello world2 python
```

### Regex

```regex
\w+(?!\d)
```

### Python Example

```python
import re

text = "test1 hello world2 python"

matches = re.findall(r"\w+(?!\d)", text)

print(matches)
```

### Output

```text
['hello', 'python']
```

---

# 8. Combining Multiple Lookarounds

Multiple lookarounds can be used together.

### Regex

```regex
(?<=\$)\d+(?=USD)
```

### Meaning

- Digits
- Preceded by `$`
- Followed by `USD`

### Text

```text
$100USD $200EUR $300USD
```

### Python Example

```python
import re

text = "$100USD $200EUR $300USD"

matches = re.findall(r"(?<=\$)\d+(?=USD)", text)

print(matches)
```

### Output

```text
['100', '300']
```

---

# 9. Important Rules About Lookbehinds in Python

Python requires **fixed-width lookbehinds**.

## Allowed

```regex
(?<=\$)

(?<=USD)

(?<=\d{3})
```

## Not Allowed

```regex
(?<=\d+)
```

Reason:

```text
Variable length is not permitted in Python lookbehinds.
```

---

# 10. Practical Real-World Uses

## Data Extraction

Extract numbers after currency symbols.

```regex
(?<=\$)\d+
```

---

## Filtering Patterns

Match `.py` files only.

```regex
\w+(?=\.py)
```

---

## Preventing Invalid Matches

Exclude comments in code parsing.

```regex
code(?!#)
```

---

## Password Validation

Use multiple lookaheads to enforce rules.

```regex
^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$
```

---

# 11. Summary Table

| Lookaround | Syntax | Direction | Purpose |
|------------|---------|-----------|---------|
| Positive Lookahead | `(?=...)` | Forward | Must be followed by |
| Negative Lookahead | `(?!...)` | Forward | Must NOT be followed by |
| Positive Lookbehind | `(?<=...)` | Backward | Must be preceded by |
| Negative Lookbehind | `(?<!...)` | Backward | Must NOT be preceded by |

---

# 12. Key Takeaways

Lookaround assertions allow regex to:

- Enforce conditions without consuming text
- Match patterns based on context
- Perform advanced validation rules
- Extract data surrounded by specific patterns

They are one of the most powerful features in regex.

## Quick Reference

| Assertion | Meaning |
|------------|----------|
| `(?=...)` | Positive lookahead |
| `(?!...)` | Negative lookahead |
| `(?<=...)` | Positive lookbehind |
| `(?<!...)` | Negative lookbehind |

Lookarounds make it possible to build sophisticated regex patterns that are precise, efficient, and context-aware, making them invaluable for real-world text processing and validation tasks.
