
# FastAPI Hands-On Tutorial
## Chapter 06: Dependency Injection

---

# Dependency Injection System

FastAPI includes a built-in **dependency injection system** that allows you to manage:

- Shared logic
- Resources
- Cross-cutting concerns

in a clean and structured way.

Instead of manually calling helper functions inside every route, dependencies are declared and automatically resolved by FastAPI before the route handler executes.

This creates a:

- Declarative architecture
- Loosely coupled system
- Highly reusable codebase

---

# System-Level Perspective

Dependency injection is integrated into FastAPI’s request processing pipeline.

Before a route function executes, FastAPI:

```text id="x0w7kl"
Inspects Function Parameters
            ↓
Identifies Dependencies
            ↓
Builds Dependency Graph
            ↓
Executes Dependencies
            ↓
Injects Results into Route Handler
            ↓
Executes Route Function
````

This entire process is driven by:

* Type hints
* `Depends()`
* Function signatures

---

# Concept of Dependency Injection

Dependency Injection (DI) is a design pattern where a function does **not** create or manage its own dependencies.

Instead, dependencies are provided externally.

---

# Without Dependency Injection

```python id="s2m8qf"
def get_user():
    db = connect_to_database()

    return db.query("SELECT * FROM users")
```

---

# Problem with This Approach

The function becomes tightly coupled to:

* Database creation
* Connection management
* Infrastructure details

This reduces:

* Reusability
* Testability
* Maintainability

---

# With Dependency Injection

```python id="y3n7uk"
def get_db():
    return connect_to_database()

@app.get("/users")
def get_user(
    db = Depends(get_db)
):
    return db.query("SELECT * FROM users")
```

---

# What Changed?

The route handler no longer knows:

* How the database is created
* Where it comes from

It simply declares:

```python id="t4xq9c"
"I depend on get_db"
```

FastAPI handles the rest automatically.

---

# Internal Dependency Graph

FastAPI internally builds a dependency graph for every request.

Example:

```text id="m9f8ep"
Route Handler
      ↓
Depends on get_user
      ↓
Depends on get_db
```

FastAPI determines:

* What functions must execute
* In what order
* Which results should be injected

---

# Why This Matters

Dependency injection improves:

* Modularity
* Separation of concerns
* Reusability
* Scalability
* Testability

---

# Depends() Usage

The `Depends()` function is used to declare dependencies.

---

# Basic Example

```python id="n8v2zr"
from fastapi import Depends

def common_parameters(
    q: str = None
):
    return {"q": q}

@app.get("/items")
def read_items(
    params: dict = Depends(common_parameters)
):
    return params
```

---

# Internal Execution Flow

When a request arrives:

```text id="p6u3yx"
Request Received
       ↓
Inspect Route Parameters
       ↓
Detect Depends(common_parameters)
       ↓
Execute common_parameters()
       ↓
Capture Return Value
       ↓
Inject into params
       ↓
Execute Route Function
```

---

# Internal Behavior

FastAPI internally:

## Step 1: Inspects Function Signature

It scans parameters for:

```python id="6x6d7x"
Depends(...)
```

---

## Step 2: Resolves Dependency

Executes the dependency function.

---

## Step 3: Captures Return Value

Stores the returned object.

---

## Step 4: Injects Result

Passes the value into the route parameter.

---

# Dependencies Can Have Their Own Parameters

Example:

```python id="0flh5k"
def pagination(limit: int = 10):
    return {"limit": limit}

@app.get("/products")
def get_products(
    p: dict = Depends(pagination)
):
    return p
```

---

# Internal Parameter Resolution

FastAPI performs:

```text id="w4nl5w"
Read Query Parameters
         ↓
Pass Into pagination()
         ↓
Receive Result
         ↓
Inject Into Route Handler
```

---

# Reusable Dependencies

One major advantage of dependency injection is reuse.

Instead of duplicating logic across routes, dependencies can be centralized.

---

# Example: Shared Header Logic

```python id="m2twf7"
from fastapi import Header

def get_token(
    token: str = Header(...)
):
    return token

@app.get("/users")
def get_users(
    token: str = Depends(get_token)
):
    return {"token": token}

@app.get("/orders")
def get_orders(
    token: str = Depends(get_token)
):
    return {"token": token}
```

---

# Benefits of Reusability

This approach provides:

* Centralized logic
* Reduced duplication
* Easier maintenance
* Consistent behavior

---

# Layered Dependencies

Dependencies can depend on other dependencies.

---

# Example

```python id="v5ef6x"
def get_db():
    return "database_connection"

def get_user(
    db = Depends(get_db)
):
    return {
        "user": "Ganesh",
        "db": db
    }

@app.get("/profile")
def profile(
    user = Depends(get_user)
):
    return user
```

---

# Dependency Resolution Order

Execution order:

```text id="x6mz6f"
get_db()
    ↓
get_user(db)
    ↓
profile(user)
```

---

# Dependency Tree

```text id="x3shmr"
profile
   ↓
get_user
   ↓
get_db
```

FastAPI recursively resolves the entire tree.

---

# Dependency Caching

FastAPI automatically caches dependency results **per request**.

This means:

```text id="6g1q7d"
If the same dependency is reused:
Execute Once
Reuse Result
```

This improves performance and consistency.

---

# Authentication Dependencies

Authentication is one of the most common use cases for dependency injection.

Instead of embedding authentication logic into every route, it can be centralized.

---

# Example: Token Authentication

```python id="v0cf1u"
from fastapi import (
    Header,
    HTTPException,
    Depends
)

def verify_token(
    authorization: str = Header(...)
):
    if authorization != "secret-token":
        raise HTTPException(
            status_code=401,
            detail="Unauthorized"
        )

    return authorization

@app.get("/secure-data")
def secure_data(
    token: str = Depends(verify_token)
):
    return {"message": "Access granted"}
```

---

# Authentication Flow

FastAPI performs:

```text id="p0r9o9"
Request Received
       ↓
Execute verify_token()
       ↓
Extract Authorization Header
       ↓
Validate Token
       ↓
Success → Continue
Failure → Return 401
```

---

# Important Internal Behavior

If a dependency raises an exception:

```text id="7w4u7z"
Dependency Execution Stops
           ↓
Route Handler Never Runs
           ↓
Error Response Returned
```

---

# Why Authentication Dependencies Matter

This pattern provides:

* Centralized security logic
* Consistent enforcement
* Cleaner route handlers
* Better maintainability

---

# Layered Authentication Example

```python id="j6v4r2"
def get_current_user(
    token: str = Depends(verify_token)
):
    return {"username": "Ganesh"}

@app.get("/me")
def read_profile(
    user = Depends(get_current_user)
):
    return user
```

---

# Layered Security Flow

```text id="i2o4oz"
verify_token()
        ↓
get_current_user()
        ↓
read_profile()
```

---

# Single Responsibility Principle

Each dependency handles one responsibility:

| Dependency         | Responsibility  |
| ------------------ | --------------- |
| `verify_token`     | Authentication  |
| `get_current_user` | User extraction |
| `read_profile`     | Route logic     |

---

# Internal Dependency Resolution Flow

When a request arrives, FastAPI performs the following sequence:

```text id="r3fj7t"
Request Received
        ↓
Route Identified
        ↓
Inspect Function Signature
        ↓
Extract Dependencies
        ↓
Build Dependency Graph
        ↓
Resolve Dependencies
(Topological Order)
        ↓
Cache Results Per Request
        ↓
Inject Final Values
        ↓
Execute Route Handler
```

---

# Topological Execution Order

Dependencies execute according to their relationships.

Parent dependencies execute only after child dependencies complete.

---

# Failure Handling

If any dependency fails:

```text id="7g2r6m"
Raise Exception
      ↓
Stop Dependency Resolution
      ↓
Generate Error Response
      ↓
Terminate Request
```

---

# Practical Advantages

Dependency injection provides several real-world benefits.

---

# Separation of Concerns

Logic becomes isolated into focused components:

* Authentication
* Database access
* Configuration
* Validation

---

# Reusability

Shared logic can be reused across multiple routes.

---

# Improved Testability

Dependencies can be:

* Mocked
* Replaced
* Overridden during tests

---

# Consistency

Critical behavior such as security remains uniform across the application.

---

# End-to-End Example

```python id="e6v9bx"
from fastapi import (
    FastAPI,
    Depends,
    Header,
    HTTPException
)

app = FastAPI()

def get_db():
    return "db_connection"

def verify_token(
    token: str = Header(...)
):
    if token != "secret":
        raise HTTPException(
            status_code=401,
            detail="Unauthorized"
        )

    return token

def get_user(
    db = Depends(get_db),
    token = Depends(verify_token)
):
    return {
        "user": "Ganesh",
        "db": db
    }

@app.get("/dashboard")
def dashboard(
    user = Depends(get_user)
):
    return user
```

---

# Execution Flow

```text id="o9j5ys"
Request Arrives
       ↓
verify_token() Executes
       ↓
Validate Request Header
       ↓
get_db() Executes
       ↓
Database Connection Returned
       ↓
get_user() Combines Data
       ↓
dashboard() Receives Final User Object
       ↓
Response Returned
```

---

# Key Takeaways

* FastAPI includes a powerful built-in dependency injection system
* `Depends()` declares dependencies declaratively
* Dependencies are automatically resolved before route execution
* Dependency graphs are built recursively
* Dependencies support reuse and layering
* Authentication and database handling are ideal DI use cases
* Dependency results are cached per request
* Dependency injection improves scalability, maintainability, and testability
* Understanding dependency resolution is essential for production-grade FastAPI applications

```
