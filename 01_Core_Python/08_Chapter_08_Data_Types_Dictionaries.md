# Python Programming: Chapter 8 Python Data Types: Dictionaries

## 1. Introduction to Dictionaries

A dictionary in Python is a built-in data structure used to store data in **key–value pairs**. Each key in a dictionary maps to a specific value, allowing efficient data retrieval based on the key.

Dictionaries are widely used in Python because they provide fast lookup, insertion, and deletion operations.

### Example

```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "London"
}
```

In this example:

- `"name"`, `"age"`, `"city"` are **keys**
- `"Alice"`, `25`, `"London"` are **values**

---

## 2. Characteristics of Dictionaries

Important properties of Python dictionaries:

- Store key-value pairs
- Keys must be unique
- Keys must be hashable (immutable types)
- Values can be any data type
- Dictionaries are mutable
- Python 3.7+ preserves insertion order

### Example

```python
data = {
    "id": 101,
    "name": "John",
    "salary": 50000
}
```

---

## 3. Dictionary Creation

There are multiple ways to create dictionaries.

### 3.1 Using Curly Braces

The most common method.

```python
student = {
    "name": "Rahul",
    "age": 21,
    "course": "Computer Science"
}
```

### 3.2 Creating an Empty Dictionary

```python
data = {}
```

### 3.3 Using `dict()` Constructor

```python
person = dict(name="Alice", age=30)
```

**Result:**

```python
{'name': 'Alice', 'age': 30}
```

### 3.4 From List of Tuples

```python
data = dict([
    ("name", "Alice"),
    ("age", 25)
])
```

### 3.5 Using Dictionary Comprehension

```python
squares = {x: x * x for x in range(5)}
```

**Result:**

```python
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

---

## 4. Key-Value Mapping

Dictionaries associate keys with values.

### Example

```python
employee = {
    "id": 101,
    "name": "John",
    "salary": 60000
}
```

Access value using a key:

```python
print(employee["name"])
```

**Output:**

```text
John
```

---

## 5. Accessing Dictionary Elements

### 5.1 Using Bracket Notation

```python
employee["salary"]
```

### 5.2 Using `get()`

```python
employee.get("salary")
```

### Difference Between `[]` and `get()`

```python
employee["bonus"]
```

Raises:

```text
KeyError
```

But:

```python
employee.get("bonus")
```

Returns:

```python
None
```

You can specify a default value:

```python
employee.get("bonus", 0)
```

---

## 6. Modifying Dictionaries

### Add a New Key

```python
employee["department"] = "IT"
```

### Modify an Existing Value

```python
employee["salary"] = 70000
```

---

## 7. Dictionary Methods

Python provides many built-in methods for dictionaries.

### 7.1 `clear()`

Removes all items.

```python
data.clear()
```

### 7.2 `copy()`

Creates a shallow copy.

```python
new_dict = data.copy()
```

### 7.3 `fromkeys()`

Creates a dictionary from a sequence of keys.

```python
keys = ["a", "b", "c"]

dict.fromkeys(keys, 0)
```

**Output:**

```python
{'a': 0, 'b': 0, 'c': 0}
```

### 7.4 `get()`

Returns the value for a key.

```python
data.get("name")
```

### 7.5 `items()`

Returns key-value pairs.

```python
data.items()
```

Example:

```python
for key, value in data.items():
    print(key, value)
```

### 7.6 `keys()`

Returns dictionary keys.

```python
data.keys()
```

Example:

```python
for key in data.keys():
    print(key)
```

### 7.7 `values()`

Returns dictionary values.

```python
data.values()
```

Example:

```python
for value in data.values():
    print(value)
```

### 7.8 `pop()`

Removes a key and returns its value.

```python
data.pop("age")
```

### 7.9 `popitem()`

Removes the last inserted key-value pair.

```python
data.popitem()
```

Example output:

```python
('salary', 60000)
```

### 7.10 `setdefault()`

Returns the value if the key exists; otherwise inserts the key.

```python
data.setdefault("country", "India")
```

### 7.11 `update()`

Updates a dictionary with another dictionary.

```python
data.update({
    "age": 30,
    "city": "Delhi"
})
```

---

## 8. Nested Dictionaries

A dictionary can contain other dictionaries.

### Example

```python
students = {
    "student1": {
        "name": "Alice",
        "age": 21
    },
    "student2": {
        "name": "Bob",
        "age": 22
    }
}
```

Access nested values:

```python
students["student1"]["name"]
```

**Output:**

```text
Alice
```

---

## 9. Dictionary Views

Dictionary views are special objects that provide a dynamic view of dictionary data.

These include:

- Keys view
- Values view
- Items view

### 9.1 Keys View

```python
data.keys()
```

Returns:

```python
dict_keys(['name', 'age'])
```

### 9.2 Values View

```python
data.values()
```

Returns:

```python
dict_values(['Alice', 25])
```

### 9.3 Items View

```python
data.items()
```

Returns:

```python
dict_items([('name', 'Alice'), ('age', 25)])
```

---

## 10. Properties of Dictionary Views

Dictionary views are dynamic.

### Example

```python
data = {"a": 1, "b": 2}

keys = data.keys()

data["c"] = 3

print(keys)
```

**Output:**

```python
dict_keys(['a', 'b', 'c'])
```

Views automatically reflect changes.

---

## 11. Ordered Dictionary Behavior

Before Python 3.7, dictionaries did not guarantee order.

From Python 3.7 onward, dictionaries preserve insertion order.

### Example

```python
data = {
    "a": 1,
    "b": 2,
    "c": 3
}

print(data)
```

**Output:**

```python
{'a': 1, 'b': 2, 'c': 3}
```

Order remains the same as insertion.

---

## 12. OrderedDict (Historical Context)

Before Python 3.7, ordered behavior required:

```python
from collections import OrderedDict
```

### Example

```python
from collections import OrderedDict

data = OrderedDict()

data["a"] = 1
data["b"] = 2
```

Today this is rarely needed because standard dictionaries preserve order.

---

## 13. Dictionary Membership Testing

Check whether a key exists:

```python
"name" in data
```

**Returns:**

```python
True
```

---

## 14. Dictionary Iteration

### Iterate Over Keys

```python
for key in data:
    print(key)
```

### Iterate Over Values

```python
for value in data.values():
    print(value)
```

### Iterate Over Key-Value Pairs

```python
for key, value in data.items():
    print(key, value)
```

---

## 15. Performance Characteristics

Dictionaries are implemented using **hash tables**.

This allows:

- Average **O(1)** lookup
- Fast insertion
- Fast deletion

However:

- Worst-case performance can degrade if many hash collisions occur.

---

## 16. Real-World Use Cases

### Configuration Storage

```python
config = {
    "host": "localhost",
    "port": 8080
}
```

### Counting Occurrences

```python
word_count = {}
```

### Mapping IDs to Objects

```python
users = {
    101: "Alice",
    102: "Bob"
}
```

---

## 17. Summary

Python dictionaries are powerful and flexible data structures that:

- Store key-value mappings
- Allow fast data access
- Support nested structures
- Provide many built-in methods
- Preserve insertion order

### Key Takeaways

- Dictionaries store data as key-value pairs.
- Keys must be unique and hashable.
- Values can be any Python object.
- Dictionary views dynamically reflect changes.
- Dictionaries are optimized using hash tables for fast operations.
- They are extensively used in APIs, databases, configuration systems, and data processing applications.

Understanding dictionaries is essential for working with structured data, APIs, databases, and large-scale applications in Python.
