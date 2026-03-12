# Docker Cheat Sheet

A quick reference for commonly used Docker commands and Dockerfile instructions.

---

# 1. Container Commands

### Run a container

```bash
docker run -d -p 8080:80 nginx
```

### List running containers

```bash
docker ps
```

### List all containers (including stopped)

```bash
docker ps -a
```

### Stop a container

```bash
docker stop <container_id>
```

### Remove a container

```bash
docker rm <container_id>
```

### Execute a command inside a running container

```bash
docker exec -it <container_id> /bin/bash
```

### View container logs

```bash
docker logs <container_id>
```

### Follow logs in real-time

```bash
docker logs -f <container_id>
```

---

# 2. Image Commands

### Build an image from Dockerfile

```bash
docker build -t myimage:1.0 .
```

### Pull image from registry

```bash
docker pull nginx
```

### Push image to registry

```bash
docker push username/myimage:1.0
```

### Tag an image

```bash
docker tag myimage:1.0 username/myimage:latest
```

### List images

```bash
docker images
```

### Remove image

```bash
docker rmi <image_id>
```

---

# 3. Volume Commands

### Create a volume

```bash
docker volume create myvolume
```

### List volumes

```bash
docker volume ls
```

### Inspect a volume

```bash
docker volume inspect myvolume
```

### Remove a volume

```bash
docker volume rm myvolume
```

---

# 4. Network Commands

### Create a network

```bash
docker network create mynetwork
```

### List networks

```bash
docker network ls
```

### Inspect a network

```bash
docker network inspect mynetwork
```

### Connect container to a network

```bash
docker network connect mynetwork <container_id>
```

---

# 5. Docker Compose Commands

### Start services

```bash
docker compose up
```

### Start services in detached mode

```bash
docker compose up -d
```

### Stop and remove services

```bash
docker compose down
```

### List running services

```bash
docker compose ps
```

### View logs

```bash
docker compose logs
```

### Build services

```bash
docker compose build
```

---

# 6. Cleanup Commands

### Remove unused containers, networks, and images

```bash
docker system prune
```

### Remove all unused images

```bash
docker image prune -a
```

### Show disk usage

```bash
docker system df
```

---

# 7. Dockerfile Instructions

### Base image

```dockerfile
FROM python:3.11
```

### Run command during build

```dockerfile
RUN apt-get update && apt-get install -y curl
```

### Copy files

```dockerfile
COPY . /app
```

### Set working directory

```dockerfile
WORKDIR /app
```

### Expose container port

```dockerfile
EXPOSE 5000
```

### Default command

```dockerfile
CMD ["python", "app.py"]
```

### Set entrypoint

```dockerfile
ENTRYPOINT ["python"]
```

---

# Quick Example Docker Workflow

```bash
# Build image
docker build -t myapp .

# Run container
docker run -d -p 5000:5000 myapp

# Check containers
docker ps

# View logs
docker logs <container_id>

# Stop container
docker stop <container_id>
```

---

If you want, I can also create an **advanced DevOps Docker cheat sheet (interview + real production commands)** including:

* multi-stage builds
* container debugging
* docker inspect usage
* docker stats monitoring
* registry authentication
* image optimization techniques.


'''
# Advanced Docker Cheat Sheet (DevOps + Production)

This cheat sheet covers **real-world Docker commands used by DevOps engineers** for debugging, optimization, monitoring, and CI/CD pipelines.

---

# 1. Container Debugging Commands

### Inspect container details

Shows full JSON configuration (network, mounts, env variables).

```bash
docker inspect <container_id>
```

### Show container resource usage

```bash
docker stats
```

Example output includes:

* CPU %
* Memory usage
* Network I/O
* Block I/O

---

### Enter running container shell

```bash
docker exec -it <container_id> /bin/bash
```

If bash is not available:

```bash
docker exec -it <container_id> sh
```

---

### Copy files between host and container

Host → Container

```bash
docker cp file.txt <container_id>:/app
```

Container → Host

```bash
docker cp <container_id>:/app/log.txt .
```

---

# 2. Image Optimization (Production Best Practices)

### Check image layers

```bash
docker history <image_name>
```

Helps identify **large layers** that increase image size.

---

### Reduce image size

Best practices:

```
Use alpine or slim images
Combine RUN commands
Use multi-stage builds
Remove package caches
Use .dockerignore
```

Example:

```dockerfile
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*
```

---

# 3. Docker Registry Commands

### Login to Docker Hub

```bash
docker login
```

### Push image

```bash
docker push username/myapp:1.0
```

### Pull image

```bash
docker pull username/myapp:1.0
```

### Tag image for registry

```bash
docker tag myapp:1.0 username/myapp:1.0
```

---

# 4. Volume Mounting (Persistent Storage)

### Run container with volume

```bash
docker run -d -v myvolume:/app/data nginx
```

### Bind mount (host directory)

```bash
docker run -d -v $(pwd):/app node
```

Use cases:

* Database persistence
* Log storage
* Development environments

---

# 5. Network Troubleshooting

### Show container IP

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_id>
```

### Test connectivity inside container

```bash
docker exec -it <container_id> ping google.com
```

### Connect container to network

```bash
docker network connect mynetwork container1
```

---

# 6. Container Lifecycle Management

### Restart container

```bash
docker restart <container_id>
```

### Start stopped container

```bash
docker start <container_id>
```

### Pause container

```bash
docker pause <container_id>
```

### Unpause container

```bash
docker unpause <container_id>
```

---

# 7. Docker System Cleanup

### Remove stopped containers

```bash
docker container prune
```

### Remove unused volumes

```bash
docker volume prune
```

### Remove unused networks

```bash
docker network prune
```

### Clean everything

```bash
docker system prune -a
```

---

# 8. Docker Monitoring Commands

### Container resource usage

```bash
docker stats
```

### Top processes inside container

```bash
docker top <container_id>
```

### Container logs with timestamps

```bash
docker logs -t <container_id>
```

---

# 9. Docker Build Optimization (CI/CD)

### Build with no cache

```bash
docker build --no-cache -t myapp .
```

### Build using specific Dockerfile

```bash
docker build -f Dockerfile.prod -t myapp .
```

### Build multi-platform images

```bash
docker buildx build --platform linux/amd64,linux/arm64 .
```

---

# 10. .dockerignore (Important for DevOps)

Example `.dockerignore`

```
.git
node_modules
.env
__pycache__
*.log
Dockerfile
README.md
```

Benefits:

* Faster builds
* Smaller images
* Prevents secrets from entering image

---

# 11. Production Dockerfile Best Practices

Example:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["gunicorn", "app:app"]
```

Best practices:

* Use **slim or alpine images**
* Avoid running as **root**
* Use **multi-stage builds**
* Keep layers minimal

---

# 12. Docker Compose Production Example

```yaml
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
      POSTGRES_DB: appdb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
```

Run services:

```bash
docker compose up -d
```

---

# 13. DevOps Interview Commands (Must Know)

Most commonly asked commands:

```
docker ps
docker images
docker run
docker exec
docker logs
docker inspect
docker build
docker pull
docker push
docker system prune
```

---

# 14. Real Production Debugging Flow

Typical debugging process:

```
1. docker ps
2. docker logs <container>
3. docker exec -it <container> bash
4. check config / logs
5. docker inspect <container>
6. docker restart <container>
```

---

# Quick Summary

| Category   | Key Commands              |
| ---------- | ------------------------- |
| Containers | run, ps, exec, logs, stop |
| Images     | build, pull, push, tag    |
| Volumes    | create, inspect, rm       |
| Networks   | create, connect           |
| Monitoring | stats, logs, top          |
| Cleanup    | prune, system df          |

---

# DevOps Tip

In **production Kubernetes environments**, Docker knowledge is still critical because:

* Kubernetes uses **container images**
* CI/CD pipelines build Docker images
* Debugging often starts with Docker locally

---

If you'd like, I can also create a **complete DevOps Docker Master Guide (much deeper)** including:

* 25 real **Docker interview questions**
* **Docker vs Containerd vs Podman**
* **Image security scanning**
* **Dockerfile optimization (reduce image 70%)**
* **Docker networking deep dive**
* **Real CI/CD pipeline with Docker + GitLab**

'''
