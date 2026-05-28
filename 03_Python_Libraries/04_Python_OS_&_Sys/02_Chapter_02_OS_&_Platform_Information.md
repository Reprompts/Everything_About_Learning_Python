# OS & Sys Hands-On Tutorial
## Python OS & SYS: Operating System and Platform Information in Python

Many Python programs need to understand the environment in which they are running. For example, a program may need to:

* Run different commands on Windows vs Linux
* Detect system architecture (32-bit vs 64-bit)
* Log system information for debugging
* Adjust file paths based on the operating system

To accomplish this, Python provides several tools to retrieve operating system and platform information.

The most commonly used modules are:

* `os`
* `sys`
* `platform`

These modules allow programs to inspect system characteristics such as:

* Operating system name
* System architecture
* Platform type
* OS version
* Hardware details

Understanding how to retrieve this information is essential when writing portable and cross-platform Python programs.

---

# Getting Operating System Name

One of the simplest pieces of information you can retrieve is the name of the operating system.

Python provides multiple ways to do this.

---

# Using `os.name`

The `os` module provides a variable called `os.name` which indicates the operating system interface being used.

## Example

```python id="f9n1kw"
import os

print(os.name)
```

## Example Outputs

```python id="c6i5tp"
nt
```

```python id="a7u9rs"
posix
```

```python id="h3w2zx"
java
```

---

# Meaning of Values

| Value   | Operating System     |
| ------- | -------------------- |
| `nt`    | Windows              |
| `posix` | Linux / macOS / Unix |
| `java`  | Java platform        |

This method is useful when your code only needs to distinguish major OS families.

---

# Getting Platform Details

For more detailed system information, Python provides the `platform` module.

This module gathers comprehensive information about the operating system and hardware environment.

---

# Using `platform.system()`

This function returns the human-readable name of the operating system.

## Example

```python id="l2o8vu"
import platform

print(platform.system())
```

## Example Outputs

```python id="j5d3xe"
Windows
```

```python id="0shsdt"
Linux
```

```python id="q0l2mm"
Darwin
```

---

# Output Meanings

| Output    | Meaning             |
| --------- | ------------------- |
| `Windows` | Microsoft Windows   |
| `Linux`   | Linux-based systems |
| `Darwin`  | macOS               |

This function is often preferred because the result is clearer and easier to read than `os.name`.

---

# Using `platform.platform()`

This function returns a more detailed platform description.

## Example

```python id="t0qj8x"
import platform

print(platform.platform())
```

## Example Output

```python id="8c7xmf"
Windows-10-10.0.19045-SP0
```

This string includes:

* Operating system
* Version number
* Additional system details

---

# Getting System Architecture

Modern computers can run 32-bit or 64-bit architectures, and sometimes programs need to detect this information.

Python provides architecture information through the `platform` module.

---

# Using `platform.architecture()`

This function returns information about the bit architecture of the Python interpreter.

## Example

```python id="w8yq6k"
import platform

print(platform.architecture())
```

## Example Output

```python id="o2qzya"
('64bit', 'WindowsPE')
```

---

# Explanation

| Value   | Meaning             |
| ------- | ------------------- |
| `64bit` | 64-bit architecture |
| `32bit` | 32-bit architecture |

The function returns a tuple containing:

```text id="jz5s9k"
(bits, linkage format)
```

This information helps determine whether Python is running in a 32-bit or 64-bit environment.

---

# Using `platform.machine()`

You can also retrieve the hardware type.

## Example

```python id="evh9qt"
import platform

print(platform.machine())
```

## Example Output

```python id="p8g2sl"
x86_64
```

---

# Possible Values

| Value    | Architecture     |
| -------- | ---------------- |
| `x86_64` | 64-bit Intel/AMD |
| `ARM`    | ARM processors   |
| `i386`   | 32-bit Intel     |

---

# Retrieving System Version Information

Programs sometimes need to check specific operating system versions for compatibility or feature support.

The `platform` module provides functions to retrieve this information.

---

# Using `platform.release()`

Returns the OS release number.

## Example

```python id="0wp7dp"
import platform

print(platform.release())
```

## Example Output

```python id="mf8lm8"
10
```

### Linux Example

```python id="u0zj0j"
5.15.0-78-generic
```

---

# Using `platform.version()`

Returns detailed version information for the operating system.

## Example

```python id="86a3wd"
import platform

print(platform.version())
```

## Example Output

```python id="d5my6p"
10.0.19045
```

### Linux Example

```python id="4dypl8"
#85-Ubuntu SMP Fri Jul 21 14:19:00 UTC 2023
```

This provides more low-level OS build information.

---

# Checking Current Working Platform

Another common task is determining which platform the program is currently running on.

This is useful when writing cross-platform code.

---

# Using `sys.platform`

The `sys` module contains the attribute `sys.platform`, which provides a string identifying the platform.

## Example

```python id="7st8xj"
import sys

print(sys.platform)
```

## Example Outputs

```python id="6ytrpw"
win32
```

```python id="mjlwmq"
linux
```

```python id="4swfmb"
darwin
```

---

# Meaning of Values

| Value    | Platform |
| -------- | -------- |
| `win32`  | Windows  |
| `linux`  | Linux    |
| `darwin` | macOS    |

This method is commonly used in scripts that need to execute different logic depending on the operating system.

---

# Conditional Platform Detection Example

```python id="pdwvvj"
import sys

if sys.platform.startswith("win"):
    print("Running on Windows")

elif sys.platform.startswith("linux"):
    print("Running on Linux")

elif sys.platform == "darwin":
    print("Running on macOS")

else:
    print("Unknown platform")
```

This pattern is widely used in cross-platform applications and installation scripts.

---

# Getting Complete System Information

Sometimes developers want all system information in one place.

The `platform.uname()` function provides this.

---

# Using `platform.uname()`

## Example

```python id="c4c29x"
import platform

system_info = platform.uname()
print(system_info)
```

## Example Output

```python id="bdt36m"
uname_result(
    system='Linux',
    node='hostname',
    release='5.15.0',
    version='#78-Ubuntu SMP',
    machine='x86_64',
    processor='x86_64'
)
```

This function returns a tuple-like object containing detailed system attributes.

---

# Information Included

| Field       | Meaning          |
| ----------- | ---------------- |
| `system`    | Operating system |
| `node`      | Network hostname |
| `release`   | OS release       |
| `version`   | OS version       |
| `machine`   | Hardware type    |
| `processor` | CPU information  |

---

# Example: Complete System Information Script

Below is a small Python program that prints comprehensive system information.

```python id="evlgpz"
import os
import sys
import platform

print("Operating System:", os.name)
print("Platform:", sys.platform)
print("System:", platform.system())
print("Release:", platform.release())
print("Version:", platform.version())
print("Architecture:", platform.architecture())
print("Machine:", platform.machine())
print("Processor:", platform.processor())
```

## Example Output

```python id="1o8c9w"
Operating System: nt
Platform: win32
System: Windows
Release: 10
Version: 10.0.19045
Architecture: ('64bit', 'WindowsPE')
Machine: AMD64
Processor: Intel64 Family 6 Model 158
```

---

# Why System Information Matters

Retrieving platform information is important in many real-world scenarios.

---

# Cross-Platform Software

Programs may need different implementations for:

* Windows
* Linux
* macOS

---

# Installation Scripts

Tools like:

* `pip`
* `conda`
* `docker`

often detect system information to determine which binaries to install.

---

# Debugging and Logging

Applications frequently log system information to help diagnose problems:

* OS version
* CPU architecture
* Python version

---

# DevOps and Automation

System information helps automation scripts decide:

* Which commands to run
* Which configuration files to use
* How to manage system resources

---

# Summary

Python provides powerful tools for retrieving operating system and platform information.

## Common Techniques

| Task                     | Method                    |
| ------------------------ | ------------------------- |
| Get OS family            | `os.name`                 |
| Get OS name              | `platform.system()`       |
| Get platform identifier  | `sys.platform`            |
| Get OS version           | `platform.version()`      |
| Get OS release           | `platform.release()`      |
| Get architecture         | `platform.architecture()` |
| Get detailed system info | `platform.uname()`        |

Using these functions allows developers to build robust, portable, and environment-aware Python applications that work reliably across multiple operating systems.

---
