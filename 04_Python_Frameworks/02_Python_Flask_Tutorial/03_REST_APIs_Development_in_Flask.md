# Flask Hands-On Tutorial
## Chapter 3: APIs Development in Flask

# REST API Development in Flask

Building REST APIs in Flask involves manually handling HTTP requests, parsing input data, and returning structured responses. Unlike frameworks that provide built-in abstractions, Flask gives you full control over how APIs are designed and implemented. This flexibility comes with responsibility — you must explicitly handle routing, validation, and response formatting.

At a system level, Flask processes incoming HTTP requests via WSGI, maps them to view functions, and returns responses. REST API development in Flask builds on this mechanism by structuring endpoints around resources and HTTP methods.

---

# Building REST APIs Manually

In Flask, REST APIs are typically built using standard routes and view functions.

## Basic Example

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/users", methods=["GET"])
def get_users():
    return jsonify([{"id": 1, "name": "Ganesh"}])

@app.route("/users", methods=["POST"])
def create_user():
    data = request.json
    return jsonify({
        "message": "User created",
        "user": data
    }), 201
```

## Here

* `GET /users` retrieves data
* `POST /users` creates a new resource

---

## System-Level Behavior

1. Flask receives HTTP request
2. Matches route and method
3. Parses request body using `request.json`
4. Executes function logic
5. Converts response into JSON and sends it back

---

## Manual API Development Requires You To

* Parse input data
* Validate data manually
* Construct responses explicitly
* Handle errors yourself

---

# JSON Responses

JSON is the standard format for REST APIs. Flask provides the `jsonify` function to convert Python objects into JSON responses.

## Example

```python
from flask import jsonify

@app.route("/data")
def get_data():
    return jsonify({"key": "value"})
```

---

## Internally

* `jsonify` converts Python dictionary to JSON string
* Sets `Content-Type: application/json`
* Returns a proper HTTP response

---

## Without `jsonify`

```python
import json

@app.route("/raw")
def raw():
    return json.dumps({"key": "value"})
```

This works, but:

* Does not automatically set headers
* Less safe and less convenient

---

## Custom Response with Status Code

```python
return jsonify({"error": "Not found"}), 404
```

### Response Structure

* **Body** → JSON data
* **Status code** → HTTP status
* **Headers** → Metadata

---

# Handling HTTP Methods

REST APIs rely on HTTP methods to define operations on resources.

## Common Methods

| Method | Purpose                |
| ------ | ---------------------- |
| GET    | Retrieve data          |
| POST   | Create resource        |
| PUT    | Update entire resource |
| PATCH  | Partial update         |
| DELETE | Remove resource        |

---

## Example

```python
@app.route("/users/<int:user_id>", methods=["GET", "PUT", "DELETE"])
def user_operations(user_id):

    if request.method == "GET":
        return jsonify({"user_id": user_id})

    elif request.method == "PUT":
        data = request.json
        return jsonify({"updated": data})

    elif request.method == "DELETE":
        return jsonify({"message": "Deleted"}), 204
```

---

## Internal Workflow

* Flask checks request method
* Matches it against allowed methods
* Executes corresponding logic branch

---

## Alternative Approach (Cleaner Separation)

```python
@app.route("/users/<int:user_id>", methods=["GET"])
def get_user(user_id):
    return jsonify({"user_id": user_id})

@app.route("/users/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):
    return jsonify({"message": "Deleted"})
```

### Benefits

* Better readability
* Improved maintainability
* Clear separation of concerns

---

# Using Flask-RESTful

Flask-RESTful is an extension that simplifies REST API development by introducing resource-based routing.

## Installation

```bash
pip install flask-restful
```

---

## Example

```python
from flask import Flask
from flask_restful import Api, Resource

app = Flask(__name__)
api = Api(app)

class UserResource(Resource):

    def get(self):
        return {"message": "Get users"}

    def post(self):
        return {"message": "Create user"}

api.add_resource(UserResource, "/users")
```

---

## Here

* Each resource is a class
* Methods correspond to HTTP verbs
* Routing is handled by the `Api` object

---

## Benefits

* Cleaner structure
* Separation of logic by resource
* Less manual method checking

---

# Using Flask-RESTX

Flask-RESTX is an enhanced version of Flask-RESTful with additional features like API documentation and request validation.

## Installation

```bash
pip install flask-restx
```

---

## Example

```python
from flask import Flask
from flask_restx import Api, Resource

app = Flask(__name__)
api = Api(app)

ns = api.namespace("users")

@ns.route("/")
class UserList(Resource):

    def get(self):
        return {"users": []}

    def post(self):
        return {"message": "User created"}
```

---

## Features

* Namespace support (grouping endpoints)
* Built-in Swagger documentation
* Request parsing and validation tools

---

## Internal Advantages

* Generates API documentation automatically
* Provides structured API design
* Reduces boilerplate code

---

# Error Handling in Flask APIs

## Manual Error Handling

```python
from flask import abort

@app.route("/users/<int:user_id>")
def get_user(user_id):

    if user_id != 1:
        abort(404)

    return jsonify({"user_id": user_id})
```

---

## Custom Error Response

```python
@app.errorhandler(404)
def not_found(error):
    return jsonify({"error": "Not found"}), 404
```

---

# Request Validation (Manual)

Flask does not provide automatic validation like FastAPI.

## Example

```python
@app.route("/users", methods=["POST"])
def create_user():

    data = request.json

    if "name" not in data:
        return jsonify({"error": "Name required"}), 400

    return jsonify(data)
```

---

## Responsibility Lies with Developer To

* Check required fields
* Validate types
* Handle invalid input

---

# End-to-End REST Flow in Flask

1. Client sends HTTP request
2. WSGI server forwards request to Flask
3. Flask matches route and method
4. Request data is parsed
5. View function executes logic
6. Response is constructed (JSON)
7. Response is returned to client

---

# Best Practices

* Use clear and consistent endpoint naming
* Separate logic into different functions or classes
* Always return JSON responses for APIs
* Handle errors properly with appropriate status codes
* Validate incoming data before processing
* Use extensions like Flask-RESTful or Flask-RESTX for better structure in larger applications

---

# Key Takeaways

* REST API development in Flask requires manual handling of routes, methods, and data processing, giving developers full control over implementation.
* JSON responses are generated using `jsonify`.
* HTTP methods define operations on resources.
* Extensions like Flask-RESTful and Flask-RESTX simplify API development through structured resource-based design and automatic documentation.
* Understanding these concepts is essential for building robust and maintainable APIs in Flask.
