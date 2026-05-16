# FastAPI Hands-On Tutorial
## Chapter 12: API Documentation

# API Documentation

API documentation is a fundamental part of building usable and maintainable services. In FastAPI, documentation is not an afterthought; it is generated automatically from your code using standards like OpenAPI. This ensures that the documentation always stays in sync with the actual implementation.

At a system level, FastAPI inspects your route definitions, type hints, request models, response models, and metadata to construct an OpenAPI schema. This schema is then used to render interactive documentation interfaces such as Swagger UI and ReDoc.

---

# Auto-generated Documentation

FastAPI automatically generates API documentation based on your code structure. This is possible because of:

* Python type hints
* Pydantic models
* Route definitions and metadata

## Example

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    age: int

@app.post("/users")
def create_user(user: User):
    return user
```

From this code, FastAPI generates:

* Request schema (based on `User` model)
* Response schema (based on return type)
* Endpoint details (method, path)
* Validation rules

## Internal Workflow

Internally:

1. FastAPI builds an OpenAPI JSON schema at runtime
2. This schema describes all endpoints, parameters, and data structures
3. The schema is accessible at:

```text
/openapi.json
```

## Benefits

This approach ensures:

* No duplication between code and documentation
* Always up-to-date API specifications
* Machine-readable format for tools and integrations

---

# Swagger UI

Swagger UI is an interactive web interface that allows you to explore and test API endpoints directly from the browser.

## Default URL

```text
/docs
```

## Features

* Lists all available endpoints
* Shows request parameters and schemas
* Allows sending real requests
* Displays responses in real time

## Example Interaction

1. Select `POST /users`
2. Click `"Try it out"`
3. Enter JSON input
4. Execute request
5. View response instantly

## Internal Workflow

Internally:

* Swagger UI reads the OpenAPI schema
* Dynamically renders forms and endpoint details
* Sends HTTP requests using JavaScript

## Use Cases

This makes it extremely useful for:

* Testing APIs during development
* Debugging endpoints
* Sharing APIs with frontend developers

---

# ReDoc

ReDoc is another documentation interface provided by FastAPI, focused on readability and structured presentation.

## Default URL

```text
/redoc
```

## Features

* Clean, three-panel layout
* Detailed schema visualization
* Better suited for production documentation

## Swagger UI vs ReDoc

| Swagger UI                      | ReDoc                                     |
| ------------------------------- | ----------------------------------------- |
| Interactive and testing-focused | Documentation-focused and highly readable |
| Best for development/testing    | Best for production docs                  |

## Internal Workflow

Internally:

* ReDoc also consumes the same OpenAPI schema
* Renders it in a more structured and hierarchical format

This allows teams to provide both:

* Interactive testing interface (Swagger)
* Clean documentation view (ReDoc)

---

# Customizing Documentation

FastAPI allows customization of API documentation to match project requirements.

---

# Custom Metadata

You can define metadata when creating the FastAPI app:

```python
app = FastAPI(
    title="User API",
    description="API for managing users",
    version="1.0.0"
)
```

This metadata appears in both Swagger UI and ReDoc.

---

# Adding Tags

Tags help group endpoints logically.

```python
@app.get("/users", tags=["Users"])
def get_users():
    return []
```

This organizes endpoints into sections in the documentation.

---

# Custom Descriptions

You can add detailed descriptions to endpoints:

```python
@app.post(
    "/users",
    summary="Create user",
    description="Creates a new user in the system"
)
def create_user():
    return {"message": "created"}
```

This improves clarity for API consumers.

---

# Custom Response Documentation

You can define expected responses:

```python
from fastapi import status

@app.get(
    "/users/{user_id}",
    responses={
        200: {"description": "Successful response"},
        404: {"description": "User not found"}
    }
)
def get_user(user_id: int):
    return {"user_id": user_id}
```

This ensures that documentation reflects all possible outcomes.

---

# Customizing Docs URLs

You can change or disable documentation endpoints:

```python
app = FastAPI(
    docs_url="/documentation",
    redoc_url="/redoc-ui",
    openapi_url="/api-schema.json"
)
```

Or disable them completely:

```python
app = FastAPI(
    docs_url=None,
    redoc_url=None
)
```

## Why This Matters

This is useful for production environments where documentation access may need to be restricted.

---

# Including Examples

You can provide example data for better usability.

```python
class User(BaseModel):
    name: str
    age: int

    class Config:
        schema_extra = {
            "example": {
                "name": "Ganesh",
                "age": 22
            }
        }
```

This example appears in Swagger UI, making it easier for users to understand request formats.

---

# OpenAPI Schema Customization

FastAPI allows direct modification of the OpenAPI schema if needed.

```python
def custom_openapi():
    if app.openapi_schema:
        return app.openapi_schema

    openapi_schema = app.openapi()

    openapi_schema["info"]["x-logo"] = {
        "url": "https://example.com/logo.png"
    }

    app.openapi_schema = openapi_schema

    return app.openapi_schema

app.openapi = custom_openapi
```

## Use Cases

This enables:

* Adding custom fields
* Integrating with external tools
* Branding documentation

---

# End-to-End Documentation Flow

When the application starts:

1. FastAPI scans all routes and models
2. Builds OpenAPI schema dynamically
3. Exposes schema at `/openapi.json`
4. Swagger UI and ReDoc consume this schema
5. Documentation is rendered in browser
6. Users can explore and test endpoints interactively

---

# Key Takeaways

* FastAPI provides automatic, standards-based API documentation using OpenAPI
* Swagger UI offers an interactive interface for testing endpoints
* ReDoc provides a clean and structured documentation view
* Customization options allow tailoring documentation with metadata, tags, examples, and response definitions
* OpenAPI schema generation ensures documentation always stays synchronized with application code
* Proper API documentation improves developer experience, testing efficiency, and long-term maintainability
