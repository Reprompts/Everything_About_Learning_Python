# Flask Hands-On Tutorial

## Chapter 1: Introduction

Flask is one of the most widely used web frameworks in Python, known for its simplicity, flexibility, and minimalistic design. It is often referred to as a **micro-framework** because it provides only the core components required to build a web application, leaving developers free to choose additional tools and libraries as needed.

At a system level, Flask operates on the **WSGI (Web Server Gateway Interface)** standard, which defines how web servers communicate with Python applications. This is different from modern asynchronous frameworks, but it remains highly effective for many use cases.

---

# What is Flask

Flask is a lightweight web framework designed to make it easy to build web applications and APIs quickly. It provides essential features such as routing, request handling, and response generation, while avoiding unnecessary complexity.

## Basic Example

```python id="9w3mrf"
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Flask!"
```

## Running the Application

```bash id="2vbk6h"
python app.py
```

Or using the Flask CLI:

```bash id="x7k1la"
flask run
```

## Internal Workflow

1. A request is sent from the client (browser or API client)
2. The WSGI server forwards the request to the Flask application
3. Flask matches the URL to a route
4. The corresponding function is executed
5. A response is generated and returned to the client

Flask uses:

* **Werkzeug** → WSGI toolkit for request handling
* **Jinja2** → Templating engine

## Key Characteristics

* Minimal core with extensibility
* Explicit configuration
* Full control over components
* Suitable for small to medium applications and APIs

---

# Flask vs FastAPI Comparison

Flask and FastAPI serve similar purposes but are built on different philosophies and technologies.

## Execution Model

* Flask is synchronous and based on WSGI
* FastAPI is asynchronous and based on ASGI

## Performance

* Flask handles one request per thread (blocking)
* FastAPI supports non-blocking I/O and higher concurrency

## Example Comparison

### Flask Route

```python id="dtnu5n"
@app.route("/data")
def get_data():
    return {"message": "Flask"}
```

### FastAPI Route

```python id="px11z0"
@app.get("/data")
async def get_data():
    return {"message": "FastAPI"}
```

## Data Validation

* Flask does not provide built-in validation
* FastAPI uses Pydantic for automatic validation and serialization

## Documentation

* Flask requires external tools like Swagger or Postman documentation
* FastAPI auto-generates OpenAPI docs (Swagger UI, ReDoc)

## Typing Support

* Flask does not enforce type hints
* FastAPI heavily relies on type hints for validation and documentation

## Use Cases

### Flask is suitable for:

* Simple APIs
* Traditional web applications
* Projects requiring full control over components

### FastAPI is suitable for:

* High-performance APIs
* Async applications
* Systems requiring automatic validation and documentation

## System-Level Difference

* Flask uses thread-based concurrency
* FastAPI uses event loop-based concurrency

---

# Installation and Setup

Flask is easy to install and set up.

---

# Step 1: Create Virtual Environment

```bash id="g0huxz"
python -m venv venv
```

## Activate Environment

### Windows

```bash id="h34xvq"
venv\Scripts\activate
```

### Linux/macOS

```bash id="3yz1yj"
source venv/bin/activate
```

---

# Step 2: Install Flask

```bash id="d0w9pm"
pip install flask
```

---

# Step 3: Create Application File

Create `app.py`:

```python id="c6g2zj"
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return {"message": "Hello, Flask"}
```

---

# Step 4: Run the Application

## Set Environment Variable

### Windows

```bash id="w7qgmj"
set FLASK_APP=app.py
```

### Linux/macOS

```bash id="8gvt9i"
export FLASK_APP=app.py
```

## Run Server

```bash id="db7ms4"
flask run
```

## Output

```text id="rskx1l"
Running on http://127.0.0.1:5000/
```

---

# Development vs Production Setup

## Development Server

* Provided by Flask
* Not suitable for production
* Single-threaded and not optimized

## Production Setup

Use WSGI servers like:

* Gunicorn
* uWSGI

### Example

```bash id="td2l9x"
gunicorn app:app
```

## Internal Difference

* Development server is simple and for testing
* Production server handles concurrency, stability, and performance

---

# Request Handling Internals

When a request is made:

1. Web server (Gunicorn/uWSGI) receives request
2. Passes it to Flask via WSGI
3. Flask creates a request context
4. Route is matched
5. View function executes
6. Response is generated
7. Response is returned to client

Flask uses:

* **Request object** → Incoming data
* **Response object** → Outgoing data
* **Context stack** → Manages request lifecycle

---

# Extensibility

Flask does not include many features by default, but extensions provide additional functionality.

## Examples

* `Flask-SQLAlchemy` → Database ORM
* `Flask-JWT-Extended` → Authentication
* `Flask-Migrate` → Database migrations

## Benefits of This Modular Approach

* Choose only required components
* Avoid unnecessary overhead
* Build customized architectures

---

# Project Structure (Basic)

```text id="7lbw6z"
project/
├── app.py
├── venv/
└── requirements.txt
```

As the application grows, it can be modularized similarly to FastAPI.

---

# Key Takeaways

* Flask is a lightweight and flexible Python web framework built on WSGI, designed for simplicity and control.
* It provides essential tools for building web applications while allowing developers to choose additional components as needed.
* Compared to FastAPI, Flask is synchronous and more minimal, while FastAPI offers built-in validation, async support, and automatic documentation.
* Flask remains a strong choice for many applications, especially where simplicity and flexibility are priorities.
