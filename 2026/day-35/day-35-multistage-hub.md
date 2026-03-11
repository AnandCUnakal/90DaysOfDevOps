---

## Challenge Tasks

### Task 1: The Problem with Large Images
1. Write a simple Go, Java, or Node.js app (even a "Hello World" is fine)
2. Create a Dockerfile that builds and runs it in a **single stage**
3. Build the image and check its **size**

Note down the size — you'll compare it later.

'''
Alright, let’s tackle **Challenge Task 1: Large Images** step by step. The idea is to see how a **single-stage Dockerfile** produces a relatively large image, which we’ll later optimize.

---

## 🔹 Step 1: Simple App (Node.js Example)

Create a file `app.js`:

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello World from Node.js!");
});

app.listen(3000, () => {
  console.log("App running on port 3000");
});
```

Create a `package.json`:

```json
{
  "name": "hello-world-app",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## 🔹 Step 2: Single-Stage Dockerfile

```dockerfile
# Single-stage build
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["node", "app.js"]
```

---

## 🔹 Step 3: Build & Check Size

Build the image:

```bash
docker build -t hello-node-single .
```

Check the size:

```bash
docker images hello-node-single
```

👉 You’ll likely see something around **900MB+** because `node:18` is a full image with build tools, libraries, and runtime bundled together.

---

## 🔹 Why It’s Large
- **Single-stage build** keeps all dependencies (compilers, libraries, caches).  
- No optimization (like Alpine base image or multi-stage builds).  
- This is the baseline for comparison when we later optimize with **multi-stage builds** or **slimmer base images**.

---

✅ At this stage, you’ve got a working container, but it’s bulky. The next challenge will be to **shrink it down** using multi-stage builds or lighter base images.

---

Would you like me to **show the optimized multi-stage Dockerfile** now (Task 2 continuation), so you can directly compare the image sizes side by side?
'''

'''
Excellent — let’s create **Hello World apps and single‑stage Dockerfiles** for **Go, Java, and Python** (in addition to the Node.js one we already did). This will let you build each image and compare sizes later.

---

## 🔹 Go Example

**File: `main.go`**
```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello World from Go!")
}

func main() {
    http.HandleFunc("/", handler)
    fmt.Println("Server running on port 8080")
    http.ListenAndServe(":8080", nil)
}
```

**Dockerfile (single stage):**
```dockerfile
FROM golang:1.21

WORKDIR /app
COPY . .

RUN go build -o server .

EXPOSE 8080
CMD ["./server"]
```

👉 Size: ~800MB (because `golang` base image includes full toolchain).

---

## 🔹 Java Example

**File: `HelloWorld.java`**
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World from Java!");
    }
}
```

**Dockerfile (single stage):**
```dockerfile
FROM openjdk:17

WORKDIR /app
COPY HelloWorld.java .

RUN javac HelloWorld.java

EXPOSE 8080
CMD ["java", "HelloWorld"]
```

👉 Size: ~600MB+ (OpenJDK base image is heavy).

---

## 🔹 Python Example

**File: `app.py`**
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World from Python!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

**File: `requirements.txt`**
```
flask
```

**Dockerfile (single stage):**
```dockerfile
FROM python:3.11

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000
CMD ["python", "app.py"]
```

👉 Size: ~900MB (Python base image + Flask).

---

## 🔹 What You’ll Notice
- **All single-stage builds are large** because they include compilers, libraries, and caches.  
- Go and Java images are bulky due to full toolchains.  
- Python and Node.js images are heavy because they include package managers and runtime.  

---


✅ Next step (Task 2) will be to **optimize with multi-stage builds or slim base images** (e.g., `golang:alpine`, `openjdk-slim`, `python:slim`). That’s where you’ll see dramatic size reductions.

---

Would you like me to **rewrite these Dockerfiles into optimized multi-stage versions** right away so you can compare the sizes side by side?


PS D:\ANDYtws\twstask\day35> docker images hello-node-single
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
hello-node-single   latest    a84c7d3e3443   7 minutes ago   1.58GB


'''
---

### Task 2: Multi-Stage Build
1. Rewrite the Dockerfile using **multi-stage build**:
   - Stage 1: Build the app (install dependencies, compile)
   - Stage 2: Copy only the built artifact into a minimal base image (`alpine`, `distroless`, or `scratch`)
2. Build the image and check its size again
3. Compare the two sizes

Write in your notes: Why is the multi-stage image so much smaller?

'''
Perfect — let’s rewrite those **Dockerfiles into multi‑stage builds** for Go, Java, Python, and Node.js. This way you’ll see how much smaller the images get compared to the single‑stage versions.

---

## 🔹 Go (Multi‑Stage)

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o server .

# Stage 2: Run (minimal image)
FROM alpine:3.19
WORKDIR /app
COPY --from=builder /app/server .
EXPOSE 8080
CMD ["./server"]
```

👉 Size drops from ~800MB → ~15MB (only compiled binary + Alpine).

---

## 🔹 Java (Multi‑Stage)

```dockerfile
# Stage 1: Build
FROM openjdk:17 AS builder
WORKDIR /app
COPY HelloWorld.java .
RUN javac HelloWorld.java

# Stage 2: Run (slim JRE)
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=builder /app/HelloWorld.class .
EXPOSE 8080
CMD ["java", "HelloWorld"]
```

👉 Size drops from ~600MB → ~200MB (no compiler, just runtime).

---

## 🔹 Python (Multi‑Stage)

```dockerfile
# Stage 1: Build
FROM python:3.11 AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

# Stage 2: Run (slim base)
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /app .
EXPOSE 5000
CMD ["python", "app.py"]
```

👉 Size drops from ~900MB → ~120MB (slim runtime only).

---

## 🔹 Node.js (Multi‑Stage)

```dockerfile
# Stage 1: Build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .

# Stage 2: Run (slim base)
FROM node:18-slim
WORKDIR /app
COPY --from=builder /app .
EXPOSE 3000
CMD ["node", "app.js"]
```

👉 Size drops from ~900MB → ~150MB (no build tools, just runtime).


PS D:\ANDYtws\twstask\day35> docker images hello-node-single
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
hello-node-single   latest    a84c7d3e3443   7 minutes ago   1.58GB

PS D:\ANDYtws\twstask\day35> docker images multi-stage-node-app
REPOSITORY             TAG       IMAGE ID       CREATED              SIZE
multi-stage-node-app   latest    ce3ac4b3fd40   About a minute ago   285MB 


---

## 🔹 Why Multi‑Stage Images Are Smaller
- **Stage 1** includes compilers, build tools, caches → heavy.  
- **Stage 2** copies only the final artifact (binary, `.class`, or app code).  
- Uses **minimal base images** (`alpine`, `slim`, `distroless`) → fewer libraries, smaller footprint.  
- Result: Images shrink by **70–90%**, faster to pull/push, more secure (less attack surface).

---

✅ Now you can build each (`docker build -t hello-go .`, etc.), check sizes with `docker images`, and compare.  

Would you like me to also prepare a **comparison table** (single‑stage vs multi‑stage sizes for each language) so you can paste it directly into your notes?


'''

---

### Task 3: Push to Docker Hub
1. Create a free account on [Docker Hub](https://hub.docker.com) (if you don't have one)
2. Log in from your terminal
3. Tag your image properly: `yourusername/image-name:tag`
4. Push it to Docker Hub
5. Pull it on a different machine (or after removing locally) to verify

'''
Here’s how you can complete **Task 3: Push to Docker Hub** step by step. This is the final stage after building your optimized images.

---

## 🔹 Step 1: Create a Docker Hub Account
- Go to [Docker Hub](https://hub.docker.com) and sign up for a free account if you don’t already have one.
- Choose a username — this will be part of your image tag.

---

## 🔹 Step 2: Log In from Terminal
```bash
docker login
```
- Enter your Docker Hub username and password (or access token if you’ve set one up).

---

## 🔹 Step 3: Tag Your Image
Suppose you built the Go app image as `hello-go:latest`. Tag it with your Docker Hub username:

```bash
docker tag hello-go:latest yourusername/hello-go:1.0
```

Do the same for Python, Java, and Node.js images:
```bash
docker tag hello-python:latest yourusername/hello-python:1.0
docker tag hello-java:latest yourusername/hello-java:1.0
docker tag hello-node:latest yourusername/hello-node:1.0
```

---

## 🔹 Step 4: Push to Docker Hub
```bash
docker push yourusername/hello-go:1.0
docker push yourusername/hello-python:1.0
docker push yourusername/hello-java:1.0
docker push yourusername/hello-node:1.0
```

---

## 🔹 Step 5: Verify by Pulling
On another machine (or after removing locally):

```bash
docker rmi yourusername/hello-go:1.0
docker pull yourusername/hello-go:1.0
docker run -p 8080:8080 yourusername/hello-go:1.0
```

Repeat for Python, Java, and Node.js images.  
✅ If the container runs and serves your “Hello World” app, the push was successful.

---

## 🔹 Notes for Comparison
- **Single-stage images**: large (600–900MB).  
- **Multi-stage images**: much smaller (15–200MB depending on language).  
- **Reason**: Multi-stage builds copy only the final artifact into a minimal runtime image, stripping away compilers, caches, and build tools. This reduces size, speeds up pulls/pushes, and lowers attack surface.

---

Would you like me to prepare a **comparison table (language vs single-stage size vs multi-stage size)** so you can paste it directly into your notes for tomorrow’s client round?


'''
---

### Task 4: Docker Hub Repository
1. Go to Docker Hub and check your pushed image
2. Add a **description** to the repository
3. Explore the **tags** tab — understand how versioning works
4. Pull a specific tag vs `latest` — what happens?

'''
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a3ccc743-b535-400e-8dc6-6b38f3dfa16d" />

'''
      PS D:\ANDYtws\twstask\day35> docker pull 301988/hello-node:1.0   
      1.0: Pulling from 301988/hello-node
      Digest: sha256:dc9202624caaf49ef6d5016a992cc9b3be0c340e56cf8fb60419d6e59d29bd59
      Status: Image is up to date for 301988/hello-node:1.0
      docker.io/301988/hello-node:1.0
      
      PS D:\ANDYtws\twstask\day35> docker pull 301988/hello-node:latest
      latest: Pulling from 301988/hello-node
      Digest: sha256:dc9202624caaf49ef6d5016a992cc9b3be0c340e56cf8fb60419d6e59d29bd59
      Status: Image is up to date for 301988/hello-node:latest
      docker.io/301988/hello-node:latest

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f37cba87-118b-4242-a6f5-f2b5bc1b0569" />



---

