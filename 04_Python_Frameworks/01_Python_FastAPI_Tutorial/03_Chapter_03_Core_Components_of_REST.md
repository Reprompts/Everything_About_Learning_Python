# FastAPI Hands-On Tutorial
## Chapter 3: Core Components of REST

---

# Basic API Development (Core REST Concepts)

This section explains how FastAPI translates HTTP concepts into executable Python logic and how requests move through the system.

The goal is to understand both:

- Practical API usage
- Internal system behavior

This helps make API development predictable, scalable, and aligned with real-world systems.

---

# Creating Routes (Path Operations)

In FastAPI, a **route** (or **path operation**) defines how the application responds to a specific combination of:

- URL path
- HTTP method

Example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"message": "API is running"}
````

---

# Internal Route Registration

When this code executes:

```python
@app.get("/")
```

FastAPI internally registers the route.

The decorator associates:

* Path → `/`
* HTTP Method → `GET`

with the Python function:

```python
root()
```

This mapping is stored in an internal routing table managed by **Starlette**.

---

# Internal Request Matching Process

When a request arrives:

## Step 1: Path Matching

FastAPI checks whether the request path matches a registered route.

---

## Step 2: Method Matching

If the path matches, FastAPI checks the HTTP method.

Example:

* `GET`
* `POST`
* `DELETE`

---

## Step 3: Function Execution

If both path and method match:

* The corresponding Python function is executed.

---

# Performance Optimization

FastAPI uses optimized routing structures instead of sequential route checks.

### Benefits

* High performance
* Efficient lookup
* Scalability with many routes

---

# HTTP Methods in FastAPI

FastAPI provides decorators for different HTTP methods.

Example:

```python
@app.post("/users")
def create_user():
    return {"status": "user created"}

@app.delete("/users/{user_id}")
def delete_user(user_id: int):
    return {"deleted": user_id}
```

---

# REST Semantics of HTTP Methods

Each HTTP method represents a specific intent.

| Method | Purpose          | Idempotent |
| ------ | ---------------- | ---------- |
| GET    | Retrieve data    | Yes        |
| POST   | Create resource  | No         |
| PUT    | Replace resource | Yes        |
| PATCH  | Partial update   | Usually    |
| DELETE | Remove resource  | Yes        |

---

# Internal HTTP Method Handling

FastAPI extracts the HTTP method from the ASGI request scope.

Then it performs:

```text
Path Match
    ↓
Method Match
    ↓
Execute Handler
```

If the path exists but the method does not match:

FastAPI automatically returns:

```http
405 Method Not Allowed
```

---

# Why Method Separation Matters

This separation ensures:

* Clear API intent
* REST-compliant design
* Predictable behavior

---

# Path Parameters

Path parameters allow dynamic values inside URLs.

Example:

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

---

# How Path Parameters Work Internally

Request:

```text
/users/10
```

FastAPI performs:

## Step 1: Extract Value

Extracts:

```text
10
```

from the URL.

---

## Step 2: Type Conversion

Uses the type hint:

```python
user_id: int
```

to convert:

```text
"10" → 10
```

---

## Step 3: Validation

If conversion fails:

Example:

```text
/users/abc
```

FastAPI automatically returns:

```http
422 Unprocessable Entity
```

---

# Important Concept

Validation occurs **before** the function executes.

This guarantees:

* Business logic receives valid data
* Invalid input never reaches application logic

---

# Characteristics of Path Parameters

* Always required
* Part of route structure
* Used to identify resources

Examples:

```text
/users/1
/products/100
/orders/55
```

---

# Query Parameters

Query parameters provide optional data in URLs.

Example:

```python
@app.get("/users")
def list_users(limit: int = 10, active: bool = True):
    return {"limit": limit, "active": active}
```

---

# Example Request

```text
/users?limit=5&active=false
```

---

# Internal Query Parameter Processing

FastAPI performs:

## Step 1: Parse URL Query String

Extracts:

```text
limit=5
active=false
```

---

## Step 2: Type Conversion

Converts values automatically:

```text
"5" → 5
"false" → False
```

using Python type hints.

---

# Optional vs Required Parameters

## Optional Parameter

Has default value:

```python
limit: int = 10
```

---

## Required Parameter

No default value provided:

```python
limit: int
```

If missing, FastAPI returns:

```http
422 Unprocessable Entity
```

---

# Common Uses of Query Parameters

* Filtering
* Sorting
* Pagination
* Searching

Examples:

```text
/users?page=1
/products?category=books
/orders?status=completed
```

---

# Request Body Handling

Request bodies are used for sending structured data.

Typically used with:

* POST
* PUT
* PATCH

---

# Pydantic Model Example

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

@app.post("/users")
def create_user(user: User):
    return user
```

---

# Internal Request Body Processing

When a request arrives:

## Step 1: Read Raw Request Body

FastAPI reads raw bytes from the HTTP request.

---

## Step 2: Parse JSON

Converts JSON into a Python dictionary.

Example:

```json
{
    "name": "Ganesh",
    "age": 22
}
```

↓

```python
{
    "name": "Ganesh",
    "age": 22
}
```

---

## Step 3: Pydantic Validation

The dictionary is passed to the Pydantic model.

Validation includes:

* Required fields
* Data types
* Constraints

---

## Step 4: Object Creation

If validation succeeds:

A `User` object is created and passed into the function.

---

# Automatic Validation Errors

If validation fails:

FastAPI automatically returns:

```http
422 Unprocessable Entity
```

with detailed validation messages.

---

# Why This Matters

This ensures:

* Invalid data never enters business logic
* APIs remain predictable
* Data contracts stay consistent

---

# Response Handling

FastAPI automatically converts Python return values into HTTP responses.

Example:

```python
@app.get("/status")
def status():
    return {"status": "ok"}
```

---

# Internal Response Flow

FastAPI performs:

## Step 1: Serialize Python Object

Dictionary →

```json
{
    "status": "ok"
}
```

---

## Step 2: Add HTTP Headers

Example:

```http
Content-Type: application/json
```

---

## Step 3: Send Response

Response is transmitted back to the client.

---

# Custom Responses

Example:

```python
from fastapi import Response

@app.get("/custom")
def custom_response():
    return Response(
        content="Plain text",
        media_type="text/plain"
    )
```

This allows:

* Plain text responses
* XML responses
* File responses
* Streaming responses

---

# Response Models

FastAPI supports response validation using Pydantic models.

Example:

```python
class UserResponse(BaseModel):
    name: str

@app.get("/user", response_model=UserResponse)
def get_user():
    return {"name": "Ganesh", "age": 22}
```

---

# Internal Response Validation

Returned data:

```python
{
    "name": "Ganesh",
    "age": 22
}
```

Response model:

```python
class UserResponse(BaseModel):
    name: str
```

Final response:

```json
{
    "name": "Ganesh"
}
```

---

# Why Response Models Matter

They help:

* Filter sensitive data
* Enforce response consistency
* Maintain API contracts
* Improve documentation accuracy

---

# Status Codes

Status codes communicate request outcomes.

They are a critical part of API communication.

---

# Default Status Codes

FastAPI automatically assigns standard codes.

Examples:

| Operation       | Default Status             |
| --------------- | -------------------------- |
| Successful GET  | 200 OK                     |
| Successful POST | 200 OK (unless overridden) |

---

# Custom Status Codes

Example:

```python
from fastapi import status

@app.post(
    "/users",
    status_code=status.HTTP_201_CREATED
)
def create_user():
    return {"message": "created"}
```

Response:

```http
201 Created
```

---

# Error Handling with Exceptions

Example:

```python
from fastapi import HTTPException

@app.get("/users/{user_id}")
def get_user(user_id: int):
    if user_id != 1:
        raise HTTPException(
            status_code=404,
            detail="User not found"
        )

    return {"user_id": user_id}
```

---

# Internal Exception Handling

When an exception is raised:

FastAPI:

1. Catches the exception
2. Creates structured error response
3. Attaches proper status code
4. Sends response to client

---

# Example Error Response

```json
{
    "detail": "User not found"
}
```

with status:

```http
404 Not Found
```

---

# End-to-End Request Flow

When a request is sent to a FastAPI application:

```text
Client Sends HTTP Request
            ↓
ASGI Server (Uvicorn)
            ↓
HTTP Parsing
            ↓
ASGI Scope Creation
            ↓
FastAPI Route Matching
            ↓
Path & Query Parameter Extraction
            ↓
Request Body Parsing
            ↓
Pydantic Validation
            ↓
Business Logic Execution
            ↓
Response Serialization
            ↓
Attach Status Code
            ↓
HTTP Response Returned
```

---

# Key Takeaways

* FastAPI maps HTTP concepts directly to Python constructs
* Routes define request handling behavior
* HTTP methods express operation intent
* Path and query parameters provide structured input
* Request bodies are validated automatically
* Responses are serialized consistently
* Pydantic powers validation and data conversion
* Understanding the request lifecycle is essential for building scalable APIs

```
```
