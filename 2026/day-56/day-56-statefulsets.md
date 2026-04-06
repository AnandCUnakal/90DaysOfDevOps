### Task 1: Understand the Problem
1. Create a Deployment with 3 replicas using nginx
2. Check the pod names — they are random (`app-xyz-abc`)
3. Delete a pod and notice the replacement gets a different random name

This is fine for web servers but not for databases where you need stable identity.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/599e7b75-9814-448f-8f9a-3aaf793ac9c8" />


| Feature | Deployment | StatefulSet |
|---|---|---|
| Pod names | Random | Stable, ordered (`app-0`, `app-1`) |
| Startup order | All at once | Ordered: pod-0, then pod-1, then pod-2 |
| Storage | Shared PVC | Each pod gets its own PVC |
| Network identity | No stable hostname | Stable DNS per pod |

Delete the Deployment before moving on.

**Verify:** Why would random pod names be a problem for a database cluster?

---

### Task 2: Create a Headless Service
1. Write a Service manifest with `clusterIP: None` — this is a Headless Service
2. Set the selector to match the labels you will use on your StatefulSet pods
3. Apply it and confirm CLUSTER-IP shows `None`

A Headless Service creates individual DNS entries for each pod instead of load-balancing to one IP. StatefulSets require this.

**Verify:** What does the CLUSTER-IP column show?

---

### Task 3: Create a StatefulSet
1. Write a StatefulSet manifest with `serviceName` pointing to your Headless Service
2. Set replicas to 3, use the nginx image
3. Add a `volumeClaimTemplates` section requesting 100Mi of ReadWriteOnce storage
4. Apply and watch: `kubectl get pods -l <your-label> -w`

Observe ordered creation — `web-0` first, then `web-1` after `web-0` is Ready, then `web-2`.

Check the PVCs: `kubectl get pvc` — you should see `web-data-web-0`, `web-data-web-1`, `web-data-web-2` (names follow the pattern `<template-name>-<pod-name>`).

**Verify:** What are the exact pod names and PVC names?

---

### Task 4: Stable Network Identity
Each StatefulSet pod gets a DNS name: `<pod-name>.<service-name>.<namespace>.svc.cluster.local`

1. Run a temporary busybox pod and use `nslookup` to resolve `web-0.<your-headless-service>.default.svc.cluster.local`
2. Do the same for `web-1` and `web-2`
3. Confirm the IPs match `kubectl get pods -o wide`

**Verify:** Does the nslookup IP match the pod IP?

---

### Task 5: Stable Storage — Data Survives Pod Deletion
1. Write unique data to each pod: `kubectl exec web-0 -- sh -c "echo 'Data from web-0' > /usr/share/nginx/html/index.html"`
2. Delete `web-0`: `kubectl delete pod web-0`
3. Wait for it to come back, then check the data — it should still be "Data from web-0"

The new pod reconnected to the same PVC.

**Verify:** Is the data identical after pod recreation?

---

### Task 6: Ordered Scaling
1. Scale up to 5: `kubectl scale statefulset web --replicas=5` — pods create in order (web-3, then web-4)
2. Scale down to 3 — pods terminate in reverse order (web-4, then web-3)
3. Check `kubectl get pvc` — all five PVCs still exist. Kubernetes keeps them on scale-down so data is preserved if you scale back up.

**Verify:** After scaling down, how many PVCs exist?



'''
Great — this is exactly how you build intuition for **StatefulSets vs Deployments**. I’ll walk through each **Verify** answer clearly so you can confirm your understanding like in a real DevOps interview.

---

# ✅ Task 1 — Verify

**Why are random pod names a problem for a database cluster?**

Because databases need **stable identity + predictable networking**:

* Replication configs often refer to nodes by **hostname**
* Example: primary → replica mapping (`db-0`, `db-1`)
* If pod name changes:

  * Replication breaks ❌
  * Cluster membership becomes inconsistent ❌
* Also:

  * Persistent storage may get misaligned
  * Leader election can fail

👉 In short:
**Databases need fixed identity — Deployments give random identity.**

---

# ✅ Task 2 — Verify

**What does the CLUSTER-IP column show?**

When you run:

```bash
kubectl get svc
```

You should see:

```text
NAME         TYPE        CLUSTER-IP   PORT(S)
web-headless ClusterIP   None         80/TCP
```

👉 **Answer:**
**CLUSTER-IP = `None`**

✔ This confirms it’s a **Headless Service**

---

# ✅ Task 3 — Verify

**What are the exact pod names and PVC names?**

### Pods (stable & ordered)

```text
web-0
web-1
web-2
```

### PVCs (one per pod)

```text
web-data-web-0
web-data-web-1
web-data-web-2
```

👉 Pattern:

* Pod → `web-0`
* PVC → `<volume-name>-<pod-name>`
  → `web-data-web-0`

✔ Each pod gets **its own dedicated storage**

---

# ✅ Task 4 — Verify

**Does nslookup IP match the pod IP?**

You run:

```bash
nslookup web-0.web-headless.default.svc.cluster.local
```

Then:

```bash
kubectl get pods -o wide
```

👉 **Answer: YES ✅**

* DNS resolves to **exact pod IP**
* No load balancing (unlike normal service)

✔ This proves:

* Each pod has **stable DNS identity**
* Required for clustering systems (MongoDB, Kafka, etc.)

---

# ✅ Task 5 — Verify

**Is the data identical after pod recreation?**

👉 **Answer: YES ✅**

* You deleted `web-0`
* Kubernetes recreated it
* It reattached to **same PVC**

✔ Result:

```text
Data from web-0
```

still exists

👉 This proves:
**StatefulSets preserve storage across pod restarts**

---

# ✅ Task 6 — Verify

**After scaling down, how many PVCs exist?**

Steps:

```bash
kubectl scale statefulset web --replicas=5
kubectl scale statefulset web --replicas=3
kubectl get pvc
```

👉 **Answer: 5 PVCs still exist ✅**

```text
web-data-web-0
web-data-web-1
web-data-web-2
web-data-web-3
web-data-web-4
```

✔ Even though pods `web-3` and `web-4` are deleted:

* Their **PVCs are NOT deleted**

👉 Reason:

* Kubernetes protects data
* If you scale up again → pods reuse same storage

---

# 🧠 Final Understanding (Important for Interviews)

| Concept          | Key Takeaway                     |
| ---------------- | -------------------------------- |
| Deployment       | Stateless, replaceable pods      |
| StatefulSet      | Stable identity + storage        |
| Headless Service | Direct pod DNS, no LB            |
| PVC behavior     | Never auto-deleted on scale-down |
| Scaling          | Ordered (create ↑, delete ↓)     |

---

# 🚀 Real-world Mapping

Use **StatefulSet for:**

* MySQL / PostgreSQL
* MongoDB
* Kafka
* Zookeeper
* Elasticsearch

Use **Deployment for:**

* APIs
* Frontend apps
* Stateless microservices

---

If you want next level 🔥
I can give you:

* Full **MongoDB StatefulSet YAML (production-grade)**
* StatefulSet **interview questions (top 25)**
* Real-world **failure scenarios + troubleshooting**

Just tell me 👍

'''
