---

## Challenge Tasks

### Task 1: Resource Requests and Limits
1. Write a Pod manifest with `resources.requests` (cpu: 100m, memory: 128Mi) and `resources.limits` (cpu: 250m, memory: 256Mi)
2. Apply and inspect with `kubectl describe pod` — look for the Requests, Limits, and QoS Class sections
3. Since requests and limits differ, the QoS class is `Burstable`. If equal, it would be `Guaranteed`. If missing, `BestEffort`.

CPU is in millicores: `100m` = 0.1 CPU. Memory is in mebibytes: `128Mi`.

**Requests** = guaranteed minimum (scheduler uses this for placement). **Limits** = maximum allowed (kubelet enforces at runtime).

**Verify:** What QoS class does your Pod have?

---

### Task 2: OOMKilled — Exceeding Memory Limits
1. Write a Pod manifest using the `polinux/stress` image with a memory limit of `100Mi`
2. Set the stress command to allocate 200M of memory: `command: ["stress"] args: ["--vm", "1", "--vm-bytes", "200M", "--vm-hang", "1"]`
3. Apply and watch — the container gets killed immediately

CPU is throttled when over limit. Memory is killed — no mercy.

Check `kubectl describe pod` for `Reason: OOMKilled` and `Exit Code: 137` (128 + SIGKILL).

**Verify:** What exit code does an OOMKilled container have?

---

### Task 3: Pending Pod — Requesting Too Much
1. Write a Pod manifest requesting `cpu: 100` and `memory: 128Gi`
2. Apply and check — STATUS stays `Pending` forever
3. Run `kubectl describe pod` and read the Events — the scheduler says exactly why: insufficient resources

**Verify:** What event message does the scheduler produce?

---

### Task 4: Liveness Probe
A liveness probe detects stuck containers. If it fails, Kubernetes restarts the container.

1. Write a Pod manifest with a busybox container that creates `/tmp/healthy` on startup, then deletes it after 30 seconds
2. Add a liveness probe using `exec` that runs `cat /tmp/healthy`, with `periodSeconds: 5` and `failureThreshold: 3`
3. After the file is deleted, 3 consecutive failures trigger a restart. Watch with `kubectl get pod -w`

**Verify:** How many times has the container restarted?

---

### Task 5: Readiness Probe
A readiness probe controls traffic. Failure removes the Pod from Service endpoints but does NOT restart it.

1. Write a Pod manifest with nginx and a `readinessProbe` using `httpGet` on path `/` port `80`
2. Expose it as a Service: `kubectl expose pod <name> --port=80 --name=readiness-svc`
3. Check `kubectl get endpoints readiness-svc` — the Pod IP is listed
4. Break the probe: `kubectl exec <pod> -- rm /usr/share/nginx/html/index.html`
5. Wait 15 seconds — Pod shows `0/1` READY, endpoints are empty, but the container is NOT restarted

**Verify:** When readiness failed, was the container restarted?

---

### Task 6: Startup Probe
A startup probe gives slow-starting containers extra time. While it runs, liveness and readiness probes are disabled.

1. Write a Pod manifest where the container takes 20 seconds to start (e.g., `sleep 20 && touch /tmp/started`)
2. Add a `startupProbe` checking for `/tmp/started` with `periodSeconds: 5` and `failureThreshold: 12` (60 second budget)
3. Add a `livenessProbe` that checks the same file — it only kicks in after startup succeeds

**Verify:** What would happen if `failureThreshold` were 2 instead of 12?

---

### Task 7: Clean Up
Delete all pods and services you created.

---


'''
Nice — this set is exactly how Kubernetes behaves in real production. I’ll give you **clear Verify answers + reasoning** (this is how interviewers expect you to explain).

---

# ✅ Task 1 — Verify

**What QoS class does your Pod have?**

👉 **Answer: `Burstable`**

### Why?

* You set:

  * Requests ≠ Limits
* Example:

  * CPU: `100m` (request) vs `250m` (limit)
  * Memory: `128Mi` vs `256Mi`

✔ Rule:

* **Guaranteed** → requests == limits
* **Burstable** → requests < limits
* **BestEffort** → no requests/limits

---

# ✅ Task 2 — Verify

**What exit code does an OOMKilled container have?**

👉 **Answer: `137`**

### Why?

* OOM kill = SIGKILL (signal 9)
* Formula:

  ```
  128 + 9 = 137
  ```

✔ In `kubectl describe pod`:

```text
Reason: OOMKilled
Exit Code: 137
```

👉 Key concept:

* CPU → throttled
* Memory → **killed immediately**

---

# ✅ Task 3 — Verify

**What event message does the scheduler produce?**

👉 **Answer (typical message):**

```text
0/3 nodes are available: insufficient cpu, insufficient memory.
```

Or more detailed:

```text
pod has unbound immediate PersistentVolumeClaims (if PVC issue)
```

But in your case:

```text
FailedScheduling: 0/3 nodes are available: insufficient cpu, insufficient memory
```

✔ Reason:

* You requested:

  * `cpu: 100` (100 cores ❌)
  * `memory: 128Gi` (very high ❌)

👉 Scheduler cannot find any node → Pod stays **Pending**

---

# ✅ Task 4 — Verify

**How many times has the container restarted?**

👉 **Answer: It keeps increasing (typically 1, 2, 3… loop)**

### What happens:

1. `/tmp/healthy` exists → probe passes ✅
2. After 30 sec → file deleted ❌
3. Liveness probe fails:

   * Every 5 sec
   * After 3 failures → restart

👉 Restart cycle:

```
Fail → Fail → Fail → Restart
```

✔ So restart count:

```text
RESTARTS: 1 → 2 → 3 → ...
```

👉 Important:

* Liveness = **self-healing mechanism**

---

# ✅ Task 5 — Verify

**When readiness failed, was the container restarted?**

👉 **Answer: NO ❌**

### What actually happens:

* Pod status:

  ```text
  READY: 0/1
  ```
* Service endpoints:

  ```text
  <none>
  ```
* Container:
  ✅ Still running

👉 Key difference:

| Probe     | Action              |
| --------- | ------------------- |
| Liveness  | Restart container   |
| Readiness | Remove from traffic |

✔ So:

* No restart
* Only traffic is stopped

---

# ✅ Task 6 — Verify

**What happens if `failureThreshold = 2` instead of 12?**

👉 **Answer: The container will restart too early ❌**

### Why?

* Startup probe budget:

  ```
  periodSeconds × failureThreshold
  ```

### Your correct config:

```
5 sec × 12 = 60 sec (safe)
```

### If changed:

```
5 sec × 2 = 10 sec ❌
```

👉 But app needs **20 sec to start**

### Result:

* Probe fails before startup completes
* Kubernetes thinks container is broken
* Container gets restarted repeatedly

👉 This causes:

* CrashLoopBackOff 🔥

---

# ✅ Task 7 — Cleanup

```bash
kubectl delete pod --all
kubectl delete svc --all
```

---

# 🧠 Final Mental Model (VERY IMPORTANT)

| Concept         | Behavior                     |
| --------------- | ---------------------------- |
| Requests        | Scheduling guarantee         |
| Limits          | Runtime enforcement          |
| OOMKilled       | Memory exceeded → kill (137) |
| Liveness Probe  | Restart container            |
| Readiness Probe | Stop traffic only            |
| Startup Probe   | Protect slow apps            |
| Pending Pod     | Scheduler can't place        |

---

# 🔥 Real Interview Tips

If interviewer asks:

👉 “Why use startup probe?”
You say:

> To prevent liveness probe from killing slow-starting applications.

👉 “Difference between liveness & readiness?”
You say:

> Liveness restarts containers; readiness only controls traffic.

👉 “Why OOMKilled happens?”
You say:

> Container exceeded memory limit; kernel sends SIGKILL (exit 137).

---

If you want next level:

* 🔧 I can give **real YAMLs for all 3 probes combined**
* 🧪 Debugging **CrashLoopBackOff scenarios**
* 🚀 Production **resource tuning strategy**

Just tell me 👍

'''
