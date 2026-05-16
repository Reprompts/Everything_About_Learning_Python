# Flask Hands-On Tutorial

## Chapter 05: Database Integration

Database integration is a core part of building real-world Flask applications. It allows your application to persist data, retrieve it efficiently, and maintain consistency across operations.

Flask itself does not include built-in database support, but it integrates seamlessly with extensions like **Flask-SQLAlchemy** for ORM functionality and **Flask-Migrate** for database schema migrations.

At a system level, the application interacts with the database through an abstraction layer (ORM), which translates Python objects into SQL queries and maps database records back into Python objects. This reduces the need to write raw SQL while maintaining flexibility and performance.

---

# Using Flask-SQLAlchemy

Flask-SQLAlchemy is an extension that simplifies working with databases using SQLAlchemy ORM.

## Installation

```bash id="q9m2la"
pip install flask-sqlalchemy
```

---

# Basic Setup

```python id="6dq7xt"
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///example.db"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False

db = SQLAlchemy(app)
```

## Explanation

* `SQLALCHEMY_DATABASE_URI` defines the database connection
* SQLite is used for simplicity, but PostgreSQL/MySQL can also be used
* `db` is the main interface for database operations

---

# Defining Models

Models represent database tables.

## Example

```python id="p8e4ms"
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100))
    age = db.Column(db.Integer)
```

## Internal Mapping

* Class → Table
* Attributes → Columns
* Instances → Rows

## SQL Equivalent

```sql id="4n4jtz"
CREATE TABLE user (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100),
    age INTEGER
);
```

---

# Creating the Database

```python id="6icrdr"
with app.app_context():
    db.create_all()
```

## What Happens Internally

* Connects to database
* Creates tables based on models
* Executes SQL behind the scenes

---

# CRUD Operations

CRUD stands for:

* **Create**
* **Read**
* **Update**
* **Delete**

These are the fundamental operations performed on a database.

---

# Create Operation

```python id="jlwmgo"
new_user = User(name="Ganesh", age=22)

db.session.add(new_user)
db.session.commit()
```

## Internal Flow

1. Object is added to session
2. Session tracks changes
3. `commit()` generates SQL `INSERT`
4. Data is stored in database

---

# Read Operation

## Fetch All Users

```python id="v7mduh"
users = User.query.all()
```

## Fetch Single User

```python id="7mnd5u"
user = User.query.get(1)
```

## Filter Query

```python id="ig1jz6"
user = User.query.filter_by(name="Ganesh").first()
```

## Internal Behavior

* ORM generates `SELECT` queries
* Results are converted into Python objects

---

# Update Operation

```python id="1f5g2u"
user = User.query.get(1)

user.age = 25

db.session.commit()
```

## Flow

1. Object is retrieved
2. Attribute is modified
3. ORM detects change
4. Generates `UPDATE` query

---

# Delete Operation

```python id="9g9w0m"
user = User.query.get(1)

db.session.delete(user)

db.session.commit()
```

## Flow

1. Object is marked for deletion
2. `commit()` executes `DELETE` query

---

# Session Management

The session is a key component of SQLAlchemy.

## Responsibilities

* Tracks changes to objects
* Batches operations
* Ensures transactional consistency

## Example

```python id="o70hy8"
db.session.add(user)
db.session.commit()
```

## Rollback on Error

```python id="z6epcu"
db.session.rollback()
```

## Benefits

* Data consistency
* Atomic operations

---

# Migrations with Flask-Migrate

Flask-Migrate is used to manage database schema changes over time.

---

# Installation

```bash id="9pmd80"
pip install flask-migrate
```

---

# Setup

```python id="w0e0ta"
from flask_migrate import Migrate

migrate = Migrate(app, db)
```

---

# Initialize Migration Repository

```bash id="gv0v4m"
flask db init
```

Creates a `migrations/` folder.

---

# Generate Migration

```bash id="rqcyfh"
flask db migrate -m "Initial migration"
```

## Internal Workflow

* Compares models with database
* Generates migration script

---

# Apply Migration

```bash id="x1d8q9"
flask db upgrade
```

Executes SQL changes in the database.

---

# Example Schema Change

## Modified Model

```python id="y4tq7f"
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100))
    email = db.Column(db.String(120))
```

## Migration Commands

```bash id="43zc85"
flask db migrate -m "Add email field"

flask db upgrade
```

## Internal Workflow

1. Detect schema difference
2. Generate `ALTER TABLE` statement
3. Apply changes safely

---

# Relationships Between Models

Flask-SQLAlchemy supports relationships.

## Example

```python id="v7h0ih"
class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100))
    user_id = db.Column(db.Integer, db.ForeignKey("user.id"))

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100))

    posts = db.relationship("Post", backref="user")
```

## Relationship Type

* One-to-many relationship
* A user can have multiple posts

## Usage

```python id="jlwmzj"
user = User.query.get(1)

user.posts
```

---

# Database Configuration for Production

## PostgreSQL Example

```python id="ukteyn"
app.config["SQLALCHEMY_DATABASE_URI"] = \
    "postgresql://user:password@localhost/db"
```

## Best Practices

* Use environment variables
* Avoid hardcoding credentials
* Use connection pooling

---

# Handling Transactions

Transactions ensure consistency.

## Example

```python id="93udyi"
try:
    user = User(name="Test", age=30)

    db.session.add(user)
    db.session.commit()

except:
    db.session.rollback()
```

## Why This Matters

This ensures:

* Either all changes succeed
* Or none are applied

---

# End-to-End Database Flow

1. Client sends request
2. Flask route receives request
3. Data is validated
4. Service logic creates/queries model
5. ORM converts operation to SQL
6. Database executes query
7. Result is returned as Python object
8. Response is sent to client

---

# Common Mistakes to Avoid

* Forgetting `commit()` after changes
* Not handling rollbacks on errors
* Mixing business logic inside models
* Hardcoding database credentials
* Not using migrations for schema changes

---

# Key Takeaways

* Flask integrates with databases using **Flask-SQLAlchemy**.
* ORM maps Python objects to database tables and SQL queries.
* CRUD operations allow efficient data management.
* The session ensures transactional consistency.
* Flask-Migrate enables version-controlled schema evolution.
* Relationships allow structured connections between models.
* Proper database design and migration handling are essential for scalable applications.
