
# Python FastAPI Hands-On Tutorial

## Chapter 1: Before FastAPI

Below is a professional, system-level, step-by-step deep dive into the foundational concepts required before learning FastAPI.

This chapter is designed to build **mental models**, not just surface-level knowledge.

---

# 1. Foundations (Before FastAPI)

FastAPI is not just a framework — it is a thin, efficient layer built on top of:

- Python execution model
- HTTP protocol
- Data serialization systems

To truly understand FastAPI, you must understand what happens internally when a request is processed.

---

# 1.1 Python Fundamentals (Execution-Level Understanding)

## How Python Code Actually Runs

When you write Python code:

```python
def add(a: int, b: int) -> int:
    return a + b
````

Internally:

### 1. Source Code → Bytecode

* Python compiles `.py` files into bytecode (`.pyc`)
* Bytecode is platform-independent

### 2. Python Virtual Machine (PVM) Executes Bytecode

* The PVM reads instructions line by line
* Uses a stack-based execution model

### 3. Function Call Mechanism

When:

```python
add(2, 3)
```

is executed:

* A stack frame is created
* Arguments are pushed into memory
* Execution context is maintained

---

## Stack Frames (Critical for API Understanding)

Each function call creates a stack frame:

```text
|----------------------|
| Local variables      |
| Function arguments   |
| Return address       |
|----------------------|
```

### Why This Matters

Each API request:

* Triggers a function
* Creates independent execution frames
* Enables concurrent handling (especially with async support)

---

## Type Hints (Static Metadata, Not Runtime Enforcement)

Example:

```python
def create_user(name: str, age: int) -> dict:
    return {"name": name, "age": age}
```

### Important Clarification

Python does **NOT** enforce types at runtime.

Type hints are annotations stored in function metadata.

Example:

```python
print(create_user.__annotations__)
```

Output:

```python
{
    'name': <class 'str'>,
    'age': <class 'int'>,
    'return': <class 'dict'>
}
```

### Why FastAPI Cares

FastAPI reads these annotations to:

* Validate input
* Convert types
* Generate OpenAPI schemas

This is the bridge between Python functions and web APIs.

---

## Objects and Memory Model

In Python:

```python
x = {"name": "Ganesh"}
```

* `x` is a reference to an object in heap memory
* Data is not copied unless explicitly done

### API Implications

Mutable data structures can cause:

* Shared-state bugs
* Concurrency issues

This becomes important in API request handling.

---

# 1.2 Virtual Environments (Dependency Isolation at System Level)

## Problem Without Virtual Environments

Python installs packages globally:

```text
/usr/lib/python3.x/site-packages/
```

### Issues

* Version conflicts
* Dependency clashes
* System instability

---

## What `venv` Actually Does

When you run:

```bash
python -m venv env
```

It creates:

```text
env/
 ├── bin/ or Scripts/
 ├── lib/
 ├── pyvenv.cfg
```

### Key Mechanism

* Copies Python interpreter
* Creates isolated `site-packages`
* Overrides `sys.path`

---

## `sys.path` Manipulation

Python searches modules using:

```python
import sys
print(sys.path)
```

Inside a virtual environment:

* Project-specific paths come first
* Global packages are ignored

---

## `pip` (Package Manager Internals)

When you run:

```bash
pip install fastapi
```

Behind the scenes:

1. Resolves dependencies
2. Downloads wheel files (`.whl`)
3. Extracts packages into `site-packages`
4. Updates metadata (`.dist-info`)

---

## Dependency Freezing

```bash
pip freeze > requirements.txt
```

This captures:

* Exact package versions
* Reproducible environments

---

# 1.3 HTTP Fundamentals (Protocol-Level Understanding)

HTTP is a stateless application-layer protocol built on top of TCP.

---

## Full Request-Response Lifecycle

```text
Client
   ↓
DNS Resolution
   ↓
TCP Handshake
   ↓
HTTP Request
   ↓
Server Processing
   ↓
HTTP Response
   ↓
Connection Close / Keep-Alive
```

---

## Step-by-Step Breakdown

### 1. DNS Resolution

Converts:

```text
Domain Name → IP Address
```

---

### 2. TCP Handshake

Connection establishment process:

```text
Client → SYN
Server → SYN-ACK
Client → ACK
```

This establishes a reliable connection.

---

### 3. HTTP Request Sent

Example:

```http
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 32

{"name": "Ganesh", "age": 22}
```

---

### 4. Server Processing

The server:

1. Parses the request
2. Routes it to the correct handler
3. Executes business logic

---

### 5. HTTP Response

Example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{"id": 1, "name": "Ganesh"}
```

---

## Stateless Nature of HTTP

Each request is independent.

The server does not remember previous requests automatically.

State is managed using:

* Cookies
* Tokens
* Sessions

---

# 1.4 HTTP Methods (Semantic Operations)

HTTP methods define **intent**, not implementation.

---

## Deep Semantics

### GET

* Safe (does not modify resources)
* Idempotent
* Cacheable

---

### POST

* Creates resources
* Not idempotent

---

### PUT

* Replaces entire resource
* Idempotent

---

### PATCH

* Partial updates

---

### DELETE

* Removes resources
* Idempotent

---

## Idempotency Explained

An operation is idempotent if:

> Multiple identical requests produce the same result.

Example:

```text
DELETE /users/1
```

Deleting the same resource repeatedly still results in the same final state.

---

# 1.5 HTTP Status Codes (Protocol Feedback System)

Status codes communicate server outcomes.

---

## Status Code Categories

### 2xx — Success

Request processed successfully.

Examples:

* `200 OK`
* `201 Created`

---

### 4xx — Client Errors

Problem exists in the request.

Examples:

* `400 Bad Request`
* `401 Unauthorized`
* `404 Not Found`

---

### 5xx — Server Errors

Failure occurred inside server logic.

Examples:

* `500 Internal Server Error`
* `503 Service Unavailable`

---

## System-Level Importance

Status codes allow:

* Load balancers to react
* Clients to retry or fail gracefully
* Monitoring systems to detect issues

---

# 1.6 Headers, Query Params, and Body (Data Transport Layers)

---

## Headers (Metadata Layer)

Example:

```http
Authorization: Bearer token
Content-Type: application/json
```

### Internally

Headers are parsed into key-value structures before body processing.

---

## Query Parameters (URL-Encoded Data)

Example:

```text
/users?age=25&active=true
```

### Internally

* Parsed from URL strings
* Automatically type-cast in FastAPI

---

## Request Body (Payload Layer)

Used for structured data transfer.

### Common Formats

* JSON
* Form Data
* Multipart Data (file uploads)

---

# 1.7 JSON and Serialization (Data Transformation Layer)

## Why JSON?

JSON is:

* Language-independent
* Human-readable
* Lightweight

---

## Serialization Pipeline

### Python → JSON

Example:

```python
import json

json.dumps(data)
```

### Internal Steps

1. Python object converted to intermediate structures
2. Serialized into JSON string format

---

## Deserialization Pipeline

### JSON → Python

Example:

```python
json.loads(json_string)
```

---

## FastAPI Internal Flow

FastAPI uses:

* Pydantic models
* Validation layers
* Automatic conversion systems

---

## Critical Concept: Data Validation Layer

Incoming request:

```json
{
    "age": "22"
}
```

FastAPI:

1. Converts `"22"` → `22`
2. Validates schema
3. Rejects invalid data automatically

---

# End-to-End System Flow

```text
[Client]
   ↓
DNS Resolution
   ↓
TCP Connection
   ↓
HTTP Request
   ↓
[FastAPI Server]
   ↓
Routing → Function
   ↓
Type Validation
   ↓
Business Logic
   ↓
Serialization (Python → JSON)
   ↓
HTTP Response + Status Code
   ↓
[Client]
```

---

# Final Conceptual Model

FastAPI is essentially:

```text
Python Functions
+ Type Hints
+ HTTP Protocol
+ JSON Serialization
= Modern API System
```

```
```
