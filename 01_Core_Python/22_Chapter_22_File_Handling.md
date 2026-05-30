
# Python Programming: Chapter 22 File Handling

File handling is a fundamental capability in Python that allows programs to store, retrieve, and manipulate data on disk. Many real-world applications interact with files, including:

- log processing  
- configuration management  
- data analysis  
- report generation  
- database exports  
- image and binary processing  

Python provides a powerful and flexible set of tools for working with files. This article explains file handling in detail, covering:

- Opening files  
- File modes  
- Reading files  
- Writing files  
- File iteration  
- File buffering  
- Binary file operations  
- Context managers (with statement)  

---

## 1. What is File Handling?

File handling refers to the process of:

- Opening a file  
- Reading or writing data  
- Closing the file  

Basic workflow:

```

open file → perform operations → close file

````

Example:

```python
file = open("example.txt")
content = file.read()
file.close()
````

However, Python provides better mechanisms such as context managers to handle files safely.

---

## 2. Opening Files

Files are opened using the `open()` function.

**Syntax:**

```python
open(filename, mode)
```

Example:

```python
file = open("data.txt", "r")
```

Parameters:

* filename → path to file
* mode → how the file should be opened

Return value:

```python
print(type(file))
# <class '_io.TextIOWrapper'>
```

---

## 3. File Modes

File modes determine how a file is accessed.

### Read Mode (r)

```python
file = open("data.txt", "r")
```

### Write Mode (w)

```python
file = open("data.txt", "w")
file.write("Hello Python")
file.close()
```

### Append Mode (a)

```python
file = open("data.txt", "a")
file.write("New line\n")
```

### Exclusive Creation (x)

```python
file = open("data.txt", "x")
```

### Text Mode (t)

```python
open("file.txt", "rt")
```

### Binary Mode (b)

```python
open("image.png", "rb")
```

---

## 4. Reading Files

### 4.1 read()

```python
file = open("data.txt", "r")
content = file.read()
print(content)
file.close()
```

Read limited characters:

```python
file.read(10)
```

### 4.2 readline()

```python
file = open("data.txt")
line = file.readline()
print(line)
```

### 4.3 readlines()

```python
file = open("data.txt")
lines = file.readlines()
print(lines)
```

---

## 5. Writing Files

### write()

```python
file = open("data.txt", "w")
file.write("Hello\n")
file.write("Python\n")
file.close()
```

### writelines()

```python
lines = ["Apple\n", "Banana\n", "Mango\n"]
file = open("data.txt", "w")
file.writelines(lines)
file.close()
```

---

## 6. File Iteration

```python
file = open("data.txt")
for line in file:
    print(line.strip())
```

---

## 7. File Positioning

### tell()

```python
file.tell()
```

### seek()

```python
file.seek(0)
```

---

## 8. File Buffering

Example:

```python
open("file.txt", "r", buffering=1024)
```

Buffering modes:

* 0 → unbuffered
* 1 → line buffered
* > 1 → fixed buffer size

---

## 9. Binary File Operations

### Reading binary

```python
file = open("image.png", "rb")
data = file.read()
file.close()
```

### Writing binary

```python
file = open("copy.png", "wb")
file.write(data)
file.close()
```

### Copy example

```python
source = open("image.png", "rb")
target = open("copy.png", "wb")
target.write(source.read())
source.close()
target.close()
```

---

## 10. Context Managers (with)

```python
with open("data.txt", "r") as file:
    content = file.read()
```

---

## 11. Advantages of with

* Automatic cleanup
* Cleaner code
* Prevents leaks
* Safe exception handling

---

## 12. Log File Example

```python
with open("server.log") as file:
    for line in file:
        if "ERROR" in line:
            print(line)
```

---

## 13. Word Counter

```python
with open("text.txt") as file:
    text = file.read()

words = text.split()
print("Total words:", len(words))
```

---

## 14. File Copier

```python
def copy_file(source, target):
    with open(source, "rb") as s:
        with open(target, "wb") as t:
            t.write(s.read())
```

---

## 15. Checking File Existence

```python
import os

if os.path.exists("data.txt"):
    print("File exists")
```

---

## 16. File Methods

* read()
* readline()
* readlines()
* write()
* writelines()
* seek()
* tell()
* close()

---

## 17. Best Practices

* Use `with` always
* Avoid loading huge files fully
* Iterate large files
* Handle exceptions

Example:

```python
try:
    with open("data.txt") as f:
        print(f.read())
except FileNotFoundError:
    print("File missing")
```

---

## 18. Summary

Python file handling enables:

* File reading and writing
* Safe resource management
* Binary data handling
* Large file processing

It is a core skill for real-world Python development.

```

---

