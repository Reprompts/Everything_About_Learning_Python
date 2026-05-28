# OS & SYS Hands-On Tutorial
## Python OS & SYS: Listing and Traversing Directories in Python

Many Python applications need to inspect and process files within directories. Examples include:

* Searching for specific files
* Processing large datasets stored in folders
* Organizing files automatically
* Building backup tools
* Indexing documents
* Analyzing log files

To accomplish these tasks, Python provides several tools in the `os` module for:

* Listing directory contents
* Iterating through directories
* Traversing directory trees recursively
* Filtering files and folders

These features allow programs to efficiently explore and manage filesystem structures.

---

# Listing Directory Contents

The simplest way to view the contents of a directory is by using:

```python id="wkl94n"
os.listdir()
```

This function returns a list of all files and subdirectories inside a given directory.

---

# Syntax

```python id="p4rvjp"
os.listdir(path)
```

Where:

* `path` is the directory to inspect.

If no path is given, Python lists the current working directory.

---

# Example

```python id="rgc2o4"
import os

files = os.listdir()
print(files)
```

## Example Output

```python id="hvykh2"
['data.txt', 'script.py', 'images', 'notes.md']
```

This output includes:

* Files
* Directories

However, `os.listdir()` does not indicate which entries are files or directories.

---

# Listing a Specific Directory

```python id="x6r4lo"
import os

files = os.listdir("documents")
print(files)
```

## Output

```python id="gl80bo"
['report.pdf', 'data.csv', 'archive']
```

---

# Recursive Directory Traversal

Sometimes we need to explore not just one directory but all of its subdirectories.

This is known as recursive directory traversal.

---

# Example Structure

```text id="18mwna"
project/
    data/
        raw/
            file1.csv
        processed/
            result.csv
    scripts/
        analyze.py
```

A recursive traversal visits every folder and file in the tree.

Python provides tools like:

```python id="qnjm7w"
os.walk()
```

which automatically handles recursion.

---

# Filtering Directory Contents

When listing directory contents, we often need to filter results.

Examples:

* Only files
* Only directories
* Files with specific extensions
* Large files
* Recently modified files

Filtering can be achieved by combining `os.listdir()` with `os.path` functions.

---

# Filtering Files

## Example: Show Only Files

```python id="5r0p5j"
import os

for item in os.listdir():
    if os.path.isfile(item):
        print(item)
```

## Output

```text id="2j0ryx"
script.py
data.txt
notes.md
```

---

# Filtering Directories

## Example: Show Only Folders

```python id="vgbq9q"
import os

for item in os.listdir():
    if os.path.isdir(item):
        print(item)
```

## Output

```text id="7tfguk"
images
archive
```

---

# Filtering by File Extension

## Example: Find All `.txt` Files

```python id="c0q7rl"
import os

for file in os.listdir():
    if file.endswith(".txt"):
        print(file)
```

## Output

```text id="m0u18t"
data.txt
notes.txt
```

This technique is commonly used in data processing pipelines.

---

# Using Directory Iterators

The function `os.listdir()` loads the entire directory list into memory.

For very large directories, a more efficient approach is to use:

```python id="ofncy7"
os.scandir()
```

This function returns an iterator, allowing Python to process entries one at a time.

---

# Syntax

```python id="s6s8h8"
os.scandir(path)
```

---

# Example

```python id="6ohvdz"
import os

with os.scandir(".") as entries:
    for entry in entries:
        print(entry.name)
```

## Example Output

```text id="w0vkmh"
script.py
data.txt
images
logs
```

---

# Checking Entry Type Efficiently

`os.scandir()` provides faster checks for files and directories.

## Example

```python id="aqxk13"
import os

with os.scandir(".") as entries:
    for entry in entries:
        if entry.is_file():
            print("File:", entry.name)

        elif entry.is_dir():
            print("Directory:", entry.name)
```

## Output

```text id="bim0i7"
File: script.py
File: data.txt
Directory: images
Directory: logs
```

This approach is more efficient than `os.listdir()` combined with `os.path` checks.

---

# Walking Through Directory Trees

The most powerful tool for traversing directories is:

```python id="3jxjlwm"
os.walk()
```

This function automatically traverses an entire directory tree recursively.

It visits:

* The root directory
* Every subdirectory
* Every file

---

# Syntax

```python id="4x71u3"
os.walk(top_directory)
```

The function returns three values for each directory:

```python id="u1y32m"
(root, directories, files)
```

Where:

| Value         | Meaning                |
| ------------- | ---------------------- |
| `root`        | Current directory path |
| `directories` | List of subdirectories |
| `files`       | List of files          |

---

# Example

```python id="w0jjlwm"
import os

for root, dirs, files in os.walk("project"):
    print("Directory:", root)
    print("Subdirectories:", dirs)
    print("Files:", files)
    print()
```

## Example Output

```text id="v2tjlwm"
Directory: project
Subdirectories: ['data', 'scripts']
Files: []

Directory: project/data
Subdirectories: ['raw']
Files: ['data.csv']

Directory: project/scripts
Subdirectories: []
Files: ['analyze.py']
```

---

# Processing Files with `os.walk()`

`os.walk()` is often used to process all files in a directory tree.

## Example: Print All File Paths

```python id="8y2qqm"
import os

for root, dirs, files in os.walk("project"):
    for file in files:
        path = os.path.join(root, file)
        print(path)
```

## Output

```text id="83mjlwm"
project/data/data.csv
project/scripts/analyze.py
```

---

# Searching for Specific Files

## Example: Find All `.py` Files

```python id="6jlwm5"
import os

for root, dirs, files in os.walk("."):
    for file in files:
        if file.endswith(".py"):
            print(os.path.join(root, file))
```

## Output

```text id="fjlwm2"
./script.py
./tools/helper.py
```

This technique is used by tools such as:

* Code analyzers
* Linters
* Build systems

---

# Example: Directory Indexing Script

Below is a simple program that lists all files in a directory tree.

```python id="d4k8z1"
import os

start_directory = "project"

for root, dirs, files in os.walk(start_directory):
    for file in files:
        full_path = os.path.join(root, file)
        print(full_path)
```

This script prints the full path of every file under the directory.

---

# Comparing Directory Traversal Methods

| Method         | Purpose                   | Recursion | Memory Efficient |
| -------------- | ------------------------- | --------- | ---------------- |
| `os.listdir()` | List directory contents   | No        | Moderate         |
| `os.scandir()` | Iterate directory entries | No        | Yes              |
| `os.walk()`    | Traverse directory trees  | Yes       | Yes              |

---

# Best Practices for Directory Traversal

When working with large directories, developers should follow several best practices.

---

# Use `os.scandir()` for Performance

It is faster than `os.listdir()` when checking file attributes.

---

# Avoid Hardcoding Paths

Always use:

```python id="6jlwmn"
os.path.join()
```

to build paths safely.

---

# Be Careful with Recursive Traversal

Directory trees may contain thousands of files, so always design traversal logic carefully.

---

# Filter Early

Filtering files early in the traversal improves performance.

## Example

Process only `.csv` files.

---

# Real-World Applications

Directory traversal is used in many real systems.

---

# Examples Include

## Backup Tools

Scanning directories to copy files to backup locations.

---

## Data Processing Pipelines

Processing datasets stored across nested folders.

---

## Log Analysis

Scanning log directories to extract information.

---

## Search Engines

Indexing documents across folder structures.

---

# Summary

Python provides several powerful tools for listing and traversing directories.

## Key Techniques

| Task                          | Function           |
| ----------------------------- | ------------------ |
| List directory contents       | `os.listdir()`     |
| Efficient directory iteration | `os.scandir()`     |
| Recursive directory traversal | `os.walk()`        |
| Filter files                  | `os.path.isfile()` |
| Filter directories            | `os.path.isdir()`  |

Using these tools, developers can build programs that explore and manage complex directory structures efficiently.

---
