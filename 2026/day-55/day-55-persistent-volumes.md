---

## Challenge Tasks

### Task 1: See the Problem — Data Lost on Pod Deletion
1. Write a Pod manifest that uses an `emptyDir` volume and writes a timestamped message to `/data/message.txt`
2. Apply it, verify the data exists with `kubectl exec`
3. Delete the Pod, recreate it, check the file again — the old message is gone

**Verify:** Is the timestamp the same or different after recreation?

---

### Task 2: Create a PersistentVolume (Static Provisioning)
1. Write a PV manifest with `capacity: 1Gi`, `accessModes: ReadWriteOnce`, `persistentVolumeReclaimPolicy: Retain`, and `hostPath` pointing to `/tmp/k8s-pv-data`
2. Apply it and check `kubectl get pv` — status should be `Available`

Access modes to know:
- `ReadWriteOnce (RWO)` — read-write by a single node
- `ReadOnlyMany (ROX)` — read-only by many nodes
- `ReadWriteMany (RWX)` — read-write by many nodes

`hostPath` is fine for learning, not for production.

**Verify:** What is the STATUS of the PV?

---

### Task 3: Create a PersistentVolumeClaim
1. Write a PVC manifest requesting `500Mi` of storage with `ReadWriteOnce` access
2. Apply it and check both `kubectl get pvc` and `kubectl get pv`
3. Both should show `Bound` — Kubernetes matched them by capacity and access mode

**Verify:** What does the VOLUME column in `kubectl get pvc` show?

---

### Task 4: Use the PVC in a Pod — Data That Survives
1. Write a Pod manifest that mounts the PVC at `/data` using `persistentVolumeClaim.claimName`
2. Write data to `/data/message.txt`, then delete and recreate the Pod
3. Check the file — it should contain data from both Pods

**Verify:** Does the file contain data from both the first and second Pod?

---

### Task 5: StorageClasses and Dynamic Provisioning
1. Run `kubectl get storageclass` and `kubectl describe storageclass`
2. Note the provisioner, reclaim policy, and volume binding mode
3. With dynamic provisioning, developers only create PVCs — the StorageClass handles PV creation automatically

**Verify:** What is the default StorageClass in your cluster?

---

### Task 6: Dynamic Provisioning
1. Write a PVC manifest that includes `storageClassName: standard` (or your cluster's default)
2. Apply it — a PV should appear automatically in `kubectl get pv`
3. Use this PVC in a Pod, write data, verify it works

**Verify:** How many PVs exist now? Which was manual, which was dynamic?

---

### Task 7: Clean Up
1. Delete all pods first
2. Delete PVCs — check `kubectl get pv` to see what happened
3. The dynamic PV is gone (Delete reclaim policy). The manual PV shows `Released` (Retain policy).
4. Delete the remaining PV manually

**Verify:** Which PV was auto-deleted and which was retained? Why?

---


This is a **core Kubernetes storage milestone**—once you understand this, you understand how real apps persist data. I’ll give you **working YAML + expected outputs + exact verification answers**.

---

# ✅ Task 1: emptyDir → Data Loss

## Pod manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: emptydir-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "date >> /data/message.txt && sleep 3600"]
    volumeMounts:
    - name: temp-data
      mountPath: /data
  volumes:
  - name: temp-data
    emptyDir: {}
```

---

### Verify

```bash
kubectl apply -f emptydir-pod.yaml
kubectl exec -it emptydir-pod -- cat /data/message.txt
```

---

### Delete & recreate

```bash
kubectl delete pod emptydir-pod
kubectl apply -f emptydir-pod.yaml
kubectl exec -it emptydir-pod -- cat /data/message.txt
```

---

### ✅ Verify Answer

```text
Timestamp is DIFFERENT
```

👉 Because:

```text
emptyDir = deleted when Pod is deleted
```

---

# ✅ Task 2: PersistentVolume (PV)

## PV YAML

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: manual-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /tmp/k8s-pv-data
```

---

### Apply

```bash
kubectl apply -f pv.yaml
kubectl get pv
```

---

### ✅ Verify Answer

```text
STATUS = Available
```

---

# ✅ Task 3: PersistentVolumeClaim (PVC)

## PVC YAML

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

---

### Apply

```bash
kubectl apply -f pvc.yaml
kubectl get pvc
kubectl get pv
```

---

### Expected

```text
PVC → Bound
PV → Bound
```

---

### ✅ Verify Answer

```text
VOLUME column = manual-pv
```

👉 PVC is now linked to PV.

---

# ✅ Task 4: Use PVC in Pod

## Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pvc-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "date >> /data/message.txt && sleep 3600"]
    volumeMounts:
    - name: persistent-storage
      mountPath: /data
  volumes:
  - name: persistent-storage
    persistentVolumeClaim:
      claimName: my-pvc
```

---

### Test

```bash
kubectl apply -f pvc-pod.yaml
kubectl exec -it pvc-pod -- cat /data/message.txt
```

Delete & recreate:

```bash
kubectl delete pod pvc-pod
kubectl apply -f pvc-pod.yaml
kubectl exec -it pvc-pod -- cat /data/message.txt
```

---

### ✅ Verify Answer

```text
YES → file contains BOTH timestamps
```

👉 Data persists because of PV.

---

# 🔥 Key Insight

```text
Pod dies ❌
Data survives ✅ (via PV)
```

---

# ✅ Task 5: StorageClass

```bash
kubectl get storageclass
kubectl describe storageclass
```

---

### Example Output

```text
standard (default)
```

---

### ✅ Verify Answer

```text
Default StorageClass = standard
```

(Your cluster may differ slightly)

---

# ✅ Task 6: Dynamic Provisioning

## PVC with StorageClass

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: dynamic-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  resources:
    requests:
      storage: 500Mi
```

---

### Apply

```bash
kubectl apply -f dynamic-pvc.yaml
kubectl get pvc
kubectl get pv
```

---

### Expected

```text
New PV automatically created
```

---

### ✅ Verify Answer

✔ How many PVs?

```text
2 PVs:
1 → manual (hostPath)
1 → dynamic (StorageClass)
```

---

# ✅ Task 7: Cleanup

```bash
kubectl delete pod pvc-pod
kubectl delete pvc my-pvc dynamic-pvc
kubectl get pv
```

---

### Expected

| PV Type    | Result   |
| ---------- | -------- |
| Manual PV  | Released |
| Dynamic PV | Deleted  |

---

### ✅ Verify Answer

✔ Which deleted?

```text
Dynamic PV → auto-deleted
```

✔ Which retained?

```text
Manual PV → Released (Retain policy)
```

✔ Why?

```text
Reclaim Policy:
Delete → auto remove
Retain → keep data
```

---

# 🔥 Final Concepts (Very Important)

## Storage Flow

```text
Pod → PVC → PV → Storage
```

---

## Types

| Type         | Behavior       |
| ------------ | -------------- |
| emptyDir     | temporary      |
| PV           | actual storage |
| PVC          | request        |
| StorageClass | automation     |

---

# 🧠 Real DevOps Understanding

### Without PV

```text
Pod restart = data loss ❌
```

### With PV

```text
Pod restart = data safe ✅
```

---

# 💡 Interview Questions

### ❓ PV vs PVC?

```text
PV = actual storage
PVC = request for storage
```

---

### ❓ Why StorageClass?

```text
Automates PV creation
```

---

### ❓ What is reclaim policy?

```text
Defines what happens when PVC is deleted
```

---


---

If you want next:

🔥 I’ll show **Deploy MySQL/PostgreSQL with persistent storage in Kubernetes (real-world setup)**
