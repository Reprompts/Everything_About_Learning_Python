# Python Programming: Chapter 7 Python Data Types: Sets and Frozen Sets

## 1. Introduction to Sets in Python

A **set** in Python is an unordered collection of unique elements. Sets are primarily used when you need to store a collection of items where duplicate values are not allowed and where mathematical set operations such as union, intersection, and difference are required.

Sets are implemented internally using a **hash table**, which allows very fast membership testing.

### Key Characteristics of Python Sets

- Unordered collection
- No duplicate elements
- Mutable (elements can be added or removed)
- Elements must be hashable
- Efficient membership testing (`in` operator)

### Example

```python
numbers = {1, 2, 3, 4}
```

Here:

- `{1, 2, 3, 4}` is a set
- `numbers` is the variable referencing the set

---

## 2. Set Creation

Sets can be created in several different ways.

### 2.1 Using Curly Braces

The most common way to create a set is using curly braces.

```python
numbers = {1, 2, 3, 4}
```

### 2.2 Automatic Duplicate Removal

Sets automatically remove duplicate values.

```python
numbers = {1, 2, 2, 3, 3, 4}
print(numbers)
```

**Output**

```python
{1, 2, 3, 4}
```

### 2.3 Creating an Empty Set

An empty set cannot be created using `{}` because that creates a dictionary.

```python
empty_set = set()
```

### 2.4 Using the `set()` Constructor

Sets can be created from any iterable.

```python
numbers = set([1, 2, 3, 4])
```

From a string:

```python
letters = set("hello")
print(letters)
```

**Output**

```python
{'h', 'e', 'l', 'o'}
```

Duplicates are removed automatically.

---

## 3. Set Properties

Important characteristics of sets:

### 3.1 Unordered

Elements do not maintain insertion order.

```python
numbers = {3, 1, 4, 2}
print(numbers)
```

Output may appear in any order.

### 3.2 Unique Elements

Duplicate values are automatically eliminated.

### 3.3 Hashable Elements Only

Set elements must be immutable (hashable) types such as:

- Integers
- Floats
- Strings
- Tuples

Valid example:

```python
valid_set = {1, "hello", (1, 2)}
```

Invalid example:

```python
invalid_set = {[1, 2], 3}
```

Lists are mutable and cannot be stored in sets.

---

## 4. Set Operations

Sets support mathematical operations similar to those in set theory.

### 4.1 Union

Union combines all unique elements from two sets.

**Operator:** `|`

```python
A = {1, 2, 3}
B = {3, 4, 5}

print(A | B)
```

**Output**

```python
{1, 2, 3, 4, 5}
```

Method version:

```python
A.union(B)
```

---

### 4.2 Intersection

Intersection returns elements common to both sets.

**Operator:** `&`

```python
A = {1, 2, 3}
B = {2, 3, 4}

print(A & B)
```

**Output**

```python
{2, 3}
```

Method version:

```python
A.intersection(B)
```

---

### 4.3 Difference

Returns elements present in the first set but not the second.

**Operator:** `-`

```python
A = {1, 2, 3}
B = {2, 3, 4}

print(A - B)
```

**Output**

```python
{1}
```

Method version:

```python
A.difference(B)
```

---

### 4.4 Symmetric Difference

Returns elements present in either set but not both.

**Operator:** `^`

```python
A = {1, 2, 3}
B = {3, 4, 5}

print(A ^ B)
```

**Output**

```python
{1, 2, 4, 5}
```

Method version:

```python
A.symmetric_difference(B)
```

---

## 5. Set Methods

Python sets provide several built-in methods.

### 5.1 `add()`

Adds a single element.

```python
numbers = {1, 2, 3}
numbers.add(4)
```

**Result**

```python
{1, 2, 3, 4}
```

---

### 5.2 `update()`

Adds multiple elements from another iterable.

```python
numbers = {1, 2}
numbers.update([3, 4, 5])
```

**Result**

```python
{1, 2, 3, 4, 5}
```

---

### 5.3 `remove()`

Removes an element.

```python
numbers.remove(3)
```

Raises a `KeyError` if the element does not exist.

---

### 5.4 `discard()`

Removes an element without raising an error if it does not exist.

```python
numbers.discard(10)
```

---

### 5.5 `pop()`

Removes and returns an arbitrary element.

```python
numbers.pop()
```

---

### 5.6 `clear()`

Removes all elements.

```python
numbers.clear()
```

---

### 5.7 `copy()`

Creates a shallow copy.

```python
new_set = numbers.copy()
```

---

### 5.8 `union()`

Returns the union of sets.

```python
A.union(B)
```

---

### 5.9 `intersection()`

Returns common elements.

```python
A.intersection(B)
```

---

### 5.10 `difference()`

Returns the difference between sets.

```python
A.difference(B)
```

---

### 5.11 `symmetric_difference()`

Returns elements not shared by both sets.

```python
A.symmetric_difference(B)
```

---

### 5.12 `issubset()`

Checks if one set is a subset of another.

```python
A = {1, 2}
B = {1, 2, 3}

A.issubset(B)
```

**Output**

```python
True
```

---

### 5.13 `issuperset()`

Checks if one set contains another.

```python
B.issuperset(A)
```

---

### 5.14 `isdisjoint()`

Checks whether two sets share no common elements.

```python
A = {1, 2}
B = {3, 4}

A.isdisjoint(B)
```

**Output**

```python
True
```

---

### 5.15 `intersection_update()`

Updates the set with the intersection.

```python
A = {1, 2, 3}
B = {2, 3, 4}

A.intersection_update(B)
```

**Result**

```python
{2, 3}
```

---

### 5.16 `difference_update()`

Removes common elements.

```python
A.difference_update(B)
```

---

### 5.17 `symmetric_difference_update()`

Updates the set with the symmetric difference.

```python
A.symmetric_difference_update(B)
```

---

## 6. Frozen Sets

A **frozenset** is an immutable version of a set.

Once created, its elements cannot be modified.

---

## 7. Creating Frozen Sets

Frozen sets are created using the `frozenset()` constructor.

```python
fs = frozenset([1, 2, 3])
```

---

## 8. Properties of Frozen Sets

Frozen sets:

- Are immutable
- Support set operations
- Can be used as dictionary keys
- Can be stored inside other sets

### Example

```python
fs = frozenset({1, 2, 3})
```

---

## 9. Frozen Set Operations

Frozen sets support the same mathematical operations as sets except those that modify data.

```python
A = frozenset({1, 2, 3})
B = frozenset({3, 4, 5})

print(A | B)
```

**Output**

```python
frozenset({1, 2, 3, 4, 5})
```

---

## 10. Frozen Set Limitations

Because frozen sets are immutable, the following methods are not allowed:

- `add()`
- `remove()`
- `update()`
- `discard()`
- `pop()`
- `clear()`

Attempting to use them raises an error.

---

## 11. Frozen Sets as Dictionary Keys

Unlike regular sets, frozen sets are hashable.

```python
data = {
    frozenset({1, 2}): "value"
}
```

---

## 12. Frozen Sets Inside Sets

Since sets cannot contain mutable objects, frozen sets can be used instead.

```python
outer_set = {
    frozenset({1, 2}),
    frozenset({3, 4})
}
```

---

## 13. Practical Use Cases of Sets

Sets are commonly used for:

- Removing duplicates
- Membership testing
- Mathematical operations
- Filtering unique values

### Example

```python
unique_numbers = set([1, 2, 2, 3, 4, 4])
```

**Result**

```python
{1, 2, 3, 4}
```

---

## 14. Practical Use Cases of Frozen Sets

Frozen sets are useful when:

- Immutable sets are required
- Sets must be dictionary keys
- Sets must be stored in other sets
- Configuration constants are needed

---

## 15. Summary

Sets and frozen sets are powerful data structures in Python designed for efficient management of unique elements.

### Sets Provide

- Dynamic modification
- Powerful mathematical operations
- Fast membership testing

### Frozen Sets Provide

- Immutability
- Hashability
- Safe use in dictionaries and nested sets

### Key Takeaways

- Sets store unique elements only.
- Sets are mutable and support efficient set-theory operations.
- Set elements must be hashable.
- Frozen sets are immutable versions of sets.
- Frozen sets can be used as dictionary keys and nested within other sets.
- Both structures are highly optimized for membership testing and duplicate removal.

Understanding these structures is essential for efficient data handling and algorithm design in Python.
