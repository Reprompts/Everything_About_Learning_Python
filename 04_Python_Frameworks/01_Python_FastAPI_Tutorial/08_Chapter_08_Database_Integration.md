# FastAPI Hands-On Tutorial
## Chapter 8: Database Integration

---

# Database Integration

Integrating a database into a FastAPI application is a critical step in building real-world systems.

Database integration involves:

- Choosing the right database type
- Establishing reliable connections
- Structuring data models
- Performing CRUD operations
- Managing schema changes over time

At a system level, this layer connects application logic to persistent storage, ensuring:

- Data durability
- Consistency
- Scalability

---

# SQL vs NoSQL Overview

Databases are broadly divided into:

- Relational Databases (SQL)
- Non-Relational Databases (NoSQL)

The choice depends on:

- Data structure
- Scalability requirements
- Consistency needs
- Query complexity

---

# Relational Databases (SQL)

Examples:

- PostgreSQL
- MySQL
- SQLite

SQL databases store data in structured tables with predefined schemas.

They support:

- Relationships using foreign keys
- Structured queries using SQL
- Strong transactional guarantees

---

# Example SQL Table

```text id="d5x2lm"
Users Table

+----+--------+-----+
| id | name   | age |
+----+--------+-----+
| 1  | Ganesh | 22  |
+----+--------+-----+
````

---

# Characteristics of SQL Databases

* Strong schema enforcement
* ACID compliance
* Structured relationships
* Reliable transactions
* Complex query support

---

# ACID Properties

| Property    | Meaning                                 |
| ----------- | --------------------------------------- |
| Atomicity   | All operations succeed or fail together |
| Consistency | Data remains valid                      |
| Isolation   | Concurrent transactions do not conflict |
| Durability  | Data persists after commit              |

---

# NoSQL Databases

Examples:

* MongoDB
* Redis
* Cassandra

NoSQL databases store data in flexible formats such as:

* JSON documents
* Key-value pairs
* Graph structures

---

# Example NoSQL Document

```json id="r4k9pz"
{
  "name": "Ganesh",
  "age": 22
}
```

---

# Characteristics of NoSQL Databases

* Flexible schema
* Horizontal scalability
* Faster scaling for distributed systems
* Suitable for rapidly evolving data

---

# System-Level Decision

Use SQL when:

* Relationships are important
* Data consistency is critical
* Transactions are required

Use NoSQL when:

* Flexibility is more important
* Massive scalability is required
* Data structure changes frequently

---

# FastAPI and Databases

FastAPI works well with both SQL and NoSQL databases.

However, SQL databases are commonly paired with:

```text id="m2p7vc"
SQLAlchemy
```

for ORM-based development.

---

# Using ORM: SQLAlchemy

ORM stands for:

```text id="v8q3dt"
Object Relational Mapper
```

An ORM allows interaction with databases using Python objects instead of raw SQL queries.

---

# Why Use an ORM

ORMs provide:

* Cleaner code
* Reduced boilerplate
* Database abstraction
* Easier maintenance
* Object-oriented workflows

---

# SQLAlchemy Basic Setup

```python id="w5n8ly"
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String)
    age = Column(Integer)
```

---

# What This Model Represents

| Python Component | Database Equivalent |
| ---------------- | ------------------- |
| Class            | Table               |
| Attribute        | Column              |
| Object Instance  | Row                 |

---

# Internal ORM Workflow

SQLAlchemy internally:

* Translates Python operations into SQL queries
* Tracks object states
* Manages transactions
* Handles database sessions

---

# Object State Tracking

SQLAlchemy tracks objects as:

* New
* Modified
* Deleted
* Persistent

This enables automatic SQL generation during commits.

---

# Connecting to Databases

To use a database in FastAPI:

1. Define a database URL
2. Create an engine
3. Create sessions

---

# SQLite Connection Example

```python id="c6m4jr"
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./test.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

SessionLocal = sessionmaker(bind=engine)
```

---

# PostgreSQL Connection Example

```text id="h7w2pf"
postgresql://user:password@localhost/dbname
```

---

# MySQL Connection Example

```text id="n1r5dv"
mysql+pymysql://user:password@localhost/dbname
```

---

# Internal Connection Flow

## `create_engine`

Responsible for:

* Establishing database connectivity
* Managing connection pooling

---

## `sessionmaker`

Creates a session factory.

Each session represents:

```text id="j9e8kq"
A transactional scope
```

---

# Database Session Dependency

```python id="y3t6mf"
from fastapi import Depends

def get_db():
    db = SessionLocal()

    try:
        yield db

    finally:
        db.close()
```

---

# Why This Dependency Matters

This ensures:

* One session per request
* Proper cleanup
* Safe transaction handling
* Controlled resource usage

---

# Internal Session Lifecycle

```text id="u8k4cb"
Request Starts
      ↓
Create Session
      ↓
Perform Queries
      ↓
Commit / Rollback
      ↓
Close Session
```

---

# CRUD Operations

CRUD stands for:

| Operation | Meaning              |
| --------- | -------------------- |
| Create    | Insert data          |
| Read      | Retrieve data        |
| Update    | Modify existing data |
| Delete    | Remove data          |

---

# CREATE Operation

```python id="o4s7ny"
@app.post("/users")
def create_user(
    name: str,
    age: int,
    db = Depends(get_db)
):
    user = User(name=name, age=age)

    db.add(user)
    db.commit()
    db.refresh(user)

    return user
```

---

# Internal Create Flow

```text id="a5l2xd"
Object Created in Memory
        ↓
Added to Session
        ↓
INSERT Query Generated
        ↓
Transaction Committed
        ↓
Object Refreshed
```

---

# Why `db.refresh()` Is Needed

It reloads values generated by the database, such as:

* Auto-increment IDs
* Default timestamps

---

# READ Operation

```python id="x6q3pk"
@app.get("/users/{user_id}")
def get_user(
    user_id: int,
    db = Depends(get_db)
):
    return (
        db.query(User)
        .filter(User.id == user_id)
        .first()
    )
```

---

# Internal Read Flow

```text id="v7d1hr"
Python Query Constructed
        ↓
Converted to SQL
        ↓
SQL Executed
        ↓
Result Returned
        ↓
Mapped to Python Object
```

---

# UPDATE Operation

```python id="m4z7qp"
@app.put("/users/{user_id}")
def update_user(
    user_id: int,
    name: str,
    db = Depends(get_db)
):
    user = (
        db.query(User)
        .filter(User.id == user_id)
        .first()
    )

    user.name = name

    db.commit()

    return user
```

---

# Internal Update Flow

```text id="f2n8cw"
Object Retrieved
       ↓
Modified in Memory
       ↓
Changes Detected
       ↓
UPDATE Query Generated
       ↓
Commit Executed
```

---

# DELETE Operation

```python id="s5v9xt"
@app.delete("/users/{user_id}")
def delete_user(
    user_id: int,
    db = Depends(get_db)
):
    user = (
        db.query(User)
        .filter(User.id == user_id)
        .first()
    )

    db.delete(user)

    db.commit()

    return {"deleted": user_id}
```

---

# Internal Delete Flow

```text id="r1k6hv"
Object Marked for Deletion
          ↓
DELETE Query Generated
          ↓
Commit Executed
```

---

# Migrations with Alembic

As applications evolve, database schemas change.

Examples:

* Adding columns
* Renaming tables
* Modifying relationships

Alembic is used to manage schema changes safely.

---

# What Alembic Does

Alembic:

* Tracks schema versions
* Generates migration scripts
* Applies incremental changes
* Supports rollbacks

---

# Initialize Alembic

```bash id="l8q3mb"
alembic init alembic
```

This creates:

* `alembic.ini`
* Migration folder structure

---

# Configure Database URL

Edit:

```text id="z2f5nr"
alembic.ini
```

Example:

```ini id="q7w8tc"
sqlalchemy.url = sqlite:///./test.db
```

---

# Create Migration

```bash id="d4m7yk"
alembic revision --autogenerate -m "create users table"
```

---

# What Alembic Compares

Alembic compares:

* Current SQLAlchemy models
* Existing database schema

Then generates migration scripts automatically.

---

# Example Migration Script

```python id="k5n9pd"
def upgrade():
    op.create_table(
        'users',
        sa.Column('id', sa.Integer(), primary_key=True),
        sa.Column('name', sa.String()),
        sa.Column('age', sa.Integer())
    )
```

---

# Apply Migration

```bash id="p8v4rm"
alembic upgrade head
```

This applies all pending migrations.

---

# Internal Alembic Workflow

```text id="c3j7wt"
Model Changes
      ↓
Generate Migration
      ↓
Store Version ID
      ↓
Apply Sequential Changes
      ↓
Update Database Schema
```

---

# Alembic Version Tracking

Alembic maintains a special version table inside the database.

Each migration has:

* Unique revision ID
* Dependency order
* Upgrade and downgrade steps

---

# Rollback Support

Alembic supports rollback operations.

Example:

```bash id="e9x2vf"
alembic downgrade -1
```

This reverts the last migration.

---

# End-to-End Database Flow

```text id="n5r8yb"
Client Request
       ↓
FastAPI Route Triggered
       ↓
Database Session Injected
       ↓
ORM Query Constructed
       ↓
SQLAlchemy Generates SQL
       ↓
Database Executes Query
       ↓
Result Returned
       ↓
Mapped to Python Objects
       ↓
Commit / Rollback
       ↓
Response Serialized to JSON
       ↓
HTTP Response Sent
```

---

# System-Level Responsibilities

| Layer                | Responsibility         |
| -------------------- | ---------------------- |
| FastAPI              | Request handling       |
| Dependency Injection | Session management     |
| SQLAlchemy           | ORM + query generation |
| Database Engine      | Query execution        |
| Alembic              | Schema migrations      |

---

# Best Practices

## Use Dependency Injection for Sessions

Avoid creating sessions manually inside routes.

---

## Separate Models and Schemas

* SQLAlchemy Models → Database structure
* Pydantic Schemas → Validation & serialization

---

## Keep Transactions Short

Long transactions can:

* Lock tables
* Reduce concurrency
* Hurt performance

---

## Use Migrations Consistently

Never modify production schemas manually.

Use Alembic for controlled schema evolution.

---

# Key Takeaways

* Database integration connects FastAPI applications to persistent storage
* SQL databases provide structured and reliable transactional systems
* NoSQL databases prioritize flexibility and scalability
* SQLAlchemy enables object-oriented database interaction
* Sessions manage transactional boundaries and resource cleanup
* CRUD operations form the foundation of data interaction
* Alembic handles schema evolution safely and systematically
* Proper database architecture is essential for scalable and production-ready APIs

```
