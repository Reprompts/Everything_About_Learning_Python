# Python Programming: Chapter 3 Python Data Types: Numeric Types

# Python Data Types

## 1. Introduction to Numeric Types in Python

Numeric data types represent numbers and support mathematical operations. Python provides several built-in numeric types designed to handle different kinds of numerical data. These types are:

- Integers (`int`)
- Floating-point numbers (`float`)
- Complex numbers (`complex`)
- Boolean values (`bool`)

All numeric types in Python are implemented as objects. This means they behave consistently with Python's object model and support methods, attributes, and operator overloading.

Python's numeric system is designed to be:

- Mathematically expressive
- Precise for integer arithmetic
- Flexible for scientific computing
- Consistent across platforms

---

## 2. Integers (`int`)

### 2.1 Definition

Integers represent whole numbers without fractional components.

Examples:

- `-10`
- `0`
- `25`
- `100000`

In Python, integers are represented using the built-in type `int`.

```python
x = 10
y = -5
z = 0
```

### 2.2 Characteristics of Python Integers

Python integers have several unique properties:

- Unlimited size (arbitrary precision)
- Support for multiple number bases
- Exact arithmetic
- Automatic memory management

Unlike languages such as C or Java, Python integers do not overflow when exceeding a fixed size.

### 2.3 Integer Representation in Different Bases

Python supports multiple numeric bases.

#### Decimal (Base 10)

```python
a = 10
```

#### Binary (Base 2)

```python
b = 0b1010
```

#### Octal (Base 8)

```python
c = 0o12
```

#### Hexadecimal (Base 16)

```python
d = 0xA
```

All of these represent the same value:

```text
10
```

### 2.4 Converting Between Bases

Python provides built-in functions for conversion.

```python
bin(10)
oct(10)
hex(10)
```

Output:

```python
'0b1010'
'0o12'
'0xa'
```

### 2.5 Integer Arithmetic Operations

Python supports all standard arithmetic operations.

```python
a = 10
b = 3

print(a + b)   # addition
print(a - b)   # subtraction
print(a * b)   # multiplication
print(a / b)   # division
print(a // b)  # floor division
print(a % b)   # modulus
print(a ** b)  # exponentiation
```

### 2.6 Integer Methods

Some useful methods include:

```python
(10).bit_length()
```

This returns the number of bits required to represent the integer.

Example:

```python
(1024).bit_length()
```

---

## 3. Floating-Point Numbers (`float`)

### 3.1 Definition

Floating-point numbers represent real numbers with fractional parts.

Examples:

- `3.14`
- `-0.001`
- `10.0`

```python
x = 3.14
y = -2.5
```

### 3.2 Internal Representation

Python floats follow the IEEE 754 double-precision standard.

Key characteristics:

- 64-bit representation
- Approximately 15–17 decimal digits of precision
- Binary fraction storage

This means some decimal numbers cannot be represented exactly.

Example:

```python
0.1 + 0.2
```

Output:

```python
0.30000000000000004
```

This is not a bug but a limitation of binary floating-point representation.

### 3.3 Floating-Point Operations

Basic arithmetic operations work similarly to integers.

```python
a = 5.5
b = 2.0

print(a + b)
print(a - b)
print(a * b)
print(a / b)
```

### 3.4 Special Floating Values

Floating numbers include special representations.

#### Infinity

```python
float("inf")
```

#### Negative Infinity

```python
float("-inf")
```

#### Not a Number (NaN)

```python
float("nan")
```

Example:

```python
x = float("nan")
print(x == x)
```

Output:

```python
False
```

This behavior follows IEEE standards.

### 3.5 Float Methods

Example:

```python
x = 3.14159
print(x.as_integer_ratio())
```

Output:

```python
(3537115888337719, 1125899906842624)
```

This reveals the exact fractional representation.

---

## 4. Complex Numbers (`complex`)

### 4.1 Definition

Complex numbers consist of a real part and an imaginary part.

Mathematical form:

```text
a + bj
```

Example:

```python
z = 3 + 4j
```

Where:

- `3` = real part
- `4` = imaginary part

### 4.2 Creating Complex Numbers

```python
z1 = 2 + 3j
z2 = complex(2, 3)
```

Both create the same complex number.

### 4.3 Accessing Components

```python
z = 5 + 6j

print(z.real)
print(z.imag)
```

Output:

```python
5.0
6.0
```

### 4.4 Complex Arithmetic

Python supports standard operations.

```python
a = 2 + 3j
b = 1 + 4j

print(a + b)
print(a - b)
print(a * b)
print(a / b)
```

### 4.5 Use Cases

Complex numbers are used in:

- Signal processing
- Electrical engineering
- Quantum physics
- Fourier transforms

---

## 5. Boolean Values (`bool`)

### 5.1 Definition

Boolean values represent logical truth values.

There are only two possible values:

- `True`
- `False`

Example:

```python
x = True
y = False
```

### 5.2 Boolean as a Subclass of Integer

In Python:

```python
True == 1
False == 0
```

Example:

```python
print(True + True)
```

Output:

```python
2
```

This occurs because `bool` inherits from `int`.

### 5.3 Boolean Operations

```python
a = True
b = False

print(a and b)
print(a or b)
print(not a)
```

---

## 6. Numeric Conversions

Python allows explicit conversion between numeric types.

### 6.1 Converting to Integer

```python
int(3.7)
```

Output:

```python
3
```

### 6.2 Converting to Float

```python
float(5)
```

Output:

```python
5.0
```

### 6.3 Converting to Complex

```python
complex(5)
complex(2, 3)
```

### 6.4 Boolean Conversion

```python
bool(0)   # False
bool(1)   # True
```

---

## 7. Numeric Precision

Precision refers to how accurately numbers can be represented.

### 7.1 Integer Precision

Python integers have exact precision.

```python
a = 10**100
print(a)
```

Python handles extremely large numbers correctly.

### 7.2 Floating-Point Precision

Floating-point numbers use binary fractions.

Some decimals cannot be represented exactly.

Example:

```python
0.1 + 0.2
```

Produces:

```python
0.30000000000000004
```

### 7.3 Controlling Precision

For precise decimal arithmetic, Python provides the `decimal` module.

Example:

```python
from decimal import Decimal

a = Decimal("0.1")
b = Decimal("0.2")

print(a + b)
```

Output:

```python
0.3
```

---

## 8. Arbitrary Precision Integers

One of Python's strongest features is arbitrary precision integers.

Unlike languages with fixed integer sizes:

- C `int` → 32 bits
- Java `long` → 64 bits

Python integers can grow as large as memory allows.

Example:

```python
x = 10**1000
print(x)
```

This produces a number with 1001 digits.

Python automatically expands the internal representation.

### 8.1 Internal Representation Concept

Large integers are internally stored as arrays of digits in base `2^30` (or a similar implementation-dependent base).

This allows efficient operations even on very large numbers.

---

## 9. Performance Considerations

Because integers are objects and support arbitrary precision:

- Arithmetic is slightly slower than in low-level languages
- Provides safety and correctness

Floating-point arithmetic is generally faster due to hardware support.

---

## 10. Summary

Python numeric types provide a powerful and flexible system for mathematical operations.

### Key Features

- Unlimited precision integers
- IEEE-754 floating-point numbers
- Built-in complex number support
- Boolean logic integrated with integers
- Automatic numeric conversions

These features make Python suitable for both general programming and advanced numerical computing.

### Further Topics

A deeper continuation of this topic can include:

- Binary representation of numbers
- Floating-point rounding errors in detail
- The `decimal` and `fractions` modules
- Bitwise operations on integers
- Performance characteristics of Python numeric arithmetic
