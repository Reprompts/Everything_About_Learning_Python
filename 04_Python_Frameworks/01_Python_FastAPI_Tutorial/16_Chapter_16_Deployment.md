# FastAPI Hands-On Tutorial
## Fhapter 16: Deployment

# Deployment

Deployment is the process of making your FastAPI application available for real-world usage. It involves running the application with a production-ready server, packaging it for consistency using containers, deploying it on cloud platforms, and placing it behind a reverse proxy for scalability and security.

At a system level, deployment transforms your application from a local development environment into a distributed system capable of handling real users, traffic spikes, failures, and scaling requirements.

---

# Running with Production Server

During development, FastAPI is typically run using:

```bash id="p8xq1d"
uvicorn main:app --reload
```

However, the `--reload` option is not suitable for production. In production, you need a stable, high-performance server setup.

## Basic Production Command

```bash id="k7m4zn"
uvicorn main:app --host 0.0.0.0 --port 8000
```

## Using Multiple Workers

```bash id="t9b2lw"
uvicorn main:app --workers 4 --host 0.0.0.0 --port 8000
```

Here:

* Each worker is a separate process
* Requests are distributed across workers
* Utilizes multiple CPU cores

## Using Gunicorn with Uvicorn Workers

```bash id="q4h8yr"
gunicorn main:app -k uvicorn.workers.UvicornWorker -w 4
```

## Internal Architecture

* Uvicorn handles ASGI communication
* Gunicorn manages worker processes
* Each worker runs an independent event loop

## Benefits

* High concurrency
* Fault isolation (if one worker crashes, others continue)
* Better CPU utilization

---

# Dockerizing FastAPI Applications

Docker allows you to package your application along with its dependencies into a container, ensuring consistent behavior across environments.

## Basic Dockerfile

```dockerfile id="f2v6cp"
FROM python:3.11

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
```

## Build Docker Image

```bash id="n5j1xs"
docker build -t fastapi-app .
```

## Run Container

```bash id="w8z3ku"
docker run -d -p 8000:80 fastapi-app
```

## Internal Workflow

1. Base image provides Python runtime
2. Application code is copied into container
3. Dependencies are installed
4. Uvicorn starts the server inside container

## Benefits

* Environment consistency
* Easy deployment across systems
* Isolation from host machine

For production, Docker Compose or orchestration tools like Kubernetes can be used to manage multiple containers.

---

# Deployment Platforms (AWS, GCP, Azure)

Cloud platforms provide infrastructure and services for hosting applications.

---

## AWS

### Common Services

* EC2 (virtual servers)
* ECS/EKS (container orchestration)
* RDS (managed databases)

### Basic Deployment Flow

1. Launch EC2 instance
2. Install Docker or Python environment
3. Deploy FastAPI app
4. Configure security groups (firewall)

---

## GCP

### Common Services

* Compute Engine
* Cloud Run (serverless containers)
* Cloud SQL

### Why Cloud Run Is Useful

* Deploy container directly
* Automatically scales based on traffic
* No server management required

---

## Azure

### Common Services

* Azure App Service
* Azure Kubernetes Service (AKS)
* Azure SQL Database

### App Service Benefits

* Direct deployment of Python apps
* Built-in scaling and monitoring

---

## System-Level Considerations for Cloud Deployment

* Scalability (auto-scaling based on load)
* Availability (multiple regions/zones)
* Security (firewalls, IAM roles)
* Monitoring (logs, metrics)

---

# Reverse Proxy (NGINX)

A reverse proxy sits between clients and your FastAPI application. It handles incoming requests and forwards them to the backend server.

NGINX is commonly used for this purpose.

## Basic NGINX Configuration

```nginx id="u6d0qe"
server {
    listen 80;

    location / {
        proxy_pass http://127.0.0.1:8000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## Internal Flow

1. Client sends request to NGINX
2. NGINX processes request
3. Forwards it to FastAPI server
4. Receives response
5. Sends response back to client

## Benefits

* Load balancing across multiple backend instances
* SSL termination (HTTPS handling)
* Request buffering and caching
* Protection against direct exposure of application server

---

# HTTPS with NGINX

To secure communication, SSL certificates are used.

## Example Configuration

```nginx id="r1m7yb"
server {
    listen 443 ssl;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

## Benefits

* Encrypted communication
* Secure data transfer
* Trust for clients

---

# Full Deployment Architecture

A typical production setup looks like this:

```text id="e4k2vd"
Client (browser or app)
        ↓
NGINX (reverse proxy)
        ↓
FastAPI (Uvicorn/Gunicorn workers)
        ↓
Database / Cache
```

## Request Flow

1. Client sends request
2. NGINX handles SSL and routing
3. Request forwarded to FastAPI
4. FastAPI processes request
5. Database/cache accessed if needed
6. Response returned through NGINX to client

---

# Scaling Strategy

For handling large traffic:

* Multiple FastAPI instances (horizontal scaling)
* Load balancer distributes traffic
* Containers managed via orchestration (Kubernetes)
* Auto-scaling adjusts resources dynamically

---

# End-to-End Deployment Flow

1. Application is developed and tested locally
2. Docker image is created
3. Image is deployed to cloud platform
4. NGINX is configured as reverse proxy
5. Uvicorn/Gunicorn runs application with multiple workers
6. Traffic is routed through proxy to application
7. Application handles requests and interacts with database
8. Responses are returned securely to clients

---

# Key Takeaways

Deployment transforms a FastAPI application into a production-ready system.

* Uvicorn with multiple workers ensures concurrency and reliability
* Gunicorn improves process management
* Docker provides portability and environment consistency
* AWS, GCP, and Azure offer scalable hosting infrastructure
* NGINX improves security, HTTPS handling, and load balancing
* Horizontal scaling and orchestration enable handling large traffic loads

A well-designed deployment architecture ensures that your FastAPI application can handle real-world traffic efficiently, securely, and reliably.
