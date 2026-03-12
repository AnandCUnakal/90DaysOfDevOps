# Docker Master Guide for DevOps Engineers

*A practical guide from beginner → production → interview preparation*

---

# 1. What is Docker?

Docker is a **containerization platform** that packages an application with its dependencies so it can run **consistently across environments**.

### Traditional Deployment Problem

Different environments cause issues:

```
Developer Laptop → QA Server → Production
Python 3.9        Python 3.10   Python 3.8
Different libraries
Different OS configs
```

This leads to the classic issue:

```
"It works on my machine."
```

### Docker Solution

Docker packages everything:

```
Application
Dependencies
Runtime
Libraries
OS packages
```

into a **container image**.

---

# 2. Docker Architecture

## Docker Components

### 1. Docker Client

Command line tool.

Example:

```
docker build
docker run
docker push
```

### 2. Docker Daemon

Background service that manages:

* containers
* images
* volumes
* networks

### 3. Docker Registry

Stores images.

Examples:

* Docker Hub
* Private Registry
* AWS ECR
* Azure ACR
* GitHub Container Registry

### 4. Docker Objects

| Object    | Purpose                            |
| --------- | ---------------------------------- |
| Image     | Template used to create containers |
| Container | Running instance of image          |
| Volume    | Persistent storage                 |
| Network   | Communication between containers   |

---

# 3. Docker Workflow (Real DevOps Flow)

```
Developer writes code
       ↓
Create Dockerfile
       ↓
docker build → Image
       ↓
docker push → Registry
       ↓
CI/CD pipeline deploys image
       ↓
Container runs in production
```

Example pipeline:

```
GitHub → Jenkins → Docker build → Push to registry → Kubernetes deploy
```

---

# 4. Dockerfile Deep Dive

A Dockerfile is used to **build container images**.

Example:

```
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python","app.py"]
```

### Important Dockerfile Instructions

| Instruction | Purpose                      |
| ----------- | ---------------------------- |
| FROM        | Base image                   |
| WORKDIR     | Set working directory        |
| COPY        | Copy files                   |
| ADD         | Copy + extract archives      |
| RUN         | Execute command during build |
| ENV         | Set environment variable     |
| EXPOSE      | Document container port      |
| CMD         | Default command              |
| ENTRYPOINT  | Container executable         |

---

# 5. Multi-Stage Docker Builds (Production Standard)

Used to **reduce image size**.

Example:

```
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

# Runtime stage
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
```

Benefits:

* Smaller images
* Faster deployments
* Improved security

---

# 6. Docker Networking

Containers communicate through **Docker networks**.

### Default Networks

| Network | Purpose                   |
| ------- | ------------------------- |
| bridge  | Default container network |
| host    | Uses host network         |
| none    | No networking             |

### Create Network

```
docker network create app-network
```

### Run containers in network

```
docker run -d --network app-network --name web nginx
docker run -d --network app-network --name db mysql
```

Now containers communicate using:

```
web → db:3306
```

---

# 7. Docker Volumes (Persistent Storage)

Containers are **ephemeral**.

If container is deleted → data is lost.

Volumes solve this.

### Create volume

```
docker volume create mysql-data
```

### Run container with volume

```
docker run -d \
-v mysql-data:/var/lib/mysql \
mysql
```

Now database data persists.

---

# 8. Docker Compose (Multi-Container Apps)

Used to manage **multiple containers**.

Example:

```
version: "3"

services:

  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
```

Run services:

```
docker compose up -d
```

Stop services:

```
docker compose down
```

---

# 9. Docker Debugging (Real Production)

### Step 1 Check containers

```
docker ps
```

### Step 2 Check logs

```
docker logs container_id
```

### Step 3 Enter container

```
docker exec -it container_id bash
```

### Step 4 Inspect container

```
docker inspect container_id
```

### Step 5 Monitor resources

```
docker stats
```

---

# 10. Docker Security Best Practices

### Never run container as root

Bad:

```
FROM node
```

Better:

```
RUN useradd appuser
USER appuser
```

### Use minimal images

```
python:3.11-slim
node:18-alpine
```

### Scan images

Example tools:

* Trivy
* Clair
* Snyk

---

# 11. Image Size Optimization

Large images slow CI/CD.

Tips:

### Use slim images

```
python:3.11-slim
```

### Remove cache

```
apt-get clean
```

### Combine RUN commands

Bad:

```
RUN apt update
RUN apt install curl
```

Good:

```
RUN apt update && apt install -y curl
```

### Use .dockerignore

Example:

```
.git
node_modules
.env
__pycache__
*.log
```

---

# 12. Docker in CI/CD Pipelines

Typical pipeline:

```
Git Push
   ↓
CI Pipeline
   ↓
Docker Build
   ↓
Docker Push
   ↓
Deployment
```

Example CI script:

```
docker build -t myapp:$CI_COMMIT_SHA .
docker push myapp:$CI_COMMIT_SHA
```

---

# 13. Docker Monitoring

Monitor containers:

```
docker stats
```

Show processes:

```
docker top container_id
```

View logs:

```
docker logs -f container_id
```

Production monitoring tools:

* Prometheus
* Grafana
* ELK stack

---

# 14. Docker vs Virtual Machines

| Feature      | Docker        | VM      |
| ------------ | ------------- | ------- |
| Startup Time | Seconds       | Minutes |
| Size         | MBs           | GBs     |
| Isolation    | Process level | Full OS |
| Performance  | Faster        | Slower  |

---

# 15. Docker vs Kubernetes

| Docker            | Kubernetes              |
| ----------------- | ----------------------- |
| Builds containers | Orchestrates containers |
| Single host       | Multi-node cluster      |
| Manual scaling    | Auto scaling            |

Typical flow:

```
Docker → build image
Kubernetes → run containers
```

---

# 16. 25 Docker Interview Questions

### Basic

1. What is Docker?
2. What is containerization?
3. Difference between container and VM?
4. What is Dockerfile?
5. What is Docker image?

### Intermediate

6. Difference between CMD and ENTRYPOINT
7. What is Docker Compose?
8. What is multi-stage build?
9. What is Docker volume?
10. What is Docker network?

### Advanced

11. How to reduce Docker image size?
12. How to debug failing container?
13. How to secure Docker images?
14. What is Docker registry?
15. What is Docker overlay network?

### Real DevOps

16. How Docker works with Kubernetes?
17. How to push image to private registry?
18. How to run multi-container apps?
19. How to handle secrets?
20. How to monitor containers?

---

# 17. Real Production Deployment Example

Example architecture:

```
User
 ↓
Load Balancer
 ↓
Nginx Container
 ↓
App Containers
 ↓
Database Container
```

Example stack:

```
Nginx
Django
Redis
PostgreSQL
```

All running in containers.

---

# 18. Complete Docker Development Workflow

```
Write Code
   ↓
Create Dockerfile
   ↓
docker build
   ↓
docker run
   ↓
docker push
   ↓
CI/CD pipeline
   ↓
Deploy to Kubernetes
```

---

# 19. Most Important Docker Commands

```
docker build
docker run
docker ps
docker logs
docker exec
docker images
docker pull
docker push
docker inspect
docker system prune
```

---

# 20. DevOps Pro Tips

### Always version images

```
myapp:v1.0
myapp:v1.1
```

Never rely on `latest`.

### Use health checks

```
HEALTHCHECK CMD curl -f http://localhost:8080 || exit 1
```

### Use multi-stage builds

Reduces image size by **60–80%**.

### Use environment variables

```
ENV APP_ENV=production
```

---

# Final Advice for DevOps Engineers

Master these topics:

```
Dockerfile
Image optimization
Docker networking
Docker volumes
Docker Compose
Container debugging
CI/CD integration
Security best practices
```

If you understand these well, **Docker becomes easy to use in Kubernetes and cloud platforms.**

---

END OF GUIDE

If you want, I can also give you **a DevOps engineer’s “Docker Real-World Playbook”** including:

* 15 **production incidents caused by Docker**
* how engineers **debug container crashes**
* **image size reduction from 1.2GB → 120MB**
* **CI/CD pipeline with Docker + GitLab (step-by-step)**
* **Docker interview answers used by FAANG companies**.
