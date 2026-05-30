
# Python Programming: Chapter 17 Closures & Scope

Understanding scope is fundamental to writing correct Python programs. Scope determines where variables are accessible and how Python resolves variable names during execution.

Python follows a rule called the **LEGB rule**, which defines how variable names are searched in different scopes. Closures build on this system and allow functions to remember variables from their enclosing environment, even after the outer function has finished executing.

This article explains in detail:

- Local scope  
- Global scope  
- Enclosing scope  
- Built-in scope  
- Closures  
- Free variables  
- Nonlocal variables  

---

## 1. What is Scope?

Scope refers to the region of a program where a variable is accessible.

```python
x = 10

def show():
    print(x)

show()
````

**Output:**

```
10
```

The function accesses `x` from outside its body because Python searches for variables according to scope rules.

---

## 2. LEGB Rule

Python resolves variable names using the LEGB rule.

Search order:

1. Local
2. Enclosing
3. Global
4. Built-in

If a variable is not found in one scope, Python continues searching in the next.

Example:

```python
len([1, 2, 3])
```

Python searches:

Local → Enclosing → Global → Built-in

`len` is found in the built-in scope.

---

## 3. Local Scope

Local scope refers to variables defined inside a function. These exist only during function execution.

```python
def calculate():
    result = 5 + 3
    print(result)

calculate()
```

**Output:**

```
8
```

Trying to access `result` outside causes an error:

```
NameError: name 'result' is not defined
```

### Local Variable Lifetime

Local variables are created when the function is called and destroyed when it finishes.

```python
def test():
    x = 10
    print(x)

test()
```

After execution, `x` no longer exists.

---

## 4. Global Scope

Global variables are defined outside functions and accessible throughout the module.

```python
x = 20

def show():
    print(x)

show()
```

**Output:**

```
20
```

### Modifying Global Variables

Use the `global` keyword:

```python
count = 0

def increment():
    global count
    count += 1

increment()
print(count)
```

**Output:**

```
1
```

Without `global`, Python assumes local scope:

```
UnboundLocalError
```

---

## 5. Enclosing Scope

Occurs when a function is defined inside another function.

```python
def outer():
    x = 10

    def inner():
        print(x)

    inner()

outer()
```

**Output:**

```
10
```

Python finds `x` in the enclosing scope.

---

## 6. Built-in Scope

Contains Python’s built-in functions and names.

Examples:

* `len()`
* `print()`
* `range()`
* `type()`

```python
numbers = [1, 2, 3]
print(len(numbers))
```

You can inspect built-ins:

```python
dir(__builtins__)
```

---

## 7. Closures

A closure occurs when a function remembers variables from its enclosing scope even after the outer function has finished executing.

Closures are widely used in:

* decorators
* function factories
* callbacks

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    return inner

func = outer()
func()
```

**Output:**

```
Hello
```

Even after `outer()` finishes, `inner()` remembers `message`.

---

## 8. Free Variables

A free variable is a variable used inside a function but defined in an enclosing scope.

```python
def outer():
    x = 10

    def inner():
        print(x)

    return inner

func = outer()
print(func.__code__.co_freevars)
```

**Output:**

```
('x',)
```

---

## 9. Nonlocal Variables

The `nonlocal` keyword allows modification of variables in the enclosing scope.

```python
def outer():
    x = 10

    def inner():
        nonlocal x
        x += 5
        print(x)

    inner()

outer()
```

**Output:**

```
15
```

Without `nonlocal`, Python creates a new local variable.

---

## 10. global vs nonlocal

| Keyword  | Affects Scope            |
| -------- | ------------------------ |
| global   | module scope             |
| nonlocal | enclosing function scope |

---

## 11. Practical Closure Example

```python
def multiplier(n):
    def multiply(x):
        return x * n
    return multiply

double = multiplier(2)
triple = multiplier(3)

print(double(5))
print(triple(5))
```

**Output:**

```
10
15
```

Each function remembers its own `n`.

---

## 12. Closure Inspection

```python
print(double.__closure__)
```

To access stored values:

```python
print(double.__closure__[0].cell_contents)
```

**Output:**

```
2
```

---

## 13. Use Cases of Closures

Closures are used in:

* Function factories
* Callback functions
* Decorators
* State retention functions

Example:

```python
def power(exponent):
    def calculate(base):
        return base ** exponent
    return calculate
```

---

## 14. Common Closure Mistake

### Problem (late binding):

```python
funcs = []

for i in range(3):
    def f():
        print(i)
    funcs.append(f)

for func in funcs:
    func()
```

**Output:**

```
2
2
2
```

### Fix:

```python
funcs = []

for i in range(3):
    def f(i=i):
        print(i)
    funcs.append(f)
```

**Output:**

```
0
1
2
```

---

## 15. Summary

Python uses the **LEGB rule** for variable resolution:

* **Local scope** → inside functions
* **Enclosing scope** → outer functions
* **Global scope** → module level
* **Built-in scope** → Python defaults

### Closures

Functions that remember variables from enclosing scope.

### Free variables

Variables referenced in a function but defined outside it.

### Nonlocal variables

Allow modification of enclosing scope variables.

---

Understanding scope and closures is essential for writing correct, maintainable, and advanced Python programs, especially in:

* nested functions
* decorators
* functional programming patterns


