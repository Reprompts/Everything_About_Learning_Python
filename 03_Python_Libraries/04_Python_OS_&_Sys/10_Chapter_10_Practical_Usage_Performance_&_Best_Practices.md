# OS & SYS Hands-On Tutorial 
## Python OS & SYS: Practical Usage, Performance, and Best Practices with os and sys

Python's `os` and `sys` modules are powerful tools for interacting with the operating system and runtime environment. While earlier sections explain their individual features, real-world software development requires understanding how to apply them effectively in practical scenarios.

These modules are widely used in:

- command-line utilities
- automation and DevOps scripts
- system administration tools
- file processing pipelines
- deployment and build systems

This article explores practical usage patterns, performance strategies, and best practices when using `os` and `sys`.

---

# Building Command Line Tools

One of the most common uses of `sys` is creating command-line applications (CLI tools).

CLI programs allow users to interact with software using terminal commands and arguments.

Examples of popular CLI tools written in Python:

- `pip`
- `django-admin`
- `black`
- `pytest`

These tools rely heavily on:

- `sys.argv`
- `sys.exit()`
- `sys.stdout`
- `sys.stderr`

---

# Basic CLI Tool Example

```python
import sys

if len(sys.argv) < 2:
    print("Usage: python greet.py <name>")
    sys.exit(1)

name = sys.argv[1]
print("Hello", name)
````

Run the program:

```bash
python greet.py Alice
```

Output:

```text
Hello Alice
```

---

# Handling Multiple Arguments

CLI tools often accept multiple arguments.

```python
import sys

print("Arguments received:")

for arg in sys.argv[1:]:
    print(arg)
```

Example:

```bash
python script.py apple banana mango
```

Output:

```text
apple
banana
mango
```

---

# Returning Exit Codes

Command-line tools should return meaningful exit codes.

* `0` → success
* `1` → error

Example:

```python
import sys

try:
    number = int(sys.argv[1])

except:
    print("Invalid number")
    sys.exit(1)

print("Number:", number)

sys.exit(0)
```

Exit codes help scripts and automation systems detect failures.

---

# Writing Automation Scripts

Python is widely used for automation because it can easily interact with the operating system.

Automation scripts may perform tasks such as:

* organizing files
* monitoring system status
* scheduling backups
* running system commands
* processing logs

---

# Example: Automated Directory Cleanup

```python
import os

folder = "logs"

for file in os.listdir(folder):
    path = os.path.join(folder, file)

    if os.path.isfile(path):
        os.remove(path)

print("Log files removed")
```

This script automatically removes all files in a directory.

---

# Example: Batch File Renaming

```python
import os

folder = "images"

for i, file in enumerate(os.listdir(folder)):
    old_path = os.path.join(folder, file)
    new_path = os.path.join(folder, f"image_{i}.jpg")

    os.rename(old_path, new_path)
```

Automation scripts like this are commonly used for data processing pipelines.

---

# Managing Files and Directories Programmatically

Programs often need to create, delete, move, and inspect files.

The `os` module provides many functions for these operations.

---

# Creating Directory Structures

```python
import os

os.makedirs("project/data/raw", exist_ok=True)
```

This creates nested directories.

---

# Moving Files

```python
import os
import shutil

shutil.move("data.txt", "archive/data.txt")
```

---

# Copying Files

```python
import shutil

shutil.copy("file.txt", "backup/file.txt")
```

---

# Checking File Existence

```python
import os

if os.path.exists("data.txt"):
    print("File exists")
```

---

# Example: Organizing Files by Type

```python
import os
import shutil

for file in os.listdir():

    if file.endswith(".jpg"):
        shutil.move(file, "images/")

    elif file.endswith(".txt"):
        shutil.move(file, "documents/")
```

This type of script is useful for large data folders.

---

# Handling Exceptions and Runtime Information

When interacting with files and operating systems, many things can go wrong:

* file not found
* permission denied
* disk full
* invalid path

Programs must handle these cases safely.

---

# Example: Safe File Reading

```python
import os

try:
    with open("data.txt") as f:
        content = f.read()

except FileNotFoundError:
    print("File not found")

except PermissionError:
    print("Permission denied")
```

---

# Logging Errors to stderr

Error messages should go to `stderr`.

```python
import sys

try:
    open("data.txt")

except Exception as e:
    sys.stderr.write(str(e) + "\n")
```

This helps separate normal output from errors.

---

# Security Considerations

Programs interacting with the operating system must be designed with security in mind.

Poor system programming practices can lead to vulnerabilities.

---

# File Permission Awareness

Before accessing files, programs should verify permissions.

```python
import os

if os.access("secret.txt", os.R_OK):
    print("File can be read")
```

Permission flags include:

| Flag      | Meaning            |
| --------- | ------------------ |
| `os.R_OK` | Read permission    |
| `os.W_OK` | Write permission   |
| `os.X_OK` | Execute permission |

---

# Avoid Unsafe System Commands

Running shell commands using user input can cause command injection attacks.

Unsafe example:

```python
os.system("rm " + user_input)
```

If `user_input` is malicious, it could execute dangerous commands.

Safe approach:

Use `subprocess` with argument lists.

Example:

```python
import subprocess

subprocess.run(["rm", filename])
```

This prevents shell injection.

---

# Validate Paths

Programs should sanitize paths before using them.

Example:

```python
import os

path = os.path.abspath(user_input)
```

This ensures the path is properly resolved.

---

# Performance Optimization for File Operations

File operations can become slow when dealing with large datasets or directories.

Efficient techniques improve performance significantly.

---

# Use Directory Iterators Instead of Large Lists

`os.listdir()` loads all filenames into memory.

Better approach:

`os.scandir()`

Example:

```python
import os

for entry in os.scandir("data"):

    if entry.is_file():
        print(entry.name)
```

Benefits:

* faster
* lower memory usage

---

# Efficient File Reading

Avoid loading large files entirely into memory.

Bad approach:

```python
data = open("large.txt").read()
```

Better approach:

```python
with open("large.txt") as f:

    for line in f:
        process(line)
```

This reads the file line by line.

---

# Avoid Repeated Disk Access

Cache file paths or results when possible.

Example:

```python
files = os.listdir("data")

for f in files:
    process(f)
```

This avoids repeated disk operations.

---

# Writing Cross Platform Code

Programs should work across:

* Windows
* Linux
* macOS

Different operating systems use different path formats.

Example:

* Windows: `C:\Users\Alice\file.txt`
* Linux: `/home/alice/file.txt`

---

# Use os.path Instead of Hardcoded Paths

Bad:

```python
"C:/Users/data/file.txt"
```

Better:

```python
os.path.join("data", "file.txt")
```

---

# Detecting Operating System

```python
import os

print(os.name)
```

Possible values:

| OS            | Value   |
| ------------- | ------- |
| Windows       | `nt`    |
| Linux / macOS | `posix` |

Example usage:

```python
if os.name == "nt":
    print("Running on Windows")
```

---

# Safe Resource Handling

Programs should properly manage resources such as:

* files
* directories
* memory
* processes

---

# Using Context Managers

The `with` statement ensures files are closed automatically.

Bad:

```python
f = open("data.txt")
data = f.read()
f.close()
```

Better:

```python
with open("data.txt") as f:
    data = f.read()
```

This prevents resource leaks.

---

# Cleaning Temporary Resources

Temporary files should be deleted after use.

Example:

```python
import tempfile

with tempfile.NamedTemporaryFile() as temp:
    temp.write(b"Temporary data")
```

The file is automatically removed afterward.

---

# Designing Robust Scripts

Professional system scripts should follow these principles:

* validate inputs
* handle errors gracefully
* log useful information
* avoid unsafe commands
* clean up resources
* maintain portability

---

# Real-World Example: File Organizer Tool

```python
import os
import shutil

for file in os.listdir():

    if os.path.isfile(file):

        ext = file.split(".")[-1]
        folder = ext + "_files"

        os.makedirs(folder, exist_ok=True)

        shutil.move(file, os.path.join(folder, file))
```

This tool automatically organizes files by extension.

Example result:

```text
images_files/
documents_files/
videos_files/
```

---

# Summary

Using `os` and `sys`, Python programs can interact deeply with the operating system and runtime environment.

Key practical applications include:

| Area                 | Example Use                         |
| -------------------- | ----------------------------------- |
| CLI Tools            | Processing command line arguments   |
| Automation           | Batch file operations               |
| System Utilities     | Monitoring processes                |
| File Management      | Creating and organizing directories |
| DevOps Scripts       | Running system commands             |
| Cross Platform Tools | Writing OS-independent scripts      |

---

# Best Practices

Best practices include:

* validating user input
* avoiding unsafe system commands
* using efficient file operations
* handling exceptions properly
* writing cross-platform code
* managing resources safely

These practices ensure that Python programs are secure, efficient, portable, and reliable when interacting with the system environment.

--- 
