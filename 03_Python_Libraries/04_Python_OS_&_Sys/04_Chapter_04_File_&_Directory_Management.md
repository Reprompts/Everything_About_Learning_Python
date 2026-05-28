# OS & SYS Hands-On Tutorial
## Python OS & SYS: File and Directory Management in Python

File and directory management is a fundamental part of system programming and automation. Many Python programs need to:

* Create folders for storing data
* Remove temporary files
* Move files between directories
* Rename files after processing
* Check if files or folders already exist

Python provides built-in support for these operations through the `os` module. This module acts as an interface between Python and the operating system's filesystem.

With the `os` module, developers can safely perform file system operations in a cross-platform manner, meaning the same code works on Windows, Linux, and macOS.

---

# Creating Directories

Programs often need to create directories for storing output files, logs, or user data.

Python provides the function:

```python id="jlwmu4"
os.mkdir()
```

to create a single directory.

---

# Syntax

```python id="cmn1gh"
os.mkdir(path)
```

Where:

* `path` is the name or location of the directory to be created.

---

# Example

```python id="m3azn4"
import os

os.mkdir("new_folder")
```

This creates a directory named:

```text id="r8pt9m"
new_folder
```

in the current working directory.

---

# Example with Absolute Path

```python id="1u9n4k"
import os

os.mkdir("C:/Users/Ganesh/Documents/project")
```

This creates a folder in the specified location.

---

# Important Notes

If the directory already exists, Python raises:

```python id="j4rsk0"
FileExistsError
```

If the parent directory does not exist, the operation fails.

---

# Creating Nested Directories

Sometimes programs need to create multiple levels of directories at once.

## Example Structure

```text id="0mgbbi"
project/
    data/
        raw/
```

Creating this structure with `mkdir()` would require creating each folder separately.

Python solves this using:

```python id="m7rf7m"
os.makedirs()
```

---

# Syntax

```python id="48mzql"
os.makedirs(path)
```

This function creates all intermediate directories automatically.

---

# Example

```python id="s6yxx9"
import os

os.makedirs("project/data/raw")
```

## Result

```text id="42e1om"
project/
    data/
        raw/
```

All folders are created in one command.

---

# Avoiding Errors if Directory Exists

You can prevent errors by using:

```python id="jvjxmq"
os.makedirs("project/data/raw", exist_ok=True)
```

This means:

* Create directories only if they do not exist

---

# Removing Directories

Python allows directories to be deleted using:

```python id="yz2zw7"
os.rmdir()
```

---

# Syntax

```python id="5v41of"
os.rmdir(path)
```

However, this function only works if the directory is empty.

---

# Example

```python id="0ltpt6"
import os

os.rmdir("old_folder")
```

This removes the directory:

```text id="hns4ks"
old_folder
```

---

# Example Error

If the folder contains files:

```python id="jsrrqz"
OSError: Directory not empty
```

In such cases, you must delete the contents first or use the `shutil` module.

---

# Removing Files

To delete files, Python provides the function:

```python id="cdt6u1"
os.remove()
```

---

# Syntax

```python id="7j8n3w"
os.remove(path)
```

---

# Example

```python id="o1w34w"
import os

os.remove("data.txt")
```

This deletes the file:

```text id="gsv2ek"
data.txt
```

---

# Example with Path

```python id="9lnz0w"
import os

os.remove("documents/report.pdf")
```

---

# Important Note

If the file does not exist, Python raises:

```python id="1fqsd8"
FileNotFoundError
```

Therefore, it is good practice to check before deleting.

---

# Example

```python id="n48sdo"
import os

if os.path.exists("data.txt"):
    os.remove("data.txt")
```

---

# Renaming Files and Directories

Files and directories can be renamed using:

```python id="2xk3fy"
os.rename()
```

---

# Syntax

```python id="pv5j2j"
os.rename(old_name, new_name)
```

---

# Example: Renaming a File

```python id="38kk4f"
import os

os.rename("old.txt", "new.txt")
```

## Result

```text id="df3lg4"
old.txt → new.txt
```

---

# Example: Renaming a Directory

```python id="1g7c9q"
import os

os.rename("data", "dataset")
```

## Result

```text id="ks1h0x"
data → dataset
```

---

# Moving Files and Directories

The `os.rename()` function can also move files between directories.

---

# Example

```python id="lu5fg7"
import os

os.rename("report.txt", "archive/report.txt")
```

## Result

```text id="d4tlgr"
report.txt → archive/report.txt
```

This effectively moves the file to another folder.

---

# Limitation

`os.rename()` may fail when moving files across different file systems.

For more advanced moving operations, developers often use the `shutil.move()` function.

---

# Example

```python id="06lhx6"
import shutil

shutil.move("report.txt", "archive/")
```

---

# Checking if File or Directory Exists

Before performing file operations, programs often need to verify whether a file or directory exists.

Python provides the function:

```python id="txi6mu"
os.path.exists()
```

---

# Syntax

```python id="chf07q"
os.path.exists(path)
```

## Returns

* `True` → if file or directory exists
* `False` → if it does not exist

---

# Example

```python id="xuh5a5"
import os

if os.path.exists("data.txt"):
    print("File exists")
else:
    print("File does not exist")
```

## Output

```text id="fxab6u"
File exists
```

---

# Checking File vs Directory

Sometimes it is necessary to distinguish between files and directories.

Python provides two useful functions.

---

# Checking if Path is a File

```python id="j7xjlwm"
os.path.isfile()
```

## Example

```python id="hcx7x8"
import os

print(os.path.isfile("data.txt"))
```

## Output

```text id="mndg62"
True
```

---

# Checking if Path is a Directory

```python id="dzw0um"
os.path.isdir()
```

## Example

```python id="9xbh04"
import os

print(os.path.isdir("documents"))
```

## Output

```text id="0a3uwt"
True
```

---

# Example: File Management Script

Below is a small script demonstrating several file and directory operations.

```python id="l0a5xv"
import os

# create directory
os.makedirs("project/data", exist_ok=True)

# create file
with open("project/data/file.txt", "w") as f:
    f.write("Hello World")

# check existence
if os.path.exists("project/data/file.txt"):
    print("File exists")

# rename file
os.rename(
    "project/data/file.txt",
    "project/data/new_file.txt"
)

# remove file
os.remove("project/data/new_file.txt")

# remove directory
os.rmdir("project/data")
```

---

# This Script Performs the Following Operations

* Creates directories
* Creates a file
* Checks file existence
* Renames the file
* Deletes the file
* Removes the directory

---

# Best Practices for File Management

When working with file systems, it is important to follow safe practices.

---

# Always Check Existence Before Deleting

Deleting a non-existent file will cause errors.

## Correct Approach

```python id="x7lgmj"
if os.path.exists(file):
    os.remove(file)
```

---

# Use `os.makedirs()` for Nested Directories

This simplifies directory creation and avoids errors.

---

# Avoid Hardcoded Paths

Use:

```python id="skvl7q"
os.path.join()
```

instead of manually writing paths.

---

# Handle Exceptions

Wrap file operations in `try-except` blocks when writing production code.

## Example

```python id="y3hjlwm"
try:
    os.remove("data.txt")

except FileNotFoundError:
    print("File not found")
```

---

# Summary

Python provides powerful tools for managing files and directories using the `os` module.

## Key Operations

| Operation                 | Function                         |
| ------------------------- | -------------------------------- |
| Create directory          | `os.mkdir()`                     |
| Create nested directories | `os.makedirs()`                  |
| Remove directory          | `os.rmdir()`                     |
| Remove file               | `os.remove()`                    |
| Rename file/directory     | `os.rename()`                    |
| Move files                | `os.rename()` or `shutil.move()` |
| Check existence           | `os.path.exists()`               |
| Check file                | `os.path.isfile()`               |
| Check directory           | `os.path.isdir()`                |

These functions allow Python programs to automate filesystem operations efficiently and safely across operating systems.

---
