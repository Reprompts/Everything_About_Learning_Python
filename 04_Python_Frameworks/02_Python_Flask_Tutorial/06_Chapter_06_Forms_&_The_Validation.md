# Flask Hands-On Tutorial

# Chapter 6: Forms and Validation

Handling user input is a critical part of any web application. Whether data comes from HTML forms or API requests, it must be validated to ensure correctness, security, and consistency.

In Flask, form handling and validation are typically implemented using **WTForms**, a powerful library for defining forms, validating input, and rendering form fields.

At a system level, form handling involves:

1. Receiving user input
2. Parsing it into structured data
3. Validating it against defined rules
4. Processing valid data or returning errors

Proper validation prevents invalid data from entering the system and protects against common security issues such as injection attacks.

---

# Using WTForms

WTForms is a flexible form-handling library that allows you to define form structures using Python classes.

---

# Installation

## Install WTForms

```bash id="u2j8az"
pip install wtforms
```

## Install Flask-WTF (Recommended)

```bash id="7k6yte"
pip install flask-wtf
```

---

# Basic Form Definition

## Example

```python id="r5xq0w"
from flask_wtf import FlaskForm
from wtforms import StringField, IntegerField, SubmitField
from wtforms.validators import DataRequired

class UserForm(FlaskForm):
    name = StringField(
        "Name",
        validators=[DataRequired()]
    )

    age = IntegerField(
        "Age",
        validators=[DataRequired()]
    )

    submit = SubmitField("Submit")
```

## Explanation

* Each field represents an input element
* Validators define rules for input validation

## Internal Structure

* Class → Form definition
* Attributes → Form fields
* Validators → Validation rules

---

# Rendering Forms in Templates

## Route

```python id="7q4zlj"
from flask import render_template

@app.route("/form", methods=["GET", "POST"])
def form():
    form = UserForm()
    return render_template("form.html", form=form)
```

---

## Template (`form.html`)

```html id="smly1l"
<form method="POST">

    {{ form.hidden_tag() }}

    <label>{{ form.name.label }}</label>
    {{ form.name() }}

    <label>{{ form.age.label }}</label>
    {{ form.age() }}

    {{ form.submit() }}

</form>
```

## Explanation

* `form.hidden_tag()` includes CSRF token
* `form.field()` renders input element
* Labels and fields are dynamically generated

---

# Handling Form Submission

```python id="6v6a7r"
from flask import request

@app.route("/form", methods=["GET", "POST"])
def form():
    form = UserForm()

    if form.validate_on_submit():
        name = form.name.data
        age = form.age.data

        return {
            "name": name,
            "age": age
        }

    return render_template("form.html", form=form)
```

---

# Internal Flow

1. User submits form
2. Flask receives POST request
3. WTForms populates form with request data
4. Validators are executed
5. If valid → data is processed
6. If invalid → errors are stored

---

# Input Validation

Validation ensures that user input meets required conditions before processing.

---

# Built-in Validators

## Example

```python id="7eqykm"
from wtforms.validators import (
    DataRequired,
    Length,
    NumberRange
)

class UserForm(FlaskForm):

    name = StringField(
        "Name",
        validators=[
            DataRequired(),
            Length(min=2, max=50)
        ]
    )

    age = IntegerField(
        "Age",
        validators=[
            DataRequired(),
            NumberRange(min=18, max=100)
        ]
    )
```

## Validator Behavior

* `DataRequired()` → field must not be empty
* `Length()` → restricts string length
* `NumberRange()` → restricts numeric values

---

# Displaying Validation Errors

## Template Example

```html id="hm3s8r"
{% for error in form.name.errors %}
    <p style="color:red">{{ error }}</p>
{% endfor %}
```

## Example Error Output

```text id="jlwmk4"
"This field is required"

"Field must be between 2 and 50 characters"
```

## Internal Behavior

* Errors are collected during validation
* Stored in `form.field.errors`
* Accessible in templates

---

# Custom Validators

You can define custom validation logic.

## Example

```python id="xb0m3s"
from wtforms.validators import ValidationError

class UserForm(FlaskForm):

    name = StringField("Name")

    def validate_name(self, field):
        if field.data == "admin":
            raise ValidationError(
                "This name is not allowed"
            )
```

## Validation Flow

1. WTForms detects `validate_<fieldname>`
2. Executes it during validation
3. Raises error if condition fails

---

# CSRF Protection

Flask-WTF includes built-in CSRF (Cross-Site Request Forgery) protection.

## Setup

```python id="cywbh5"
app.config["SECRET_KEY"] = "secret"
```

## Why It Matters

* Prevents malicious form submissions
* Ensures requests originate from trusted sources

## Internal Mechanism

1. Hidden token is added to form
2. Token is validated on submission
3. Invalid or missing token results in error

---

# Handling Multiple Form Fields

## Example

```python id="5m3ftd"
class RegistrationForm(FlaskForm):

    username = StringField(
        "Username",
        validators=[DataRequired()]
    )

    email = StringField(
        "Email",
        validators=[DataRequired()]
    )

    password = StringField(
        "Password",
        validators=[DataRequired()]
    )
```

## Usage

```python id="ed41i8"
if form.validate_on_submit():

    username = form.username.data
    email = form.email.data
```

---

# File Upload Validation

WTForms supports file inputs using Flask-WTF.

## Example

```python id="m4r58v"
from flask_wtf.file import (
    FileField,
    FileAllowed
)

class UploadForm(FlaskForm):

    file = FileField(
        "Upload",
        validators=[
            FileAllowed(
                ["jpg", "png"],
                "Images only!"
            )
        ]
    )
```

---

# Manual Validation Without WTForms

Flask can also handle validation manually.

## Example

```python id="2o4hvv"
@app.route("/submit", methods=["POST"])
def submit():

    data = request.json

    if "name" not in data:
        return {"error": "Name required"}, 400

    if len(data["name"]) < 2:
        return {"error": "Name too short"}, 400

    return {"message": "Valid"}
```

## Drawbacks

* Repetitive code
* Harder to maintain
* Less structured

---

# Form Lifecycle

## Step-by-Step Process

1. Client loads form page (`GET` request)
2. Flask renders template with form
3. User fills form and submits (`POST` request)
4. Flask receives request
5. WTForms binds data to form
6. Validators are executed

### If Valid

* Data is processed
* Example: saved to database

### If Invalid

* Errors are returned and displayed

---

# Integration with Database

## Example

```python id="7jscr8"
@app.route("/register", methods=["GET", "POST"])
def register():

    form = UserForm()

    if form.validate_on_submit():

        user = User(
            name=form.name.data,
            age=form.age.data
        )

        db.session.add(user)
        db.session.commit()

        return {"message": "User created"}

    return render_template(
        "register.html",
        form=form
    )
```

---

# Best Practices

* Keep validation logic centralized in form classes
* Always validate both client-side and server-side
* Use built-in validators whenever possible
* Display meaningful error messages
* Protect forms with CSRF tokens
* Avoid trusting raw user input without validation

---

# Key Takeaways

* WTForms provides a structured way to define forms and validation rules.
* Flask-WTF improves integration with Flask and adds CSRF protection.
* Validation ensures that only correct and safe data enters the system.
* Built-in and custom validators help enforce application rules.
* Error handling and feedback improve user experience.
* Proper validation is essential for security, reliability, and maintainability in Flask applications.
