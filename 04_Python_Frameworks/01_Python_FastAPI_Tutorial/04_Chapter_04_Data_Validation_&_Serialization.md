# FastAPI Hands-On Tutorial
## Chapter 4: Data Validation & Serialization

---

# Data Validation & Serialization

In FastAPI, data validation and serialization are handled primarily through **Pydantic models**.

This layer is critical because it ensures that:

- Incoming data is correct before reaching business logic
- Outgoing data is structured, consistent, and safe

From a system perspective, this creates a strict contract between:

- Client
- Server

---

# Using Pydantic Models

Pydantic models are Python classes that define:

- Data structure
- Field types
- Validation rules

They inherit from:

```python
BaseModel
```

and use Python type hints.

---

# Basic Example

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
````

---

# Internal Processing Flow

When FastAPI receives request data, it does **not** pass raw JSON directly into your function.

Instead, FastAPI performs the following pipeline:

```text id="x3m7lh"
Raw HTTP Request
        ↓
Read Request Body
        ↓
Parse JSON
        ↓
Convert to Python Dictionary
        ↓
Validate Using Pydantic
        ↓
Create Model Instance
        ↓
Pass Structured Object to Function
```

---

# Example Endpoint

```python id="xch8yo"
@app.post("/users")
def create_user(user: User):
    return user
```

---

# Example Request

```json id="epr1ut"
{
    "name": "Ganesh",
    "age": 22
}
```

---

# Internal Validation Steps

FastAPI internally performs:

## Step 1: Read Raw Request Body

Reads incoming bytes from the HTTP request.

---

## Step 2: Parse JSON

Converts JSON into a Python dictionary.

---

## Step 3: Pass Data to Pydantic

Pydantic validates:

* Required fields
* Data types
* Constraints

---

## Step 4: Construct Model Object

Creates a `User` instance.

---

## Step 5: Pass Object to Function

Your function receives clean, validated data.

---

# Why This Matters

Your business logic always works with:

* Structured objects
* Predictable data
* Validated inputs

This significantly improves reliability.

---

# Request Validation

Request validation ensures incoming data matches the expected schema **before** function execution begins.

---

# Example Model

```python id="qkhfd5"
class User(BaseModel):
    name: str
    age: int
```

---

# Automatic Type Coercion

Request:

```json id="0qj8gf"
{
    "name": "Ganesh",
    "age": "22"
}
```

Pydantic automatically converts:

```text id="0w7mlm"
"22" → 22
```

---

# Invalid Request Example

Request:

```json id="6tpg3h"
{
    "name": "Ganesh"
}
```

Response:

```json id="qitjy1"
{
  "detail": [
    {
      "loc": ["body", "age"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

---

# Why Validation Happens Early

Validation occurs **before** the route function executes.

This ensures:

* Invalid data never reaches business logic
* Errors are standardized automatically
* Application behavior remains predictable

---

# Internal Validation Pipeline

```text id="hcbxwv"
Incoming Data
      ↓
Schema Validation
      ↓
Type Conversion
      ↓
Constraint Checking
      ↓
Success → Execute Function
Failure → Return 422 Error
```

---

# Response Models

Response models define the structure of data returned to clients.

They are used to:

* Enforce output consistency
* Filter sensitive fields
* Improve documentation accuracy

---

# Example

```python id="1v3jcu"
class UserResponse(BaseModel):
    name: str

@app.get("/user", response_model=UserResponse)
def get_user():
    return {
        "name": "Ganesh",
        "age": 22
    }
```

---

# Actual Response Sent

```json id="5kr2wk"
{
    "name": "Ganesh"
}
```

---

# Internal Response Processing

FastAPI performs:

## Step 1: Receive Function Return Value

```python id="f47i5z"
{
    "name": "Ganesh",
    "age": 22
}
```

---

## Step 2: Pass Through Response Model

Filters fields not defined in:

```python id="g1ybkt"
UserResponse
```

---

## Step 3: Serialize to JSON

Produces final response.

---

# Why Response Models Matter

Response models help with:

* Data privacy
* Preventing accidental leaks
* Maintaining API contracts
* Keeping responses predictable

---

# Field Types and Constraints

Pydantic supports many field types and validation constraints.

---

# Example with Constraints

```python id="cyq8i6"
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str
    price: float = Field(gt=0)
    quantity: int = Field(ge=0)
```

---

# Constraint Meaning

| Constraint | Meaning               |
| ---------- | --------------------- |
| `gt`       | Greater than          |
| `lt`       | Less than             |
| `ge`       | Greater than or equal |
| `le`       | Less than or equal    |

---

# Validation Rules

In this example:

* `price` must be greater than `0`
* `quantity` must be greater than or equal to `0`

---

# Invalid Example

```json id="jrxgdi"
{
    "name": "Item",
    "price": -10,
    "quantity": 5
}
```

FastAPI automatically returns a validation error before executing the function.

---

# String Constraints

Example:

```python id="1qwe9d"
class User(BaseModel):
    name: str = Field(
        min_length=3,
        max_length=50
    )
```

---

# Common String Constraints

| Constraint   | Purpose               |
| ------------ | --------------------- |
| `min_length` | Minimum string length |
| `max_length` | Maximum string length |
| `regex`      | Pattern matching      |

---

# Documentation Integration

Constraints are automatically reflected in:

* Swagger UI
* OpenAPI schema
* ReDoc documentation

---

# Nested Models

Real-world data is often hierarchical.

Pydantic supports nested models for representing complex structures.

---

# Nested Model Example

```python id="wbq0zr"
class Address(BaseModel):
    city: str
    zip_code: str

class User(BaseModel):
    name: str
    address: Address
```

---

# Example Request

```json id="zuhvl8"
{
  "name": "Ganesh",
  "address": {
    "city": "Pune",
    "zip_code": "411001"
  }
}
```

---

# Internal Nested Validation Flow

FastAPI performs:

```text id="fd2q6m"
Validate Outer Model (User)
            ↓
Validate Nested Model (Address)
            ↓
Validate Nested Fields
            ↓
Construct Final Object
```

---

# Recursive Validation

Each nested level is validated independently.

This ensures:

* Data consistency
* Deep validation
* Structured correctness

---

# Nested Validation Error Example

```json id="vuwvvx"
{
  "detail": [
    {
      "loc": ["body", "address", "zip_code"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

---

# Why Nested Validation Matters

Nested validation is essential for:

* Real-world APIs
* Complex request bodies
* Large data structures

---

# Optional Fields and Defaults

Not all fields are required.

Pydantic supports:

* Optional fields
* Default values

---

# Optional Field Example

```python id="4gs2ax"
from typing import Optional

class User(BaseModel):
    name: str
    age: Optional[int] = None
```

---

# Field Behavior

| Field  | Required |
| ------ | -------- |
| `name` | Yes      |
| `age`  | No       |

---

# Valid Request

```json id="slv3ar"
{
    "name": "Ganesh"
}
```

This is valid because:

```python id="7it4rj"
age = None
```

by default.

---

# Default Values Example

```python id="njpq3m"
class Product(BaseModel):
    name: str
    in_stock: bool = True
```

If `in_stock` is missing:

```python id="mk4u0n"
True
```

is assigned automatically.

---

# Internal Default Assignment

FastAPI checks:

```text id="ucx9sm"
Field Present?
      ↓
Yes → Validate Value
No  → Use Default (if available)
      ↓
If No Default → Validation Error
```

---

# Serialization (Python → JSON Conversion)

After processing, data must be returned to clients as JSON.

FastAPI handles serialization automatically.

---

# Serialization Pipeline

```text id="l08uqr"
Pydantic Model
      ↓
Python Dictionary
      ↓
JSON String
      ↓
HTTP Response Body
```

---

# Example

```python id="swy0v7"
@app.get("/user")
def get_user():
    return User(
        name="Ganesh",
        age=22
    )
```

---

# Response

```json id="5x0v9t"
{
    "name": "Ganesh",
    "age": 22
}
```

---

# Internal Serialization Steps

FastAPI performs:

## Step 1: Convert Model to Dictionary

```python id="71upiu"
User → dict
```

---

## Step 2: Convert Python Types to JSON-Compatible Types

Examples:

| Python Type | JSON Representation |
| ----------- | ------------------- |
| `datetime`  | ISO string          |
| `Decimal`   | float/string        |
| `UUID`      | string              |

---

## Step 3: Serialize to JSON

Produces JSON string for HTTP response.

---

# Custom Serialization Support

FastAPI and Pydantic also support:

* Custom encoders
* Custom JSON serialization rules
* Advanced formatting

---

# End-to-End Data Flow

When a request is processed:

```text id="gtp4bk"
Client Sends JSON
        ↓
FastAPI Reads Raw Body
        ↓
JSON Parsed into Dictionary
        ↓
Pydantic Validation
        ↓
Model Instance Created
        ↓
Function Executes
        ↓
Response Model Applied
        ↓
Serialization to JSON
        ↓
HTTP Response Returned
```

---

# Key Takeaways

* Pydantic models are the foundation of data handling in FastAPI
* Request validation ensures only valid data enters the system
* Response models guarantee secure and consistent output
* Field constraints enforce precise validation rules
* Nested models support complex hierarchical data
* Optional fields and defaults provide flexibility
* Serialization converts Python objects into JSON responses
* Mastering this layer is essential for building production-ready APIs

```
