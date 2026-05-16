# FastAPI Hands-On Tutorial
## Chapter 9: Authentication & Authorization

---

# Authentication & Authorization

Authentication and authorization are critical components of any production-grade API.

They answer two fundamental questions:

| Concept | Question |
|---|---|
| Authentication | Who is the user? |
| Authorization | What is the user allowed to do? |

In FastAPI, these concerns are implemented using:

- Dependency Injection
- Security Utilities
- OAuth2
- JWT Tokens
- Role-Based Access Control (RBAC)

---

# System-Level Security Flow

Every secured request follows a structured pipeline:

```text id="s8w2dk"
Client Request
       ↓
Extract Credentials
       ↓
Validate Identity
       ↓
Decode User Information
       ↓
Check Permissions
       ↓
Allow or Reject Request
````

---

# OAuth2 Fundamentals

OAuth2 is an industry-standard protocol for authentication and authorization.

It defines:

* How users obtain access tokens
* How tokens are used to access protected resources

---

# Common OAuth2 Flow in FastAPI

The most commonly used OAuth2 flow in FastAPI is:

```text id="d6k9pl"
OAuth2 Password Flow
```

In this flow:

1. User sends username and password
2. Server validates credentials
3. Server generates an access token
4. Client sends token with future requests

---

# OAuth2 Setup

```python id="m4t8qy"
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)
```

---

# What `OAuth2PasswordBearer` Does

This tells FastAPI to:

* Expect an `Authorization` header
* Extract the Bearer token
* Provide the token to dependencies

---

# Example Usage

```python id="k9r3wf"
from fastapi import Depends

@app.get("/protected")
def protected_route(
    token: str = Depends(oauth2_scheme)
):
    return {"token": token}
```

---

# Internal OAuth2 Processing

```text id="h7v2cx"
Read Authorization Header
          ↓
Extract Bearer Token
          ↓
Inject Token into Dependency
          ↓
Continue Request Processing
```

---

# Missing Token Behavior

If the `Authorization` header is missing:

```text id="f5n8rm"
FastAPI automatically returns:

401 Unauthorized
```

---

# Important OAuth2 Concept

OAuth2 defines:

```text id="u3m7zd"
How tokens are obtained and transmitted
```

It does **not** define:

```text id="r8p4lv"
How tokens are structured internally
```

This is where JWT is commonly used.

---

# JWT Tokens

JWT stands for:

```text id="c2w9ks"
JSON Web Token
```

A JWT is a compact, self-contained token used to securely transmit identity information.

---

# Structure of a JWT

A JWT contains three parts:

| Part      | Purpose                  |
| --------- | ------------------------ |
| Header    | Algorithm and token type |
| Payload   | User data and claims     |
| Signature | Verifies integrity       |

---

# Example JWT Payload

```json id="v8q1pe"
{
  "sub": "ganesh",
  "exp": 1710000000
}
```

---

# Common JWT Claims

| Claim  | Meaning                 |
| ------ | ----------------------- |
| `sub`  | Subject (user identity) |
| `exp`  | Expiration timestamp    |
| `iat`  | Issued-at time          |
| `role` | User role               |

---

# Creating a JWT

```python id="w4m6tj"
from jose import jwt
from datetime import datetime, timedelta

SECRET_KEY = "secret"
ALGORITHM = "HS256"

def create_access_token(data: dict):

    to_encode = data.copy()

    expire = datetime.utcnow() + timedelta(minutes=30)

    to_encode.update({"exp": expire})

    return jwt.encode(
        to_encode,
        SECRET_KEY,
        algorithm=ALGORITHM
    )
```

---

# Internal JWT Creation Process

```text id="q6n2hy"
Payload Created
      ↓
Expiration Added
      ↓
Payload Signed with Secret Key
      ↓
JWT Token Generated
```

---

# Decoding and Verifying JWT

```python id="p3x8vf"
from jose import JWTError

def verify_token(token: str):

    try:
        payload = jwt.decode(
            token,
            SECRET_KEY,
            algorithms=[ALGORITHM]
        )

        return payload

    except JWTError:
        return None
```

---

# Internal JWT Verification Flow

```text id="g5r9ub"
Receive Token
      ↓
Verify Signature
      ↓
Check Expiration
      ↓
Decode Payload
      ↓
Extract User Identity
```

---

# Why JWT Is Popular

JWT enables:

* Stateless authentication
* Scalability
* Reduced server memory usage
* Easy distribution across services

---

# Stateless Authentication

With JWT:

```text id="t4k8pm"
The server does not store user sessions
```

All identity information exists inside the token itself.

---

# Password Hashing (bcrypt)

Passwords should **never** be stored as plain text.

Instead, passwords must be:

```text id="m9c3xr"
Hashed before storage
```

---

# What Is bcrypt?

bcrypt is a password hashing algorithm designed specifically for secure password storage.

It provides:

* Salting
* Slow hashing
* Brute-force resistance

---

# Password Hashing Example

```python id="n2w7dk"
from passlib.context import CryptContext

pwd_context = CryptContext(
    schemes=["bcrypt"],
    deprecated="auto"
)

def hash_password(password: str):
    return pwd_context.hash(password)

def verify_password(
    plain_password: str,
    hashed_password: str
):
    return pwd_context.verify(
        plain_password,
        hashed_password
    )
```

---

# Registration Workflow

```text id="k8m5qh"
User Registers
       ↓
Password Hashed
       ↓
Only Hashed Password Stored
```

---

# Login Workflow

```text id="x5v9pd"
User Enters Password
        ↓
Input Password Hashed
        ↓
Compared Against Stored Hash
        ↓
Authentication Success or Failure
```

---

# Why bcrypt Is Secure

bcrypt internally uses:

* Salt generation
* Multiple hashing rounds
* Computationally expensive algorithms

This makes brute-force attacks significantly harder.

---

# Important Security Property

Even identical passwords produce:

```text id="z4j7rl"
Different hashes
```

because each hash includes a unique salt.

---

# Role-Based Access Control (RBAC)

RBAC restricts access based on user roles.

Examples:

* Admin
* User
* Moderator

---

# Example User Structure

```json id="r1k8yc"
{
  "username": "ganesh",
  "role": "admin"
}
```

---

# Role Validation Dependency

```python id="h6q2tn"
from fastapi import HTTPException

def require_role(required_role: str):

    def role_checker(
        user = Depends(get_current_user)
    ):

        if user["role"] != required_role:

            raise HTTPException(
                status_code=403,
                detail="Forbidden"
            )

        return user

    return role_checker
```

---

# Using RBAC

```python id="f9m4ze"
@app.get("/admin")
def admin_dashboard(
    user = Depends(require_role("admin"))
):
    return {"message": "Welcome admin"}
```

---

# Internal RBAC Flow

```text id="v3p8cw"
Extract User Identity
         ↓
Read User Role
         ↓
Compare with Required Role
         ↓
Allow or Reject Access
```

---

# Why RBAC Matters

RBAC provides:

* Centralized access control
* Clear permission management
* Better scalability
* Separation of authentication and authorization

---

# Securing Endpoints

Securing endpoints combines:

* Authentication
* Token verification
* Authorization checks

into the request lifecycle.

---

# Complete Security Example

```python id="j8n6rx"
from fastapi import (
    FastAPI,
    Depends,
    HTTPException
)

from fastapi.security import OAuth2PasswordBearer

from jose import jwt, JWTError

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)

SECRET_KEY = "secret"
ALGORITHM = "HS256"

def get_current_user(
    token: str = Depends(oauth2_scheme)
):

    try:
        payload = jwt.decode(
            token,
            SECRET_KEY,
            algorithms=[ALGORITHM]
        )

        return {
            "username": payload.get("sub"),
            "role": payload.get("role")
        }

    except JWTError:

        raise HTTPException(
            status_code=401,
            detail="Invalid token"
        )

def require_admin(
    user = Depends(get_current_user)
):

    if user["role"] != "admin":

        raise HTTPException(
            status_code=403,
            detail="Forbidden"
        )

    return user

@app.get("/secure")
def secure_route(
    user = Depends(get_current_user)
):
    return {"user": user}

@app.get("/admin")
def admin_route(
    user = Depends(require_admin)
):
    return {"message": "Admin access granted"}
```

---

# Step-by-Step Request Flow

```text id="s2x7km"
Client Sends Authorization Header
                ↓
OAuth2 Dependency Extracts Token
                ↓
JWT Is Decoded and Verified
                ↓
User Identity Is Constructed
                ↓
RBAC Dependency Checks Permissions
                ↓
Route Executes if Authorized
                ↓
Response Returned
```

---

# Error Handling in Security

| Situation                | Status Code        |
| ------------------------ | ------------------ |
| Missing token            | `401 Unauthorized` |
| Invalid token            | `401 Unauthorized` |
| Insufficient permissions | `403 Forbidden`    |

---

# End-to-End Authentication Lifecycle

## Registration

```text id="w7n4cp"
User Registers
      ↓
Password Hashed
      ↓
Stored in Database
```

---

## Login

```text id="e3v8qy"
Credentials Verified
        ↓
JWT Token Generated
        ↓
Token Sent to Client
```

---

## Protected Request

```text id="p5m1tk"
Client Sends JWT
        ↓
Server Verifies Token
        ↓
User Identity Extracted
        ↓
Authorization Checks Applied
        ↓
Request Allowed or Denied
```

---

# System-Level Security Architecture

| Component            | Responsibility           |
| -------------------- | ------------------------ |
| OAuth2               | Token transport protocol |
| JWT                  | Identity representation  |
| bcrypt               | Password security        |
| Dependency Injection | Authentication workflow  |
| RBAC                 | Permission enforcement   |

---

# Best Practices

## Never Store Plain Passwords

Always hash passwords using:

```text id="y8r3vl"
bcrypt or equivalent secure algorithms
```

---

## Use HTTPS

JWT tokens must always be transmitted securely.

---

## Set Token Expiration

Tokens should expire after a limited time.

---

## Separate Authentication and Authorization

Keep identity verification separate from permission checks.

---

## Use Role-Based Dependencies

Centralize permission logic instead of duplicating checks.

---

# Key Takeaways

* Authentication identifies users
* Authorization determines permissions
* OAuth2 defines token-based authentication workflows
* JWT provides stateless identity transmission
* bcrypt securely hashes passwords
* RBAC enforces role-based permissions
* Dependency injection powers FastAPI security workflows
* Proper authentication architecture is essential for secure, scalable, production-grade APIs

```
