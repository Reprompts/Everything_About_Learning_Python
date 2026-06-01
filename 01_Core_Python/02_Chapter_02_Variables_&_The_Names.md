# Python Programming: Chapter 2 Variables & the Names

## 1. Introduction

In Python, variables are fundamentally different from variables in many other languages such as C or Java. A variable in Python is **not a container that holds data directly**. Instead, it is a **name (identifier)** that refers to an object stored in memory.

This distinction is critical for understanding:

- Assignment behavior
- Mutability
- Function arguments
- Memory efficiency

A correct mental model is:

```text
Variable (name) → Reference → Object (in memory)
```

---

## 2. Variables and Assignment

### Basic Assignment

```python
x = 10
```

Execution steps:

1. Python creates an integer object `10` in memory.
2. The name `x` is bound to that object.

### Assignment Does Not Copy Objects

```python
a = [1, 2, 3]
b = a
```

Both `a` and `b` refer to the same object.

No duplication of the list occurs.

```python
b.append(4)
print(a)
```

Output:

```python
[1, 2, 3, 4]
```

This demonstrates that assignment copies **references**, not objects.

### Multiple Assignment

```python
a = b = 5
```

A single object `5` is created.

Both names point to the same object.

---

## 3. Dynamic Typing

Python is dynamically typed, meaning:

- Type is associated with the object, not the variable.
- A variable can refer to objects of different types over time.

```python
x = 10       # x refers to an integer object
x = "hello"  # x now refers to a string object
```

The name `x` is simply rebound to a different object.

---

## 4. Name Binding

Assignment is technically called **binding a name to an object**.

```python
x = 100
```

This binds the name `x` to the object `100`.

### Rebinding

```python
x = 200
```

`x` is now bound to a new object.

The previous object may be garbage collected if no references remain.

---

## 5. Tuple Unpacking

Tuple unpacking allows assigning multiple values at once.

```python
a, b = 10, 20
```

This is equivalent to:

```python
a = 10
b = 20
```

### Practical Example

```python
point = (3, 4)
x, y = point
```

---

## 6. Extended Unpacking

Python allows capturing multiple elements using `*`.

```python
a, *b = [1, 2, 3, 4]
```

Result:

```python
a = 1
b = [2, 3, 4]
```

### More Complex Example

```python
first, *middle, last = [1, 2, 3, 4, 5]
```

Result:

```python
first = 1
middle = [2, 3, 4]
last = 5
```

---

## 7. Swapping Variables

Python provides built-in syntax for swapping values.

```python
a = 5
b = 10

a, b = b, a
```

Internally, Python creates a temporary tuple and unpacks it.

---

## 8. Variable Naming Conventions

### Rules

- Must start with a letter or underscore (`_`)
- Cannot start with a digit
- Case-sensitive

### Conventions

```python
user_name = "valid"
MAX_SIZE = 100
_private_var = "internal use"
```

Naming conventions improve readability and maintainability.

---

## 9. Variable Annotations (Type Hints)

```python
x: int = 10
name: str = "Ganesh"
```

### Important Characteristics

- They do not enforce types at runtime.
- Used by tools like linters and IDEs.
- Improve documentation and readability.

---

## 10. Variable Lifecycle

A variable goes through several stages:

1. Creation (object is created)
2. Binding (name refers to object)
3. Usage (operations performed)
4. Rebinding (name points to another object)
5. Deletion (`del`)
6. Garbage collection (if no references remain)

### Example

```python
x = 10
x = 20
del x
```

---

## 11. Name Resolution Rules

When a variable is used, Python must determine where it is defined.

This process follows a specific order.

---

## 12. LEGB Rule

Python resolves names in the following order:

1. Local scope
2. Enclosing scope
3. Global scope
4. Built-in scope

### Example

```python
x = "global"

def outer():
    x = "enclosing"

    def inner():
        x = "local"
        print(x)

    inner()

outer()
```

Output:

```python
local
```

---

## 13. Global Variables

```python
x = 10

def modify():
    global x
    x = 20
```

Without `global`, a new local variable is created instead.

`global` allows access to and modification of global variables inside a function.

---

## 14. Nonlocal Variables

Used in nested functions.

```python
def outer():
    x = 10

    def inner():
        nonlocal x
        x = 20

    inner()
    print(x)

outer()
```

Output:

```python
20
```

`nonlocal` allows access to and modification of variables in the enclosing scope.

---

## 15. Deleting Variables

```python
x = 10
del x
```

This removes the name binding.

If no references remain, the object becomes eligible for garbage collection.

---

## 16. Interning of Objects

Interning is an optimization where Python reuses immutable objects.

---

## 17. Small Integer Caching

Python caches integers in the range:

```text
-5 to 256
```

Example:

```python
a = 100
b = 100

print(a is b)
```

Output:

```python
True
```

---

## 18. String Interning

```python
a = "hello"
b = "hello"

print(a is b)
```

Output (often):

```python
True
```

Strings may be reused to save memory.

---

## 19. Variable Identity (`id()`)

```python
x = 10
print(id(x))
```

Returns a unique identifier for the object.

It often corresponds to the memory location used internally by the Python implementation.

---

## 20. Identity vs Equality

```python
a = [1, 2]
b = [1, 2]

print(a == b)
print(a is b)
```

Output:

```python
True
False
```

Explanation:

- `==` checks whether values are equal.
- `is` checks whether both variables refer to the same object.

---

## 21. Mutability and Its Effects

### Mutable Objects

Examples:

- List
- Dictionary
- Set

These can be modified in place.

```python
a = [1, 2]
b = a

b.append(3)

print(a)
```

Output:

```python
[1, 2, 3]
```

### Immutable Objects

Examples:

- `int`
- `float`
- `str`
- `tuple`

These cannot be modified after creation.

```python
a = 10
b = a

b = 20

print(a)
```

Output:

```python
10
```

A new object is created instead of modifying the original.

---

## 22. Side Effects of Mutability

Mutability can lead to unintended shared changes.

### Example

```python
def add_item(lst=[]):
    lst.append(1)
    return lst
```

Each call modifies the same list object.

This is a common Python pitfall involving mutable default arguments.

---

## 23. Best Practices

- Avoid sharing mutable objects unintentionally.
- Use `copy()` or `deepcopy()` when needed.
- Prefer immutable data structures where possible.
- Limit use of global variables.
- Use clear and consistent naming conventions.

---

## 24. Final Summary

### Key Concepts

- Variables are names referencing objects.
- Assignment binds names to objects.
- Python uses dynamic typing.
- Memory is managed via references.
- Name resolution follows the LEGB rule.
- Mutability affects behavior significantly.
- Interning improves efficiency.

### Final Insight

A precise understanding of variables in Python comes from recognizing that:

> There are no "boxes storing values."

Instead:

> There are objects in memory and names pointing to them.

This mental model explains most behaviors that appear confusing at first, especially in real-world applications involving data structures, functions, and performance optimization.
