
# FastAPI Hands-On Tutotrial

---

## Chapter 2: FastAPI Introduction & Setup

## What is FastAPI?

FastAPI is a modern Python web framework designed specifically for building APIs with:

- High performance
- Strong correctness guarantees
- Automatic validation
- Native asynchronous support

At a conceptual level, FastAPI sits between incoming HTTP requests and your Python business logic, acting as:

- Translator
- Validator
- Dispatcher

---

# Core Architecture of FastAPI

From a system perspective, FastAPI is built on three foundational layers:

## 1. ASGI (Asynchronous Server Gateway Interface)

ASGI defines how Python applications communicate asynchronously with web servers.

### Responsibilities

- Handles asynchronous communication
- Supports concurrent requests
- Replaces older WSGI limitations

---

## 2. Starlette

Starlette provides the web-handling layer.

### Responsibilities

- Routing
- Middleware
- Request/response objects
- Background tasks

---

## 3. Pydantic

Pydantic handles:

- Data validation
- Parsing
- Serialization

using Python type hints.

---

# Why FastAPI is Powerful

This architecture enables:

- High performance (comparable to Node.js and Go in many cases)
- Automatic request validation
- Automatic API documentation generation
- Native async support for concurrency

---

# Internal Request Lifecycle in FastAPI

When an HTTP request arrives, FastAPI does not directly handle low-level networking.

Instead, the following pipeline occurs:

```text
Incoming Request
        ↓
ASGI Server (Uvicorn)
        ↓
FastAPI Routing System
        ↓
Data Validation (Pydantic)
        ↓
Business Logic Execution
        ↓
Response Serialization
        ↓
HTTP Response
```

---

# Detailed Internal Flow

## Step 1: Receive Structured Request Data

The ASGI server receives raw HTTP traffic and converts it into structured request data.

---

## Step 2: Route Matching

FastAPI:

- Matches HTTP method
- Matches URL path
- Selects the correct Python function

---

## Step 3: Data Validation

Incoming data is validated using:

- Python type hints
- Pydantic models

FastAPI automatically:

- Converts types
- Rejects invalid input

---

## Step 4: Execute Business Logic

The corresponding Python function executes.

---

## Step 5: Serialize Output

Returned Python objects are converted into JSON automatically.

---

## Step 6: Send HTTP Response

The response is sent back through the ASGI server to the client.

---

# Installing FastAPI and the Server

FastAPI itself is only a framework.

To actually run a FastAPI application, you need an ASGI server.

The most commonly used server is:

- Uvicorn

---

# Installation Command

```bash
pip install fastapi uvicorn
```

---

# What Happens Internally During Installation

When this command executes:

## 1. pip Connects to PyPI

`pip` connects to the Python Package Index.

---

## 2. Dependency Resolution

Dependencies are resolved automatically.

### FastAPI Dependencies

- Starlette
- Pydantic

### Uvicorn Dependencies

- h11
- click
- asyncio-related libraries

---

## 3. Package Download

Wheel files (`.whl`) are downloaded.

---

## 4. Extraction

Packages are extracted into:

```text
site-packages/
```

inside your active Python environment.

---

## 5. Metadata Registration

Python package metadata is registered so imports work correctly.

---

# Best Practices

## Use Virtual Environments

Always install dependencies inside a virtual environment.

### Benefits

- Prevents dependency conflicts
- Isolates project dependencies
- Improves reproducibility

---

## Maintain `requirements.txt`

Generate dependency snapshot:

```bash
pip freeze > requirements.txt
```

This ensures consistent setup across systems.

---

# Running Your First FastAPI Application

---

# Step 1: Create the Application

Create a file named:

```text
main.py
```

Add the following code:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, World"}
```

---

# Code Breakdown

## `FastAPI()`

```python
app = FastAPI()
```

Creates the FastAPI application instance.

This object manages:

- Routes
- Middleware
- Validation
- Application lifecycle

---

## Route Decorator

```python
@app.get("/")
```

Registers a route inside FastAPI’s internal routing table.

### Meaning

- HTTP Method: `GET`
- Path: `/`

---

## Route Handler Function

```python
def read_root():
```

This function becomes the handler for requests to:

```text
GET /
```

---

## Return Value

```python
return {"message": "Hello, World"}
```

The dictionary is:

1. Automatically serialized into JSON
2. Wrapped inside an HTTP response

---

# Step 2: Start the Server

Run the application using:

```bash
uvicorn main:app --reload
```

---

# Command Breakdown

## `uvicorn`

Starts the ASGI server.

---

## `main`

Refers to:

```text
main.py
```

---

## `app`

Refers to:

```python
app = FastAPI()
```

inside `main.py`.

---

## `--reload`

Enables automatic server restart when source code changes.

### Important

Use only during development.

---

# Step 3: What Happens Internally

When the server starts, the following system-level operations occur.

---

# 1. Process Initialization

Uvicorn:

- Starts a Python process
- Initializes an asynchronous event loop (`asyncio`)

---

# 2. Socket Binding

Uvicorn binds to:

```text
127.0.0.1:8000
```

and starts listening for incoming TCP connections.

---

# 3. Connection Handling

The server:

- Accepts incoming connections
- Reads raw HTTP data from sockets

---

# 4. HTTP Parsing

Raw bytes are parsed into structured HTTP components:

- Method
- Path
- Headers
- Body

---

# 5. ASGI Scope Creation

Request data is wrapped into an ASGI `scope` dictionary.

The scope contains metadata such as:

- Request path
- Method
- Headers
- Client information

---

# 6. FastAPI Execution

FastAPI receives the scope and:

1. Matches the route
2. Executes the corresponding Python function

---

# 7. Response Processing

The returned Python object is:

- Validated
- Serialized into JSON
- Converted into an HTTP response object

---

# 8. Response Transmission

The response is sent back to the client through the socket connection.

---

# Step 4: Access the Application

Once the server is running:

## API Endpoint

```text
http://127.0.0.1:8000
```

---

## Swagger UI Documentation

```text
http://127.0.0.1:8000/docs
```

---

## ReDoc Documentation

```text
http://127.0.0.1:8000/redoc
```

---

# Automatic Documentation Generation

FastAPI automatically generates API documentation using:

- OpenAPI specification
- Python type hints
- Route metadata

---

# Project Structure Basics

Small projects may use a single file.

As applications grow, proper structure becomes essential for:

- Scalability
- Maintainability
- Team collaboration

---

# Recommended Project Structure

```text
project/
│
├── app/
│   ├── main.py
│   ├── routes/
│   ├── models/
│   ├── schemas/
│   ├── services/
│   └── core/
│
├── venv/
├── requirements.txt
```

---

# Component-Level Explanation

---

# `main.py`

## Responsibilities

- Entry point of the application
- Initializes FastAPI instance
- Includes routers from other modules

---

# `routes/`

## Responsibilities

Contains API endpoint definitions.

### Benefits

- Groups related endpoints
- Keeps routing separate from business logic

Example:

```text
users.py
products.py
orders.py
```

---

# `models/`

## Responsibilities

Defines database models.

Usually contains ORM layer definitions.

### Purpose

Represents how data is stored in databases.

---

# `schemas/`

## Responsibilities

Contains Pydantic models used for:

- Request validation
- Response formatting

### Important Role

Acts as a contract between:

- Client
- Server

---

# `services/`

## Responsibilities

Contains business logic.

### Why Important

Prevents route handlers from becoming bloated.

### Benefits

- Reusability
- Testability
- Cleaner architecture

---

# `core/`

## Responsibilities

Stores:

- Configuration
- Settings
- Utilities
- Security configurations
- Environment handling

---

# `venv/`

Contains isolated dependencies for the project.

---

# `requirements.txt`

Tracks installed dependencies.

Ensures reproducible environments.

---

# Why This Structure Matters

From a system-design perspective, this structure:

- Separates concerns
- Reduces coupling
- Improves maintainability
- Enables modular testing
- Supports scalability
- Simplifies team onboarding

---

# End-to-End Request Flow Summary

When everything is configured correctly:

```text
Client Sends HTTP Request
            ↓
Uvicorn Receives Request
            ↓
HTTP Parsing
            ↓
ASGI Scope Creation
            ↓
FastAPI Route Matching
            ↓
Input Validation
            ↓
Business Logic Execution
            ↓
Response Serialization
            ↓
HTTP Response Returned
```

---

# Key Takeaways

- FastAPI is built on ASGI, Starlette, and Pydantic
- Uvicorn handles low-level networking and protocol communication
- Python functions become API endpoints through decorators
- Type hints power validation, parsing, and documentation
- Proper project structure is essential for scalability
- Understanding the request lifecycle is critical for mastering FastAPI

````
