# Flask Hands-On Tutorial
## Chapter 2: Core Flask

## Core Flask Basics

Understanding the core building blocks of Flask is essential before moving into advanced features. These fundamentals define how a Flask application receives requests, processes them, and sends responses back to the client.

At a system level, Flask acts as a WSGI application that maps incoming HTTP requests to Python functions (views), processes data using request objects, and generates responses that are returned through the web server.

---

# Routes and Views

In Flask, a route defines a URL pattern, and a view function defines what happens when that URL is accessed.

### Basic Example

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Flask!"
```

### Explanation

* `@app.route("/")` maps the root URL to the `home` function
* When a client accesses `/`, Flask calls the `home` function
* The return value is converted into an HTTP response

---

## Handling Different HTTP Methods

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/users", methods=["GET", "POST"])
def users():
    if request.method == "GET":
        return {"message": "Get users"}
    elif request.method == "POST":
        return {"message": "Create user"}
```

### Internally

* Flask inspects the incoming request method
* Matches it against allowed methods
* Calls the corresponding logic in the view function

### Key Idea

* **Routes define where**
* **Views define what happens**

---

# Request and Response Objects

Flask provides a global `request` object to access incoming data and a `response` object to control outgoing data.

---

## Request Object

### Example

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/data", methods=["POST"])
def get_data():
    data = request.json
    return {"received": data}
```

### The `request` Object Contains

| Attribute         | Description      |
| ----------------- | ---------------- |
| `request.method`  | HTTP method      |
| `request.args`    | Query parameters |
| `request.form`    | Form data        |
| `request.json`    | JSON body        |
| `request.headers` | Request headers  |

### Internal Behavior

* Flask creates a request object for each incoming request
* It is stored in a thread-local context
* Accessible globally within the request lifecycle

---

## Response Object

Flask automatically converts return values into responses, but you can also customize them.

### Example

```python
from flask import Flask, make_response

app = Flask(__name__)

@app.route("/custom")
def custom_response():
    response = make_response({"message": "Hello"}, 200)
    response.headers["X-Custom-Header"] = "Value"
    return response
```

### Here

* `make_response` creates a response object
* Status code and headers can be modified

### Response Components

* Body (data)
* Status code
* Headers

---

# URL Routing

Flask supports dynamic URLs using variable rules.

### Example

```python
from flask import Flask

app = Flask(__name__)

@app.route("/users/<int:user_id>")
def get_user(user_id):
    return {"user_id": user_id}
```

### Here

* `<int:user_id>` captures part of the URL
* Converts it to an integer
* Passes it to the function

---

## Supported Converters

| Converter | Description                 |
| --------- | --------------------------- |
| `string`  | Default string converter    |
| `int`     | Integer values              |
| `float`   | Floating-point values       |
| `path`    | Full path including slashes |

---

## Example with `path`

```python
@app.route("/files/<path:filename>")
def get_file(filename):
    return {"file": filename}
```

### Internal Routing Process

* Flask maintains a routing table
* Incoming URL is matched against patterns
* Extracted parameters are passed to the view function

---

## URL Building

```python
from flask import url_for

url = url_for("get_user", user_id=1)
```

### Generated URL

```text
/users/1
```

### Benefits

* Avoids hardcoding URLs
* Easier refactoring

---

# Debug Mode

Debug mode is used during development to provide detailed error messages and automatic reloading.

---

## Enable Debug Mode

```python
app.run(debug=True)
```

Or using environment variable:

### Linux/macOS

```bash
export FLASK_ENV=development
```

---

## Features of Debug Mode

### 1. Automatic Reloading

* Server restarts when code changes
* No need to manually restart application

### 2. Interactive Debugger

* Displays detailed error traceback
* Allows inspection of variables in browser

### 3. Detailed Error Pages

* Shows exact line where error occurred
* Helps identify issues quickly

---

## Example Error

```python
@app.route("/error")
def error():
    return 1 / 0
```

### In Debug Mode

* Browser shows stack trace
* Developer can inspect variables and execution flow

---

## System-Level Behavior

* Debug mode runs a development server
* Includes additional overhead for debugging tools
* Not optimized for performance or security

> **Important:**
> Debug mode must never be enabled in production because it exposes internal details and can become a security risk.

---

# Request Lifecycle in Flask

Understanding how Flask processes a request helps connect all concepts.

## Step-by-Step Flow

1. Client sends HTTP request
2. WSGI server receives request
3. Passes request to Flask application
4. Flask creates request context
5. URL is matched to a route
6. View function executes
7. Response is generated
8. Response is passed back to WSGI server
9. Sent to client

---

# Putting It All Together

## Complete Example

```python
from flask import Flask, request, make_response

app = Flask(__name__)

@app.route("/users/<int:user_id>", methods=["GET"])
def get_user(user_id):
    name = request.args.get("name", "Guest")

    response = make_response(
        {"user_id": user_id, "name": name},
        200
    )

    response.headers["X-App"] = "FlaskApp"

    return response
```

---

## Flow

1. URL parameter is extracted
2. Query parameter is read
3. Response is constructed
4. Custom header is added
5. Response is returned

---

# Key Takeaways

* Flask's core functionality revolves around routes, views, and the request-response cycle.
* Routes map URLs to view functions.
* The `request` object provides access to incoming data.
* The `response` object controls outgoing data.
* URL routing enables dynamic path handling.
* Debug mode enhances development with detailed error information and auto-reloading.
* Mastering these basics is essential for building and understanding any Flask application.
