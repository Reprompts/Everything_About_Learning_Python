# Python Programming: Chapter 1 Introduction

# Python Program Execution Internals

## Example: Program for Adding Two Numbers (Complete Step-by-Step Explanation)

---

## 1. Introduction

Python programs do not run directly on the processor like C or C++ programs. Instead, Python follows a two-stage execution model:

1. Source code is compiled into bytecode.
2. Bytecode is executed by the Python interpreter (written in C).

To understand this clearly, we will analyze a simple Python program and follow its execution from start to finish.

---

## 2. The Python Program (Source Code)

Create a file named `add.py`:

```python
a = 10
b = 20
result = a + b
print("Sum is:", result)
```

This file is called the **source code**, which is written in a human-readable form.

---

## 3. Running the Program

To execute the program, use the command:

```bash
python add.py
```

When this command runs, Python does not directly execute the code line by line. Instead, several internal steps occur.

---

## 4. Step 1: Reading the Source File

The Python interpreter first:

- Opens the file `add.py`
- Reads the program as text
- Converts characters into an internal representation

At this stage, the program is still plain text.

---

## 5. Step 2: Lexical Analysis (Tokenization)

Python breaks the program into **tokens**.

Example:

```python
a = 10
```

is divided into:

- Identifier → `a`
- Assignment operator → `=`
- Constant → `10`

This step ensures Python understands the structure of the program.

---

## 6. Step 3: Syntax Analysis (Parsing)

After tokenization, Python checks whether the program follows proper grammar rules.

It builds an **Abstract Syntax Tree (AST)**.

Example (simplified):

```text
Assign
 ├── a
 └── 10
```

If the syntax is incorrect, an error is generated at this stage.

---

## 7. Step 4: Compilation into Bytecode

After building the AST, Python compiles it into bytecode.

Bytecode is:

- A low-level, platform-independent instruction set
- Not readable like Python code
- Not hardware-dependent like machine code

---

## 8. Command to Generate Bytecode Manually

You can manually compile the program using:

```bash
python -m py_compile add.py
```

This generates a file inside a folder called:

```text
__pycache__/
```

Example:

```text
add.cpython-312.pyc
```

This file contains the compiled bytecode.

---

## 9. Step 5: Understanding the Structure of Bytecode

You can view the bytecode using:

```bash
python -m dis add.py
```

Example output (simplified):

```text
LOAD_CONST 10
STORE_NAME a

LOAD_CONST 20
STORE_NAME b

LOAD_NAME a
LOAD_NAME b
BINARY_ADD
STORE_NAME result

LOAD_NAME print
LOAD_CONST "Sum is:"
LOAD_NAME result
CALL_FUNCTION 2

RETURN_VALUE
```

Each line represents a bytecode instruction.

---

## 10. Meaning of Bytecode Instructions

Some important instructions:

| Instruction | Purpose |
|------------|----------|
| `LOAD_CONST` | Load a constant value |
| `STORE_NAME` | Store value in a variable |
| `LOAD_NAME` | Load a variable's value |
| `BINARY_ADD` | Add two values |
| `CALL_FUNCTION` | Call a function |
| `RETURN_VALUE` | End execution |

This bytecode is what Python actually executes.

---

## 11. Step 6: Execution by the Python Virtual Machine (PVM)

The bytecode is not executed directly by the CPU.

Instead, it is executed by a component called the:

**Python Virtual Machine (PVM)**

The PVM:

- Reads bytecode instruction by instruction
- Executes the corresponding operations

---

## 12. Important Concept: Bytecode Is Executed Using C

The Python interpreter (**CPython**) is written in C.

That means:

- Every bytecode instruction has an equivalent implementation in C.
- When Python sees an instruction like `BINARY_ADD`, it runs C code that performs addition.

So the execution flow becomes:

```text
Bytecode
    ↓
C code inside interpreter
    ↓
Machine code
    ↓
CPU execution
```

---

## 13. How the CPU Finally Executes the Program

The interpreter (written in C) is compiled like this:

```text
C code
   ↓
Assembly
   ↓
Machine Code (0s and 1s)
```

So when Python runs a program:

- The CPU is not executing Python directly.
- The CPU is executing the machine code of the interpreter.
- The interpreter reads bytecode and performs the required operations.

---

## 14. Complete Execution Flow

The complete pipeline looks like this:

```text
Python Source Code (.py)
        ↓
Lexical Analysis (Tokens)
        ↓
Parsing (AST)
        ↓
Compilation
        ↓
Bytecode (.pyc)
        ↓
Python Virtual Machine (written in C)
        ↓
Machine Code Execution (by CPU)
        ↓
Output
```

---

## 15. Final Output of the Program

When the program runs, the result printed is:

```text
Sum is: 30
```

This output is produced after:

- Compilation into bytecode
- Interpretation by C code
- Execution by the CPU

---

## 16. Useful Commands Related to This Process

### Run the Python Program

```bash
python add.py
```

### Compile a Single File to Bytecode

```bash
python -m py_compile add.py
```

### Compile All Python Files in the Current Directory

```bash
python -m compileall .
```

### View Bytecode (Disassemble the Program)

```bash
python -m dis add.py
```

### View the Abstract Syntax Tree (AST)

```bash
python -m ast add.py
```

### Run Python in Verbose Mode

```bash
python -v add.py
```

---

## 17. Conclusion

A simple Python program such as adding two numbers goes through several internal steps before execution:

1. The source code is read.
2. The code is tokenized.
3. Python builds an Abstract Syntax Tree (AST).
4. The AST is compiled into bytecode.
5. The Python Virtual Machine executes the bytecode.
6. The interpreter's machine code ultimately runs on the CPU.

### Key Takeaway

Python is often described as both:

- **A compiled language** because Python source code is compiled into bytecode (`.pyc` files).
- **An interpreted language** because the bytecode is executed by the Python Virtual Machine rather than directly by the CPU.

Understanding this execution pipeline provides a strong foundation for learning Python internals, performance optimization, debugging, and advanced programming concepts.
