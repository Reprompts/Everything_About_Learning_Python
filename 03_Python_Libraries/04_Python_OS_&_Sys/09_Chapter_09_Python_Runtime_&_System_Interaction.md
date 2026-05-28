# OS & SYS Hands-On tutorial 
## Python OS & SYS: Python Runtime and System Interaction (`sys` Module)

The `sys` module is one of Python's most fundamental modules for interacting with the Python runtime environment and interpreter.

While the `os` module focuses on interacting with the operating system, the `sys` module focuses on Python itself — how the interpreter runs, how programs receive input, how modules are loaded, and how programs terminate.

The `sys` module allows developers to:

* Access command-line arguments
* Inspect the Python interpreter version
* Work with standard input/output streams
* Control the module import search path
* Terminate programs gracefully
* Inspect and modify runtime limits and memory behavior

Because of this, `sys` is widely used in:

* Command-line tools
* Scripting environments
* System utilities
* Debugging tools
* Package management systems

To use it, the module must first be imported.

```python id="w7r2pa"
import sys
```

---

# Command Line Arguments (`sys.argv`)

When Python programs are executed from the command line, additional values can be passed to the program. These values are called **command-line arguments**.

The `sys.argv` variable stores these arguments.

```python id="c3t1de"
sys.argv
```

It is a list of strings where:

| Index     | Meaning              |
| --------- | -------------------- |
| `argv[0]` | Script name          |
| `argv[1]` | First argument       |
| `argv[2]` | Second argument      |
| `...`     | Additional arguments |

---

# Example Program

```python id="k2x9bv"
import sys

print("Arguments:", sys.argv)
```

Run the program like this:

```bash id="j4m8nw"
python script.py apple banana mango
```

## Output

```text id="x5l1sz"
Arguments: ['script.py', 'apple', 'banana', 'mango']
```

---

# Example: Simple Calculator

```python id="h8v0rq"
import sys

num1 = int(sys.argv[1])
num2 = int(sys.argv[2])

print("Sum:", num1 + num2)
```

Run:

```bash id="r6c3eu"
python calc.py 10 20
```

## Output

```text id="f2a7mp"
Sum: 30
```

---

# Important Notes

Command-line arguments are always strings, so they often need conversion:

* `int()`
* `float()`

Programs should also validate argument counts:

```python id="m9z4ka"
if len(sys.argv) < 3:
    print("Usage: python calc.py num1 num2")
```

---

# Python Version and Interpreter Details

The `sys` module provides information about the Python interpreter currently running the program.

This information is helpful for:

* Debugging
* Compatibility checks
* Environment validation

---

# Python Version

```python id="g7q8dn"
import sys

print(sys.version)
```

## Example Output

```text id="z3k9vt"
3.12.2 (main, Jan 10 2026, 10:00:00)
[GCC 11.3.0]
```

This contains:

* Python version number
* Build information
* Compiler used

---

# Structured Version Information

A structured version tuple is available:

```python id="v1s6pc"
print(sys.version_info)
```

## Example Output

```text id="n5e2ly"
sys.version_info(major=3, minor=12, micro=2, releaselevel='final', serial=0)
```

You can check versions programmatically:

```python id="q4u7jm"
if sys.version_info.major < 3:
    print("Python 3 required")
```

---

# Python Interpreter Location

```python id="d8f0hr"
print(sys.executable)
```

## Example

```text id="y6w3cq"
/usr/bin/python3
```

This shows the path to the Python interpreter running the program.

---

# Standard Input, Output, and Error Streams

Every program interacts with three standard streams.

| Stream   | Purpose        |
| -------- | -------------- |
| `stdin`  | Input stream   |
| `stdout` | Normal output  |
| `stderr` | Error messages |

These streams are accessible through the `sys` module.

---

# Standard Input (`sys.stdin`)

This represents input from the keyboard or redirected input.

## Example

```python id="a9j5mk"
import sys

data = sys.stdin.readline()

print("You entered:", data)
```

Example usage:

```bash id="p2x6dv"
echo Hello | python program.py
```

## Output

```text id="w1r4gn"
You entered: Hello
```

---

# Standard Output (`sys.stdout`)

This represents normal program output.

The `print()` function internally writes to `sys.stdout`.

You can write directly:

```python id="c8u2ze"
import sys

sys.stdout.write("Hello from stdout\n")
```

---

# Standard Error (`sys.stderr`)

Error messages should be sent to `stderr`.

## Example

```python id="s4f7ly"
import sys

sys.stderr.write("This is an error message\n")
```

Using `stderr` is important for:

* Debugging
* Logging
* Separating output from errors

Many CLI tools rely on this separation.

---

# Redirecting Streams

Streams can be redirected.

## Example

```python id="u3m8ta"
import sys

sys.stdout = open("output.txt", "w")

print("This goes into the file")
```

Output will be saved to `output.txt` instead of the terminal.

---

# Python Module Search Path (`sys.path`)

When Python imports a module, it searches through a list of directories.

This search path is stored in:

```python id="t6b1wr"
sys.path
```

---

# Viewing the Module Search Path

```python id="j7n0ce"
import sys

for path in sys.path:
    print(path)
```

## Example Output

```text id="o4y9ps"
/home/user/project
/usr/lib/python3.12
/usr/lib/python3.12/site-packages
```

Python searches these directories in order.

---

# Adding a Custom Module Path

Sometimes programs need to import modules from custom directories.

## Example

```python id="l5v3kx"
import sys

sys.path.append("/home/user/mylibraries")
```

After this, Python can import modules from that directory.

## Example

```python id="r9d6uq"
import mymodule
```

---

# Temporary vs Permanent Changes

* `sys.path.append()` affects only the current program
* Environment variables or installation directories affect all programs

---

# Program Termination (`sys.exit`)

Python programs sometimes need to stop execution intentionally.

The function used for this is:

```python id="k1w7fe"
sys.exit()
```

---

# Basic Example

```python id="e4t9qs"
import sys

print("Program started")

sys.exit()

print("This will never run")
```

## Output

```text id="h6x2jm"
Program started
```

---

# Exit with Status Code

Programs can return exit codes to the operating system.

```python id="m3y8np"
sys.exit(0)
```

Meaning:

* `0` → success
* Non-zero → error

## Example

```python id="f9v2lu"
import sys

if len(sys.argv) < 2:
    print("Missing arguments")
    sys.exit(1)
```

Exit code `1` indicates failure.

---

# Memory Usage and Recursion Limits

Python protects programs from infinite recursion by setting a maximum recursion depth.

This limit prevents stack overflow crashes.

---

# Viewing Recursion Limit

```python id="n7r5xa"
import sys

print(sys.getrecursionlimit())
```

## Typical Output

```text id="q2k4mv"
1000
```

This means Python allows `1000` nested function calls.

---

# Changing Recursion Limit

If necessary, the recursion limit can be modified.

```python id="z8u1cb"
import sys

sys.setrecursionlimit(2000)
```

However, increasing this limit too much may cause program crashes due to stack overflow.

---

# Example: Recursion Demonstration

```python id="b4m7wp"
import sys

print("Current limit:", sys.getrecursionlimit())

def recursive(n):
    print(n)
    recursive(n + 1)

recursive(1)
```

Eventually this will produce:

```text id="x9c5le"
RecursionError: maximum recursion depth exceeded
```

---

# Checking Object Memory Size

The `sys` module can also estimate memory usage of objects.

```python id="w2f8dr"
import sys

x = [1, 2, 3, 4, 5]

print(sys.getsizeof(x))
```

## Example Output

```text id="t5p1yv"
120
```

This value represents memory used by the object in bytes.

However, it does not include memory used by referenced objects inside containers.

---

# Practical Example: CLI Tool Using `sys`

## Example Script

```python id="y8m6nk"
import sys

if len(sys.argv) < 2:
    print("Usage: python greet.py name")
    sys.exit(1)

name = sys.argv[1]

sys.stdout.write("Hello " + name + "\n")
```

Run:

```bash id="u1d4ge"
python greet.py Alice
```

## Output

```text id="c7r9lx"
Hello Alice
```

---

# Common `sys` Module Attributes

| Attribute / Function      | Purpose                    |
| ------------------------- | -------------------------- |
| `sys.argv`                | Command-line arguments     |
| `sys.version`             | Python version             |
| `sys.version_info`        | Structured version details |
| `sys.executable`          | Python interpreter path    |
| `sys.stdin`               | Standard input stream      |
| `sys.stdout`              | Standard output stream     |
| `sys.stderr`              | Standard error stream      |
| `sys.path`                | Module search path         |
| `sys.exit()`              | Terminate program          |
| `sys.getrecursionlimit()` | Maximum recursion depth    |
| `sys.setrecursionlimit()` | Change recursion limit     |
| `sys.getsizeof()`         | Object memory size         |

---

# Real-World Applications

The `sys` module is widely used in professional Python environments.

---

# Command-Line Tools

Tools like:

* `pip`
* `black`
* `pytest`
* `django-admin`

rely heavily on `sys.argv`.

---

# Environment Detection

Programs check Python version:

```python id="d3k7qw"
sys.version_info
```

to ensure compatibility.

---

# Module Loading Systems

Package managers modify:

```python id="m8t2vc"
sys.path
```

to locate installed packages.

---

# Debugging and Logging

Error messages often use:

```python id="r4p9xb"
sys.stderr
```

to separate errors from normal output.

---

# Summary

The `sys` module provides essential access to the Python runtime environment and interpreter behavior.

It allows programs to:

* Process command-line arguments
* Inspect the Python interpreter
* Manage standard input/output streams
* Control the module import system
* Terminate execution safely
* Inspect recursion and memory behavior

These features make `sys` a critical module for building command-line applications, debugging tools, and system utilities in Python.

---
