# Python OS & SYS

## 1. Introduction to `os` and `sys` Modules

### Purpose of the `os` Module

* Interacting with the operating system
* Managing files and directories
* Handling environment variables
* Working with processes and system-level operations

### Purpose of the `sys` Module

* Accessing Python runtime information
* Handling command-line arguments
* Managing interpreter settings
* Working with input/output streams

### Role in System Programming

* Automating system tasks
* Interacting with the filesystem
* Managing processes
* Building command-line utilities

### Differences Between `os` and `sys`

| Feature                 | `os` Module                  | `sys` Module               |
| ----------------------- | ---------------------------- | -------------------------- |
| Main Purpose            | Operating system interaction | Python runtime interaction |
| File Management         | Yes                          | No                         |
| Process Management      | Yes                          | Limited                    |
| Environment Variables   | Yes                          | No                         |
| Interpreter Information | No                           | Yes                        |

### Importing the Modules

```python
import os
import sys
```

### Platform Dependent vs Platform Independent Behavior

* Windows-specific operations
* Linux/macOS-specific operations
* Writing cross-platform code using Python utilities

---

# 2. Operating System and Platform Information

### Getting Operating System Name

```python
import os
print(os.name)
```

### Getting Platform Details

```python
import platform
print(platform.system())
print(platform.release())
```

### Getting System Architecture

```python
print(platform.architecture())
```

### Retrieving System Version Information

```python
print(sys.version)
```

### Checking Current Working Platform

```python
print(sys.platform)
```

---

# 3. Working with Directories and Paths

### Getting the Current Working Directory

```python
os.getcwd()
```

### Changing the Current Working Directory

```python
os.chdir("path")
```

### Understanding Relative vs Absolute Paths

* Relative paths
* Absolute paths
* Path resolution concepts

### Joining Paths

```python
os.path.join("folder", "file.txt")
```

### Splitting Paths

```python
os.path.split(path)
```

### Getting File and Directory Names from Paths

```python
os.path.basename(path)
os.path.dirname(path)
```

### Normalizing and Resolving Absolute Paths

```python
os.path.abspath(path)
os.path.normpath(path)
```

---

# 4. File and Directory Management

### Creating Directories

```python
os.mkdir("folder")
```

### Creating Nested Directories

```python
os.makedirs("parent/child")
```

### Removing Directories

```python
os.rmdir("folder")
```

### Removing Files

```python
os.remove("file.txt")
```

### Renaming Files and Directories

```python
os.rename("old.txt", "new.txt")
```

### Moving Files and Directories

```python
import shutil
shutil.move("source.txt", "destination/")
```

### Checking if File or Directory Exists

```python
os.path.exists(path)
```

---

# 5. Listing and Traversing Directories

### Listing Directory Contents

```python
os.listdir()
```

### Recursive Directory Traversal

* Traversing nested folders
* Processing files recursively

### Filtering Directory Contents

```python
[file for file in os.listdir() if file.endswith(".txt")]
```

### Using Directory Iterators

```python
os.scandir()
```

### Walking Through Directory Trees

```python
os.walk(path)
```

---

# 6. File Information, Metadata, and Permissions

### Getting File Size

```python
os.path.getsize("file.txt")
```

### File Creation, Modification, and Access Time

```python
os.path.getmtime("file.txt")
os.path.getctime("file.txt")
os.path.getatime("file.txt")
```

### Checking File Types

```python
os.path.isfile(path)
os.path.isdir(path)
```

### Understanding File Permissions

* Read permission
* Write permission
* Execute permission

### Changing File Permissions

```python
os.chmod("file.txt", 0o644)
```

### Working with Permission Flags

```python
import stat
```

---

# 7. Environment Variables and Temporary Resources

### Accessing Environment Variables

```python
os.environ.get("PATH")
```

### Setting Environment Variables

```python
os.environ["DEBUG"] = "1"
```

### Deleting Environment Variables

```python
del os.environ["DEBUG"]
```

### Using Environment Variables in Applications

* Configuration management
* Secure credential handling
* Deployment customization

### Creating Temporary Files

```python
import tempfile
tempfile.NamedTemporaryFile()
```

### Creating Temporary Directories

```python
tempfile.TemporaryDirectory()
```

---

# 8. Process and System Command Management

### Getting Current Process ID

```python
os.getpid()
```

### Getting Parent Process ID

```python
os.getppid()
```

### Running System Commands

```python
os.system("dir")
```

### Executing External Programs

```python
import subprocess
subprocess.run(["python", "--version"])
```

### Waiting for Processes

* Blocking process execution
* Synchronizing child processes

### Handling Process Exit Status

```python
result = subprocess.run(["python", "--version"])
print(result.returncode)
```

### Capturing Command Output and Errors

```python
subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)
```

---

# 9. Python Runtime and System Interaction (`sys`)

### Command Line Arguments (`sys.argv`)

```python
print(sys.argv)
```

### Python Version and Interpreter Details

```python
print(sys.version)
print(sys.executable)
```

### Standard Input, Output, and Error Streams

```python
sys.stdin
sys.stdout
sys.stderr
```

### Python Module Search Path (`sys.path`)

```python
print(sys.path)
```

### Program Termination (`sys.exit`)

```python
sys.exit(0)
```

### Memory Usage and Recursion Limits

```python
sys.getsizeof(object)
sys.getrecursionlimit()
sys.setrecursionlimit(2000)
```

---

# 10. Practical Usage, Performance, and Best Practices

### Building Command Line Tools

* Argument parsing
* Interactive scripts
* Automation utilities

### Writing Automation Scripts

* File organization
* Log processing
* Scheduled maintenance scripts

### Managing Files and Directories Programmatically

* Batch file operations
* Backup systems
* File synchronization

### Handling Exceptions and Runtime Information

```python
try:
    # risky operation
    pass
except Exception as e:
    print(e)
```

### Security Considerations

* File permissions
* Safe command execution
* Avoiding shell injection
* Validating user input

### Performance Optimization for File Operations

* Using iterators
* Efficient directory traversal
* Reducing unnecessary file access

### Writing Cross-Platform Code and Safe Resource Handling

* Using `os.path`
* Using `pathlib`
* Avoiding hardcoded paths
* Cleaning temporary resources properly

---
