---
'''
## Self-Assessment Checklist
Mark yourself honestly — **can do**, **shaky**, or **haven't done**:

- [Y] Run a container from Docker Hub (interactive + detached)
- [Y] List, stop, remove containers and images
- [Y] Explain image layers and how caching works
- [Y] Write a Dockerfile from scratch with FROM, RUN, COPY, WORKDIR, CMD
- [Y] Explain CMD vs ENTRYPOINT
- [Y] Build and tag a custom image
- [Y] Create and use named volumes
- [Y] Use bind mounts
- [Y] Create custom networks and connect containers
- [Y] Write a docker-compose.yml for a multi-container app
- [Y] Use environment variables and .env files in Compose
- [Y] Write a multi-stage Dockerfile
- [Y] Push an image to Docker Hub
- [Y] Use healthchecks and depends_on
'''
---
---

## Quick-Fire Questions
Answer from memory, then verify:
1. What is the difference between an image and a container?
2. What happens to data inside a container when you remove it?
3. How do two containers on the same custom network communicate?
4. What does `docker compose down -v` do differently from `docker compose down`?
5. Why are multi-stage builds useful?
6. What is the difference between `COPY` and `ADD`?
7. What does `-p 8080:80` mean?
8. How do you check how much disk space Docker is using?

   '''
   Great exercise 👍 — this is exactly how **DevOps interviews test Docker knowledge**.
Answer **from memory**, then verify below.

---

## 1️⃣ Difference between Image and Container

**Docker Image**

* A **read-only template** used to create containers.
* Contains application code, runtime, libraries, and dependencies.

**Docker Container**

* A **running instance of an image**.
* It is a **lightweight, isolated environment** where the application runs.

**Example**

```bash
docker run nginx
```

* `nginx` → image
* running instance → container

---

## 2️⃣ What happens to data inside a container when you remove it?

When a container is removed:

```bash
docker rm <container_id>
```

➡ **All data inside the container filesystem is deleted.**

Containers are **ephemeral**.

To persist data, use:

* **Volumes**
* **Bind mounts**

Example:

```bash
docker run -v myvolume:/data mysql
```

---

## 3️⃣ How do two containers on the same custom network communicate?

Containers on the same **custom Docker network** communicate using:

**container names as DNS hostnames**

Example:

```bash
docker network create mynet
```

Run containers:

```bash
docker run -d --network mynet --name web nginx
docker run -d --network mynet --name db mysql
```

Now communication works like:

```
web → db:3306
```

Docker provides **built-in DNS resolution**.

---

## 4️⃣ Difference between `docker compose down` and `docker compose down -v`

### `docker compose down`

Stops and removes:

* containers
* networks

But **keeps volumes**.

---

### `docker compose down -v`

Stops and removes:

* containers
* networks
* **volumes**

So **all data is deleted**.

Example:

```bash
docker compose down -v
```

⚠ Dangerous if using databases.

---

## 5️⃣ Why are multi-stage builds useful?

Multi-stage builds allow:

* separating **build environment** from **runtime environment**

Example:

```
Stage 1 → compile/build app
Stage 2 → run minimal image
```

Benefits:

✔ Smaller images
✔ Faster deployments
✔ Better security
✔ Removes build dependencies

Example reduction:

```
1.2GB → 150MB image
```

---

## 6️⃣ Difference between `COPY` and `ADD`

### COPY

Simply copies files from host → container.

```
COPY src/ /app/
```

---

### ADD

Has extra features:

1️⃣ Can **extract tar archives automatically**
2️⃣ Can **download files from URLs**

Example:

```
ADD app.tar.gz /app
```

Best practice:

✔ Prefer **COPY**
❌ Use **ADD only when needed**

---

## 7️⃣ What does `-p 8080:80` mean?

Docker **port mapping**.

Format:

```
-p <host_port>:<container_port>
```

Example:

```
-p 8080:80
```

Meaning:

```
Browser → localhost:8080
           ↓
Container → port 80
```

---

## 8️⃣ How do you check how much disk space Docker is using?

Command:

```bash
docker system df
```

Shows:

* image size
* container size
* volume size
* build cache

Example output:

```
TYPE            TOTAL     ACTIVE    SIZE
Images          10        3         2.5GB
Containers      5         2         200MB
Volumes         4         3         1.2GB
```

---

# ⭐ DevOps Interview Tip

If interviewer asks **“How do you free Docker disk space?”**

Answer:

```bash
docker system prune
```

Or aggressive cleanup:

```bash
docker system prune -a
```

---

If you want, I can also give you the **“Top 30 Docker Interview Questions asked in DevOps roles (with perfect answers)”** — these are the **exact questions asked in AWS / Kubernetes / DevOps interviews**.

   '''

---

---

## Build Your Docker Cheat Sheet
Create `docker-cheatsheet.md` organized by category:
- **Container commands** — run, ps, stop, rm, exec, logs
- **Image commands** — build, pull, push, tag, ls, rm
- **Volume commands** — create, ls, inspect, rm
- **Network commands** — create, ls, inspect, connect
- **Compose commands** — up, down, ps, logs, build
- **Cleanup commands** — prune, system df
- **Dockerfile instructions** — FROM, RUN, COPY, WORKDIR, EXPOSE, CMD, ENTRYPOINT

Keep it short — one line per command, something you'd actually reference on the job.


**CRETED docker-cheetsheet.md file **

---

