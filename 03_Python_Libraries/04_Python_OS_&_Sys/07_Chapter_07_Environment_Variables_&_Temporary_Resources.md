# OS & SYS Hands-On Tutorial
## Python OS & SYS: Environment Variables and Temporary Resources in Python

Many programs need to interact with system-level configuration settings and manage temporary resources during execution. Examples include:

* Storing configuration settings
* Managing sensitive data like API keys
* Setting system paths
* Creating temporary files for intermediate processing
* Managing temporary directories for runtime data

Python provides powerful tools through the `os` module and the `tempfile` module to manage environment variables and temporary resources safely and efficiently.

Understanding these tools is important for building secure, portable, and scalable applications.

---

# Environment Variables

Environment variables are key-value pairs stored in the operating system environment.

They are commonly used to store:

* System configuration
* Application settings
* File paths
* Database credentials
* API keys
* Runtime flags

## Example Environment Variables

* `PATH`
* `HOME`
* `USER`
* `JAVA_HOME`
* `PYTHONPATH`

Programs can read these variables to adapt their behavior depending on the environment.

---

# Accessing Environment Variables

Python allows access to environment variables using:

```python id="d7q9lm"
os.environ
```

This object behaves like a dictionary containing all environment variables.

---

# Example

```python id="g4djlwm"
import os

print(os.environ)
```

## Output Example

```python id="vx7n1q"
{
 'PATH': '/usr/bin:/bin',
 'HOME': '/home/ganesh',
 'USER': 'ganesh'
}
```

---

# Accessing a Specific Environment Variable

To retrieve a specific variable, use dictionary-style access.

## Example

```python id="f6tq1o"
import os

home = os.environ["HOME"]

print(home)
```

## Output

```text id="6m5y5f"
/home/ganesh
```

---

# Safer Access with `get()`

If a variable does not exist, direct access causes a `KeyError`.

## Safer Approach

```python id="nyvjlwm"
import os

home = os.environ.get("HOME")

print(home)
```

If the variable does not exist, it returns:

```python id="g4s7mv"
None
```

You can also provide a default value.

```python id="w6v24x"
import os

port = os.environ.get("PORT", "8000")

print(port)
```

---

# Setting Environment Variables

Programs can also create or modify environment variables during runtime.

This can be done using:

```python id="jlwm0c"
os.environ["VARIABLE_NAME"] = value
```

---

# Example

```python id="c9t8fc"
import os

os.environ["APP_MODE"] = "development"

print(os.environ["APP_MODE"])
```

## Output

```text id="wrn9zz"
development
```

---

# Important Note

These changes only affect the current Python process and its child processes.

They do not permanently change the system environment variables.

---

# Deleting Environment Variables

Environment variables can be removed using:

```python id="6tpkjlwm"
del os.environ["VARIABLE_NAME"]
```

---

# Example

```python id="jlwm9o"
import os

del os.environ["APP_MODE"]
```

After deletion, the variable no longer exists in the environment.

---

# Safe Deletion

To avoid errors if the variable does not exist:

```python id="jlwm8u"
import os

os.environ.pop("APP_MODE", None)
```

---

# Using Environment Variables in Applications

Environment variables are widely used in modern software development for configuration.

Instead of hardcoding values like database passwords or API keys in source code, applications read them from environment variables.

---

# Example Configuration Variables

* `DATABASE_URL`
* `API_KEY`
* `SECRET_KEY`
* `PORT`
* `DEBUG`

---

# Example: Application Configuration

```python id="0u5lcb"
import os

db_url = os.environ.get("DATABASE_URL")

if db_url:
    print("Connecting to database:", db_url)

else:
    print("Database URL not configured")
```

---

# Example: Application Mode

```python id="jlwm6f"
import os

mode = os.environ.get("APP_MODE", "production")

if mode == "development":
    print("Debugging enabled")

else:
    print("Running in production mode")
```

---

# Advantages of Using Environment Variables

Environment variables provide several benefits.

---

# Security

Sensitive data like passwords and API keys are not stored in source code.

---

# Portability

Applications can run in different environments without code changes.

## Example Environments

* Development
* Testing
* Production

---

# Configuration Management

System administrators can configure applications without modifying the program.

---

# Temporary Files

Many programs need temporary files for tasks such as:

* Intermediate data processing
* Caching results
* Temporary downloads
* Data transformation

Python provides the `tempfile` module to safely create temporary files.

---

# Creating Temporary Files

The simplest way to create a temporary file is:

```python id="4a4mjlwm"
tempfile.TemporaryFile()
```

---

# Example

```python id="jlwm3r"
import tempfile

temp = tempfile.TemporaryFile()

temp.write(b"Hello World")

temp.seek(0)

print(temp.read())

temp.close()
```

## Output

```text id="jlwm5x"
b'Hello World'
```

---

# Important Characteristics

* The file is automatically deleted when closed.
* It exists only for the duration of the program.

---

# Named Temporary Files

Sometimes we need a temporary file with a visible filename.

Python provides:

```python id="0rjlwm"
tempfile.NamedTemporaryFile()
```

---

# Example

```python id="jlwm7k"
import tempfile

with tempfile.NamedTemporaryFile(delete=True) as temp:
    print("Temporary file:", temp.name)
```

## Example Output

```text id="jlwm4j"
Temporary file: /tmp/tmpabc123
```

---

# Features

* Has a visible file name
* Automatically deleted when closed

---

# Creating Temporary Directories

Programs often need temporary directories to store multiple files.

Python provides:

```python id="jlwm2p"
tempfile.TemporaryDirectory()
```

---

# Example

```python id="jlwm1y"
import tempfile
import os

with tempfile.TemporaryDirectory() as temp_dir:
    print("Temporary directory:", temp_dir)

    file_path = os.path.join(temp_dir, "test.txt")

    with open(file_path, "w") as f:
        f.write("Temporary data")
```

## Example Output

```text id="jlwm9q"
Temporary directory: /tmp/tmpxyz456
```

After the block finishes, the directory and its contents are automatically removed.

---

# Benefits of Using `tempfile`

The `tempfile` module provides several advantages.

---

# Security

Temporary files are created with secure random names, preventing conflicts or attacks.

---

# Automatic Cleanup

Files and directories are automatically removed when no longer needed.

---

# Cross-Platform Support

Works consistently on:

* Windows
* Linux
* macOS

---

# Example: Temporary Data Processing Script

Below is an example program that demonstrates environment variables and temporary resources together.

```python id="jlwm8m"
import os
import tempfile

# Access environment variable
user = os.environ.get("USER", "unknown")

print("Running as:", user)

# Create temporary directory
with tempfile.TemporaryDirectory() as temp_dir:

    file_path = os.path.join(temp_dir, "data.txt")

    with open(file_path, "w") as f:
        f.write("Temporary processing data")

    print("Temporary file created:", file_path)

print("Temporary resources cleaned up")
```

## Example Output

```text id="jlwm0v"
Running as: ganesh
Temporary file created: /tmp/tmpabc123/data.txt
Temporary resources cleaned up
```

---

# Real-World Applications

Environment variables and temporary resources are used widely in modern systems.

---

# Web Applications

Frameworks like:

* Django
* Flask
* FastAPI

use environment variables for configuration.

---

# DevOps and Deployment

Tools like:

* Docker
* Kubernetes
* CI/CD pipelines

pass configuration through environment variables.

---

# Data Processing

Temporary directories are used for:

* Intermediate datasets
* Temporary caches
* File transformations

---

# Best Practices

When working with environment variables and temporary resources, developers should follow several best practices.

---

# Never Hardcode Secrets

Use environment variables for:

* API keys
* Passwords
* Tokens

---

# Always Use `get()` When Reading Variables

This prevents runtime errors.

---

# Prefer `tempfile` Over Manual Temporary Files

The module ensures:

* Secure names
* Automatic cleanup

---

# Use Context Managers

Context managers (`with`) automatically release resources.

---

# Summary

Python provides powerful tools for managing environment variables and temporary resources.

## Key Operations

| Task                         | Method                          |
| ---------------------------- | ------------------------------- |
| Access environment variables | `os.environ`                    |
| Safe retrieval               | `os.environ.get()`              |
| Set environment variables    | `os.environ["VAR"] = value`     |
| Delete environment variables | `del os.environ["VAR"]`         |
| Create temporary file        | `tempfile.TemporaryFile()`      |
| Create named temporary file  | `tempfile.NamedTemporaryFile()` |
| Create temporary directory   | `tempfile.TemporaryDirectory()` |

These tools allow developers to build secure, configurable, and resource-efficient applications.

---
