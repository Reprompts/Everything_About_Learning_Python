# Regular Expressions Hands-On Tutorial 
## Python Regular Expressions Library: Anchors and Boundaries in Regular Expressions (Python)

In regular expressions, **anchors** and **boundaries** control where a pattern can match within text.

Instead of just matching characters, they allow us to specify the **position** of a match.

For example, we might want to match:

* a word only at the beginning of a line
* a number only at the end of a string
* a word as a complete word, not part of another word

Anchors and boundaries make this possible.

In Python, these features are used with the `re` module.

---

# 1. What Are Anchors in Regex?

Anchors are **zero-width assertions**.

This means:

* they do not match characters
* they match positions in text

---

## Example

```regex id="anchor_example_1"
^hello
```

Matches:

```text id="anchor_match_1"
hello world
```

But not:

```text id="anchor_no_match_1"
say hello
```

Because `^` requires the match to occur at the beginning.

---

# 2. Start of String Anchor `^`

The caret symbol `^` represents the **start of a string**.

## Pattern

```regex id="start_anchor_pattern"
^pattern
```

Meaning:

```text id="start_anchor_meaning"
pattern must appear at the beginning
```

---

## Example

Text:

```text id="start_anchor_text"
Python is great
```

Regex:

```regex id="start_anchor_regex"
^Python
```

Match:

```text id="start_anchor_match"
Python
```

---

## Python Example

```python id="start_anchor_python"
import re

text = "Python is great"

match = re.search(r"^Python", text)

print(match.group())
```

Output:

```text id="start_anchor_output"
Python
```

---

## When It Doesn't Match

Text:

```text id="start_anchor_fail_text"
I love Python
```

Regex:

```regex id="start_anchor_fail_regex"
^Python
```

Result:

```text id="start_anchor_fail_result"
No match
```

Because `Python` is not at the start.

---

# 3. End of String Anchor `$`

The dollar symbol `$` represents the **end of a string**.

## Pattern

```regex id="end_anchor_pattern"
pattern$
```

Meaning:

```text id="end_anchor_meaning"
pattern must appear at the end
```

---

## Example

Text:

```text id="end_anchor_text"
I love Python
```

Regex:

```regex id="end_anchor_regex"
Python$
```

Matches:

```text id="end_anchor_match"
Python
```

---

## Python Example

```python id="end_anchor_python"
import re

text = "I love Python"

match = re.search(r"Python$", text)

print(match.group())
```

Output:

```text id="end_anchor_output"
Python
```

---

## When It Doesn't Match

Text:

```text id="end_anchor_fail_text"
Python is powerful
```

Regex:

```regex id="end_anchor_fail_regex"
Python$
```

Result:

```text id="end_anchor_fail_result"
No match
```

Because `Python` is not at the end.

---

# 4. Using `^` and `$` Together

You can combine both anchors to ensure the **entire string** matches a pattern.

## Example

```regex id="both_anchors_regex"
^hello$
```

Meaning:

```text id="both_anchors_meaning"
string must be exactly "hello"
```

---

## Matches

```text id="both_anchors_match"
hello
```

---

## Does Not Match

```text id="both_anchors_no_match"
hello world
say hello
```

---

## Python Example

```python id="both_anchors_python"
import re

text = "hello"

match = re.search(r"^hello$", text)

print(match is not None)
```

Output:

```text id="both_anchors_output"
True
```

---

# 5. Word Boundary `\b`

A word boundary ensures that a pattern matches a **whole word**, not part of another word.

## Syntax

```regex id="word_boundary_syntax"
\b
```

Word boundaries exist between:

* word character (`\w`)
* and
* non-word character (`\W`)

---

## Examples of Boundaries

* space
* punctuation
* start of string
* end of string

---

## Example

Text:

```text id="word_boundary_text"
cat scatter category
```

Regex:

```regex id="word_boundary_regex"
\bcat\b
```

Matches:

```text id="word_boundary_match"
cat
```

Does not match:

* `scatter`
* `category`

Because `cat` is part of another word.

---

## Python Example

```python id="word_boundary_python"
import re

text = "cat scatter category"

matches = re.findall(r"\bcat\b", text)

print(matches)
```

Output:

```python id="word_boundary_output"
['cat']
```

---

# 6. Word Boundary Practical Example

Text:

```text id="word_boundary_practical_text"
The car is fast
cartoon movie
```

Regex:

```regex id="word_boundary_practical_regex"
\bcar\b
```

Matches:

```text id="word_boundary_practical_match"
car
```

But not:

```text id="word_boundary_practical_no_match"
cartoon
```

---

## Python Example

```python id="word_boundary_practical_python"
import re

text = "The car is fast. cartoon movie."

matches = re.findall(r"\bcar\b", text)

print(matches)
```

Output:

```python python_id="word_boundary_practical_output"
['car']
```

---

# 7. Non-Word Boundary `\B`

`\B` is the opposite of a word boundary.

## Pattern

```regex id="non_word_boundary_pattern"
\Bpattern\B
```

Meaning:

```text id="non_word_boundary_meaning"
pattern must be inside a word
```

---

## Example

Text:

```text id="non_word_boundary_text"
cat scatter category
```

Regex:

```regex id="non_word_boundary_regex"
\Bcat
```

Matches:

* `scatter`
* `category`

Because `cat` appears inside a larger word.

---

## Python Example

```python id="non_word_boundary_python"
import re

text = "cat scatter category"

matches = re.findall(r"\Bcat", text)

print(matches)
```

Output:

```python id="non_word_boundary_output"
['cat', 'cat']
```

These correspond to:

* `scatter`
* `category`

---

# 8. Using Anchors in Multiline Text

Normally:

* `^` → start of entire string
* `$` → end of entire string

But when working with multiline text, Python provides a flag:

```python id="multiline_flag"
re.MULTILINE
```

With this flag:

* `^` → start of each line
* `$` → end of each line

---

## Example Multiline Text

```text id="multiline_text"
apple
banana
cherry
```

Regex:

```regex id="multiline_regex"
^b
```

---

## Without Multiline Mode

```python id="multiline_without_python"
import re

text = "apple\nbanana\ncherry"

matches = re.findall(r"^b", text)

print(matches)
```

Output:

```python id="multiline_without_output"
[]
```

Because `b` is not at the start of the entire string.

---

## With Multiline Mode

```python id="multiline_with_python"
import re

text = "apple\nbanana\ncherry"

matches = re.findall(r"^b", text, re.MULTILINE)

print(matches)
```

Output:

```python id="multiline_with_output"
['b']
```

Now regex checks each line.

---

# 9. Example: Matching Line Endings

Text:

```text id="line_ending_text"
apple
banana
cherry
```

Regex:

```regex id="line_ending_regex"
a$
```

---

## Python Example

```python id="line_ending_python"
import re

text = "apple\nbanana\ncherry"

matches = re.findall(r"a$", text, re.MULTILINE)

print(matches)
```

Output:

```python id="line_ending_output"
['a']
```

Matches:

* `banana`

---

# 10. Controlling Where Matches Occur

Anchors give precise control over where patterns appear.

---

## Without Anchors

```regex id="without_anchors"
Python
```

Matches anywhere.

---

## With Anchors

```regex id="with_start_anchor"
^Python
```

Matches only at start.

```regex id="with_end_anchor"
Python$
```

Matches only at end.

```regex id="with_word_boundary"
\bPython\b
```

Matches whole word only.

---

# 11. Practical Examples

# Email Validation

Pattern:

```regex id="email_validation_regex"
^\w+@\w+\.\w+$
```

Meaning:

* start
* username
* `@`
* domain
* `.`
* extension
* end

---

## Python Example

```python id="email_validation_python"
import re

email = "user@gmail.com"

pattern = r"^\w+@\w+\.\w+$"

print(bool(re.match(pattern, email)))
```

Output:

```text id="email_validation_output"
True
```

---

# Extract Words at Line Start

Text:

```text id="line_start_text"
Python is powerful
Java is popular
C++ is fast
```

Regex:

```regex id="line_start_regex"
^[A-Z]\w+
```

---

## Python Example

```python id="line_start_python"
import re

text = """Python is powerful
Java is popular
C++ is fast"""

matches = re.findall(r"^[A-Z]\w+", text, re.MULTILINE)

print(matches)
```

Output:

```python id="line_start_output"
['Python', 'Java']
```

---

# 12. Visual Example

Text:

```text id="visual_text"
Hello world
```

Positions:

```text id="visual_positions"
^ Hello world $
```

Word boundaries:

```text id="visual_boundaries"
^ |Hello| |world| $
```

Regex engine checks these positions, not characters.

---

# 13. Summary Table

| Anchor | Meaning           |
| ------ | ----------------- |
| `^`    | start of string   |
| `$`    | end of string     |
| `\b`   | word boundary     |
| `\B`   | non-word boundary |

---

# 14. Key Differences

| Pattern   | Matches         |
| --------- | --------------- |
| `cat`     | anywhere        |
| `^cat`    | start only      |
| `cat$`    | end only        |
| `\bcat\b` | whole word only |
| `\Bcat`   | inside words    |

---

# Conclusion

Anchors and boundaries are essential regex tools that control where matches occur in text.

## Key Concepts

* `^` → start of string
* `$` → end of string
* `\b` → word boundary
* `\B` → non-word boundary
* `re.MULTILINE` → apply anchors to each line

These features allow regex to perform precise pattern matching, which is crucial for tasks such as:

* input validation
* log analysis
* text parsing
* data extraction

Anchors help ensure that patterns match exactly where you intend them to appear, making regex far more reliable and accurate.
