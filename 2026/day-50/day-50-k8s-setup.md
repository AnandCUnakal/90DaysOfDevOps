Task 1: Recall the Kubernetes Story
Before touching a terminal, write down from memory:

Why was Kubernetes created? What problem does it solve that Docker alone cannot?
Who created Kubernetes and what was it inspired by?
What does the name "Kubernetes" mean?
Do not look anything up yet. Write what you remember from the session, then verify against the official docs.

Task 2: Draw the Kubernetes Architecture
From memory, draw or describe the Kubernetes architecture. Your diagram should include:

Control Plane (Master Node):

API Server — the front door to the cluster, every command goes through it
etcd — the database that stores all cluster state
Scheduler — decides which node a new pod should run on
Controller Manager — watches the cluster and makes sure the desired state matches reality
Worker Node:

kubelet — the agent on each node that talks to the API server and manages pods
kube-proxy — handles networking rules so pods can communicate
Container Runtime — the engine that actually runs containers (containerd, CRI-O)
After drawing, verify your understanding:

What happens when you run kubectl apply -f pod.yaml? Trace the request through each component.
What happens if the API server goes down?
What happens if a worker node goes down?
Task 3: Install kubectl
kubectl is the CLI tool you will use to talk to your Kubernetes cluster.

Install it:

# macOS
brew install kubectl

# Linux (amd64)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Windows (with chocolatey)
choco install kubernetes-cli
Verify:

kubectl version --client
Task 4: Set Up Your Local Cluster
Choose one of the following. Both give you a fully functional Kubernetes cluster on your machine.

Option A: kind (Kubernetes in Docker)

# Install kind
# macOS
brew install kind

# Linux
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create a cluster
kind create cluster --name devops-cluster

# Verify
kubectl cluster-info
kubectl get nodes
Option B: minikube

# Install minikube
# macOS
brew install minikube

# Linux
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start a cluster
minikube start

# Verify
kubectl cluster-info
kubectl get nodes
Write down: Which one did you choose and why?

Task 5: Explore Your Cluster
Now that your cluster is running, explore it:

# See cluster info
kubectl cluster-info

# List all nodes
kubectl get nodes

# Get detailed info about your node
kubectl describe node <node-name>

# List all namespaces
kubectl get namespaces

# See ALL pods running in the cluster (across all namespaces)
kubectl get pods -A
Look at the pods running in the kube-system namespace:

kubectl get pods -n kube-system
You should see pods like etcd, kube-apiserver, kube-scheduler, kube-controller-manager, coredns, and kube-proxy. These are the architecture components you drew in Task 2 — running as pods inside the cluster.

Verify: Can you match each running pod in kube-system to a component in your architecture diagram?

Task 6: Practice Cluster Lifecycle
Build muscle memory with cluster operations:

# Delete your cluster
kind delete cluster --name devops-cluster
# (or: minikube delete)

# Recreate it
kind create cluster --name devops-cluster
# (or: minikube start)

# Verify it is back
kubectl get nodes
Try these useful commands:

# Check which cluster kubectl is connected to
kubectl config current-context

# List all available contexts (clusters)
kubectl config get-contexts

# See the full kubeconfig
kubectl config view
Write down: What is a kubeconfig? Where is it stored on your machine?

'''
Perfect—this is where you transition from **Docker → real Kubernetes thinking**.
Write your answers first, then use this to **verify and deepen understanding**.

---

# ✅ Task 1: Kubernetes Story (Verify)

### Why was Kubernetes created?

Kubernetes was created to solve problems that **Docker alone cannot handle**:

* Docker runs **single containers**, but not large-scale systems
* No built-in way to:

  * manage thousands of containers
  * auto-heal failures
  * scale applications
  * handle networking between containers across machines

👉 Kubernetes solves:

```text
Container orchestration at scale
```

---

### Who created Kubernetes?

* Created by **Google**
* Inspired by Google’s internal system called **Borg**

---

### What does "Kubernetes" mean?

* Greek word meaning:

```text
"Helmsman" or "Ship Captain"
```

That’s why the logo is a **ship wheel ⚓**

---

# ✅ Task 2: Kubernetes Architecture (Verified)

## Control Plane (Master Node)

```
API Server → Entry point
etcd → Database
Scheduler → Assigns pods to nodes
Controller Manager → Maintains desired state
```

---

## Worker Node

```
kubelet → Node agent
kube-proxy → Networking
Container Runtime → Runs containers (containerd)
```

---

## 🔄 What happens when you run:

```bash
kubectl apply -f pod.yaml
```

### Flow:

```text
1. kubectl → API Server
2. API Server validates request
3. API Server stores desired state in etcd
4. Scheduler finds best node
5. kubelet (on node) receives instruction
6. kubelet asks container runtime to start container
7. Pod is created and running
```

---

## ❌ What if API Server goes down?

* You **cannot create/update/delete resources**
* Cluster keeps running existing workloads
* No new deployments possible

---

## ❌ What if Worker Node goes down?

* Pods on that node are **lost**
* Controller detects mismatch
* Scheduler recreates pods on another node

👉 This is **self-healing**

---

# ✅ Task 3: kubectl

Verify:

```bash
kubectl version --client
```

Expected:

```text
Client Version: v1.xx.x
```

---

# ✅ Task 4: Local Cluster Choice

## Option A: kind

**Best for DevOps learning**

✔ Runs inside Docker
✔ Lightweight
✔ Fast startup
✔ Great for CI/CD

---

## Option B: minikube

**Best for beginners**

✔ Easier setup
✔ GUI dashboard
✔ Simulates real cluster better

---

### Recommended Answer

```text
I chose kind because it is lightweight, fast, and integrates well with Docker and CI/CD pipelines.
```

---

# ✅ Task 5: Explore Cluster

### Key Commands

```bash
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
kubectl get namespaces
```

---

## kube-system Pods Mapping

When you run:

```bash
kubectl get pods -n kube-system
```

You’ll see:

| Pod                     | Component          |
| ----------------------- | ------------------ |
| kube-apiserver          | API Server         |
| etcd                    | Database           |
| kube-scheduler          | Scheduler          |
| kube-controller-manager | Controller         |
| kube-proxy              | Networking         |
| coredns                 | DNS inside cluster |

---

### Important Insight

```text
Kubernetes components run as pods themselves!
```

This is a **self-hosted control plane**.

---

# ✅ Task 6: Cluster Lifecycle

### Delete cluster

```bash
kind delete cluster --name devops-cluster
```

---

### Recreate

```bash
kind create cluster --name devops-cluster
```

---

### Verify

```bash
kubectl get nodes
```

---

# 🧠 kubeconfig (Important Concept)

### What is kubeconfig?

A **configuration file** that tells `kubectl`:

* which cluster to connect to
* user credentials
* authentication details
* cluster endpoint

---

### Where is it stored?

```bash
~/.kube/config
```

---

### Useful Commands

```bash
kubectl config current-context
kubectl config get-contexts
kubectl config view
```

---

# 🔥 Real DevOps Insight

Your setup now looks like:

```text
kubectl → API Server → Cluster
```

This is exactly how:

* CI/CD pipelines deploy apps
* DevOps engineers manage clusters
* Kubernetes operates in production

---

# 🚀 What You Just Achieved

You now understand:

✔ Why Kubernetes exists
✔ Core architecture
✔ Control plane vs worker nodes
✔ kubectl workflow
✔ Cluster lifecycle
✔ kubeconfig

---

# 💡 Next Step (Critical)

The **next milestone** is:

```text
Deploy your first application (Pod → Deployment → Service)
```

That’s where everything clicks.

---

If you want, I’ll guide you next through:

👉 **Deploying your Docker app into Kubernetes (end-to-end)**
(with YAML + real commands + interview questions)

'''
