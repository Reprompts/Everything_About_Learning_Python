# FastAPI Hands-On Tutorial

## Chapter 14: Testing FastAPI Applications

Testing is an essential part of building reliable APIs. It ensures that endpoints behave correctly, edge cases are handled, and future changes do not break existing functionality. In FastAPI, testing is designed to be simple yet powerful by leveraging tools like Pytest and an inbuilt test client.

At a system level, testing simulates real HTTP requests without running an actual server. Instead of sending requests over the network, tests interact directly with the application through an internal interface, making them fast and deterministic.

---

# Unit Testing

Unit testing focuses on testing individual components of the application in isolation. In FastAPI, this typically means testing route handlers, utility functions, and business logic independently.

## Example Route

```python id="t1f8a3"
@app.get("/add")
def add(a: int, b: int):
    return {"result": a + b}
```

## Unit Test Example

```python id="u7m2x5"
def test_add():
    assert add(2, 3) == {"result": 5}
```

## What Happens Here

* The function is tested directly
* No HTTP request is involved
* Execution is fast and isolated

## System-Level Importance

Unit testing:

* Identifies logic errors early
* Ensures correctness of individual components
* Makes debugging easier

However, this does not test the full request lifecycle. For that, a test client is used.

---

# Test Client Usage

FastAPI provides a test client built on top of Starlette's `TestClient`. It allows you to simulate HTTP requests to your API without running a server.

## Setup

```python id="c4k9r1"
from fastapi.testclient import TestClient

client = TestClient(app)
```

## Basic Example

```python id="p6v3n8"
def test_get_root():
    response = client.get("/")

    assert response.status_code == 200
    assert response.json() == {"message": "Hello"}
```

## Internal Behavior

Internally:

1. The test client sends a request to the ASGI application
2. FastAPI processes it as if it were a real HTTP request
3. A response object is returned
4. No actual network communication occurs

## Supported Methods

* `client.get()`
* `client.post()`
* `client.put()`
* `client.delete()`

---

# Testing Request Bodies

## Example

```python id="w8q5d2"
def test_create_user():
    response = client.post(
        "/users",
        json={"name": "Ganesh", "age": 22}
    )

    assert response.status_code == 200
    assert response.json()["name"] == "Ganesh"
```

## What This Tests

* Request parsing
* Validation
* Route execution
* Response serialization

---

# Using Pytest

Pytest is the standard testing framework used with FastAPI. It provides a simple and powerful way to write and organize tests.

---

# Basic Test File Structure

```text id="n2x7l4"
test_main.py
```

## Example Test

```python id="r5m1u9"
def test_example():
    assert 1 + 1 == 2
```

## Running Tests

```bash id="h9c3z6"
pytest
```

---

# Pytest Features

Pytest provides:

* Automatic test discovery
* Simple assertion syntax
* Fixtures for setup and teardown
* Detailed failure reports

---

# Multiple Test Examples

```python id="f7k2q8"
def test_read_items():
    response = client.get("/items")

    assert response.status_code == 200

def test_invalid_route():
    response = client.get("/invalid")

    assert response.status_code == 404
```

## Internal Workflow

Internally:

1. Pytest scans files starting with `test_`
2. Executes functions starting with `test_`
3. Collects and reports results

---

# Fixtures for Setup and Reusability

Fixtures allow reusable setup logic for tests.

## Example Fixture

```python id="y4n6p1"
import pytest

@pytest.fixture
def sample_user():
    return {
        "name": "Ganesh",
        "age": 22
    }
```

## Usage Example

```python id="g8t3w5"
def test_user(sample_user):
    assert sample_user["name"] == "Ganesh"
```

## Common Fixture Use Cases

Fixtures are useful for:

* Creating test data
* Setting up database connections
* Initializing test clients

---

# Mocking Dependencies

In real applications, dependencies such as databases, external APIs, or authentication systems should not be used directly in tests. Instead, they are replaced with mock implementations.

FastAPI provides a way to override dependencies.

---

# Original Dependency

```python id="q1r8m4"
def get_db():
    return "real_database"
```

# Override in Tests

```python id="v3p7k2"
def override_get_db():
    return "test_database"

app.dependency_overrides[get_db] = override_get_db
```

# Test Example

```python id="s5n9u1"
def test_db_override():
    response = client.get("/db-check")

    assert response.json() == {
        "db": "test_database"
    }
```

## Internal Process

Internally:

1. FastAPI checks for dependency overrides
2. If an override exists, it is used instead of the original dependency

## Benefits

This allows:

* Isolation from external systems
* Faster and more reliable tests
* Controlled test environments

---

# Mocking Authentication

Authentication dependencies can also be mocked.

## Original Dependency

```python id="b2w6q9"
def get_current_user():
    raise Exception("Not authenticated")
```

## Override

```python id="m8k4x7"
def mock_user():
    return {"username": "test_user"}

app.dependency_overrides[get_current_user] = mock_user
```

Now all protected routes behave as if the user is authenticated.

---

# Testing Error Cases

Testing should also include failure scenarios.

## Not Found Example

```python id="z7t1c5"
def test_not_found():
    response = client.get("/users/999")

    assert response.status_code == 404
```

## Validation Error Example

```python id="l4p9v3"
def test_validation_error():
    response = client.post(
        "/users",
        json={"name": "Ganesh"}
    )

    assert response.status_code == 422
```

## Why This Matters

This ensures:

* Proper error handling
* Correct status codes
* Reliable API behavior

---

# End-to-End Test Flow

When a test runs:

1. Pytest discovers the test function
2. Test client sends request to FastAPI app
3. Request goes through middleware and dependencies
4. Validation and route execution occur
5. Response is generated
6. Test asserts expected outcome

This simulates real API usage without external dependencies.

---

# Best Practices

* Keep tests independent
* Ensure one test does not affect another
* Use dependency overrides to isolate external systems
* Test both success and failure scenarios
* Use fixtures to reduce repetition
* Keep tests fast and deterministic
* Run tests frequently during development

---

# Key Takeaways

* Testing ensures API reliability and stability
* Unit testing validates isolated business logic
* `TestClient` simulates real HTTP requests without running a server
* Pytest provides a powerful framework for organizing and executing tests
* Dependency overrides help isolate external systems during testing
* Error handling and validation should always be tested
* Well-structured tests improve maintainability and confidence in production systems
