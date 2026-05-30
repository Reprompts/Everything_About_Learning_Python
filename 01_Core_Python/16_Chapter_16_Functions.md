
# Python Programming: Chapter 16 Functions

Functions are one of the most important building blocks of programming. They allow developers to organize code into reusable blocks, improve readability, avoid repetition, and implement complex logic in a structured manner.

In Python, functions are powerful because they support advanced features such as first-class behavior, higher-order programming, recursion, and flexible argument handling.

This guide explains functions in detail, including:

- Function basics  
- Function arguments  
- Advanced function concepts  

---

## 1. Function Basics

A function is a named block of reusable code designed to perform a specific task.

Instead of writing the same code multiple times, you can define it once and reuse it whenever needed.

### Example

Without functions:
```python
print(5 + 10)
print(8 + 12)
print(7 + 9)
````

With functions:

```python
def add(a, b):
    return a + b
```

Now the function can be reused multiple times.

---

## 2. Defining Functions

Functions are defined using the `def` keyword.

### Syntax

```python
def function_name(parameters):
    function_body
```

### Example

```python
def greet():
    print("Hello, welcome to Python")
```

### Explanation

* `def` → keyword used to define a function
* `greet` → function name
* `()` → parameter list
* body → code executed when function is called

---

## 3. Calling Functions

A function runs only when it is called.

```python
def greet():
    print("Hello")

greet()
```

**Output:**

```text
Hello
```

You can call a function multiple times:

```python
greet()
greet()
greet()
```

---

## 4. Return Statements

The `return` statement sends a value back to the caller.

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)
```

**Output:**

```text
8
```

### Multiple Return Values

```python
def calculate(a, b):
    return a + b, a - b

x, y = calculate(10, 5)
print(x)
print(y)
```

**Output:**

```text
15
5
```

Internally, Python returns a tuple.

---

## 5. Function Arguments

Arguments are values passed to a function when calling it.

### 5.1 Positional Arguments

```python
def multiply(a, b):
    return a * b

multiply(3, 4)
```

* `a = 3`
* `b = 4`

Order matters.

---

### 5.2 Keyword Arguments

```python
multiply(a=3, b=4)
multiply(b=4, a=3)
```

Order does not matter.

---

### 5.3 Default Arguments

```python
def greet(name="Guest"):
    print("Hello", name)

greet("Alice")
greet()
```

**Output:**

```text
Hello Alice
Hello Guest
```

---

### 5.4 Variable-Length Arguments (*args)

```python
def sum_all(*args):
    total = 0
    for num in args:
        total += num
    return total

sum_all(1, 2, 3, 4)
```

**Output:**

```text
10
```

`args` is a tuple.

---

### 5.5 Keyword Variable Arguments (**kwargs)

```python
def show_info(**kwargs):
    for key, value in kwargs.items():
        print(key, value)

show_info(name="Alice", age=25)
```

**Output:**

```text
name Alice
age 25
```

Stored internally as a dictionary.

---

### 5.6 Positional-Only Parameters

```python
def divide(a, b, /):
    return a / b

divide(10, 2)
```

Invalid:

```python
divide(a=10, b=2)
```

---

### 5.7 Keyword-Only Parameters

```python
def connect(host, *, port):
    print(host, port)

connect("localhost", port=8080)
```

Invalid:

```python
connect("localhost", 8080)
```

---

## 6. First-Class Functions

Functions in Python are first-class objects:

* can be assigned to variables
* passed as arguments
* returned from other functions

```python
def greet():
    print("Hello")

func = greet
func()
```

---

## 7. Higher-Order Functions

A function that:

* takes another function as argument, or
* returns a function

```python
def apply(func, value):
    return func(value)

def square(x):
    return x * x

print(apply(square, 5))
```

**Output:**

```text
25
```

### Built-in examples:

* `map()`
* `filter()`
* `sorted()`

---

## 8. Lambda Functions

Anonymous functions using `lambda`.

```python
square = lambda x: x * x
print(square(5))
```

**Output:**

```text
25
```

### Lambda with sorting

```python
pairs = [(1, 3), (2, 1), (4, 2)]
pairs.sort(key=lambda x: x[1])
```

---

## 9. Recursion

A function calling itself.

```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)

factorial(5)
```

**Output:**

```text
120
```

### Components:

* Base case
* Recursive case

---

## 10. Function Annotations

```python
def add(a: int, b: int) -> int:
    return a + b
```

Stored in:

```python
add.__annotations__
```

Output:

```text
{'a': int, 'b': int, 'return': int}
```

---

## 11. Docstrings

```python
def add(a, b):
    """
    Returns the sum of two numbers.
    """
    return a + b
```

Access:

```python
print(add.__doc__)
```

### Multi-line docstring

```python
def area(radius):
    """
    Calculate area of a circle.

    Parameters:
    radius : float

    Returns:
    float
    """
    return 3.14 * radius * radius
```

---

## 12. Practical Example

```python
def process_numbers(*numbers, operation=lambda x: x):
    results = []
    for num in numbers:
        results.append(operation(num))
    return results

print(process_numbers(1, 2, 3, operation=lambda x: x * 2))
```

**Output:**

```text
[2, 4, 6]
```

---

## 13. Best Practices

* Use descriptive names (`calculate_total`)
* Keep functions small and focused
* Avoid doing multiple tasks in one function
* Write proper docstrings
* Use default arguments carefully

---

## 14. Summary

Functions are a core abstraction in Python.

Key concepts:

* Function definition and calling → reusable logic
* Return values → output from functions
* Arguments → positional, keyword, default, *args, **kwargs
* Advanced parameters → positional-only, keyword-only
* First-class functions → functions as objects
* Higher-order functions → functions operating on functions
* Lambda → concise anonymous functions
* Recursion → self-calling functions
* Annotations & docstrings → documentation and readability

Understanding functions deeply is essential for writing modular, reusable, and scalable Python programs.


