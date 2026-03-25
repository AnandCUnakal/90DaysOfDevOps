---

## Challenge Tasks

### Task 1: Create a ConfigMap from Literals
1. Use `kubectl create configmap` with `--from-literal` to create a ConfigMap called `app-config` with keys `APP_ENV=production`, `APP_DEBUG=false`, and `APP_PORT=8080`
2. Inspect it with `kubectl describe configmap app-config` and `kubectl get configmap app-config -o yaml`
3. Notice the data is stored as plain text — no encoding, no encryption

**Verify:** Can you see all three key-value pairs?

---

### Task 2: Create a ConfigMap from a File
1. Write a custom Nginx config file that adds a `/health` endpoint returning "healthy"
2. Create a ConfigMap from this file using `kubectl create configmap nginx-config --from-file=default.conf=<your-file>`
3. The key name (`default.conf`) becomes the filename when mounted into a Pod

**Verify:** Does `kubectl get configmap nginx-config -o yaml` show the file contents?

---

### Task 3: Use ConfigMaps in a Pod
1. Write a Pod manifest that uses `envFrom` with `configMapRef` to inject all keys from `app-config` as environment variables. Use a busybox container that prints the values.
2. Write a second Pod manifest that mounts `nginx-config` as a volume at `/etc/nginx/conf.d`. Use the nginx image.
3. Test that the mounted config works: `kubectl exec <pod> -- curl -s http://localhost/health`

Use environment variables for simple key-value settings. Use volume mounts for full config files.

**Verify:** Does the `/health` endpoint respond?


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bfd621dd-fdac-407c-b3ab-e8689e803b77" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5c968ef0-5f9b-4e0e-9d99-eb678822f7a5" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3c9e961d-37e4-4aff-bd5e-57a2c527b48f" />

'''
Excellent—this is **real Kubernetes configuration management (ConfigMaps + Secrets)**. I’ll guide you with **working commands + YAML + exact verification answers**.

---

# ✅ Task 1: ConfigMap from Literals

### Create

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080
```

---

### Inspect

```bash
kubectl describe configmap app-config
kubectl get configmap app-config -o yaml
```

---

### ✅ Verify Answer

✔ Can you see values?

```text
APP_ENV=production
APP_DEBUG=false
APP_PORT=8080
```

✔ Stored as:

```text
Plain text (NOT encoded)
```

---

# ✅ Task 2: ConfigMap from File

### Create file

```bash
nano default.conf
```

```nginx
server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }

    location /health {
        return 200 'healthy';
        add_header Content-Type text/plain;
    }
}
```

---

### Create ConfigMap

```bash
kubectl create configmap nginx-config --from-file=default.conf=default.conf
```

---

### Inspect

```bash
kubectl get configmap nginx-config -o yaml
```

---

### ✅ Verify Answer

✔ Does it show file content?

```text
YES → entire nginx config visible under data:
```

---

# ✅ Task 3: Use ConfigMaps in Pods

---

## 🔹 Pod 1: Env variables (BusyBox)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-env
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "env && sleep 3600"]
    envFrom:
    - configMapRef:
        name: app-config
```

---

### Apply

```bash
kubectl apply -f busybox-env.yaml
kubectl logs busybox-env
```

✔ You should see:

```text
APP_ENV=production
APP_DEBUG=false
APP_PORT=8080
```

---

## 🔹 Pod 2: Mount ConfigMap (Nginx)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-config-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.25
    ports:
    - containerPort: 80
    volumeMounts:
    - name: nginx-config-volume
      mountPath: /etc/nginx/conf.d
  volumes:
  - name: nginx-config-volume
    configMap:
      name: nginx-config
```

---

### Test

```bash
kubectl apply -f nginx-config-pod.yaml
kubectl exec -it nginx-config-pod -- curl -s http://localhost/health
```

---

### ✅ Verify Answer

✔ Does `/health` work?

```text
YES → "healthy"
```
'''
---

### Task 4: Create a Secret
1. Use `kubectl create secret generic db-credentials` with `--from-literal` to store `DB_USER=admin` and `DB_PASSWORD=s3cureP@ssw0rd`
2. Inspect with `kubectl get secret db-credentials -o yaml` — the values are base64-encoded
3. Decode a value: `echo '<base64-value>' | base64 --decode`

**base64 is encoding, not encryption.** Anyone with cluster access can decode Secrets. The real advantages are RBAC separation, tmpfs storage on nodes, and optional encryption at rest.

**Verify:** Can you decode the password back to plaintext?

---

### Task 5: Use Secrets in a Pod
1. Write a Pod manifest that injects `DB_USER` as an environment variable using `secretKeyRef`
2. In the same Pod, mount the entire `db-credentials` Secret as a volume at `/etc/db-credentials` with `readOnly: true`
3. Verify: each Secret key becomes a file, and the content is the decoded plaintext value

**Verify:** Are the mounted file values plaintext or base64?

---

### Task 6: Update a ConfigMap and Observe Propagation
1. Create a ConfigMap `live-config` with a key `message=hello`
2. Write a Pod that mounts this ConfigMap as a volume and reads the file in a loop every 5 seconds
3. Update the ConfigMap: `kubectl patch configmap live-config --type merge -p '{"data":{"message":"world"}}'`
4. Wait 30-60 seconds — the volume-mounted value updates automatically
5. Environment variables from earlier tasks do NOT update — they are set at pod startup only

**Verify:** Did the volume-mounted value change without a pod restart?

---

### Task 7: Clean Up
Delete all pods, ConfigMaps, and Secrets you created.

---
