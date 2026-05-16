# FastAPI Hands-On Tutorial

## Chapter 17: Advanced Topics

As applications grow in complexity and scale, traditional REST patterns are often extended with advanced architectural and communication strategies. FastAPI supports a wide range of these advanced topics, including real-time communication with WebSockets, flexible querying with GraphQL, distributed system design through microservices, asynchronous communication using event-driven architectures, and system protection using rate limiting.

At a system level, these concepts move your application from a simple request-response model into a highly interactive, scalable, and distributed system.

---

# WebSockets

WebSockets enable real-time, bidirectional communication between client and server. Unlike HTTP, which is request-response based, WebSockets maintain a persistent connection.

## Basic Example

```python
from fastapi import FastAPI, WebSocket

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()

    while True:
        data = await websocket.receive_text()
        await websocket.send_text(f"Message: {data}")
```

## Workflow

* Client establishes connection using WebSocket protocol
* Server accepts connection
* Both client and server can send messages anytime
* Connection remains open until closed

## Internal Behavior

* Uses ASGI protocol for long-lived connections
* Event loop manages multiple WebSocket connections concurrently
* No repeated HTTP handshakes

## Use Cases

* Chat applications
* Live notifications
* Real-time dashboards

## System-Level Advantages

* Reduces latency
* Eliminates repeated request overhead
* Enables continuous data streaming

---

# GraphQL Integration

GraphQL is an alternative to REST that allows clients to request exactly the data they need.

Instead of multiple endpoints:

```http
GET /users
GET /users/1/posts
```

GraphQL uses a single endpoint:

```http
POST /graphql
```

## Query Example

```graphql
{
  user(id: 1) {
    name
    posts {
      title
    }
  }
}
```

## Integration in FastAPI (Using Strawberry)

```python
import strawberry
from fastapi import FastAPI
from strawberry.fastapi import GraphQLRouter

@strawberry.type
class User:
    name: str

@strawberry.type
class Query:

    @strawberry.field
    def get_user(self) -> User:
        return User(name="Ganesh")

schema = strawberry.Schema(query=Query)

app = FastAPI()

app.include_router(GraphQLRouter(schema), prefix="/graphql")
```

## Internal Workflow

* Client sends query
* GraphQL parser interprets query structure
* Resolver functions fetch required data
* Response is constructed dynamically

## Advantages

* Reduces over-fetching and under-fetching
* Single endpoint for all queries
* Strongly typed schema

## Trade-offs

* More complex server implementation
* Requires careful performance optimization

---

# Microservices Architecture

Microservices architecture involves breaking a large application into smaller, independent services.

## Example Services

* User service
* Order service
* Payment service

Each service:

* Has its own database
* Runs independently
* Communicates via APIs or messaging systems

FastAPI is well-suited for microservices because:

* Lightweight and fast
* Easy to deploy independently
* Supports async communication

## Example Communication

```python
import httpx

async def get_user_data():
    async with httpx.AsyncClient() as client:
        response = await client.get("http://user-service/users/1")
        return response.json()
```

## System-Level Design

* Services communicate over HTTP or message queues
* Failures are isolated
* Scaling can be done per service

## Benefits

* Independent development and deployment
* Better fault isolation
* Scalable architecture

## Challenges

* Increased complexity
* Network latency
* Data consistency management

---

# Event-Driven APIs

Event-driven architecture is based on producing and consuming events rather than direct request-response communication.

Instead of:

```text
Service A calls Service B directly
```

It becomes:

```text
Service A emits an event
Service B listens and reacts
```

## Example Using a Message Broker (Conceptual)

### Producer

```python
def create_order():
    event = {
        "type": "order_created",
        "order_id": 1
    }

    send_to_queue(event)
```

### Consumer

```python
def handle_event(event):

    if event["type"] == "order_created":
        process_order(event["order_id"])
```

## Internal Workflow

* Event is published to a queue or broker (e.g., Kafka, RabbitMQ)
* Consumers subscribe to events
* Events are processed asynchronously

## Benefits

* Loose coupling between services
* High scalability
* Better fault tolerance

## Use Cases

* Order processing systems
* Notification systems
* Data pipelines

---

# Rate Limiting

Rate limiting controls the number of requests a client can make within a given time period. It protects the system from abuse and ensures fair usage.

## Basic Concept

* Limit requests per IP or user
* Reject requests exceeding limit

## Example (Conceptual)

```python
from fastapi import HTTPException

request_count = {}

def rate_limiter(client_id: str):

    if request_count.get(client_id, 0) > 100:
        raise HTTPException(
            status_code=429,
            detail="Too many requests"
        )

    request_count[client_id] = (
        request_count.get(client_id, 0) + 1
    )
```

## Production Implementations

Rate limiting is commonly implemented using:

* Redis (for distributed counters)
* API gateways (NGINX, Kong)
* Libraries like `slowapi`

## Internal Mechanism

* Track request count per client
* Apply time window (e.g., per minute)
* Reset counters periodically

## Advanced Strategies

* Token bucket algorithm
* Leaky bucket algorithm

## Benefits

* Prevents server overload
* Protects against abuse
* Ensures fair resource distribution

---

# Combining These Concepts

In modern systems, these advanced techniques are often used together:

* WebSockets for real-time updates
* GraphQL for flexible querying
* Microservices for scalability
* Event-driven systems for decoupled communication
* Rate limiting for protection

## Example Architecture

* Client connects via WebSocket for live updates
* Uses GraphQL for data queries
* Backend consists of multiple FastAPI microservices
* Services communicate via event queues
* Rate limiting applied at API gateway

---

# End-to-End Advanced Flow

A request in an advanced system may look like:

1. Client sends GraphQL query
2. API gateway applies rate limiting
3. Request routed to appropriate microservice
4. Service processes request or emits event
5. Other services react to event asynchronously
6. Real-time updates pushed via WebSocket
7. Response returned to client

---

# Key Takeaways

* Advanced topics in FastAPI extend beyond basic REST APIs into real-time communication, flexible querying, distributed systems, and scalable architectures.
* WebSockets enable persistent communication.
* GraphQL provides efficient data querying.
* Microservices allow modular system design.
* Event-driven APIs decouple services for scalability.
* Rate limiting protects system resources.
* Mastering these concepts allows you to design high-performance, resilient, and enterprise-grade systems.
