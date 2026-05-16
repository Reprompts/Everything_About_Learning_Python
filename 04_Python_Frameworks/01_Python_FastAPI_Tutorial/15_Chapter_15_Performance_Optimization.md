# FastAPI Hands-On Tutorial
## Chapter 15: Performance Optimization

# Performance Optimization

Performance optimization in FastAPI is about maximizing throughput, minimizing latency, and ensuring the system can handle increasing load efficiently. This involves choosing the right execution model (`async` vs `sync`), reducing redundant computations through caching, offloading heavy work to background jobs, and designing strategies to handle high traffic.

At a system level, performance is influenced by how efficiently your application uses CPU, memory, I/O, and network resources. FastAPI, built on ASGI, provides the foundation for high-performance applications, but proper design decisions are required to fully leverage it.

---

# Async vs Sync Performance

FastAPI supports both synchronous (`def`) and asynchronous (`async def`) endpoints, but their performance characteristics differ significantly.

## Synchronous Execution

* Each request blocks the worker until completion
* Suitable for CPU-bound tasks
* Limited concurrency per worker

## Asynchronous Execution

* Uses event loop and non-blocking I/O
* Handles multiple requests concurrently
* Ideal for I/O-bound operations

## Example Comparison

```python
@app.get("/sync")
def sync_route():
    import time
    time.sleep(2)
    return {"message": "done"}

@app.get("/async")
async def async_route():
    import asyncio
    await asyncio.sleep(2)
    return {"message": "done"}
```

### In the sync version

* Worker is blocked for 2 seconds per request

### In the async version

* Task yields control during wait
* Other requests are processed concurrently

## System-Level Impact

* Async improves throughput under high concurrency
* Sync can become a bottleneck if many requests are waiting

> Important distinction:
>
> Async improves I/O performance, not CPU performance.
> CPU-heavy tasks still block execution even in async functions.

---

# Caching (Redis)

Caching reduces repeated computation and database access by storing frequently used data in a fast-access layer.

Redis is commonly used as an in-memory cache.

## Basic Idea

1. First request → fetch from database → store in cache
2. Subsequent requests → fetch directly from cache

## Example Using Redis

```python
import redis

r = redis.Redis(host="localhost", port=6379, db=0)

@app.get("/data")
def get_data():
    cached = r.get("data")

    if cached:
        return {
            "source": "cache",
            "data": cached.decode()
        }

    data = "expensive computation"

    r.set("data", data, ex=60)

    return {
        "source": "db",
        "data": data
    }
```

## Internal Flow

1. Check cache for key
2. If found, return cached data
3. If not, compute/fetch data
4. Store result in cache with expiration
5. Return response

## Benefits

* Reduces database load
* Decreases response time
* Improves scalability

## Common Caching Strategies

* Time-based expiration (TTL)
* Cache invalidation on updates
* Key-based caching for specific queries

---

# Background Jobs

Background jobs are used to offload non-critical or long-running tasks from the request-response cycle.

## Example Using FastAPI Background Tasks

```python
from fastapi import BackgroundTasks

def send_email():
    print("Email sent")

@app.post("/notify")
def notify(background_tasks: BackgroundTasks):
    background_tasks.add_task(send_email)

    return {
        "message": "Notification scheduled"
    }
```

## What Happens Here

* Response is returned immediately
* Task runs after response is sent

## System-Level Benefit

* Reduces response latency
* Prevents blocking client requests

## Limitations

* Runs in same process
* Not suitable for heavy or distributed workloads

## External Job Queues for Large Systems

* Celery
* RabbitMQ
* Redis queues

These systems:

* Distribute tasks across workers
* Provide retry mechanisms
* Handle failures more robustly

---

# Load Handling Strategies

Handling high traffic requires designing the system to distribute and manage load effectively.

---

## Horizontal Scaling

Instead of increasing resources of a single server, multiple instances are used.

### Example

* Run multiple FastAPI instances
* Use a load balancer to distribute requests

### Benefits

* Improved fault tolerance
* Better scalability

---

## Worker Processes

Uvicorn can run multiple workers:

```bash
uvicorn main:app --workers 4
```

Each worker:

* Runs independently
* Handles separate requests

### System-Level Effect

* Utilizes multiple CPU cores
* Increases parallel processing

---

## Rate Limiting

Rate limiting prevents abuse by restricting the number of requests per client.

### Concept

* Limit requests per IP or user
* Return error if limit exceeded

### Example (Conceptual)

```python
if requests_per_minute > limit:
    raise HTTPException(
        status_code=429,
        detail="Too many requests"
    )
```

### This Protects

* Server resources
* Backend systems
* API availability

---

## Connection Pooling

Database connections are expensive. Connection pooling reuses existing connections instead of creating new ones.

### Example with SQLAlchemy

```python
engine = create_engine(
    DATABASE_URL,
    pool_size=10,
    max_overflow=20
)
```

### Benefits

* Reduces connection overhead
* Improves database performance

---

## Response Compression

Compressing responses reduces data transfer size.

### Example Using GZip Middleware

```python
from fastapi.middleware.gzip import GZipMiddleware

app.add_middleware(
    GZipMiddleware,
    minimum_size=1000
)
```

### Effect

* Smaller response size
* Faster network transfer

---

## Efficient Query Design

Poor database queries can become bottlenecks.

### Best Practices

* Use indexes
* Avoid unnecessary joins
* Limit returned data
* Use pagination

---

# End-to-End Performance Flow

When a request is processed in an optimized system:

1. Request arrives at load balancer
2. Routed to one of multiple FastAPI instances
3. Middleware handles logging, compression, etc.
4. Cache is checked for data
5. If cache miss, database query is executed
6. Query uses connection pool
7. Result is cached for future use
8. Response is generated
9. Background tasks are scheduled if needed
10. Response is returned to client

---

# Key Takeaways

Performance optimization in FastAPI involves multiple layers of improvement.

* Async execution enhances concurrency for I/O-bound workloads
* Caching with Redis reduces repeated computation and database load
* Background jobs help offload non-critical tasks, improving response times
* Horizontal scaling, worker processes, rate limiting, and connection pooling improve load handling
* Response compression reduces network overhead
* Efficient database queries are essential for scalability

A well-optimized API balances all these aspects to achieve scalability, reliability, and speed.
