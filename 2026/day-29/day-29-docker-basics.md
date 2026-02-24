---

## Challenge Tasks

### Task 1: What is Docker?
Research and write short notes on:
- What is a container and why do we need them?
- Containers vs Virtual Machines — what's the real difference?
- What is the Docker architecture? (daemon, client, images, containers, registry)

Draw or describe the Docker architecture in your own words.

---
## 1️⃣ What is Docker?

**Docker** is a containerization platform that allows you to package an application with all its dependencies (code, runtime, libraries, system tools) into a lightweight, portable unit called a **container**.

It solves the classic problem:

> “It works on my machine, but not in production.”

Docker ensures the app runs the same everywhere — local, test, staging, production.

---

## 2️⃣ What is a Container & Why Do We Need Them?

### 🔹 What is a Container?

A **container** is a lightweight, isolated runtime environment that shares the host OS kernel but runs independently.

It includes:

* Application code
* Runtime (Java, Python, Node, etc.)
* Libraries
* Dependencies
* Config

Think of it like:

> 📦 A mini package that contains everything your app needs to run.

---

### 🔹 Why Do We Need Containers?

Without containers:

* Different environments have different library versions
* Dependency conflicts occur
* Manual setup is error-prone
* Scaling is harder

With containers:

* ✅ Consistent environments
* ✅ Fast deployment
* ✅ Lightweight
* ✅ Easy scaling
* ✅ Perfect for CI/CD (important for your DevOps path 🚀)

---

## 3️⃣ Containers vs Virtual Machines (Real Difference)

| Feature        | Containers           | Virtual Machines            |
| -------------- | -------------------- | --------------------------- |
| OS             | Share host OS kernel | Each VM has its own full OS |
| Boot time      | Seconds              | Minutes                     |
| Size           | MBs                  | GBs                         |
| Performance    | Near native          | Slight overhead             |
| Isolation      | Process-level        | Full OS-level               |
| Resource usage | Lightweight          | Heavy                       |

---

### 🔹 VM Architecture Example

```
Hardware
   ↓
Hypervisor
   ↓
Guest OS
   ↓
App + Libraries
```

---

### 🔹 Container Architecture Example

```
Hardware
   ↓
Host OS
   ↓
Container Engine (Docker)
   ↓
App + Libraries (Container)
```

---

### 🚀 Real Difference (Simple Explanation)

* VM = Full house (with its own plumbing, electricity, kitchen)
* Container = Apartment in same building (shared plumbing, isolated space)

---

## 4️⃣ Docker Architecture

Docker follows a **Client-Server architecture**.

Main Components:

### 1️⃣ Docker Client

* CLI tool (`docker` command)
* Sends commands to Docker daemon
* Example:

  ```
  docker build
  docker run
  docker pull
  ```

---

### 2️⃣ Docker Daemon (dockerd)

* Runs in background
* Manages:

  * Images
  * Containers
  * Networks
  * Volumes
* Listens for Docker API requests

---

### 3️⃣ Docker Images

* Read-only template
* Blueprint for containers
* Built using `Dockerfile`

Example:

```
FROM nginx
```

---

### 4️⃣ Docker Containers

* Running instance of an image
* Executable unit

Image = Blueprint
Container = Running house built from blueprint

---

### 5️⃣ Docker Registry

* Stores Docker images
* Public or private

Example:

* **Docker Hub**
* AWS ECR
* Azure Container Registry

You push and pull images from registry.

---

## 5️⃣ Docker Architecture — Simple Text Diagram

```
                ┌────────────────────┐
                │   Docker Client    │
                │  (docker CLI)      │
                └─────────┬──────────┘
                          │ REST API
                          ▼
                ┌────────────────────┐
                │  Docker Daemon     │
                │   (dockerd)        │
                ├────────────────────┤
                │  Images            │
                │  Containers        │
                │  Networks          │
                │  Volumes           │
                └─────────┬──────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
   Local Images                    Remote Registry
                                     (Docker Hub)
```

---

## 6️⃣ How It Works Step-by-Step

1. Developer writes Dockerfile
2. Runs `docker build` → creates image
3. Runs `docker run` → creates container
4. Container runs app
5. Image can be pushed to registry
6. CI/CD pipeline pulls and deploys

---

## 🧠 Why Docker is Important for You (DevOps Perspective)

Since you're building CI/CD pipelines and working with Kubernetes:

* Docker is foundation for Kubernetes
* Helm deployments use container images
* CI pipelines build Docker images
* Production deployments use Docker images

Without Docker → No modern DevOps.

---
---
---

### Task 2: Install Docker
1. Install Docker on your machine (or use a cloud instance)
2. Verify the installation
3. Run the `hello-world` container
4. Read the output carefully — it explains what just happened

---
Perfect 👍 Let’s do this step-by-step like a real DevOps setup.

---

# ✅ Task 2: Install Docker

Since you're on **Windows (Chrome user agent detected)**, I’ll give Windows steps first.
(If you're using Linux cloud VM, I’ve included that too.)

---

# 🖥️ Option 1: Install Docker on Windows

### 🔹 Step 1: Install Docker Desktop

1. Go to **Docker Desktop**
2. Download Windows version
3. Install with default options
4. Enable **WSL2** if prompted
5. Restart your machine

---

### 🔹 Step 2: Verify Installation

Open PowerShell or CMD:

```bash
docker --version
```

Expected output:

```
Docker version 24.x.x, build xxxxx
```

Check if Docker daemon is running:

```bash
docker info
```

If it shows system details → Docker is running ✅

---

# 🐧 Option 2: Install Docker on Linux (Ubuntu Cloud VM)

If you're using EC2 / Azure VM:

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Verify:

```bash
docker --version
sudo systemctl status docker
```

Optional (avoid sudo every time):

```bash
sudo usermod -aG docker $USER
```

Logout & login again.

---

# 🚀 Step 3: Run `hello-world` Container

Run:

```bash
docker run hello-world
```

---

# 🔎 What Just Happened? (Very Important)

When you ran:

```bash
docker run hello-world
```

Docker performed these steps automatically:

### 1️⃣ Client contacted Docker daemon

The `docker` CLI sent a request to `dockerd`.

### 2️⃣ Image not found locally

Docker checked local images.

Since `hello-world` wasn't available locally, it:

### 3️⃣ Pulled image from registry

It downloaded image from:

**Docker Hub**

You’ll see:

```
Unable to find image 'hello-world:latest' locally
Pulling from library/hello-world
```

### 4️⃣ Created a container

Docker created a container from that image.

### 5️⃣ Executed the container

The container ran a small program that prints a message.

### 6️⃣ Container exited

After printing the message, it stopped.

---

# 📦 Check What Happened

List images:

```bash
docker images
```

List running containers:

```bash
docker ps
```

List all containers (including stopped):

```bash
docker ps -a
```

You’ll see `hello-world` container exited.

---

# 🧠 What This Proves

✔ Docker daemon is running
✔ You can pull images from registry
✔ You can create containers
✔ Your networking works
✔ Docker architecture is functioning correctly

---

# 🔥 Extra (DevOps Mindset)

This same process is what happens in:

* CI pipelines building images
* Kubernetes pulling images
* Production deployments
* Helm chart deployments

The only difference?
Instead of `hello-world`, you run:

* nginx
* node app
* Java app
* your microservices

---

# 🎯 Your Notes Should Include

* Docker version output
* What happened during `hello-world`
* Difference between image and container
* Output of:

  * `docker images`
  * `docker ps -a`

---
---

### Task 3: Run Real Containers
1. Run an **Nginx** container and access it in your browser
2. Run an **Ubuntu** container in interactive mode — explore it like a mini Linux machine
3. List all running containers
4. List all containers (including stopped ones)
5. Stop and remove a container


'''
PS D:\ANDYtws\projects\docker_proj> docker ps 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
4bf355a3f422   nginx                 "/docker-entrypoint.…"   14 seconds ago   Created
                                                    mynginx
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago     Exited (255) 3 months ago  
 0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago     Exited (0) 3 months ago    
                                                    quirky_cerf
PS D:\ANDYtws\projects\docker_proj> docker log 4bf355a3f422
docker: unknown command: docker log
Run 'docker --help' for more information
PS D:\ANDYtws\projects\docker_proj> docker logs 4bf355a3f422
PS D:\ANDYtws\projects\docker_proj> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS
PORTS                                              NAMES
4bf355a3f422   nginx                 "/docker-entrypoint.…"   2 minutes ago   Created
                                                   mynginx
0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
                                                   quirky_cerf
PS D:\ANDYtws\projects\docker_proj> docker rm 4bf355a3f422
4bf355a3f422
PS D:\ANDYtws\projects\docker_proj> docker ps -a
ORTS                                              NAMES
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago   Exited (255) 3 months ago   0
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago   Exited (0) 3 months ago      
PS D:\ANDYtws\projects\docker_proj> docker run -it -d -p 8081:80 nginx
PS D:\ANDYtws\projects\docker_proj> docker ps
                   NAMES
::]:8081->80/tcp   quirky_curie
PS D:\ANDYtws\projects\docker_proj> docker rm 24a33ac7bd51
Error response from daemon: cannot remove container "24a33ac7bd51": container is running: stop the contain
er before removing or force remove
PS D:\ANDYtws\projects\docker_proj> docker rm -f 24a33ac7bd51
24a33ac7bd51
PS D:\ANDYtws\projects\docker_proj> docker run -it -d -p 8081:80 hello-world
8a3038ed16bdf8e6eecffef0deb0d3411c567c3d8eac5d96881544b1aec66e12
PS D:\ANDYtws\projects\docker_proj> docker run -it -d -p 8081:80 hello-world
765fd80f507514cca5351a5c18e30c54a0aa062e21cf58d1bfa9097fd5f9054e
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS
765fd80f5075   hello-world           "/hello"                 10 seconds ago   Exited (0) 8 seconds ago   
8a3038ed16bd   hello-world           "/hello"                 34 seconds ago   Exited (0) 33 seconds ago  
                                                    youthful_gates
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago     Exited (255) 3 months ago  
 0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago     Exited (0) 3 months ago    
                                                    quirky_cerf
PS D:\ANDYtws\projects\docker_proj> docker rm -f 765fd80f5075
765fd80f5075
PS D:\ANDYtws\projects\docker_proj> docker rm -f 8a3038ed16bd
8a3038ed16bd
PS D:\ANDYtws\projects\docker_proj> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS D:\ANDYtws\projects\docker_proj> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS                      P
ORTS                                              NAMES
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago   Exited (255) 3 months ago   0
.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago   Exited (0) 3 months ago      
                                                  quirky_cerf
PS D:\ANDYtws\projects\docker_proj>

'''

### Task 4: Explore
1. Run a container in **detached mode** — what's different?
2. Give a container a custom **name**
3. Map a **port** from the container to your host
4. Check **logs** of a running container
5. Run a command **inside** a running container  

   '''
   PS D:\ANDYtws\projects\docker_proj> docker run -d -p 9090:80 --name web9090 nginx
526f0aa5d425bb2ca1753bbf8f4a938daaeae797bfb87bb4df00bf8980015ed7
PS D:\ANDYtws\projects\docker_proj> docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS
                 NAMES
526f0aa5d425   nginx     "/docker-entrypoint.…"   9 seconds ago   Up 9 seconds   0.0.0.0:9090->80/tcp, [::
]:9090->80/tcp   web9090
PS D:\ANDYtws\projects\docker_proj> docker logs web9090
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/02/24 17:00:41 [notice] 1#1: using the "epoll" event method
2026/02/24 17:00:41 [notice] 1#1: nginx/1.29.5
2026/02/24 17:00:41 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/02/24 17:00:41 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2026/02/24 17:00:41 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/02/24 17:00:41 [notice] 1#1: start worker processes
2026/02/24 17:00:41 [notice] 1#1: start worker process 29
2026/02/24 17:00:41 [notice] 1#1: start worker process 30
2026/02/24 17:00:41 [notice] 1#1: start worker process 31
2026/02/24 17:00:41 [notice] 1#1: start worker process 32
2026/02/24 17:00:41 [notice] 1#1: start worker process 29
2026/02/24 17:00:41 [notice] 1#1: start worker process 30
2026/02/24 17:00:41 [notice] 1#1: start worker process 31
2026/02/24 17:00:41 [notice] 1#1: start worker process 32
172.17.0.1 - - [24/Feb/2026:17:00:58 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; W
in64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36" "-"
2026/02/24 17:00:58 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file 
or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localh
ost:9090", referrer: "http://localhost:9090/"
172.17.0.1 - - [24/Feb/2026:17:00:58 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:9090/" 
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari
/537.36" "-"
PS D:\ANDYtws\projects\docker_proj> docker logs web9090
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/02/24 17:00:41 [notice] 1#1: using the "epoll" event method
2026/02/24 17:00:41 [notice] 1#1: nginx/1.29.5
2026/02/24 17:00:41 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/02/24 17:00:41 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2026/02/24 17:00:41 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/02/24 17:00:41 [notice] 1#1: start worker process 29
2026/02/24 17:00:41 [notice] 1#1: start worker process 30
2026/02/24 17:00:41 [notice] 1#1: start worker process 31
2026/02/24 17:00:41 [notice] 1#1: start worker process 32
172.17.0.1 - - [24/Feb/2026:17:00:58 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Wi
n64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36" "-"
2026/02/24 17:00:58 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file o
r directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhos
t:9090", referrer: "http://localhost:9090/"
172.17.0.1 - - [24/Feb/2026:17:00:58 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:9090/" "
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/5
37.36" "-"
PS D:\ANDYtws\projects\docker_proj> docker logs web9090
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/02/24 17:00:41 [notice] 1#1: using the "epoll" event method
2026/02/24 17:00:41 [notice] 1#1: nginx/1.29.5
2026/02/24 17:00:41 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/02/24 17:00:41 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2026/02/24 17:00:41 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/02/24 17:00:41 [notice] 1#1: start worker process 29
2026/02/24 17:00:41 [notice] 1#1: start worker process 30
2026/02/24 17:00:41 [notice] 1#1: start worker process 31
2026/02/24 17:00:41 [notice] 1#1: start worker process 32
172.17.0.1 - - [24/Feb/2026:17:00:58 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Wi
n64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36" "-"
2026/02/24 17:00:58 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file o
r directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhos
t:9090", referrer: "http://localhost:9090/"
172.17.0.1 - - [24/Feb/2026:17:00:58 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:9090/" "
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/5
37.36" "-"
PS D:\ANDYtws\projects\docker_proj> curl http://localhost:9090


StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html>
                    <html>
                    <head>
                    <title>Welcome to nginx!</title>
                    <style>
                    html { color-scheme: light dark; }
                    body { width: 35em; margin: 0 auto;
                    font-family: Tahoma, Verdana, Arial, sans-serif; }
                    </style...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 615
                    Content-Type: text/html
                    Date: Tue, 24 Feb 2026 17:04:00 GMT
                    ETag: "698361d4-267"
                    Last-Modified: Wed, 04 Feb 2026 ...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 615],
Images            : {}
InputFields       : {}
Links             : {@{innerHTML=nginx.org; innerText=nginx.org; outerHTML=<A
                    href="http://nginx.org/">nginx.org</A>; outerText=nginx.org; tagName=A;
                    href="http://nginx.com/">nginx.com</A>; outerText=nginx.com; tagName=A;
                    href=http://nginx.com/}}
ParsedHtml        : System.__ComObject
RawContentLength  : 615


PS D:\ANDYtws\projects\docker_proj> curl -i http://localhost:9090

cmdlet Invoke-WebRequest at command pipeline position 1
Supply values for the following parameters:
Uri:
PS D:\ANDYtws\projects\docker_proj> curl -I http://localhost:9090
cmdlet Invoke-WebRequest at command pipeline position 1
Supply values for the following parameters:
Uri:
PS D:\ANDYtws\projects\docker_proj> curl -l http://localhost:9090
Invoke-WebRequest : A parameter cannot be found that matches parameter name 'l'.
At line:1 char:6
+ curl -l http://localhost:9090
+      ~~
    + FullyQualifiedErrorId : NamedParameterNotFound,Microsoft.PowerShell.Commands.InvokeWebRequestComma  
 
PS D:\ANDYtws\projects\docker_proj> curl -L http://localhost:9090
Invoke-WebRequest : A parameter cannot be found that matches parameter name 'L'.
At line:1 char:6
    + FullyQualifiedErrorId : NamedParameterNotFound,Microsoft.PowerShell.Commands.InvokeWebRequestComma  
   nd
 
PS D:\ANDYtws\projects\docker_proj> ping http://localhost:9090
Ping request could not find host http://localhost:9090. Please check the name and try again.
PS D:\ANDYtws\projects\docker_proj> docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS
                 NAMES
526f0aa5d425   nginx     "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:9090->80/tcp, [::
]:9090->80/tcp   web9090
PS D:\ANDYtws\projects\docker_proj> docker exec -it -d 526f0aa5d425 bash
PS D:\ANDYtws\projects\docker_proj> docker exec -it -d 526f0aa5d425 /bin/bash
PS D:\ANDYtws\projects\docker_proj> docker exec -it 526f0aa5d425 bash        
root@526f0aa5d425:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr
root@526f0aa5d425:/# pwd
/
root@526f0aa5d425:/# ls -lrt
total 64
lrwxrwxrwx   1 root root    8 Jan  2 12:35 sbin -> usr/sbin
lrwxrwxrwx   1 root root    9 Jan  2 12:35 lib64 -> usr/lib64
lrwxrwxrwx   1 root root    7 Jan  2 12:35 lib -> usr/lib
drwxr-xr-x   2 root root 4096 Jan  2 12:35 home
drwxr-xr-x   2 root root 4096 Jan  2 12:35 boot
lrwxrwxrwx   1 root root    7 Jan  2 12:35 bin -> usr/bin
drwxr-xr-x   1 root root 4096 Feb  2 00:00 var
drwxr-xr-x   1 root root 4096 Feb  2 00:00 usr
drwxrwxrwt   2 root root 4096 Feb  2 00:00 tmp
drwxr-xr-x   2 root root 4096 Feb  2 00:00 srv
drwx------   2 root root 4096 Feb  2 00:00 root
drwxr-xr-x   2 root root 4096 Feb  2 00:00 opt
drwxr-xr-x   2 root root 4096 Feb  2 00:00 mnt
drwxr-xr-x   2 root root 4096 Feb  2 00:00 media
-rwxr-xr-x   1 root root 1620 Feb  4 23:52 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 Feb  4 23:53 docker-entrypoint.d
drwxr-xr-x   1 root root 4096 Feb 24 17:00 etc
dr-xr-xr-x  13 root root    0 Feb 24 17:00 sys
dr-xr-xr-x 242 root root    0 Feb 24 17:00 proc
drwxr-xr-x   5 root root  340 Feb 24 17:00 dev
drwxr-xr-x   1 root root 4096 Feb 24 17:00 run
root@526f0aa5d425:/#
   '''
   

   
