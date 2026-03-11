---

## Challenge Tasks

### Task 1: Build Your Own App Stack
Create a `docker-compose.yml` for a 3-service stack:
- A **web app** (use Python Flask, Node.js, or any language you know)
- A **database** (Postgres or MySQL)
- A **cache** (Redis)

Write a simple Dockerfile for the web app. The app doesn't need to be complex — even a "Hello World" that connects to the database is enough.


'''
Great task 👍 — this is exactly how a **real microservice stack** is built.
We will create a **3-service application stack**:

* Web App → **Python Flask**
* Database → **PostgreSQL**
* Cache → **Redis**

---

# 1️⃣ Project Structure

Create a folder:

```bash
mkdir app-stack
cd app-stack
```

Directory layout:

```
app-stack
│
├── app
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
└── docker-compose.yml
```

---

# 2️⃣ Simple Web Application (Flask)

Create:

### `app/app.py`

```python
from flask import Flask
import psycopg2
import redis
import os

app = Flask(__name__)

db_host = os.getenv("DB_HOST", "db")
redis_host = os.getenv("REDIS_HOST", "redis")

@app.route("/")
def hello():

    try:
        conn = psycopg2.connect(
            host=db_host,
            database="testdb",
            user="postgres",
            password="postgres"
        )
        db_status = "Connected to Postgres"
    except:
        db_status = "DB connection failed"

    try:
        r = redis.Redis(host=redis_host, port=6379)
        r.set("visits", 1)
        cache_status = "Connected to Redis"
    except:
        cache_status = "Redis connection failed"

    return f"""
    Hello from Flask App 🚀<br>
    Database: {db_status}<br>
    Cache: {cache_status}
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

# 3️⃣ Python Dependencies

### `app/requirements.txt`

```
flask
psycopg2-binary
redis
```

---

# 4️⃣ Dockerfile for Web App

### `app/Dockerfile`

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

This Dockerfile:

* Uses Python base image
* Installs dependencies
* Copies app code
* Exposes port 5000
* Starts Flask app

---

# 5️⃣ Docker Compose (Full Stack)

Create:

### `docker-compose.yml`

```yaml
version: "3.8"

services:

  web:
    build: ./app
    container_name: flask-app
    ports:
      - "5000:5000"
    environment:
      - DB_HOST=db
      - REDIS_HOST=redis
    depends_on:
      - db
      - redis

  db:
    image: postgres:15
    container_name: postgres-db
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis:7
    container_name: redis-cache

volumes:
  postgres-data:
```

---

# 6️⃣ Run the Entire Stack

From root folder:

```bash
docker compose up --build
```

This will start:

| Service  | Port |
| -------- | ---- |
| Web App  | 5000 |
| Postgres | 5432 |
| Redis    | 6379 |

---

# 7️⃣ Test the Application

Open browser:

```
http://localhost:5000
```

Expected output:

```
Hello from Flask App 🚀
Database: Connected to Postgres
Cache: Connected to Redis
```

---

# 8️⃣ Verify Containers

```bash
docker ps
```

You should see:

```
flask-app
postgres-db
redis-cache
```

---

# 9️⃣ Architecture Diagram

```
          Browser
             |
             |
         Flask App
        (Dockerfile)
          /     \
         /       \
   PostgreSQL   Redis
   (Database)   (Cache)
```

---

# 🔟 Why This Task Is Important (DevOps)

This is the **foundation of microservices deployment**:

Later this becomes:

| Local          | Production         |
| -------------- | ------------------ |
| Docker Compose | Kubernetes         |
| Single host    | Multi-node cluster |
| Local testing  | Cloud deployment   |

---

# 🚀 If you want, next I can show you

* How to convert this stack to **Kubernetes manifests**
* How to deploy this stack on **Amazon Web Services** EKS
* How to build a **CI/CD pipeline for this app**
* How to add **Prometheus monitoring**
* How to make this a **real production DevOps project**

Just tell me 👍

'''
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/321c5a3b-91d8-4b41-b948-acc06432498b" />

---

### Task 2: depends_on & Healthchecks
1. Add `depends_on` to your compose file so the app starts **after** the database
2. Add a **healthcheck** on the database service
3. Use `depends_on` with `condition: service_healthy` so the app waits for the database to be truly ready, not just started

**Test:** Bring everything down and up — does the app wait for the DB?

'''
Good task 👍 — this teaches an important **Docker Compose startup dependency pattern**.

We will modify the stack so the **web app waits until the database is truly ready**, not just started.

Database used: **PostgreSQL**

---

# 1️⃣ Problem Without Healthchecks

Normally in Docker Compose:

```yaml
depends_on:
  - db
```

This only means:

> Start DB container **before** web container.

But it **does NOT guarantee DB is ready**.

Postgres may still be **initializing**, causing app failures.

---

# 2️⃣ Add Database Healthcheck

We add a **healthcheck** so Docker can detect when DB is ready.

Update your `docker-compose.yml`.

---

# 3️⃣ Updated docker-compose.yml

```yaml
version: "3.8"

services:

  web:
    build: ./app
    container_name: flask-app
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
      REDIS_HOST: redis
    depends_on:
      db:
        condition: service_healthy
    restart: always

  db:
    image: postgres:15
    container_name: postgres-db
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres-data:/var/lib/postgresql/data

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
      start_period: 10s

  redis:
    image: redis:7
    container_name: redis-cache

volumes:
  postgres-data:
```

---

# 4️⃣ Healthcheck Explanation

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U postgres"]
```

`pg_isready` checks if Postgres accepts connections.

Other parameters:

| Field        | Meaning                     |
| ------------ | --------------------------- |
| interval     | Run check every 5 sec       |
| timeout      | Max wait per check          |
| retries      | Fail after 5 attempts       |
| start_period | Wait before starting checks |

---

# 5️⃣ depends_on With Condition

```yaml
depends_on:
  db:
    condition: service_healthy
```

Now the **web container will wait until DB healthcheck passes**.

---

# 6️⃣ Test the Setup

First stop everything.

```bash
docker compose down
```

Then start again:

```bash
docker compose up --build
```

---

# 7️⃣ Observe Startup Logs

You should see:

```
postgres-db   | database system is ready to accept connections
postgres-db   | healthcheck: healthy
flask-app     | Starting Flask application
```

Web container starts **only after DB becomes healthy**.

---

# 8️⃣ Verify Health Status

Run:

```bash
docker ps
```

You should see:

```
postgres-db   healthy
flask-app     running
redis-cache   running
```

---

# 9️⃣ Architecture Flow Now

```
Docker Compose
      |
      |
   PostgreSQL (healthcheck)
      |
   Healthy
      |
   Flask Web App Starts
      |
   Connects to Redis Cache
```

---

# 🔟 Why This Is Important in Real DevOps

Without health checks:

* App crashes during startup
* Containers restart repeatedly
* CI/CD deployments fail

With health checks:

* Reliable service startup
* Better orchestration
* Cleaner deployments

In **Kubernetes**, this concept becomes:

| Docker Compose | Kubernetes                        |
| -------------- | --------------------------------- |
| healthcheck    | readinessProbe                    |
| depends_on     | initContainers / startup ordering |

---

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5fe1b9ad-79a5-4bf5-8ece-d941ef8422dc" />


# 🚀 If you want, next I can show you

**3 extremely important DevOps topics:**

1️⃣ **Docker Compose networking deep dive**
2️⃣ **Persist database data with volumes properly**
3️⃣ **Convert this whole stack to Kubernetes (Helm ready)**

These are **very common DevOps interview tasks**.


'''
---

### Task 3: Restart Policies
1. Add `restart: always` to your database service
2. Manually kill the database container — does it come back?
3. Try `restart: on-failure` — how is it different?
4. Write in your notes: When would you use each restart policy?

'''
Good task 👍 — restart policies are very important for **container reliability in production**.

We will test this using **PostgreSQL** as the database container.

---

# 1️⃣ Add `restart: always` to Database Service

Update your `docker-compose.yml`.

```yaml
services:

  db:
    image: postgres:15
    container_name: postgres-db
    restart: always
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
```

`restart: always` means:

> Docker will **always restart the container if it stops**, even after Docker daemon restart.

---

# 2️⃣ Start the Stack

```bash
docker compose up -d
```

Check containers:

```bash
docker ps
```

You should see:

```
postgres-db
flask-app
redis-cache
```

---

# 3️⃣ Manually Kill the Database Container

Run:

```bash
docker kill postgres-db
```

Now check again:

```bash
docker ps
```

You will see:

```
postgres-db restarting
```

Then shortly:

```
postgres-db running again
```

✔ This proves **restart policy works**.

---

# 4️⃣ Change to `restart: on-failure`

Update compose:

```yaml
restart: on-failure
```

Restart the stack:

```bash
docker compose down
docker compose up -d
```

Now kill container again:

```bash
docker kill postgres-db
```

Check status.

If container exits with **failure code**, Docker restarts it.

But if container stops normally → **it will NOT restart**.

---

# 5️⃣ Difference Between Restart Policies

| Policy         | Behavior                            |
| -------------- | ----------------------------------- |
| no             | Default. Container will not restart |
| always         | Always restart container            |
| on-failure     | Restart only if container crashes   |
| unless-stopped | Restart unless manually stopped     |

---

# 6️⃣ Example Behavior

| Scenario      | always  | on-failure   |
| ------------- | ------- | ------------ |
| App crash     | Restart | Restart      |
| Manual stop   | Restart | ❌ No restart |
| Docker reboot | Restart | Restart      |

---

# 7️⃣ When To Use Each Policy (Write in Notes)

### 🔹 restart: always

Use for:

* Databases
* Core services
* Infrastructure containers

Example:

* Postgres
* Redis
* Monitoring stack

Reason:
These services **must always run**.

---

### 🔹 restart: on-failure

Use for:

* Worker containers
* Batch jobs
* Crash recovery tasks

Example:

* ETL jobs
* Data processing containers

Reason:
Restart only if **something went wrong**.

---

### 🔹 restart: unless-stopped

Best production choice for long-running services.

Example:

* Web servers
* API services
* Message queues

It restarts automatically but respects manual stops.

---

# 8️⃣ Real DevOps Insight

Restart policies are useful in:

* standalone Docker hosts
* Docker Compose environments
* CI runners

But in **Kubernetes**, restart behavior is controlled by:

| Kubernetes    | Equivalent                |
| ------------- | ------------------------- |
| restartPolicy | Container restart control |
| Deployment    | Always running pods       |
| Job           | Restart on failure        |

---

# 9️⃣ Visual Flow

```
Container Crash
      |
Restart Policy Check
      |
 restart: always  → restart
 restart: on-failure → restart only if error
 restart: no → container stays stopped
```

---

# 🚀 If you want, next I can show you a **very important DevOps concept**

**Docker Compose Networking (how containers talk internally)**

This includes:

* Service DNS
* Internal networks
* Bridge networks
* Debugging container communication

This is asked **a lot in DevOps interviews**.

'''

---
### Task 4: Custom Dockerfiles in Compose
1. Instead of using a pre-built image for your app, use `build:` in your compose file to build from a Dockerfile
2. Make a code change in your app
3. Rebuild and restart with one command

'''
This task is about using **Docker Compose to build your app image automatically from a Dockerfile instead of pulling a pre-built image**. The container platform involved is **Docker**.

---

# 1️⃣ Use `build:` Instead of `image:` in Compose

Update your **docker-compose.yml** for the web app service.

```yaml
version: "3.8"

services:

  web:
    build: ./app
    container_name: flask-app
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
      REDIS_HOST: redis
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres

  redis:
    image: redis:7
```

Explanation:

```
build: ./app
```

Means:

> Docker Compose will build the image from the **Dockerfile inside the `app` directory**.

---

# 2️⃣ Your Web App Dockerfile

Inside `app/Dockerfile`:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

---

# 3️⃣ Make a Code Change

Open:

```
app/app.py
```

Change the message:

```python
return "Hello from my updated Flask App 🚀"
```

---

# 4️⃣ Rebuild and Restart with One Command

Run:

```bash
docker compose up --build
```

What happens internally:

1️⃣ Compose detects **build directive**
2️⃣ Rebuilds the Docker image
3️⃣ Stops the old container
4️⃣ Starts a new container with updated image

---

# 5️⃣ Faster Method (Detached Mode)

```bash
docker compose up -d --build
```

---

# 6️⃣ Verify the Update

Open browser:

```
http://localhost:5000
```

You should see:

```
Hello from my updated Flask App 🚀
```

---

# 7️⃣ What Happened Behind the Scenes

```
Source Code Change
        ↓
Dockerfile Build
        ↓
New Docker Image
        ↓
Old Container Removed
        ↓
New Container Started
```

---

# 8️⃣ Why DevOps Engineers Use This

Benefits:

✔ Automatic image build
✔ Easy local development
✔ CI/CD integration
✔ No manual `docker build` required

Typical pipeline flow:

```
Git Push
   ↓
CI Pipeline
   ↓
Docker Build
   ↓
Push to Registry
   ↓
Deploy via Kubernetes
```

---

✅ **Your notes answer for this task**

* `build:` in compose builds an image from a local Dockerfile.
* After making code changes, you can rebuild and restart the containers with:

```
docker compose up --build
```

This rebuilds the image and restarts the services in one command.

---

If you want, I can also show you the **next DevOps-level task most engineers miss:**

**Docker Compose Networking & Service Discovery (how containers talk using DNS).**

This becomes **critical when moving to Kubernetes.**


'''
---




### Task 5: Named Networks & Volumes
1. Define **explicit networks** in your compose file instead of relying on the default
2. Define **named volumes** for database data
3. Add **labels** to your services for better organization

'''
This task improves your **Docker Compose architecture** by making networking, storage, and service metadata explicit in **Docker**.

You will implement:

* **Named networks** → better service communication
* **Named volumes** → persistent database storage
* **Labels** → metadata for organization and tooling

Your database will still use **PostgreSQL** and cache **Redis**.

---

# 1️⃣ Updated docker-compose.yml

```yaml
version: "3.8"

services:

  web:
    build: ./app
    container_name: flask-app
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
      REDIS_HOST: redis
    depends_on:
      db:
        condition: service_healthy
    networks:
      - app-network
    labels:
      app: "demo-stack"
      service: "web"
      environment: "dev"

  db:
    image: postgres:15
    container_name: postgres-db
    restart: always
    environment:
      POSTGRES_DB: testdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
    labels:
      app: "demo-stack"
      service: "database"

  redis:
    image: redis:7
    container_name: redis-cache
    networks:
      - app-network
    labels:
      app: "demo-stack"
      service: "cache"

networks:
  app-network:
    driver: bridge

volumes:
  postgres-data:
```

---

# 2️⃣ Named Network Explanation

Instead of Docker’s default network, we explicitly define:

```yaml
networks:
  app-network:
    driver: bridge
```

Benefits:

* Better **isolation**
* Clear **service grouping**
* Easier **multi-network architecture**

All services connect to this network:

```yaml
networks:
  - app-network
```

Inside the network, containers communicate using **service names**:

```
web → db
web → redis
```

---

# 3️⃣ Named Volume for Database

To persist database data:

```yaml
volumes:
  - postgres-data:/var/lib/postgresql/data
```

Define it at the bottom:

```yaml
volumes:
  postgres-data:
```

Benefits:

* Data survives container restart
* Data survives container deletion
* Easy backup/migration

---

# 4️⃣ Labels for Organization

Example:

```yaml
labels:
  app: "demo-stack"
  service: "web"
  environment: "dev"
```

Labels help with:

* monitoring systems
* container management tools
* filtering containers

Example command:

```bash
docker ps --filter label=app=demo-stack
```

---

# 5️⃣ Start the Stack

```bash
docker compose up -d --build
```

---

# 6️⃣ Verify Network

```bash
docker network ls
```

You should see:

```
app-stack_app-network
```

Inspect it:

```bash
docker network inspect app-stack_app-network
```

---

# 7️⃣ Verify Volume

```bash
docker volume ls
```

You should see:

```
app-stack_postgres-data
```

Inspect volume:

```bash
docker volume inspect app-stack_postgres-data
```

---

# 8️⃣ Architecture Now

```
                Browser
                   |
                   |
                Flask App
                   |
          ---------------------
          |                   |
      PostgreSQL           Redis
        (Volume)           (Cache)

All containers connected through:
        app-network
```

---

# 9️⃣ Why This Matters in DevOps

Explicit networks and volumes are required when:

* moving to **microservices**
* running multiple stacks on the same host
* integrating monitoring/logging

Concept mapping to orchestration platforms:

| Docker Compose | Kubernetes         |
| -------------- | ------------------ |
| networks       | cluster networking |
| volumes        | persistent volumes |
| labels         | pod labels         |

---

✅ **What you should write in your notes**

**Named networks**

* Provide controlled container communication
* Improve service isolation

**Named volumes**

* Persist database data outside containers
* Prevent data loss when containers restart

**Labels**

* Add metadata to services
* Used for filtering, monitoring, and automation

---

If you want, I can also show you the **next advanced DevOps step** (very useful for interviews):

**Docker Compose → Kubernetes migration of this exact stack**
(including Deployment, Service, ConfigMap, and PersistentVolume).

'''
---
### Task 6: Scaling (Bonus)
1. Try scaling your web app to 3 replicas using `docker compose up --scale`
2. What happens? What breaks?
3. Write in your notes: Why doesn't simple scaling work with port mapping?

'''
This task demonstrates a **real limitation of scaling in Docker Compose** using **Docker**.

Let’s walk through the experiment and what happens.

---

# 1️⃣ Scale the Web App to 3 Replicas

Run:

```bash
docker compose up -d --scale web=3
```

This tells Docker Compose to start:

```
web-1
web-2
web-3
```

Check containers:

```bash
docker ps
```

Example output:

```
flask-app-1
flask-app-2
flask-app-3
postgres-db
redis-cache
```

---

# 2️⃣ What Happens? (The Problem)

Your compose file likely contains:

```yaml
ports:
  - "5000:5000"
```

Docker tries to map:

```
Host Port 5000 → Container Port 5000
```

But only **one container can use host port 5000**.

Result:

```
Error: port is already allocated
```

Only **one replica starts successfully**.

---

# 3️⃣ Why Port Mapping Breaks Scaling

Host ports must be **unique**.

Example:

| Container | Port Mapping      |
| --------- | ----------------- |
| web-1     | 5000 → 5000       |
| web-2     | ❌ cannot use 5000 |
| web-3     | ❌ cannot use 5000 |

So Docker fails because:

> Multiple containers cannot bind to the same host port.

---

# 4️⃣ What Would Work Instead?

Instead of exposing every container to the host, you normally add a **load balancer container** such as **Nginx**.

Architecture becomes:

```
Browser
   |
   |
 Nginx (port 80)
   |
----------------------
|        |           |
web-1   web-2      web-3
```

Only **Nginx exposes a port**, while web containers communicate internally via Docker network.

---

# 5️⃣ Alternative (Random Ports)

If you remove the host port mapping:

```yaml
ports:
  - "5000"
```

Docker assigns random ports:

```
49153
49154
49155
```

But this still doesn't provide **automatic load balancing**.

---

# 6️⃣ How Scaling Works in Real Production

In orchestration systems like **Kubernetes**, scaling works because of:

| Feature       | Purpose                |
| ------------- | ---------------------- |
| Service       | Single stable endpoint |
| Load Balancer | Distributes traffic    |
| ReplicaSet    | Maintains pod count    |

Example:

```
Service
   |
Load Balancer
   |
Pod-1  Pod-2  Pod-3
```

---

# 7️⃣ What You Should Write in Your Notes

**Why scaling breaks with port mapping**

* Host ports must be unique.
* Multiple containers cannot bind to the same host port.
* When scaling services with `docker compose --scale`, port conflicts occur.

**How to solve it**

* Use a reverse proxy (Nginx)
* Use internal Docker networking
* Expose only the load balancer

---

# 8️⃣ Key DevOps Insight

Docker Compose scaling is mainly for:

* development environments
* testing replicas

Real scaling in production uses:

* container orchestrators
* load balancers
* service discovery

---

✅ If you want, I can also show you the **next advanced task most DevOps engineers practice**:

**Build a full production-style stack**

```
Nginx Load Balancer
        ↓
3 Flask Containers
        ↓
Postgres + Redis
```

This demonstrates **real horizontal scaling and load balancing**, which is often asked in **DevOps interviews.**

'''
---

