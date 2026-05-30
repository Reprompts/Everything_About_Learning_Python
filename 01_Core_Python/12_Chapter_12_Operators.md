

# Python Programming: Chapter 12 Operators

Operators are fundamental components of programming languages. In Python, operators allow programs to perform calculations, compare values, manipulate bits, evaluate logical conditions, assign values, and test object relationships.

Operators act on operands (values or variables) and produce a result.

Example:

```python
a = 5
b = 3
c = a + b
```

Here:

* `+` is the operator
* `a` and `b` are operands

Result:

```
8
```

Python provides several categories of operators which are explained in detail below.

---

# 1. Arithmetic Operators

Arithmetic operators perform mathematical calculations. These operators work with numeric types:

* integers
* floats
* complex numbers

---

## 1.1 Addition Operator (+)

The addition operator adds two values.

```python
a = 10
b = 5
result = a + b
print(result)
```

Output:

```
15
```

Works with floats:

```python
x = 3.5
y = 2.2
print(x + y)
```

Output:

```
5.7
```

Works with strings (concatenation):

```python
first = "Hello"
second = "World"
print(first + second)
```

Output:

```
HelloWorld
```

Works with lists:

```python
a = [1,2]
b = [3,4]
print(a + b)
```

Output:

```
[1,2,3,4]
```

---

## 1.2 Subtraction Operator (-)

```python
a = 20
b = 6
print(a - b)
```

Output:

```
14
```

---

## 1.3 Multiplication Operator (*)

```python
a = 4
b = 6
print(a * b)
```

Output:

```
24
```

String repetition:

```python
print("Hi" * 3)
```

Output:

```
HiHiHi
```

List repetition:

```python
print([1,2] * 3)
```

Output:

```
[1,2,1,2,1,2]
```

---

## 1.4 Division Operator (/)

```python
print(10 / 2)
```

Output:

```
5.0
```

```python
print(7 / 2)
```

Output:

```
3.5
```

---

## 1.5 Floor Division (//)

```python
print(7 // 2)
```

Output:

```
3
```

Negative example:

```python
print(-7 // 2)
```

Output:

```
-4
```

---

## 1.6 Modulus Operator (%)

```python
print(7 % 2)
```

Output:

```
1
```

---

## 1.7 Exponentiation Operator (**)

```python
print(2 ** 3)
```

Output:

```
8
```

---

# 2. Comparison Operators

---

## 2.1 Equal (==)

```python
print(5 == 5)
```

Output:

```
True
```

---

## 2.2 Not Equal (!=)

```python
print(5 != 3)
```

Output:

```
True
```

---

## 2.3 Greater Than (>)

```python
print(10 > 5)
```

Output:

```
True
```

---

## 2.4 Less Than (<)

```python
print(3 < 8)
```

Output:

```
True
```

---

## 2.5 Greater Than or Equal (>=)

```python
print(10 >= 10)
```

Output:

```
True
```

---

## 2.6 Less Than or Equal (<=)

```python
print(4 <= 7)
```

Output:

```
True
```

---

# 3. Logical Operators

---

## 3.1 and Operator

```python
x = 10
print(x > 5 and x < 20)
```

Output:

```
True
```

---

## 3.2 or Operator

```python
x = 3
print(x < 5 or x > 10)
```

Output:

```
True
```

---

## 3.3 not Operator

```python
print(not True)
```

Output:

```
False
```

---

# 4. Bitwise Operators

---

## 4.1 Bitwise AND (&)

```
5 & 3 = 1
```

---

## 4.2 Bitwise OR (|)

```
5 | 3 = 7
```

---

## 4.3 Bitwise XOR (^)

```
5 ^ 3 = 6
```

---

## 4.4 Bitwise NOT (~)

```
~5 = -6
```

---

## 4.5 Bit Shifting

### Left Shift (<<)

```
5 << 1 = 10
```

### Right Shift (>>)

```
8 >> 1 = 4
```

---

# 5. Assignment Operators

```python
x = 10
x += 5
x -= 3
x *= 2
x /= 4
x //= 2
x %= 3
x **= 2
```

Example:

```python
x = 5
x *= 3
print(x)
```

Output:

```
15
```

---

# 6. Identity Operators

## 6.1 is

```python
a = [1,2,3]
b = a
print(a is b)
```

Output:

```
True
```

---

## 6.2 is not

```python
a = [1,2,3]
b = [1,2,3]
print(a is not b)
```

Output:

```
True
```

---

# 7. Membership Operators

## 7.1 in

```python
numbers = [1,2,3,4]
print(3 in numbers)
```

Output:

```
True
```

```python
print("py" in "python")
```

Output:

```
True
```

---

## 7.2 not in

```python
print(10 not in numbers)
```

Output:

```
True
```

---

# 8. Operator Precedence

```python
print(2 + 3 * 4)
```

Output:

```
14
```

---

## 8.1 Precedence Order

```
()
**
*, /, //, %
+, -
<< >>
&
^
|
== != > < >= <=
not
and
or
=
```

---

## 8.2 Parentheses Override

```python
print((2 + 3) * 4)
```

Output:

```
20
```

---

# 9. Practical Example

```python
a = 10
b = 3
result = (a + b) * 2

if result > 20 and result % 2 == 0:
    print("Valid result")
```

---

# 10. Summary

Python operators include:

* Arithmetic operators → math operations
* Comparison operators → comparisons
* Logical operators → conditions
* Bitwise operators → binary operations
* Assignment operators → value updates
* Identity operators → object identity
* Membership operators → existence checks
* Operator precedence → execution order

Operators form the foundation of almost every Python expression.

---


