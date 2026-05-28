# OS & SYS Hands-On Tutorial 
## Python OS & SYS: Working with Directories and Paths in Python

In many real-world programs, interacting with the file system is essential. Programs often need to:

* Locate files
* Navigate directories
* Build file paths dynamically
* Extract file names from paths
* Convert relative paths into absolute ones

Python provides powerful tools for working with directories and paths through the `os` module, particularly the `os.getcwd()`, `os.chdir()`, and `os.path` utilities.

These functions make it possible to write programs that work reliably across different operating systems, such as Windows, Linux, and macOS.

---

# Getting the Current Working Directory

The current working directory (CWD) is the directory from which the Python program is currently running.

Many file operations use this directory as their default location.

Python provides the function:

```python id="5rjmx1"
os.getcwd()
```

to retrieve the current working directory.

---

# Example

```python id="r8vjlwm"
import os

current_directory = os.getcwd()
print(current_directory)
```

## Example Output (Windows)

```text id="1n6xg6"
C:\Users\Ganesh\Projects
```

## Example Output (Linux/macOS)

```text id="b1hml2"
/home/ganesh/projects
```

---

# Why It Matters

Knowing the current directory helps when:

* Loading configuration files
* Accessing project resources
* Saving program output
* Debugging file path issues

---

# Changing the Current Working Directory

Sometimes programs need to move into another directory before performing operations.

Python provides the function:

```python id="8nztqp"
os.chdir()
```

to change the working directory.

---

# Example

```python id="m4o0rm"
import os

os.chdir("Documents")
print(os.getcwd())
```

## Output

```text id="0tvh2h"
C:\Users\Ganesh\Documents
```

Now the program operates inside the new directory.

---

# Important Note

If the directory does not exist, Python will raise an error:

```python id="zk04u3"
FileNotFoundError
```

## Example

```python id="ylu9zx"
os.chdir("unknown_folder")
```

---

# Understanding Relative vs Absolute Paths

When working with files, paths can be written in two different forms.

---

# Absolute Paths

An absolute path specifies the complete location of a file starting from the root of the filesystem.

## Example (Windows)

```text id="s64lm4"
C:\Users\Ganesh\Documents\file.txt
```

## Example (Linux/macOS)

```text id="k4lg7h"
/home/ganesh/documents/file.txt
```

Absolute paths always point to the same location regardless of the current directory.

---

# Relative Paths

A relative path specifies a location relative to the current working directory.

## Example

```text id="jofpqz"
Documents/file.txt
```

or

```text id="84as0m"
./file.txt
```

Relative paths depend on where the program is currently running.

---

# Relative Path Symbols

| Symbol     | Meaning             |
| ---------- | ------------------- |
| `.`        | Current directory   |
| `..`       | Parent directory    |
| `/` or `\` | Directory separator |

---

# Example

```text id="qkjb12"
../data/file.txt
```

Meaning:

```text id="r7rb07"
go up one directory → enter data → open file.txt
```

---

# Joining Paths

One common mistake is manually concatenating paths.

## Example of Bad Practice

```python id="6lqv2z"
path = "folder/" + "file.txt"
```

This may break on Windows because Windows uses:

```text id="yijazv"
\
```

instead of:

```text id="2ftjxu"
/
```

---

# Correct Way: `os.path.join()`

Python provides a safe method:

```python id="5y84ru"
os.path.join()
```

which automatically uses the correct separator for the operating system.

---

# Example

```python id="z4k9ld"
import os

path = os.path.join("folder", "file.txt")
print(path)
```

## Output on Windows

```text id="nd2d0g"
folder\file.txt
```

## Output on Linux/macOS

```text id="slb5jv"
folder/file.txt
```

Python automatically chooses the correct separator.

---

# Multiple Path Components

```python id="ch1n0l"
path = os.path.join("home", "ganesh", "documents", "file.txt")
print(path)
```

## Output

```text id="56x02s"
home/ganesh/documents/file.txt
```

---

# Splitting Paths

Sometimes we need to separate a path into its components.

Python provides the function:

```python id="tqazw5"
os.path.split()
```

---

# Example

```python id="7tb6nq"
import os

path = "/home/ganesh/file.txt"

directory, file = os.path.split(path)

print(directory)
print(file)
```

## Output

```text id="m58y6y"
/home/ganesh
```

```text id="we0q4u"
file.txt
```

---

# Explanation

| Variable    | Meaning         |
| ----------- | --------------- |
| `directory` | Folder location |
| `file`      | File name       |

---

# Getting File and Directory Names from Paths

Python provides helper functions to extract specific parts of a path.

---

# Getting the File Name

Use:

```python id="k4wkrg"
os.path.basename()
```

## Example

```python id="whzj0t"
import os

path = "/home/ganesh/file.txt"

print(os.path.basename(path))
```

## Output

```text id="d7hmtt"
file.txt
```

This returns the final part of the path.

---

# Getting the Directory Name

Use:

```python id="wm4p7x"
os.path.dirname()
```

## Example

```python id="2t8ew8"
import os

path = "/home/ganesh/file.txt"

print(os.path.dirname(path))
```

## Output

```text id="5rsyh0"
/home/ganesh
```

This returns the folder containing the file.

---

# Separating File Name and Extension

Another useful function is:

```python id="j6l4yb"
os.path.splitext()
```

---

# Example

```python id="msfr34"
import os

file = "report.pdf"

name, extension = os.path.splitext(file)

print(name)
print(extension)
```

## Output

```text id="7z3h0u"
report
```

```text id="wls2mg"
.pdf
```

---

# Normalizing Paths

Paths sometimes contain redundant or inconsistent components.

## Example

```text id="l15x2m"
folder//subfolder/../file.txt
```

This path can be simplified.

Python provides:

```python id="jmf7gj"
os.path.normpath()
```

---

# Example

```python id="9f3g86"
import os

path = "folder//subfolder/../file.txt"

print(os.path.normpath(path))
```

## Output

```text id="0zwq7n"
folder/file.txt
```

Normalization cleans the path by:

* Removing duplicate separators
* Resolving `.` and `..`

---

# Resolving Absolute Paths

Programs often need to convert relative paths into absolute paths.

Python provides:

```python id="jlwm34"
os.path.abspath()
```

---

# Example

```python id="h2lhx7"
import os

relative_path = "file.txt"

absolute_path = os.path.abspath(relative_path)

print(absolute_path)
```

## Example Output

```text id="t1gqj9"
C:\Users\Ganesh\Projects\file.txt
```

Even though only `file.txt` was provided, Python calculates the full path relative to the current working directory.

---

# Example: Path Processing Program

Below is a simple script demonstrating several path operations.

```python id="4j13hs"
import os

path = "data/report.txt"

print("Current Directory:", os.getcwd())
print("Absolute Path:", os.path.abspath(path))
print("File Name:", os.path.basename(path))
print("Directory:", os.path.dirname(path))
print("Split:", os.path.split(path))
print("Joined Path:", os.path.join("data", "report.txt"))
print("Normalized Path:", os.path.normpath("data//folder/../report.txt"))
```

## Example Output

```text id="3t4w2p"
Current Directory: /home/ganesh/project
Absolute Path: /home/ganesh/project/data/report.txt
File Name: report.txt
Directory: data
Split: ('data', 'report.txt')
Joined Path: data/report.txt
Normalized Path: data/report.txt
```

---

# Best Practices for Path Handling

When writing file-system related code, developers should follow some best practices.

---

# Always Use `os.path.join()`

Avoid manually writing separators.

## Correct

```python id="v9psrm"
os.path.join("folder", "file.txt")
```

---

# Avoid Hardcoding Absolute Paths

Hardcoded paths reduce portability.

## Bad Example

```text id="wppw04"
C:\Users\Ganesh\data.txt
```

Instead, use dynamic paths.

---

# Normalize Paths When Handling User Input

If paths come from user input or external sources, normalization helps prevent errors.

---

# Summary

Working with directories and paths is a fundamental part of file handling and system programming.

Python provides powerful utilities through the `os` module.

## Key Functions

| Task                     | Function             |
| ------------------------ | -------------------- |
| Get current directory    | `os.getcwd()`        |
| Change directory         | `os.chdir()`         |
| Join paths               | `os.path.join()`     |
| Split path               | `os.path.split()`    |
| Get file name            | `os.path.basename()` |
| Get directory name       | `os.path.dirname()`  |
| Split extension          | `os.path.splitext()` |
| Normalize path           | `os.path.normpath()` |
| Convert to absolute path | `os.path.abspath()`  |

Using these functions ensures your Python programs are clean, portable, and safe across different operating systems.

---
