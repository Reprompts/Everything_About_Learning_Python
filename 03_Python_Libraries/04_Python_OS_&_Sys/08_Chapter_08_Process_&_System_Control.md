# OS & SYS Hands-On Tutorial 
## Python OS & SYS: Process and System Command Management in Python

In many real-world applications, programs must interact with the operating system process environment. These interactions include:

* Identifying running processes
* Executing system commands
* Running external programs
* Waiting for processes to complete
* Capturing command output and errors
* Handling exit status codes

Python provides several tools for process and command management, mainly through:

* `os` module
* `subprocess` module

The `os` module provides basic process operations, while the `subprocess` module offers advanced process control and communication.

Understanding these capabilities is essential for building:

* Automation scripts
* Deployment tools
* Build systems
* System monitoring tools
* Command-line utilities

---

# Understanding Processes

A process is an instance of a running program managed by the operating system.

Each process has:

* A unique Process ID (PID)
* A parent process
* Its own memory space
* Its own execution environment

---

# Example Process Hierarchy

```text
Operating System
     ↓
Terminal
     ↓
Python Program
     ↓
Child Process (External Command)
```

Python allows programs to inspect and control these processes.

---

# Getting Current Process ID

Every running program has a unique process identifier (PID) assigned by the operating system.

Python provides the function:

```python
os.getpid()
```

to retrieve the PID of the current process.

---

# Example

```python
import os

pid = os.getpid()

print("Current Process ID:", pid)
```

## Example Output

```text
Current Process ID: 5421
```

This is useful for:

* Debugging
* Logging
* System monitoring
* Managing processes

---

# Getting Parent Process ID

Processes are usually created by other processes.

The process that creates another process is called the parent process.

Python provides the function:

```python
os.getppid()
```

to retrieve the parent process ID (PPID).

---

# Example

```python
import os

ppid = os.getppid()

print("Parent Process ID:", ppid)
```

## Example Output

```text
Parent Process ID: 4100
```

This allows programs to understand which process started them.

---

# Running System Commands

Sometimes a Python program needs to execute operating system commands.

Examples include:

* `ls`
* `dir`
* `ping`
* `mkdir`
* `git`
* `docker`

The simplest way to run system commands is using:

```python
os.system()
```

---

# Using `os.system()`

This function runs a command in the system shell.

---

# Syntax

```python
os.system(command)
```

---

# Example

```python
import os

os.system("echo Hello World")
```

## Output

```text
Hello World
```

---

# Example: Listing Directory Contents

## Linux / macOS

```python
os.system("ls")
```

## Windows

```python
os.system("dir")
```

---

# Limitations of `os.system()`

`os.system()` has several limitations:

* Cannot capture output easily
* Limited error handling
* Less secure
* Not recommended for complex tasks

For advanced use cases, Python provides the `subprocess` module.

---

# Executing External Programs

Python can launch external programs using the `subprocess` module.

This module allows programs to:

* Start new processes
* Communicate with them
* Capture output
* Handle errors

One of the most common functions is:

```python
subprocess.run()
```

---

# Using `subprocess.run()`

## Syntax

```python
subprocess.run(command)
```

---

# Example

```python
import subprocess

subprocess.run(["echo", "Hello from subprocess"])
```

## Output

```text
Hello from subprocess
```

---

# Important Difference

Command arguments are passed as a list.

This improves security and reliability.

---

# Running Commands Through the Shell

Sometimes commands require shell features.

Example:

```text
ls | grep file
```

In this case, use:

```python
subprocess.run("ls", shell=True)
```

However, using `shell=True` should be done carefully due to potential security risks.

---

# Waiting for Processes

When a program launches another process, it may need to wait until the process finishes.

`subprocess.run()` waits automatically.

---

# Example

```python
import subprocess

result = subprocess.run(["echo", "Processing complete"])

print("Command finished")
```

## Output

```text
Processing complete
Command finished
```

The Python program continues only after the command completes.

---

# Running Processes Asynchronously

If you want to run a process without waiting, use:

```python
subprocess.Popen()
```

---

# Example

```python
import subprocess

process = subprocess.Popen(["echo", "Running process"])

print("Program continues immediately")
```

## Output

```text
Program continues immediately
Running process
```

This launches a background process.

---

# Handling Process Exit Status

When a process finishes execution, it returns an exit code.

Exit codes indicate success or failure.

---

# Typical Convention

* `0` → success
* Non-zero → error

Python allows access to this value.

---

# Example

```python
import subprocess

result = subprocess.run(["echo", "Hello"])

print("Exit Code:", result.returncode)
```

## Output

```text
Hello
Exit Code: 0
```

---

# Example: Detecting Command Failure

```python
import subprocess

result = subprocess.run(["ls", "unknown_file"])

print("Exit Code:", result.returncode)
```

## Example Output

```text
ls: cannot access 'unknown_file': No such file or directory
Exit Code: 2
```

Programs can use this information to handle errors gracefully.

---

# Capturing Command Output

One of the biggest advantages of `subprocess` is the ability to capture command output.

---

# Example

```python
import subprocess

result = subprocess.run(
    ["echo", "Hello World"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

## Output

```text
Hello World
```

---

# Explanation

| Argument              | Purpose                    |
| --------------------- | -------------------------- |
| `capture_output=True` | Captures stdout and stderr |
| `text=True`           | Returns output as string   |

---

# Capturing Errors

Commands may generate error output (`stderr`).

---

# Example

```python
import subprocess

result = subprocess.run(
    ["ls", "missing_file"],
    capture_output=True,
    text=True
)

print("Error:", result.stderr)
```

## Example Output

```text
Error: ls: cannot access 'missing_file': No such file or directory
```

This allows programs to analyze and respond to errors.

---

# Example: Running a Command and Processing Output

```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print("Python Version:", result.stdout)
```

## Output

```text
Python Version: Python 3.12.1
```

---

# Example: Full Process Management Script

```python
import os
import subprocess

print("Current PID:", os.getpid())
print("Parent PID:", os.getppid())

result = subprocess.run(
    ["echo", "Running external command"],
    capture_output=True,
    text=True
)

print("Output:", result.stdout)
print("Exit Status:", result.returncode)
```

## Example Output

```text
Current PID: 5214
Parent PID: 4300
Output: Running external command
Exit Status: 0
```

---

# Real-World Applications

Process and command management is widely used in many systems.

---

# DevOps Automation

Scripts run commands like:

* `docker`
* `git`
* `kubectl`
* `systemctl`

---

# Build Systems

Tools such as:

* Make
* Gradle
* CMake

execute external compilers and scripts.

---

# System Monitoring

Programs monitor processes and execute diagnostics.

---

# Data Pipelines

Data processing scripts often call external tools such as:

* `ffmpeg`
* `imagemagick`
* `curl`

---

# Best Practices

When working with system commands, developers should follow important guidelines.

---

# Prefer `subprocess` Over `os.system()`

The `subprocess` module provides:

* Better security
* Better control
* Output capture

---

# Avoid `shell=True` When Possible

Passing commands as lists reduces the risk of shell injection attacks.

---

# Always Check Exit Codes

Programs should verify whether commands succeeded.

---

# Capture Errors for Debugging

Logging command output and errors helps diagnose problems.

---

# Summary

Python provides powerful tools for process management and system command execution.

## Key Operations

| Task                        | Function               |
| --------------------------- | ---------------------- |
| Get current process ID      | `os.getpid()`          |
| Get parent process ID       | `os.getppid()`         |
| Run simple command          | `os.system()`          |
| Execute external program    | `subprocess.run()`     |
| Run background process      | `subprocess.Popen()`   |
| Wait for process completion | Automatic with `run()` |
| Get exit status             | `returncode`           |
| Capture output              | `capture_output=True`  |
| Capture errors              | `stderr`               |

These tools enable Python programs to interact with the operating system, run external commands, and manage processes effectively, making them essential for automation, infrastructure management, and advanced system programming.

---
