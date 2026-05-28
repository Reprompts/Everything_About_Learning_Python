# OS & SYS Hands-On Tutorial 
## Python OS & SYS: File Information, Metadata, and Permissions in Python

When working with files, programs often need more than just the file contents. They may also need information about the file itself, such as:

* File size
* File type
* Creation and modification time
* Access permissions
* Ownership and metadata

This information is known as **file metadata**.

Python provides several functions in the `os` module and `os.path` utilities that allow programs to retrieve and manipulate file metadata and permissions.

Understanding these capabilities is essential for building file management tools, backup utilities, logging systems, and system administration scripts.

---

# Getting File Size

One of the most common metadata operations is retrieving the size of a file.

Python provides the function:

```python
os.path.getsize()
```

This function returns the size of a file in bytes.

---

# Syntax

```python
os.path.getsize(path)
```

Where:

* `path` is the location of the file.

---

# Example

```python
import os

size = os.path.getsize("data.txt")
print(size)
```

## Example Output

```text
2048
```

This means the file size is `2048 bytes`.

---

# Converting File Size to Human-Readable Format

Sometimes it is useful to convert bytes into larger units such as KB, MB, or GB.

## Example

```python
import os

size = os.path.getsize("data.txt")

print("Size in KB:", size / 1024)
```

## Output

```text
Size in KB: 2.0
```

---

# File Creation, Modification, and Access Time

Every file has timestamps that record when the file was created, modified, or accessed.

These timestamps are part of the file's metadata.

Python provides functions to retrieve these values.

---

# Getting File Creation Time

Use:

```python
os.path.getctime()
```

## Example

```python
import os
import time

ctime = os.path.getctime("data.txt")

print(time.ctime(ctime))
```

## Example Output

```text
Mon Mar 18 14:22:05 2024
```

### Explanation

* `getctime()` returns a timestamp value
* `time.ctime()` converts it into readable date and time

---

# Getting Last Modification Time

The modification time indicates when the file was last changed.

Use:

```python
os.path.getmtime()
```

## Example

```python
import os
import time

mtime = os.path.getmtime("data.txt")

print(time.ctime(mtime))
```

## Output

```text
Tue Mar 19 09:10:12 2024
```

---

# Getting Last Access Time

The access time records the last time the file was opened or read.

Use:

```python
os.path.getatime()
```

## Example

```python
import os
import time

atime = os.path.getatime("data.txt")

print(time.ctime(atime))
```

## Output

```text
Tue Mar 19 10:45:33 2024
```

---

# Using `os.stat()` for Complete File Metadata

Instead of calling multiple functions, Python provides a single function:

```python
os.stat()
```

which returns a detailed object containing file metadata.

---

# Example

```python
import os

info = os.stat("data.txt")

print(info)
```

## Example Output

```python
os.stat_result(
    st_mode=33188,
    st_ino=123456,
    st_dev=2050,
    st_nlink=1,
    st_uid=1000,
    st_gid=1000,
    st_size=2048,
    st_atime=1710831000,
    st_mtime=1710828000,
    st_ctime=1710827000
)
```

---

# Important Fields

| Field      | Meaning                   |
| ---------- | ------------------------- |
| `st_size`  | File size                 |
| `st_atime` | Last access time          |
| `st_mtime` | Last modification time    |
| `st_ctime` | Creation time             |
| `st_mode`  | File type and permissions |

---

# Checking File Types

Programs often need to determine whether a path refers to:

* A regular file
* A directory
* A symbolic link

Python provides several helper functions.

---

# Checking If Path Is a File

Use:

```python
os.path.isfile()
```

## Example

```python
import os

print(os.path.isfile("data.txt"))
```

## Output

```text
True
```

---

# Checking If Path Is a Directory

Use:

```python
os.path.isdir()
```

## Example

```python
import os

print(os.path.isdir("documents"))
```

## Output

```text
True
```

---

# Checking If Path Is a Symbolic Link

Use:

```python
os.path.islink()
```

## Example

```python
import os

print(os.path.islink("shortcut"))
```

---

# Understanding File Permissions

File permissions control who can read, write, or execute a file.

Most operating systems use permission bits to represent access rights.

---

# Typical Permission Types

| Permission    | Meaning             |
| ------------- | ------------------- |
| Read (`r`)    | View file contents  |
| Write (`w`)   | Modify file         |
| Execute (`x`) | Run file as program |

Permissions are usually defined for:

* Owner
* Group
* Others

---

# Example Unix-Style Permission

```text
-rwxr-xr--
```

## Meaning

* Owner → read, write, execute
* Group → read, execute
* Others → read

---

# Changing File Permissions

Python allows programs to modify file permissions using:

```python
os.chmod()
```

---

# Syntax

```python
os.chmod(path, mode)
```

Where:

* `path` is the file
* `mode` specifies the new permissions

---

# Example

```python
import os

os.chmod("script.py", 0o755)
```

This sets the permission to:

```text
rwxr-xr-x
```

## Meaning

* Owner → read write execute
* Group → read execute
* Others → read execute

---

# Understanding Permission Numbers

Permissions are often expressed using octal numbers.

## Example

```text
755
```

### Breakdown

| Digit | Permission (Symbolic) | Meaning                |
| ----- | --------------------- | ---------------------- |
| 7     | `rwx`                 | read + write + execute |
| 6     | `rw-`                 | read + write           |
| 5     | `r-x`                 | read + execute         |
| 4     | `r--`                 | read                   |

---

# Working with Permission Flags

Python provides constants in the `stat` module that represent permission flags.

This makes code more readable.

---

# Example

```python
import os
import stat

os.chmod(
    "script.py",
    stat.S_IRUSR | stat.S_IWUSR | stat.S_IXUSR
)
```

---

# Explanation

| Flag           | Meaning       |
| -------------- | ------------- |
| `stat.S_IRUSR` | Owner read    |
| `stat.S_IWUSR` | Owner write   |
| `stat.S_IXUSR` | Owner execute |

---

# Common Permission Flags

| Flag           | Meaning       |
| -------------- | ------------- |
| `stat.S_IRUSR` | Owner read    |
| `stat.S_IWUSR` | Owner write   |
| `stat.S_IXUSR` | Owner execute |
| `stat.S_IRGRP` | Group read    |
| `stat.S_IROTH` | Others read   |

These flags can be combined using the bitwise OR operator (`|`).

---

# Example: File Metadata Inspector

Below is a simple program that prints file information and metadata.

```python
import os
import time

file = "data.txt"

print("Size:", os.path.getsize(file), "bytes")
print("Created:", time.ctime(os.path.getctime(file)))
print("Modified:", time.ctime(os.path.getmtime(file)))
print("Accessed:", time.ctime(os.path.getatime(file)))

print("Is File:", os.path.isfile(file))
print("Is Directory:", os.path.isdir(file))
```

## Example Output

```text
Size: 2048 bytes
Created: Mon Mar 18 14:22:05 2024
Modified: Tue Mar 19 09:10:12 2024
Accessed: Tue Mar 19 10:45:33 2024
Is File: True
Is Directory: False
```

---

# Real-World Applications

File metadata and permissions are used in many real systems.

---

# Backup Tools

Backup programs detect file modification times to determine which files changed.

---

# Security Systems

Permissions control who can access sensitive data.

---

# Logging Systems

Applications store metadata such as timestamps to track activity.

---

# File Monitoring

Programs monitor directories and detect new or modified files.

---

# Best Practices

When working with file metadata and permissions, developers should follow several best practices.

---

# Always Verify File Existence

Before accessing metadata:

```python
if os.path.exists(file):
```

---

# Avoid Changing Permissions Unnecessarily

Improper permission changes may create security risks.

---

# Use the `stat` Module for Clarity

Permission flags improve readability compared to raw numbers.

---

# Summary

Python provides powerful tools for working with file metadata and permissions.

## Key Operations

| Task                  | Function             |
| --------------------- | -------------------- |
| Get file size         | `os.path.getsize()`  |
| Get creation time     | `os.path.getctime()` |
| Get modification time | `os.path.getmtime()` |
| Get access time       | `os.path.getatime()` |
| Get full metadata     | `os.stat()`          |
| Check file type       | `os.path.isfile()`   |
| Check directory       | `os.path.isdir()`    |
| Change permissions    | `os.chmod()`         |
| Use permission flags  | `stat` module        |

These tools allow developers to build advanced filesystem tools, automation scripts, and system utilities that can analyze and control files safely and efficiently.

---
