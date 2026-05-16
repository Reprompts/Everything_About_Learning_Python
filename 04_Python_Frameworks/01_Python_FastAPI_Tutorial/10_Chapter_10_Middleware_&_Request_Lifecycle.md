# FastAPI Hands-On Tutorial
## Chapter 10: Middleware & Request Lifecycle

---

# Middleware & Request Lifecycle

Middleware is a critical layer in FastAPI that allows you to intercept and process requests and responses globally. It sits between the incoming request and your route handlers, giving you control over cross-cutting concerns such as logging, authentication, error handling, and response modification.

Understanding middleware also requires understanding the full request/response lifecycle because middleware operates at multiple stages of this flow.

---

# Middleware Concepts

Middleware is essentially a function that runs **before and after** a request is processed by your route handler.

## Basic Middleware Structure in FastAPI

```python
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def custom_middleware(request: Request, call_next):
    response = await call_next(request)
    return response
```

## Explanation

In this structure:

* `request` represents the incoming HTTP request
* `call_next` is a function that passes control to the next layer (another middleware or the route handler)
* Middleware can:

  * Modify the request before passing it forward
  * Modify the response before returning it

---

## Internal Working

Internally:

* Middleware is part of the ASGI application stack
* Each middleware wraps the application like layers in a pipeline
* Execution follows a chain where each middleware calls the next layer

### Execution Order

#### Request Flow

```text
Client → Outer Middleware → Inner Middleware → Route Handler
```

#### Response Flow

```text
Route Handler → Inner Middleware → Outer Middleware → Client
```

This layered design allows centralized handling of logic that should apply globally across the application.

---

# Logging Middleware

Logging is one of the most common middleware use cases. It helps track:

* Incoming requests
* Execution time
* Response status
* Performance bottlenecks

## Example

```python
import time

@app.middleware("http")
async def log_requests(request: Request, call_next):
    start_time = time.time()

    response = await call_next(request)

    process_time = time.time() - start_time

    print(f"{request.method} {request.url} completed in {process_time:.4f}s")

    return response
```

---

## Internal Flow

1. Request arrives
2. Middleware starts timer
3. `call_next()` executes downstream logic
4. Route handler processes request
5. Response returns to middleware
6. Execution time is calculated
7. Log is generated
8. Response sent to client

---

## System-Level Importance

Logging middleware helps:

* Monitor API performance
* Detect slow endpoints
* Debug production issues
* Generate audit trails
* Feed monitoring systems

In production environments, logs are usually forwarded to systems like:

* ELK Stack
* Grafana Loki
* Datadog
* Splunk

---

# CORS Handling

## What is CORS?

CORS (Cross-Origin Resource Sharing) is a browser security mechanism.

It controls whether a frontend running on one origin can access resources from another origin.

Example:

```text
Frontend: http://localhost:3000
Backend:  http://localhost:8000
```

Without proper CORS configuration, browsers block such requests.

---

## FastAPI CORS Middleware

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

## Configuration Breakdown

### `allow_origins`

Defines which domains can access the API.

```python
allow_origins=["https://example.com"]
```

---

### `allow_methods`

Defines allowed HTTP methods.

```python
allow_methods=["GET", "POST"]
```

---

### `allow_headers`

Defines allowed request headers.

```python
allow_headers=["Authorization", "Content-Type"]
```

---

### `allow_credentials`

Allows cookies and authentication headers.

```python
allow_credentials=True
```

---

## Internal CORS Flow

### Browser Behavior

For many cross-origin requests, browsers first send a **preflight request**:

```http
OPTIONS /endpoint
```

The server responds with allowed CORS policies.

If approved:

* Browser proceeds with actual request
* Otherwise request is blocked

---

## Example Flow

```text
Frontend Request
      ↓
Browser Checks CORS
      ↓
OPTIONS Preflight Request
      ↓
CORS Middleware Adds Headers
      ↓
Browser Validates Policy
      ↓
Actual Request Sent
```

---

# Request/Response Lifecycle

Understanding the lifecycle is essential for understanding middleware.

---

# Full Request Lifecycle

```text
Client Sends HTTP Request
        ↓
Uvicorn (ASGI Server) Receives Request
        ↓
Request Converted Into ASGI Scope
        ↓
Middleware Chain Begins
        ↓
Request Passes Through Middleware Layers
        ↓
FastAPI Router Matches Path & Method
        ↓
Dependencies Are Resolved
        ↓
Request Data Validation Occurs
        ↓
Route Handler Executes
        ↓
Response Object Generated
        ↓
Response Passes Back Through Middleware
        ↓
Final Response Sent To Client
```

---

# Detailed Middleware Execution Example

```python
@app.middleware("http")
async def example_middleware(request: Request, call_next):
    print("Before request")

    response = await call_next(request)

    print("After response")

    return response
```

## Execution Sequence

```text
Request enters middleware
        ↓
"Before request" printed
        ↓
Route handler executes
        ↓
Response generated
        ↓
Response returns to middleware
        ↓
"After response" printed
        ↓
Final response returned
```

This demonstrates that middleware wraps around the route execution lifecycle.

---

# Modifying Requests and Responses

Middleware can modify both requests and responses.

---

# Modifying Responses

## Example

```python
@app.middleware("http")
async def add_header(request: Request, call_next):
    response = await call_next(request)

    response.headers["X-Custom-Header"] = "Value"

    return response
```

### Use Cases

* Security headers
* Correlation IDs
* Cache control
* Custom metadata

---

# Modifying Requests

## Example

```python
@app.middleware("http")
async def modify_request(request: Request, call_next):
    request.state.custom_data = "extra_info"

    return await call_next(request)
```

## Explanation

`request.state` provides temporary storage for request-scoped data.

This data can later be accessed in route handlers:

```python
@app.get("/")
async def root(request: Request):
    return {"data": request.state.custom_data}
```

---

# Middleware vs Dependencies

Middleware and dependencies solve different problems.

| Middleware                      | Dependencies                  |
| ------------------------------- | ----------------------------- |
| Global request handling         | Route-level logic             |
| Runs for every request          | Runs only where declared      |
| Works before routing            | Works after routing           |
| Best for cross-cutting concerns | Best for reusable route logic |

---

## Middleware Use Cases

* Logging
* CORS
* Global authentication
* Request transformation
* Response modification
* Monitoring

---

## Dependency Use Cases

* Database sessions
* Authentication checks
* Current user extraction
* Pagination logic
* Configuration injection

---

# Performance Considerations

Since middleware executes on every request, poorly designed middleware can significantly impact performance.

---

## Best Practices

### Keep Middleware Lightweight

Avoid heavy processing.

---

### Use Async Operations

Prefer:

```python
await some_async_operation()
```

Instead of blocking calls.

---

### Avoid CPU-Heavy Tasks

Heavy computation should be delegated to:

* Background workers
* Task queues
* Separate services

---

### Minimize I/O Blocking

Avoid synchronous:

* File operations
* Network calls
* Database operations

inside middleware.

---

# End-to-End Example

```python
from fastapi import FastAPI, Request
from fastapi.middleware.cors import CORSMiddleware
import time

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.middleware("http")
async def logging_middleware(request: Request, call_next):
    start = time.time()

    response = await call_next(request)

    duration = time.time() - start

    print(f"{request.method} {request.url} took {duration:.4f}s")

    return response

@app.get("/")
def root():
    return {"message": "Hello"}
```

---

# Execution Flow

```text
Request Arrives
      ↓
CORS Middleware Executes
      ↓
Logging Middleware Executes
      ↓
Route Handler Executes
      ↓
Response Returns To Logging Middleware
      ↓
Response Returns To CORS Middleware
      ↓
Final Response Sent To Client
```

---

# Internal ASGI Middleware Stack

```text
Client
  ↓
Uvicorn (ASGI Server)
  ↓
CORS Middleware
  ↓
Logging Middleware
  ↓
FastAPI Router
  ↓
Dependencies
  ↓
Route Handler
  ↓
Response Serialization
  ↓
Client
```

---

# Key Takeaways

* Middleware is a global interception layer in FastAPI
* It operates at the ASGI level and wraps the entire request lifecycle
* Middleware can modify both requests and responses
* Logging middleware is essential for monitoring and debugging
* CORS middleware enables secure frontend-backend communication
* Understanding the request lifecycle helps place logic correctly
* Middleware differs from dependencies in scope and purpose
* Efficient middleware design is critical for high-performance APIs

---

# Final Conceptual Model

```text
HTTP Request
      ↓
ASGI Server (Uvicorn)
      ↓
Middleware Pipeline
      ↓
FastAPI Routing
      ↓
Dependency Resolution
      ↓
Validation Layer
      ↓
Business Logic
      ↓
Serialization
      ↓
Middleware Response Processing
      ↓
HTTP Response
```
