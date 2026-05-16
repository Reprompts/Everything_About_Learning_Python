# FastAPI Hands-On Tutorial

## Chapter 18: Real World Project Structure

As FastAPI applications grow, maintaining everything in a single file becomes unmanageable. A real-world project requires a modular and layered structure that separates concerns, improves readability, and enables scalability. This structure ensures that different parts of the system — API routes, business logic, data models, and configuration — are clearly organized and loosely coupled.

At a system level, a well-structured project allows teams to develop features independently, simplifies debugging, and supports long-term maintainability.

---

# Modular Architecture

Modular architecture means breaking the application into smaller, independent modules, each responsible for a specific feature or domain.

## Typical Structure

```text id="f2q8ra"
app/
│
├── main.py
├── routers/
├── services/
├── models/
├── schemas/
├── core/
├── db/
└── config/
```

Each folder represents a module with a clear responsibility.

## Example Responsibilities

* `routers/` → Handles API endpoints
* `services/` → Contains business logic
* `models/` → Defines database structure
* `schemas/` → Defines request/response validation
* `core/` → Shared utilities (security, config)
* `db/` → Database connection and setup

## Benefits

* Code is easier to navigate
* Changes in one module do not affect others directly
* Teams can work on different modules simultaneously

---

# Layered Structure

A layered architecture organizes code into logical layers, where each layer has a specific responsibility and interacts only with adjacent layers.

## Request Flow

```text id="xtoqmn"
Client
   ↓
Router
   ↓
Service
   ↓
Model / Database
   ↓
Service
   ↓
Router
   ↓
Client
```

---

# Routers (API Layer)

Routers define the API endpoints and handle HTTP-specific logic.

## Example

```python id="9j9y7j"
from fastapi import APIRouter
from app.schemas.user import UserCreate
from app.services.user_service import create_user

router = APIRouter()

@router.post("/users")
def create_user_route(user: UserCreate):
    return create_user(user)
```

## Responsibilities

* Define routes and HTTP methods
* Accept request data
* Return responses
* Delegate logic to services

## Important Principle

Routers should remain thin and should not contain business logic.

---

# Services (Business Logic Layer)

Services contain the core logic of the application. This is where actual processing, calculations, and workflows happen.

## Example

```python id="8q8xen"
from app.models.user import User

def create_user(user_data):
    user = User(
        name=user_data.name,
        age=user_data.age
    )

    # Save to database (pseudo)
    return user
```

## Responsibilities

* Process data
* Apply business rules
* Coordinate between models and external systems

## Benefits

* Reusable logic across multiple routes
* Easier testing independent of HTTP layer
* Clear separation from API layer

---

# Models (Database Layer)

Models define how data is stored in the database. These are typically SQLAlchemy models.

## Example

```python id="hkkaj6"
from sqlalchemy import Column, Integer, String
from app.db.base import Base

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    name = Column(String)
    age = Column(Integer)
```

## Responsibilities

* Define database schema
* Represent database tables as Python classes

## Important Principle

Models should not contain business logic; they are purely structural.

---

# Schemas (Validation & Serialization Layer)

Schemas define how data is validated and serialized using Pydantic.

## Example

```python id="0ah0ef"
from pydantic import BaseModel

class UserCreate(BaseModel):
    name: str
    age: int

class UserResponse(BaseModel):
    name: str
    age: int
```

## Responsibilities

* Validate incoming request data
* Define response structure
* Ensure data consistency

## Data Flow

```text id="w4xl3k"
Request
   ↓
Schema Validation
   ↓
Service
   ↓
Schema Serialization
   ↓
Response
```

This layer acts as a contract between client and server.

---

# Environment Configuration

Managing configuration is essential for real-world applications. This includes database URLs, secret keys, API keys, and environment-specific settings.

## Basic Example Using Environment Variables

```python id="p0p4t7"
import os

DATABASE_URL = os.getenv("DATABASE_URL")
SECRET_KEY = os.getenv("SECRET_KEY")
```

## Better Approach Using a Configuration Class

```python id="7m04je"
from pydantic import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str

    class Config:
        env_file = ".env"

settings = Settings()
```

## Example `.env` File

```env id="o3ph9t"
DATABASE_URL=postgresql://user:password@localhost/db
SECRET_KEY=supersecret
```

## Internal Behavior

* Values are loaded from environment variables
* `.env` file is used for local development
* Same code works across environments (dev, staging, production)

## Benefits

* Secure handling of sensitive data
* Easy environment switching
* Centralized configuration management

---

# Putting It All Together

## Example Project Flow

1. Client sends request to `/users`
2. Router receives request and validates input using schema
3. Router calls service function
4. Service processes data and interacts with model/database
5. Model maps data to database structure
6. Service returns result
7. Router formats response using schema
8. Response is sent to client

---

# Example Project Structure

```text id="rz6n5n"
app/
├── main.py
├── routers/
│   └── user.py
├── services/
│   └── user_service.py
├── models/
│   └── user.py
├── schemas/
│   └── user.py
├── core/
│   └── config.py
├── db/
│   └── session.py
└── .env
```

## `main.py`

```python id="c0mf4e"
from fastapi import FastAPI
from app.routers import user

app = FastAPI()

app.include_router(user.router)
```

---

# Advantages of This Structure

* Clear separation of concerns
* Easier to scale as application grows
* Improved code readability
* Better testability
* Supports team collaboration

---

# Common Mistakes to Avoid

* Mixing business logic inside routers
* Direct database access in routes
* Skipping schemas and using raw data
* Hardcoding configuration values
* Creating tightly coupled modules

---

# Key Takeaways

* A real-world FastAPI project should follow a modular and layered architecture to ensure scalability and maintainability.
* Routers handle HTTP logic.
* Services manage business rules.
* Models define database structure.
* Schemas ensure data validation and serialization.
* Environment configuration allows flexible and secure management of settings across different environments.
* This structured approach is essential for building production-ready, enterprise-level applications.
