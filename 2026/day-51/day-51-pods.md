Task 1: Create Your First Pod (Nginx)
Create a file called nginx-pod.yaml:

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
Apply it:

kubectl apply -f nginx-pod.yaml
Verify:

kubectl get pods
kubectl get pods -o wide
Wait until the STATUS shows Running. Then explore:

# Detailed info about the pod
kubectl describe pod nginx-pod

# Read the logs
kubectl logs nginx-pod

# Get a shell inside the container
kubectl exec -it nginx-pod -- /bin/bash

# Inside the container, run:
curl localhost:80
exit
Verify: Can you see the Nginx welcome page when you curl from inside the pod?

Task 2: Create a Custom Pod (BusyBox)
Write a new manifest busybox-pod.yaml from scratch (do not copy-paste the nginx one):

apiVersion: v1
kind: Pod
metadata:
  name: busybox-pod
  labels:
    app: busybox
    environment: dev
spec:
  containers:
  - name: busybox
    image: busybox:latest
    command: ["sh", "-c", "echo Hello from BusyBox && sleep 3600"]
Apply and verify:

kubectl apply -f busybox-pod.yaml
kubectl get pods
kubectl logs busybox-pod
Notice the command field — BusyBox does not run a long-lived server like Nginx. Without a command that keeps it running, the container would exit immediately and the pod would go into CrashLoopBackOff.

Verify: Can you see "Hello from BusyBox" in the logs?

Task 3: Imperative vs Declarative
You have been using the declarative approach (writing YAML, then kubectl apply). Kubernetes also supports imperative commands:

# Create a pod without a YAML file
kubectl run redis-pod --image=redis:latest

# Check it
kubectl get pods
Now extract the YAML that Kubernetes generated:

kubectl get pod redis-pod -o yaml
Compare this output with your hand-written manifests. Notice how much extra metadata Kubernetes adds automatically (status, timestamps, uid, resource version).

You can also use dry-run to generate YAML without creating anything:

kubectl run test-pod --image=nginx --dry-run=client -o yaml
This is a powerful trick — use it to quickly scaffold a manifest, then customize it.

Verify: Save the dry-run output to a file and compare its structure with your nginx-pod.yaml. What fields are the same? What is different?

Task 4: Validate Before Applying
Before applying a manifest, you can validate it:

# Check if the YAML is valid without actually creating the resource
kubectl apply -f nginx-pod.yaml --dry-run=client

# Validate against the cluster's API (server-side validation)
kubectl apply -f nginx-pod.yaml --dry-run=server
Now intentionally break your YAML (remove the image field or add an invalid field) and run dry-run again. See what error you get.

Verify: What error does Kubernetes give when the image field is missing?

Task 5: Pod Labels and Filtering
Labels are how Kubernetes organizes and selects resources. You added labels in your manifests — now use them:

# List all pods with their labels
kubectl get pods --show-labels

# Filter pods by label
kubectl get pods -l app=nginx
kubectl get pods -l environment=dev

# Add a label to an existing pod
kubectl label pod nginx-pod environment=production

# Verify
kubectl get pods --show-labels

# Remove a label
kubectl label pod nginx-pod environment-
Write a manifest for a third pod with at least 3 labels (app, environment, team). Apply it and practice filtering.

Task 6: Clean Up
Delete all the pods you created:

# Delete by name
kubectl delete pod nginx-pod
kubectl delete pod busybox-pod
kubectl delete pod redis-pod

# Or delete using the manifest file
kubectl delete -f nginx-pod.yaml

# Verify everything is gone
kubectl get pods
Notice that when you delete a standalone Pod, it is gone forever. There is no controller to recreate it. This is why in production you use Deployments (coming on Day 52) instead of bare Pods.

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
