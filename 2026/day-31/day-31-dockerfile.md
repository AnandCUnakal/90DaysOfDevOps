---

## Challenge Tasks

### Task 1: Your First Dockerfile
1. Create a folder called `my-first-image`
2. Inside it, create a `Dockerfile` that:
   - Uses `ubuntu` as the base image
   - Installs `curl`
   - Sets a default command to print `"Hello from my custom image!"`
3. Build the image and tag it `my-ubuntu:v1`
4. Run a container from your image

'''
Excellent 👌 Now you’re moving from **running containers** → **building your own images**.

This is where real DevOps begins.

We’ll use **Ubuntu** as the base image.

---

# ✅ Step 1: Create Project Folder

```bash
mkdir my-first-image
cd my-first-image
```

---

# ✅ Step 2: Create Dockerfile

Inside `my-first-image`, create a file named:

```
Dockerfile
```

⚠️ Important:

* No extension
* Capital D
* No `.txt`

---

## 🔹 Add This Content to Dockerfile

```dockerfile
# Use Ubuntu as base image
FROM ubuntu:latest

# Avoid interactive prompts during install
ENV DEBIAN_FRONTEND=noninteractive

# Update package list and install curl
RUN apt update && apt install -y curl

# Default command
CMD ["echo", "Hello from my custom image!"]
```

---

## 🔎 What Each Line Does

| Line          | Meaning                               |
| ------------- | ------------------------------------- |
| `FROM ubuntu` | Base image                            |
| `ENV`         | Set environment variable              |
| `RUN`         | Executes command during build         |
| `CMD`         | Default command when container starts |

---

# ✅ Step 3: Build the Image

Run this inside `my-first-image` folder:

```bash
docker build -t my-ubuntu:v1 .
```

### 🔍 What This Means:

* `-t` → tag
* `my-ubuntu` → image name
* `v1` → version tag
* `.` → current directory (where Dockerfile exists)

---

### 📦 Verify Image

```bash
docker images
```

You should see:

```
my-ubuntu   v1   <image-id>   <size>
```

---

# ✅ Step 4: Run a Container from Your Image

```bash
docker run my-ubuntu:v1
```

You should see:

```
Hello from my custom image!
```

🎉 Congratulations — you just built and ran your first custom Docker image.

---

# 🧠 What Actually Happened During Build?

Docker:

1. Pulled Ubuntu image
2. Created temporary container
3. Ran `apt update`
4. Installed curl
5. Committed changes into new image layer
6. Set default CMD

Each instruction creates a **layer**.

---

# 🔥 Test That curl Is Installed

Override CMD and open shell:

```bash
docker run -it my-ubuntu:v1 bash
```

Inside container:

```bash
curl --version
```

Exit:

```bash
exit
```

---

# 🏗 Image Build Flow (Important Concept)

```
Dockerfile
    ↓
docker build
    ↓
Image (my-ubuntu:v1)
    ↓
docker run
    ↓
Container
```

---

# 🎯 DevOps Perspective

This is exactly what happens in:

* CI pipelines building images
* Kubernetes deployments
* Helm charts pulling images
* Production releases

In real projects:

* Replace `echo` with app startup command
* Replace Ubuntu with Node/Java/Python base image
* Push to Docker registry

'''
---

---

### Task 2: Dockerfile Instructions
Create a new Dockerfile that uses **all** of these instructions:
- `FROM` — base image
- `RUN` — execute commands during build
- `COPY` — copy files from host to image
- `WORKDIR` — set working directory
- `EXPOSE` — document the port
- `CMD` — default command

'''
Perfect 👌 Now we move from basic → structured Dockerfile.

We’ll create a small web app using:

* `nginx` as base
* Custom HTML page
* Proper Dockerfile structure

We’ll use **Nginx** as the base image.

---

# ✅ Step 1: Create Project Structure

```bash
mkdir dockerfile-demo
cd dockerfile-demo
```

Create this structure:

```
dockerfile-demo/
│
├── Dockerfile
└── index.html
```

---

# ✅ Step 2: Create index.html

Create `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Docker Demo</title>
</head>
<body>
  <h1>Hello from Advanced Dockerfile 🚀</h1>
</body>
</html>
```

---

# ✅ Step 3: Create Dockerfile (Using ALL Required Instructions)

```dockerfile
# 1️⃣ FROM — Base image
FROM nginx:latest

# 2️⃣ WORKDIR — Set working directory
WORKDIR /usr/share/nginx/html

# 3️⃣ RUN — Execute command during build
RUN echo "Building custom Nginx image..."

# 4️⃣ COPY — Copy file from host to container
COPY index.html .

# 5️⃣ EXPOSE — Document container port
EXPOSE 80

# 6️⃣ CMD — Default command
CMD ["nginx", "-g", "daemon off;"]
```

---

# 🔍 What Each Instruction Does

### 🔹 FROM

Sets the base image.

```
FROM nginx:latest
```

Everything builds on top of this.

---

### 🔹 WORKDIR

Sets working directory inside container.

```
WORKDIR /usr/share/nginx/html
```

All future commands run from here.

---

### 🔹 RUN

Executes during build phase.

```
RUN echo "Building custom Nginx image..."
```

Creates a new image layer.

---

### 🔹 COPY

Copies files from host → image.

```
COPY index.html .
```

Copies your custom webpage.

---

### 🔹 EXPOSE

Documents the port container uses.

```
EXPOSE 80
```

⚠️ It does NOT publish the port.
It only documents it.

---

### 🔹 CMD

Default command when container starts.

```
CMD ["nginx", "-g", "daemon off;"]
```

Keeps nginx running in foreground.

---

# ✅ Step 4: Build the Image

```bash
docker build -t nginx-custom:v1 .
```

Verify:

```bash
docker images
```

---

# ✅ Step 5: Run the Container

```bash
docker run -d -p 8089:80 --name my-custom-nginx nginx-custom:v1
```

Open browser:

```
http://localhost:8089
```

You should see:

> Hello from Advanced Dockerfile 🚀

---

# 🧠 Important Build vs Run Understanding

| Instruction | Runs When?             |
| ----------- | ---------------------- |
| RUN         | During build           |
| CMD         | During container start |
| COPY        | During build           |
| WORKDIR     | During build           |
| EXPOSE      | Metadata only          |

---

# 🏗 Docker Build Layer Flow

```
FROM nginx
   ↓
WORKDIR
   ↓
RUN
   ↓
COPY
   ↓
EXPOSE
   ↓
Image Ready
   ↓
CMD executes when container runs
```

---

# 🚀 DevOps Insight

In real CI/CD:

1. Pipeline runs `docker build`
2. Image pushed to registry
3. Kubernetes pulls image
4. Pod runs CMD
5. EXPOSE is used for documentation / service configs

This is exactly how microservices are deployed.

---

# 🔥 Bonus Challenge (Recommended)

Modify Dockerfile to:

* Use `ubuntu`
* Install `python3`
* Run a simple Python HTTP server
* EXPOSE port 8000

That will make you 10x stronger in Docker fundamentals.

---
'''
---

### Task 3: CMD vs ENTRYPOINT
1. Create an image with `CMD ["echo", "hello"]` — run it, then run it with a custom command. What happens?
2. Create an image with `ENTRYPOINT ["echo"]` — run it, then run it with additional arguments. What happens?
3. Write in your notes: When would you use CMD vs ENTRYPOINT?

'''
Excellent 👌 This is a **very common interview question** and a very important real-world Docker concept.

Let’s experiment properly.

---

# 🔹 PART 1 — Using `CMD`

---

## ✅ Step 1: Create Folder

```bash
mkdir cmd-demo
cd cmd-demo
```

---

## ✅ Step 2: Create Dockerfile (CMD version)

```dockerfile
FROM ubuntu:latest
CMD ["echo", "hello"]
```

We are using **Ubuntu** as base.

---

## ✅ Step 3: Build Image

```bash
docker build -t cmd-image:v1 .
```

---

## ✅ Step 4: Run Image Normally

```bash
docker run cmd-image:v1
```

### 🔎 Output:

```
hello
```

Because CMD runs by default.

---

## ✅ Step 5: Run With Custom Command

```bash
docker run cmd-image:v1 ls
```

### 🔎 Output:

You’ll see directory listing instead of “hello”.

### 🧠 Why?

Because:

> CMD gets completely overridden by the command you pass at runtime.

So Docker replaces:

```
["echo", "hello"]
```

with:

```
ls
```

---

# 🔹 PART 2 — Using `ENTRYPOINT`

---

## ✅ Step 1: Create New Folder

```bash
mkdir entrypoint-demo
cd entrypoint-demo
```

---

## ✅ Step 2: Create Dockerfile (ENTRYPOINT version)

```dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo"]
```

---

## ✅ Step 3: Build Image

```bash
docker build -t entry-image:v1 .
```

---

## ✅ Step 4: Run Without Arguments

```bash
docker run entry-image:v1
```

### 🔎 Output:

Nothing (just blank line)

Because `echo` runs with no arguments.

---

## ✅ Step 5: Run With Additional Arguments

```bash
docker run entry-image:v1 hello world
```

### 🔎 Output:

```
hello world
```

---

### 🧠 Why?

Because:

> ENTRYPOINT does NOT get replaced.
> Additional arguments get appended to it.

So Docker runs:

```
echo hello world
```

---

# 🔥 Critical Difference

| Feature                    | CMD             | ENTRYPOINT                      |
| -------------------------- | --------------- | ------------------------------- |
| Can be overridden?         | ✅ Yes           | ❌ No (unless --entrypoint used) |
| Accepts runtime arguments? | Replaced        | Appended                        |
| Best used for              | Default command | Fixed executable                |

---

# 🧠 What Happens Internally

### CMD behavior:

```
docker run image <new-command>
```

➡ replaces CMD completely

### ENTRYPOINT behavior:

```
docker run image arg1 arg2
```

➡ ENTRYPOINT + args

---

# 🔹 Combining ENTRYPOINT + CMD (Advanced & Real Use)

Most production images use BOTH.

Example:

```dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
CMD ["hello"]
```

If you run:

```
docker run image
```

Output:

```
hello
```

If you run:

```
docker run image DevOps
```

Output:

```
DevOps
```

Because:

ENTRYPOINT stays fixed
CMD acts as default argument

This is very common in production images.

---

# 📝 Your Notes — When to Use CMD vs ENTRYPOINT?

### ✅ Use CMD When:

* You want a default command
* You want users to easily override it
* You are running temporary containers
* Development/testing scenarios

Example:

```
CMD ["python3", "app.py"]
```

---

### ✅ Use ENTRYPOINT When:

* You want container to always run a specific executable
* You are building CLI-style container tools
* You want to enforce behavior
* Production images

Example:

```
ENTRYPOINT ["nginx"]
```

Used in **Nginx** official image.

---

# 🚀 Real DevOps Insight

In Kubernetes:

* ENTRYPOINT = container process
* CMD = default arguments

If your ENTRYPOINT crashes → container restarts
If CMD overridden → behavior changes

---

# 🎯 Interview-Ready One-Line Answer

> CMD sets default command and can be overridden.
> ENTRYPOINT defines the main executable and cannot be overridden (only appended to).

---

# 🔥 Final Visual Summary

```
CMD → can be replaced
ENTRYPOINT → stays fixed
ENTRYPOINT + CMD → best practice for production
```
---
'''

### Task 4: Build a Simple Web App Image
1. Create a small static HTML file (`index.html`) with any content
2. Write a Dockerfile that:
   - Uses `nginx:alpine` as base
   - Copies your `index.html` to the Nginx web directory
3. Build and tag it `my-website:v1`
4. Run it with port mapping and access it in your browser

   '''
   Perfect 👌 Let’s build a **real minimal production-style web image** using **Nginx** with the lightweight `nginx:alpine` base.

---

# ✅ Step 1: Create Project Folder

```bash
mkdir my-website
cd my-website
```

Project structure should look like:

```
my-website/
│
├── Dockerfile
└── index.html
```

---

# ✅ Step 2: Create `index.html`

Create a simple static page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Docker Website</title>
</head>
<body>
  <h1>🚀 My Website Running in Docker</h1>
  <p>If you can see this, my custom image works!</p>
</body>
</html>
```

---

# ✅ Step 3: Create Dockerfile

Now create a file named `Dockerfile` (no extension).

```dockerfile
# Use lightweight nginx image
FROM nginx:alpine

# Copy our custom HTML file into nginx default web directory
COPY index.html /usr/share/nginx/html/index.html

# Document port 80
EXPOSE 80
```

### 🔎 Why `nginx:alpine`?

* Much smaller image size
* Faster pulls
* Ideal for production static sites

---

# ✅ Step 4: Build the Image

Make sure you're inside the project folder, then run:

```bash
docker build -t my-website:v1 .
```

Verify:

```bash
docker images
```

You should see:

```
my-website   v1   <image-id>   <size>
```

---

# ✅ Step 5: Run the Container

```bash
docker run -d -p 8090:80 --name my-site my-website:v1
```

* `-d` → detached mode
* `-p 8090:80` → host port 8090 → container port 80
* `--name` → easier management

---

# ✅ Step 6: Access in Browser

Open:

```
http://localhost:8090
```

You should see your custom webpage 🎉

---

# 🧠 What Just Happened?

1. Docker pulled `nginx:alpine`
2. Copied your HTML into image
3. Built a new image layer
4. Started container
5. Nginx served your file on port 80
6. Port mapping exposed it to your machine

---

# 🔥 Verify Inside Container (Optional)

```bash
docker exec -it my-site sh
```

Check file:

```bash
ls /usr/share/nginx/html
cat /usr/share/nginx/html/index.html
```

Exit:

```bash
exit
```

---

# 🧹 Cleanup (Optional)

```bash
docker stop my-site
docker rm my-site
```

---

# 🚀 DevOps Insight

This is exactly how:

* Static websites are containerized
* Frontend builds are deployed
* Kubernetes serves UI apps
* CI pipelines build web images

Replace `index.html` with:

* React build output
* Angular dist folder
* Static docs site

And you have production-ready container images.

---


   '''
   

   ### Task 5: .dockerignore
1. Create a `.dockerignore` file in one of your project folders
2. Add entries for: `node_modules`, `.git`, `*.md`, `.env`
3. Build the image — verify that ignored files are not included

---

### Task 6: Build Optimization
1. Build an image, then change one line and rebuild — notice how Docker uses **cache**
    
anand@DESKTOP-H50FBNH MINGW64 /d/ANDYtws/projects/docker_proj/entrypoint-demo/my-website
$ docker build -t my-website:v3 .
[+] Building 5.2s (8/8) FINISHED                                                  docker:desktop-linux
 => [internal] load build definition from Dockerfile                                              0.0s
 => => transferring dockerfile: 263B                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                   4.3s
 => [auth] library/nginx:pull token for registry-1.docker.io                                      0.0s
 => [internal] load .dockerignore                                                                 0.0s
 => => transferring context: 68B                                                                  0.0s
 => [internal] load build context                                                                 0.0s
 => => transferring context: 32B                                                                  0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:1d13701a5f9f3fb01aaa88cef2344d65b6b5bf6b7d9  0.1s
 => => resolve docker.io/library/nginx:alpine@sha256:1d13701a5f9f3fb01aaa88cef2344d65b6b5bf6b7d9  0.1s
 => CACHED [2/2] COPY index.html /usr/share/nginx/html/index.html                                 0.0s
 => exporting to image                                                                            0.4s
 => => exporting layers                                                                           0.0s
 => => exporting manifest sha256:1d3f14ef944d1b109bb7e696d173342c007df7d8019aace1c8dd5c7a50de73d  0.0s
 => => exporting config sha256:36c12ab55cffb9cf2b2b7fb76e136539e7a86cc54e39fccfefa2b0c70c612a7b   0.1s
 => => exporting attestation manifest sha256:02bbdf11edf11eff05e6ce232d195461668d1a2e2dc2756c127  0.1s
 => => exporting manifest list sha256:670f847d362bec3ea0a0cf5a2f15cbdfca46d31539d5ace2f77e35852a  0.0s
 => => naming to docker.io/library/my-website:v3                                                  0.0s
 => => unpacking to docker.io/library/my-website:v3                                               0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/6x694oi89bkiedxozszjqu
jyu

     
2. Reorder your Dockerfile so that frequently changing lines come **last**
    
anand@DESKTOP-H50FBNH MINGW64 /d/ANDYtws/projects/docker_proj/entrypoint-demo/my-website
$ docker build -t my-website:v3 .
[+] Building 5.2s (8/8) FINISHED                                                  docker:desktop-linux
 => [internal] load build definition from Dockerfile                                              0.0s
 => => transferring dockerfile: 263B                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                   4.3s
 => [auth] library/nginx:pull token for registry-1.docker.io                                      0.0s
 => [internal] load .dockerignore                                                                 0.0s
 => => transferring context: 68B                                                                  0.0s
 => [internal] load build context                                                                 0.0s
 => => transferring context: 32B                                                                  0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:1d13701a5f9f3fb01aaa88cef2344d65b6b5bf6b7d9  0.1s
 => => resolve docker.io/library/nginx:alpine@sha256:1d13701a5f9f3fb01aaa88cef2344d65b6b5bf6b7d9  0.1s
 => CACHED [2/2] COPY index.html /usr/share/nginx/html/index.html                                 0.0s
 => exporting to image                                                                            0.4s
 => => exporting layers                                                                           0.0s
 => => exporting manifest sha256:1d3f14ef944d1b109bb7e696d173342c007df7d8019aace1c8dd5c7a50de73d  0.0s
 => => exporting config sha256:36c12ab55cffb9cf2b2b7fb76e136539e7a86cc54e39fccfefa2b0c70c612a7b   0.1s
 => => exporting attestation manifest sha256:02bbdf11edf11eff05e6ce232d195461668d1a2e2dc2756c127  0.1s
 => => exporting manifest list sha256:670f847d362bec3ea0a0cf5a2f15cbdfca46d31539d5ace2f77e35852a  0.0s
 => => naming to docker.io/library/my-website:v3                                                  0.0s
 => => unpacking to docker.io/library/my-website:v3                                               0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/6x694oi89bkiedxozszjqu
jyu

3. Write in your notes: Why does layer order matter for build speed?

   '''
   Here’s a concise note on **why layer order matters for Docker build speed**:

---

## 📝 Why Layer Order Matters
- **Docker builds images in layers**: Each instruction in the Dockerfile (`FROM`, `COPY`, `RUN`, etc.) creates a new layer.  
- **Caching**: Docker caches layers. If a layer hasn’t changed, Docker reuses it instead of rebuilding.  
- **Impact of order**:
  - If you put frequently changing instructions (like `COPY . .`) early, it invalidates all subsequent layers, forcing Docker to rebuild everything.
  - If you put stable instructions (like installing system packages) first, they stay cached, and only the later layers rebuild when code changes.
- **Best practice**:  
  - Place **least frequently changing layers first** (e.g., base image, dependencies).  
  - Place **most frequently changing layers last** (e.g., application source code).  

---

### ⚡ Example
```dockerfile
# Good practice
FROM python:3.11
RUN pip install flask   # rarely changes
COPY app.py /app        # changes often
CMD ["python", "/app/app.py"]
```

- If you change `app.py`, only the last layer rebuilds.  
- If you had copied `app.py` first, then installed Flask, every change to `app.py` would force Flask to reinstall — slowing builds.  

---

👉 **Summary**: Layer order matters because Docker rebuilds from the first changed layer onward. Organizing your Dockerfile so stable layers come first and volatile ones last makes builds faster and more efficient.  

   '''
    
---
