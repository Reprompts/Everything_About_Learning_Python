
# FastAPI Hands-On Tutorial
## Chapter 05: Request Handling

---

# Advanced Request Handling

In real-world APIs, not all data arrives as simple JSON.

Applications frequently need to handle:

- Form submissions
- File uploads
- Request metadata through headers
- Session-related data via cookies
- Background processing for long-running tasks

FastAPI provides built-in abstractions for these scenarios while maintaining a clean, type-driven interface.

Internally, these features are built on top of:

- Starlette’s request handling system
- The ASGI protocol

---

# Form Data

Form data is typically sent using:

- `application/x-www-form-urlencoded`
- `multipart/form-data`

This format is commonly used in HTML forms where data is submitted as key-value pairs.

---

# Handling Form Data in FastAPI

FastAPI uses the `Form` class to extract form fields.

Example:

```python
from fastapi import FastAPI, Form

app = FastAPI()

@app.post("/login")
def login(
    username: str = Form(...),
    password: str = Form(...)
):
    return {"username": username}
````

---

# What `Form(...)` Means

```python id="1vab99"
Form(...)
```

tells FastAPI:

* Extract data from form body
* Do not treat it as JSON

---

# Internal Form Processing Flow

When a form request arrives:

```text id="9gmq5g"
HTTP Request
      ↓
Read Raw Body
      ↓
Detect Content-Type
      ↓
Starlette Parses Form Data
      ↓
Extract Key-Value Pairs
      ↓
Validate Using Type Hints
      ↓
Pass Data to Function
```

---

# Internal Behavior

FastAPI internally:

## Step 1: Reads Raw Request Body

The incoming body is received as bytes.

---

## Step 2: Detects Form Encoding

Example content types:

```http id="1h1u7c"
application/x-www-form-urlencoded
multipart/form-data
```

---

## Step 3: Delegates Parsing to Starlette

Starlette extracts:

```text id="ldz50q"
key → value
```

pairs from the body.

---

## Step 4: Maps Values to Function Parameters

Function arguments are populated automatically.

---

## Step 5: Performs Validation

Validation occurs using:

* Type hints
* Required field checks

---

# Required vs Optional Form Fields

Example:

```python id="x9v7ao"
username: str = Form(...)
```

means:

* Required field

---

Example:

```python id="klr6zq"
username: str = Form(None)
```

means:

* Optional field

---

# Validation Failure

If a required form field is missing:

FastAPI automatically returns:

```http id="a2k1si"
422 Unprocessable Entity
```

before the function executes.

---

# Required Dependency for Forms

Install:

```bash
pip install python-multipart
```

---

# Why This Package is Needed

`python-multipart` enables parsing of:

* Multipart form data
* File uploads
* Mixed form/file requests

---

# File Uploads

File uploads are handled using:

```text id="v8v9k3"
multipart/form-data
```

This is commonly used for:

* Profile images
* Documents
* Media uploads

---

# UploadFile Example

```python id="m7rwwv"
from fastapi import FastAPI, File, UploadFile

app = FastAPI()

@app.post("/upload")
def upload_file(
    file: UploadFile = File(...)
):
    return {"filename": file.filename}
```

---

# Why `UploadFile` is Preferred

`UploadFile` is more efficient than raw bytes because:

* Uses a file-like object internally
* Supports streaming
* Avoids loading entire files into memory

---

# Internal File Upload Flow

When a file is uploaded:

```text id="7x4iyx"
HTTP Multipart Request
          ↓
Starlette Parses Multipart Data
          ↓
File Stream Written to Temporary Storage
          ↓
Metadata Extracted
          ↓
UploadFile Object Created
          ↓
Passed to Route Handler
```

---

# Extracted Metadata

FastAPI automatically extracts:

* Filename
* Content type
* File stream handler

---

# Reading Uploaded Files

Example:

```python id="e07q4d"
@app.post("/upload")
async def upload_file(
    file: UploadFile = File(...)
):
    content = await file.read()

    return {"size": len(content)}
```

---

# Why `await` is Used

File operations use asynchronous I/O.

This allows the server to:

* Handle other requests simultaneously
* Avoid blocking the event loop

---

# Important Performance Recommendation

For large files:

✅ Process data in chunks

❌ Avoid reading the entire file into memory

---

# Headers and Cookies

Headers and cookies provide additional request metadata.

They are commonly used for:

* Authentication
* Session management
* Client identification
* Configuration

---

# Headers

Headers are accessed using the `Header` class.

Example:

```python id="9j9lmx"
from fastapi import Header

@app.get("/headers")
def read_headers(
    user_agent: str = Header(...)
):
    return {"User-Agent": user_agent}
```

---

# Automatic Header Conversion

FastAPI automatically converts:

```text id="h0mpbo"
User-Agent → user_agent
```

Features:

* Hyphens become underscores
* Matching is case-insensitive

---

# Internal Header Processing

Headers are stored in the ASGI request scope.

FastAPI performs:

```text id="d8vtw6"
Read Headers from ASGI Scope
              ↓
Convert to Dictionary Structure
              ↓
Map to Function Parameters
              ↓
Apply Type Validation
```

---

# Common Uses of Headers

Headers are frequently used for:

* Authorization tokens
* API keys
* Content negotiation
* Client metadata

---

# Cookies

Cookies are accessed using the `Cookie` class.

Example:

```python id="30nclo"
from fastapi import Cookie

@app.get("/cookies")
def read_cookies(
    session_id: str = Cookie(None)
):
    return {"session_id": session_id}
```

---

# Internal Cookie Processing

Cookies are sent inside the HTTP request headers.

FastAPI performs:

```text id="5y8sl5"
Read Cookie Header
        ↓
Parse Key-Value Pairs
        ↓
Map to Function Parameters
        ↓
Validate Types
```

---

# Common Uses of Cookies

Cookies are commonly used for:

* Session tracking
* Authentication
* User preferences
* Small client-side state

---

# Background Tasks

Background tasks allow work to continue **after** a response is sent to the client.

This is useful for:

* Logging
* Sending emails
* Notifications
* Lightweight asynchronous processing

---

# Background Task Example

```python id="07jy3z"
from fastapi import BackgroundTasks

def write_log(message: str):
    with open("log.txt", "a") as f:
        f.write(message + "\n")

@app.post("/process")
def process_data(
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(
        write_log,
        "Process started"
    )

    return {"status": "processing"}
```

---

# Internal Background Task Flow

FastAPI processes background tasks as follows:

```text id="gdc1vv"
Request Received
       ↓
Route Handler Executes
       ↓
Background Task Registered
       ↓
HTTP Response Sent
       ↓
Background Task Executes
```

---

# Important Internal Detail

Background tasks:

* Run in the same process
* Execute outside the main request lifecycle
* Start only after response dispatch

---

# Appropriate Use Cases

✅ Good for:

* Logging
* Sending notifications
* Lightweight operations

❌ Not suitable for:

* CPU-intensive tasks
* Long-running processing
* Heavy workloads

---

# For Heavy Workloads

Use external task queues such as:

* Celery
* RQ
* Dramatiq

---

# End-to-End Advanced Request Flow

When handling advanced request inputs:

```text id="jtl6od"
Client Sends Request
(Form/File/Header/Cookie)
            ↓
Uvicorn Receives Raw HTTP Data
            ↓
Starlette Parses Content-Type
            ↓
FastAPI Extracts Parameters
(Form/File/Header/Cookie)
            ↓
Validation & Type Conversion
            ↓
Route Handler Execution
            ↓
Response Generation
            ↓
HTTP Response Sent
            ↓
Background Tasks Execute
```

---

# Key Takeaways

* FastAPI supports multiple request data formats beyond JSON
* Form data enables traditional web form handling
* File uploads support efficient streaming through `UploadFile`
* Headers and cookies provide contextual request metadata
* Background tasks allow non-blocking post-response operations
* Starlette powers much of FastAPI’s advanced request parsing
* Understanding request parsing and validation is critical for production-grade APIs

```
```
