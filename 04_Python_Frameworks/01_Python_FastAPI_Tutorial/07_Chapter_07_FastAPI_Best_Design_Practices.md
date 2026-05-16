# FastAPI Hands-On Tutorial
## Chapter 7: REST API Design Best Practices

---

# REST API Design Best Practices

Designing a REST API is not just about making endpoints work.

A well-designed API should be:

- Consistent
- Predictable
- Scalable
- Maintainable

Good API design:

- Reduces ambiguity
- Improves usability
- Simplifies integrations
- Enables long-term evolution

This section focuses on practical REST patterns and the system-level reasoning behind them.

---

# Resource Naming Conventions

REST APIs are centered around **resources**, not actions.

A resource represents an entity such as:

- User
- Product
- Order

Endpoints should therefore use **nouns**, not verbs.

---

# Correct REST Design

```http id="f4mn5q"
GET    /users
POST   /users
GET    /users/1
DELETE /users/1
````

---

# Incorrect REST Design

```http id="r8q1ph"
GET  /getUsers
POST /createUser
```

---

# Why Verbs Are Incorrect

HTTP methods already define the action.

Example:

| HTTP Method | Action   |
| ----------- | -------- |
| `GET`       | Retrieve |
| `POST`      | Create   |
| `PUT`       | Replace  |
| `DELETE`    | Remove   |

Adding verbs inside URLs creates:

* Redundancy
* Inconsistency
* Poor API readability

---

# Naming Conventions

Recommended practices:

* Use plural nouns
* Use lowercase paths
* Use hyphens for readability
* Avoid unnecessary nesting

---

# Good Examples

```http id="s6e5np"
/users
/products
/order-items
```

---

# Nested Resource Example

```http id="o1v7mk"
GET /users/1/orders
```

This indicates:

```text id="0flq6k"
Orders belong to a specific user
```

---

# System-Level Importance

Consistent naming improves:

* Routing clarity
* Documentation generation
* Client predictability
* API discoverability

---

# API Versioning

APIs evolve over time.

Changes may break existing clients.

Versioning allows new functionality to be introduced without breaking older integrations.

---

# Common Versioning Strategies

## URL-Based Versioning

```http id="8bz4lf"
GET /v1/users
GET /v2/users
```

---

## Header-Based Versioning

```http id="m0k4vx"
GET /users

Accept: application/vnd.api.v1+json
```

---

## Query Parameter Versioning

```http id="l6a7rj"
GET /users?version=1
```

---

# Most Common Approach

The most widely used strategy is:

```text id="7x5d3v"
URL-based versioning
```

because it is:

* Explicit
* Easy to manage
* Easy to document

---

# FastAPI Versioning Example

```python id="t8w0ln"
@app.get("/v1/users")
def get_users_v1():
    return {"version": "v1"}

@app.get("/v2/users")
def get_users_v2():
    return {"version": "v2"}
```

---

# Why Versioning Matters

Versioning helps:

* Isolate breaking changes
* Prevent client failures
* Enable gradual migrations
* Avoid downtime during upgrades

---

# Best Practice

Only create a new API version when introducing:

```text id="3j6n0y"
Backward-incompatible changes
```

---

# Pagination

Pagination is essential for handling large datasets efficiently.

Returning all records at once can cause:

* High memory usage
* Slow responses
* Database performance issues

---

# Basic Pagination Example

```python id="c8f4oy"
@app.get("/users")
def get_users(
    limit: int = 10,
    offset: int = 0
):
    return {
        "limit": limit,
        "offset": offset
    }
```

---

# Example Request

```http id="d8v9lu"
/users?limit=10&offset=20
```

Meaning:

```text id="t5k2ec"
Skip first 20 records
Return next 10 records
```

---

# Alternative Pagination Strategy

Page-based pagination:

```http id="k4o3pl"
/users?page=2&size=10
```

---

# Why Pagination Matters

Pagination:

* Reduces response size
* Improves database efficiency
* Reduces network transfer
* Enables scalable APIs

---

# Recommended Pagination Metadata

Good APIs should return metadata:

```json id="v4m6tz"
{
  "data": [...],
  "total": 100,
  "limit": 10,
  "offset": 20
}
```

---

# Why Metadata Helps

This enables clients to build:

* Pagination UIs
* Infinite scroll systems
* Navigation controls

---

# Filtering and Sorting

Filtering and sorting allow clients to retrieve only the data they need.

This avoids unnecessary client-side processing.

---

# Filtering Example

```python id="d4k8nf"
@app.get("/products")
def get_products(
    category: str = None,
    in_stock: bool = None
):
    return {
        "category": category,
        "in_stock": in_stock
    }
```

---

# Example Request

```http id="p3h6cx"
/products?category=electronics&in_stock=true
```

---

# Sorting Examples

```http id="q1r9ep"
/products?sort=price
/products?sort=-price
```

---

# Sorting Convention

| Query         | Meaning    |
| ------------- | ---------- |
| `sort=price`  | Ascending  |
| `sort=-price` | Descending |

---

# System-Level Importance

Filtering and sorting:

* Reduce unnecessary data transfer
* Push computation closer to the database
* Improve overall performance
* Simplify client applications

---

# Best Practice

Filtering and sorting should map efficiently to:

```text id="5h2t2m"
Database queries
```

rather than filtering in memory.

---

# Idempotency

An operation is **idempotent** if performing it multiple times produces the same result.

---

# Idempotent Methods

| Method   | Idempotent |
| -------- | ---------- |
| `GET`    | Yes        |
| `PUT`    | Yes        |
| `DELETE` | Yes        |
| `POST`   | No         |

---

# Example: PUT

```python id="r0f7mx"
@app.put("/users/{user_id}")
def update_user(user_id: int):
    return {"updated": user_id}
```

Calling this repeatedly results in the same resource state.

---

# Why Idempotency Matters

Idempotency is critical because it enables:

* Safe retries
* Fault tolerance
* Distributed system reliability
* Load balancer retry support

---

# Non-Idempotent Example

```text id="r6m7zh"
POST /users
```

Repeated requests may create duplicate resources.

---

# Idempotency Keys

In critical systems, APIs may use:

```text id="q5z2el"
Idempotency Keys
```

to make POST operations behave safely during retries.

---

# Proper Use of Status Codes

HTTP status codes communicate the result of an operation.

They are essential for reliable client-server communication.

---

# Example

```python id="x9w4du"
from fastapi import HTTPException, status

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

# Common Status Codes

| Status Code                 | Meaning                 |
| --------------------------- | ----------------------- |
| `200 OK`                    | Successful request      |
| `201 Created`               | Resource created        |
| `204 No Content`            | Success without body    |
| `400 Bad Request`           | Invalid request         |
| `401 Unauthorized`          | Authentication required |
| `403 Forbidden`             | Access denied           |
| `404 Not Found`             | Resource missing        |
| `422 Unprocessable Entity`  | Validation error        |
| `500 Internal Server Error` | Server failure          |

---

# Proper POST Example

```python id="v5g2cf"
@app.post(
    "/users",
    status_code=201
)
def create_user():
    return {"message": "User created"}
```

---

# Why Status Codes Matter

Correct status codes enable:

* Proper client handling
* Automated retries
* Error detection
* Monitoring and observability
* Better debugging

---

# Problems with Incorrect Status Codes

Incorrect responses create:

* Client confusion
* Unreliable integrations
* Broken retry logic
* Difficult debugging

---

# End-to-End Design Example

```python id="r4t7pl"
@app.get("/v1/products")
def get_products(
    limit: int = 10,
    offset: int = 0,
    category: str = None,
    sort: str = None
):
    return {
        "limit": limit,
        "offset": offset,
        "category": category,
        "sort": sort
    }
```

---

# Example Request

```http id="m8j5dr"
/v1/products?limit=10&offset=20&category=electronics&sort=-price
```

---

# REST Features Demonstrated

| Feature         | Example           |
| --------------- | ----------------- |
| Versioning      | `/v1`             |
| Resource Naming | `/products`       |
| Pagination      | `limit`, `offset` |
| Filtering       | `category`        |
| Sorting         | `sort=-price`     |

---

# System-Level Request Flow

```text id="x7f2md"
Client Request
       ↓
Route Matching
       ↓
Query Parameter Parsing
       ↓
Validation & Type Conversion
       ↓
Business Logic Execution
       ↓
Database Query Optimization
(Filter/Sort/Pagination)
       ↓
Response Generation
       ↓
HTTP Status Code Returned
```

---

# Key Takeaways

* REST APIs should focus on resources, not actions
* Resource naming should use nouns and plural forms
* Versioning protects backward compatibility
* Pagination improves scalability and performance
* Filtering and sorting reduce unnecessary data transfer
* Idempotency enables safe retries in distributed systems
* Proper status codes are critical for reliable communication
* Consistency and predictability are essential for production-grade APIs
* Good API design improves maintainability, usability, and long-term scalability

```
