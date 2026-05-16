# Flask Hands-On Tutorial

## Chapter 4: Templates & Static Files

While Flask is often used for building APIs, it was originally designed for rendering server-side web applications. This means it has strong support for HTML templating and serving static assets such as CSS, JavaScript, and images.

Understanding templates and static files is important, especially when building full-stack applications or hybrid systems that combine APIs with server-rendered pages.

At a system level, Flask integrates a templating engine to dynamically generate HTML and a static file mechanism to efficiently serve client-side resources. These features allow Flask to act as both a backend API and a traditional web server.

---

# Jinja2 Templating

Flask uses **Jinja2** as its templating engine. Jinja2 allows you to create dynamic HTML pages by embedding Python-like expressions inside HTML.

## Basic Template Example

### `templates/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Hello, {{ name }}</h1>
</body>
</html>
```

### Rendering Template in Flask

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html", name="Ganesh")
```

## Internal Workflow

1. Flask locates template in the `templates/` directory
2. Jinja2 processes the template
3. Variables like `{{ name }}` are replaced with actual values
4. Final HTML is generated and returned as response

---

# Template Syntax

Jinja2 provides a powerful syntax for dynamic content.

## Variables

```html
<p>{{ user.name }}</p>
```

Used to display dynamic data.

---

## Control Structures

```html
{% if user %}
    <p>Welcome, {{ user.name }}</p>
{% else %}
    <p>Please log in</p>
{% endif %}
```

Used for conditional rendering.

---

## Loops

```html
<ul>
{% for item in items %}
    <li>{{ item }}</li>
{% endfor %}
</ul>
```

Used to iterate over data collections.

---

# Template Inheritance

## Base Template (`base.html`)

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```

## Child Template

```html
{% extends "base.html" %}

{% block title %}Home{% endblock %}

{% block content %}
<h1>Welcome</h1>
{% endblock %}
```

## Benefits

* Reusable layouts
* Consistent design across pages
* Separation of structure and content

---

# Passing Data to Templates

Data is passed as keyword arguments.

## Example

```python
@app.route("/users")
def users():
    user_list = ["Ganesh", "Rahul", "Amit"]
    return render_template("users.html", users=user_list)
```

## Template

```html
<ul>
{% for user in users %}
    <li>{{ user }}</li>
{% endfor %}
</ul>
```

This allows dynamic rendering based on backend data.

---

# Serving HTML (Optional for APIs)

Flask can serve HTML responses alongside APIs, making it suitable for hybrid applications.

## Example

```python
@app.route("/dashboard")
def dashboard():
    return render_template("dashboard.html")
```

In API-focused applications:

* HTML rendering is optional
* APIs typically return JSON instead

However, HTML is useful for:

* Admin dashboards
* Documentation pages
* Simple frontends

---

# Static Files

Static files include:

* CSS (styles)
* JavaScript (client-side logic)
* Images

Flask serves static files from a `static/` directory by default.

## Project Structure

```text
project/
├── app.py
├── templates/
│   └── index.html
└── static/
    ├── style.css
    └── script.js
```

## Referencing Static Files in Templates

```html
<link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">

<script src="{{ url_for('static', filename='script.js') }}"></script>
```

## Internal Behavior

1. Flask maps `/static/` URL to `static/` directory
2. Files are served directly without processing

---

# Static File Handling Internals

When a request is made:

```http
GET /static/style.css
```

## Flow

1. Flask checks static route
2. Locates file in `static/` directory
3. Reads file from disk
4. Sends file as response with appropriate headers

## Headers Include

* `Content-Type` (e.g., `text/css`)
* `Cache-Control` (for caching)

---

# Caching Static Files

Browsers cache static files to improve performance.

## Example

```python
app = Flask(
    __name__,
    static_folder="static",
    static_url_path="/static"
)
```

You can configure caching headers for better performance in production.

---

# Combining Templates and Static Files

## Example

```python
@app.route("/")
def home():
    return render_template("index.html", title="Home Page")
```

## Template

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>

    <link rel="stylesheet"
          href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>Welcome</h1>
</body>
</html>
```

## Flow

1. Template is rendered with dynamic data
2. Static CSS file is loaded separately
3. Browser combines both to display page

---

# When to Use Templates in Modern Systems

Templates are useful when:

* Building server-rendered applications
* Creating simple UIs without frontend frameworks
* Serving admin or internal dashboards

In modern architectures:

* APIs often return JSON
* Frontend frameworks (React, Vue) handle UI
* Flask acts as backend API only

---

# Key Takeaways

* Flask provides built-in support for dynamic HTML rendering through the **Jinja2** templating engine.
* Templates allow embedding dynamic data into HTML using variables, loops, and control structures.
* Static files handle client-side resources like CSS and JavaScript.
* Flask can serve both APIs and traditional server-rendered web applications.
* Template inheritance improves reusability and maintainability.
* Static file handling enables efficient delivery of frontend assets.
* These features make Flask a versatile framework for both traditional and modern web architectures.
