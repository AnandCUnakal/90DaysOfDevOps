<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d547390f-901a-4801-ad36-c893c1db6bab" /><img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fc5d8e6b-27f6-4293-8259-98df9318abfb" />### Task 1: Explore Default Namespaces
Kubernetes comes with built-in namespaces. List them:

```bash
kubectl get namespaces
```

You should see at least:
- `default` — where your resources go if you do not specify a namespace
- `kube-system` — Kubernetes internal components (API server, scheduler, etc.)
- `kube-public` — publicly readable resources
- `kube-node-lease` — node heartbeat tracking

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/54144eb0-1e57-48bd-b913-eb4164d66294" />

Check what is running inside `kube-system`:
```bash
kubectl get pods -n kube-system
```

These are the control plane components keeping your cluster alive. Do not touch them.

**Verify:** How many pods are running in `kube-system`?

---

### Task 2: Create and Use Custom Namespaces
Create two namespaces — one for a development environment and one for staging:

```bash
kubectl create namespace dev
kubectl create namespace staging
```

Verify they exist:
```bash
kubectl get namespaces
```

You can also create a namespace from a manifest:
```yaml
# namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

```bash
kubectl apply -f namespace.yaml
```

Now run a pod in a specific namespace:
```bash
kubectl run nginx-dev --image=nginx:latest -n dev
kubectl run nginx-staging --image=nginx:latest -n staging
```

List pods across all namespaces:
```bash
kubectl get pods -A
```

Notice that `kubectl get pods` without `-n` only shows the `default` namespace. You must specify `-n <namespace>` or use `-A` to see everything.

**Verify:** Does `kubectl get pods` show these pods? What about `kubectl get pods -A`?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45281643-9e4f-41ff-af65-1ed0fd79c5c3" />

---

### Task 3: Create Your First Deployment
A Deployment tells Kubernetes: "I want X replicas of this Pod running at all times." If a Pod crashes, the Deployment controller recreates it automatically.

Create a file `nginx-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6423e5da-7fd9-4321-b3d2-a3bac0284c30" />


Key differences from a standalone Pod:
- `kind: Deployment` instead of `kind: Pod`
- `apiVersion: apps/v1` instead of `v1`
- `replicas: 3` tells Kubernetes to maintain 3 identical pods
- `selector.matchLabels` connects the Deployment to its Pods
- `template` is the Pod template — the Deployment creates Pods using this blueprint

Apply it:
```bash
kubectl apply -f nginx-deployment.yaml
```

Check the result:
```bash
kubectl get deployments -n dev
kubectl get pods -n dev
```

You should see 3 pods with names like `nginx-deployment-xxxxx-yyyyy`.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dc10889b-3383-4a4e-b173-1e5fb38a8dc5" />


**Verify:** What do the READY, UP-TO-DATE, and AVAILABLE columns mean in the deployment output?


---

### Task 4: Self-Healing — Delete a Pod and Watch It Come Back
This is the key difference between a Deployment and a standalone Pod.

```bash
# List pods
kubectl get pods -n dev

# Delete one of the deployment's pods (use an actual pod name from your output)
kubectl delete pod <pod-name> -n dev

# Immediately check again
kubectl get pods -n dev
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd915f08-c6f9-4821-8a5c-0708eb3b4e17" />


The Deployment controller detects that only 2 of 3 desired replicas exist and immediately creates a new one. The deleted pod is replaced within seconds.

**Verify:** Is the replacement pod's name the same as the one you deleted, or different?

---

### Task 5: Scale the Deployment
Change the number of replicas:

```bash
# Scale up to 5
kubectl scale deployment nginx-deployment --replicas=5 -n dev
kubectl get pods -n dev

# Scale down to 2
kubectl scale deployment nginx-deployment --replicas=2 -n dev
kubectl get pods -n dev

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0c4ae387-a845-477b-a58d-bc23a8db0658" />

```


Watch how Kubernetes creates or terminates pods to match the desired count.

You can also scale by editing the manifest — change `replicas: 4` in your YAML file and run `kubectl apply -f nginx-deployment.yaml` again.

**Verify:** When you scaled down from 5 to 2, what happened to the extra pods?

---

### Task 6: Rolling Update
Update the Nginx image version to trigger a rolling update:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f35baf5-4adf-48b1-935d-5151c395e6d1" />

Watch the rollout in real time:
```bash
kubectl rollout status deployment/nginx-deployment -n dev
```

Kubernetes replaces pods one by one — old pods are terminated only after new ones are healthy. This means zero downtime.

Check the rollout history:
```bash
kubectl rollout history deployment/nginx-deployment -n dev
```

Now roll back to the previous version:
```bash
kubectl rollout undo deployment/nginx-deployment -n dev
kubectl rollout status deployment/nginx-deployment -n dev
```

Verify the image is back to the previous version:
```bash
kubectl describe deployment nginx-deployment -n dev | grep Image
```

**Verify:** What image version is running after the rollback?

---

### Task 7: Clean Up
```bash
kubectl delete deployment nginx-deployment -n dev
kubectl delete pod nginx-dev -n dev
kubectl delete pod nginx-staging -n staging
kubectl delete namespace dev staging production
```

Deleting a namespace removes everything inside it. Be very careful with this in production.

```bash
kubectl get namespaces
kubectl get pods -A
```

**Verify:** Are all your resources gone?

'''
Great—this is your **first real hands-on Kubernetes control loop experience**. I’ll give you **expected outputs + what you should observe + notes answers** so you can verify everything properly.

---

# ✅ Task 1: Default Namespaces

Run:

```bash
kubectl get namespaces
```

### Expected output

```text
default
kube-system
kube-public
kube-node-lease
```

---

### Check system pods

```bash
kubectl get pods -n kube-system
```

### What you’ll see (example)

```text
coredns-xxxx
etcd-xxxx
kube-apiserver-xxxx
kube-controller-manager-xxxx
kube-scheduler-xxxx
kube-proxy-xxxx
```

---

### ✅ Verify Answer

👉 **How many pods?**

* Depends on your setup (kind/minikube)
* Usually:

```text
5 – 8 pods
```

✔ That’s normal.

---

# ✅ Task 2: Custom Namespaces

After creating:

```bash
kubectl create namespace dev
kubectl create namespace staging
```

---

### Run pods

```bash
kubectl run nginx-dev --image=nginx:latest -n dev
kubectl run nginx-staging --image=nginx:latest -n staging
```

---

### Check pods

```bash
kubectl get pods
```

Output:

```text
No resources found in default namespace
```

---

### Check all namespaces

```bash
kubectl get pods -A
```

Output:

```text
dev         nginx-dev
staging     nginx-staging
```

---

### ✅ Verify Answer

✔ `kubectl get pods` → shows only **default namespace**
✔ `kubectl get pods -A` → shows **all namespaces**

---

# ✅ Task 3: Deployment

After applying:

```bash
kubectl apply -f nginx-deployment.yaml
```

---

### Check deployment

```bash
kubectl get deployments -n dev
```

Example:

```text
NAME                READY   UP-TO-DATE   AVAILABLE
nginx-deployment    3/3     3            3
```

---

### ✅ Verify Answer

| Column     | Meaning                     |
| ---------- | --------------------------- |
| READY      | Running pods / Desired pods |
| UP-TO-DATE | Pods updated to latest spec |
| AVAILABLE  | Pods ready to serve traffic |

---

# ✅ Task 4: Self-Healing

Delete a pod:

```bash
kubectl delete pod <pod-name> -n dev
```

Immediately check:

```bash
kubectl get pods -n dev
```

---

### What happens?

```text
Old pod → deleted
New pod → created automatically
```

---

### ✅ Verify Answer

👉 Replacement pod name:

```text
Different name
```

Because Kubernetes creates a **new pod**, not restores the old one.

---

# ✅ Task 5: Scaling

### Scale up

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n dev
```

You’ll see:

```text
5 pods running
```

---

### Scale down

```bash
kubectl scale deployment nginx-deployment --replicas=2 -n dev
```

---

### ✅ Verify Answer

👉 What happened to extra pods?

```text
Kubernetes TERMINATED them
```

Gracefully deleted to match desired state.

---

# ✅ Task 6: Rolling Update

### Update image

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
```

---

### Watch rollout

```bash
kubectl rollout status deployment/nginx-deployment -n dev
```

---

### What happens internally?

```text
Old pod → new pod created
New pod becomes ready
Old pod deleted
Repeat…
```

✔ Zero downtime deployment

---

### Rollback

```bash
kubectl rollout undo deployment/nginx-deployment -n dev
```

---

### Verify image

```bash
kubectl describe deployment nginx-deployment -n dev | grep Image
```

---

### ✅ Verify Answer

👉 After rollback:

```text
nginx:1.24
```

(back to previous version)

---

# ✅ Task 7: Cleanup

```bash
kubectl delete deployment nginx-deployment -n dev
kubectl delete pod nginx-dev -n dev
kubectl delete pod nginx-staging -n staging
kubectl delete namespace dev staging production
```

---

### Verify

```bash
kubectl get pods -A
```

Expected:

```text
Only kube-system pods remain
```

---

### ✅ Final Verify Answer

👉 Are all your resources gone?

```text
YES (except system pods)
```

---

# 🔥 What You Just Learned (Critical)

You now understand:

```text
Namespace → Isolation
Deployment → Desired state
Replica → Scaling
Controller → Self-healing
Rollout → Zero downtime updates
```

---

# 🧠 Real DevOps Mapping

What you just did manually = what CI/CD does automatically:

```text
GitHub Actions
   ↓
kubectl apply
   ↓
Deployment created
   ↓
Pods running
   ↓
App live
```

---

# 💡 Interview-Level Insights

Be ready to answer:

### ❓ Why Deployment over Pod?

```text
Pods are not self-healing
Deployments are self-healing + scalable
```

---

### ❓ What ensures self-healing?

```text
Controller Manager
```

---

### ❓ What ensures scheduling?

```text
Scheduler
```

---

### ❓ Where is state stored?

```text
etcd
```

---

# 🚀 Next Step (Very Important)

Now you’re ready for:

```text
Expose your app using a Service (ClusterIP / NodePort / LoadBalancer)
```

👉 This is how users actually access your app.

---

If you want, I’ll guide you next:

**Deploy your Docker app → Expose via Service → Access in browser**

That’s where Kubernetes becomes real 🔥

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1a87a644-ed2f-4596-bc25-b397ad8bc68a" />


'''
