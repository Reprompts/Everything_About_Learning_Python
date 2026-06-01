# Python Programming: Chapter 9 Python Data Types: Binary Types

Binary data types in Python are used to work with raw binary data (0s and 1s). These types are commonly used when dealing with:

- File processing
- Network communication
- Image processing
- Encryption
- Low-level system programming
- Binary protocols

Python provides three main binary types:

- `bytes`
- `bytearray`
- `memoryview`

These types allow programs to efficiently manipulate raw binary data in memory.

---

# 1. Understanding Binary Data

Binary data represents information using 8-bit units called **bytes**.

Each byte ranges from:

```text
0 – 255
```

Example representation:

```text
01000001
```

This binary value corresponds to the ASCII character:

```text
A
```

Python binary types store sequences of such bytes.

---

# 2. The `bytes` Type

## 2.1 What is `bytes`?

`bytes` is an immutable sequence of bytes.

Once created, the contents of a bytes object cannot be modified.

This is similar to how strings are immutable sequences of characters.

---

## 2.2 Creating Bytes Objects

### Using a Bytes Literal

```python
data = b"Hello"
```

Output:

```python
b'Hello'
```

The prefix `b` indicates a bytes literal.

### Using the `bytes()` Constructor

```python
data = bytes([65, 66, 67])
```

Output:

```python
b'ABC'
```

Each number corresponds to an ASCII value.

### Creating Empty Bytes

```python
data = bytes()
```

### From String with Encoding

```python
text = "Hello"
data = text.encode("utf-8")
```

Output:

```python
b'Hello'
```

---

# 3. Accessing Bytes

Bytes behave like sequences.

### Example

```python
data = b"Python"

print(data[0])
```

Output:

```text
80
```

### Explanation

The ASCII value of `'P'` is returned.

### Slicing

```python
data[1:4]
```

Output:

```python
b'yth'
```

---

# 4. Bytes Methods

The `bytes` object supports many methods similar to strings.

## 4.1 `capitalize()`

Returns a copy with the first byte capitalized.

```python
b"hello".capitalize()
```

Output:

```python
b'Hello'
```

---

## 4.2 `center()`

Centers the sequence.

```python
b"hi".center(6)
```

Output:

```python
b'  hi  '
```

---

## 4.3 `count()`

Counts occurrences.

```python
b"banana".count(b"a")
```

Output:

```python
3
```

---

## 4.4 `decode()`

Converts bytes to string.

```python
data = b"Hello"
data.decode("utf-8")
```

Output:

```text
Hello
```

---

## 4.5 `endswith()`

Checks suffix.

```python
b"hello".endswith(b"lo")
```

---

## 4.6 `find()`

Finds position.

```python
b"hello".find(b"e")
```

Output:

```python
1
```

---

## 4.7 `index()`

Similar to `find()` but raises an error if not found.

```python
b"hello".index(b"e")
```

---

## 4.8 `isalnum()`

Checks whether all bytes are alphanumeric.

```python
b"abc123".isalnum()
```

---

## 4.9 `isalpha()`

Checks whether all bytes are alphabetic.

```python
b"abc".isalpha()
```

---

## 4.10 `isdigit()`

Checks whether all bytes are numeric digits.

```python
b"123".isdigit()
```

---

## 4.11 `islower()`

Checks lowercase bytes.

```python
b"abc".islower()
```

---

## 4.12 `isupper()`

Checks uppercase bytes.

```python
b"ABC".isupper()
```

---

## 4.13 `join()`

Joins byte sequences.

```python
b"-".join([b"a", b"b", b"c"])
```

Output:

```python
b'a-b-c'
```

---

## 4.14 `lower()`

Converts to lowercase.

```python
b"HELLO".lower()
```

---

## 4.15 `upper()`

Converts to uppercase.

```python
b"hello".upper()
```

---

## 4.16 `replace()`

Replaces bytes.

```python
b"hello".replace(b"h", b"H")
```

---

## 4.17 `split()`

Splits bytes.

```python
b"a,b,c".split(b",")
```

Output:

```python
[b'a', b'b', b'c']
```

---

## 4.18 `strip()`

Removes leading and trailing whitespace.

```python
b"  hi  ".strip()
```

---

## 4.19 `startswith()`

Checks prefix.

```python
b"hello".startswith(b"he")
```

---

# 5. The `bytearray` Type

## 5.1 What is `bytearray`?

`bytearray` is a mutable sequence of bytes.

Unlike `bytes`, its contents can be modified.

### Example

```python
data = bytearray(b"hello")
```

---

## 5.2 Modifying a Bytearray

```python
data[0] = 72
```

Result:

```python
bytearray(b'Hello')
```

---

# 6. Creating Bytearray Objects

## Using Constructor

```python
data = bytearray([65, 66, 67])
```

Output:

```python
bytearray(b'ABC')
```

### From String

```python
data = bytearray("hello", "utf-8")
```

---

# 7. Bytearray Methods

`bytearray` supports all `bytes` methods plus mutable operations.

## 7.1 `append()`

Adds a byte.

```python
data.append(33)
```

Result:

```python
bytearray(b'hello!')
```

---

## 7.2 `extend()`

Adds multiple bytes.

```python
data.extend(b" world")
```

---

## 7.3 `insert()`

Insert at a specific position.

```python
data.insert(0, 72)
```

---

## 7.4 `pop()`

Removes a byte.

```python
data.pop()
```

---

## 7.5 `remove()`

Removes a specific byte.

```python
data.remove(104)
```

---

## 7.6 `reverse()`

Reverses the byte sequence.

```python
data.reverse()
```

---

## 7.7 `clear()`

Removes all elements.

```python
data.clear()
```

---

# 8. The `memoryview` Type

## 8.1 What is `memoryview`?

`memoryview` provides a view of memory without copying data.

It allows efficient access to binary data stored in other objects.

### Example

```python
data = bytearray(b"hello")
view = memoryview(data)
```

---

## 8.2 Why `memoryview` Exists

Without `memoryview`:

- Slicing binary data creates copies.

With `memoryview`:

- Slices share the same underlying memory.

This improves performance and reduces memory usage.

---

## 8.3 Accessing Data

```python
view[0]
```

Output:

```python
104
```

ASCII value of `'h'`.

---

## 8.4 Modifying Underlying Data

```python
view[0] = 72
```

Result:

```python
bytearray(b'Hello')
```

`memoryview` modifies the original object.

---

# 9. Memoryview Attributes

## `format`

Data type format.

```python
view.format
```

---

## `itemsize`

Size of one element in bytes.

```python
view.itemsize
```

---

## `nbytes`

Total number of bytes.

```python
view.nbytes
```

---

## `shape`

Dimensions of the memory view.

```python
view.shape
```

---

# 10. Memoryview Methods

## `tobytes()`

Convert view to bytes.

```python
view.tobytes()
```

---

## `tolist()`

Convert view to list.

```python
view.tolist()
```

---

## `release()`

Release the memory view.

```python
view.release()
```

---

# 11. When to Use Each Binary Type

## Use `bytes` When

- Data should be immutable
- Working with binary protocols
- Representing raw binary data

---

## Use `bytearray` When

- Binary data must be modified
- Performing in-place operations
- Building binary buffers dynamically

---

## Use `memoryview` When

- Working with large binary data
- Avoiding unnecessary memory copies
- Building high-performance applications

---

# 12. Real-World Applications

Binary types are used in:

- Networking protocols
- File I/O
- Multimedia processing
- Cryptography
- System programming

### Example: Reading a Binary File

```python
with open("image.jpg", "rb") as f:
    data = f.read()
```

---

# Summary

Python provides powerful tools for working with binary data.

## `bytes`

- Immutable sequence
- Used for fixed binary data
- Safe and memory efficient

## `bytearray`

- Mutable version of `bytes`
- Supports modification
- Useful for dynamic binary buffers

## `memoryview`

- Efficient memory access
- Avoids copying data
- Ideal for performance-critical applications

Understanding these types is essential for working with low-level data, networking, file processing, and high-performance applications in Python.
