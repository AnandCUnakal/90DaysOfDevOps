---

## Challenge Tasks

### Task 1: Create the Namespace (Day 52)
1. Create a `capstone` namespace
2. Set it as your default: `kubectl config set-context --current --namespace=capstone`
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5df1dfa7-70c2-4b6f-bcbe-6289f8571efe" />

---

### Task 2: Deploy MySQL (Days 54-56)
1. Create a Secret with `MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`, `MYSQL_USER`, and `MYSQL_PASSWORD` using `stringData`
2. Create a Headless Service (`clusterIP: None`) for MySQL on port 3306
3. Create a StatefulSet for MySQL with:
   - Image: `mysql:8.0`
   - `envFrom` referencing the Secret
   - Resource requests (cpu: 250m, memory: 512Mi) and limits (cpu: 500m, memory: 1Gi)
   - A `volumeClaimTemplates` section requesting 1Gi of storage, mounted at `/var/lib/mysql`
4. Verify MySQL works: `kubectl exec -it mysql-0 -- mysql -u <user> -p<password> -e "SHOW DATABASES;"`
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/42b2fe92-2184-4e43-ab52-b25ecc55785a" />

**Verify:** Can you see the `wordpress` database?

---

### Task 3: Deploy WordPress (Days 52, 54, 57)
1. Create a ConfigMap with `WORDPRESS_DB_HOST` set to `mysql-0.mysql.capstone.svc.cluster.local:3306` and `WORDPRESS_DB_NAME`
2. Create a Deployment with 2 replicas using `wordpress:latest` that:
   - Uses `envFrom` for the ConfigMap
   - Uses `secretKeyRef` for `WORDPRESS_DB_USER` and `WORDPRESS_DB_PASSWORD` from the MySQL Secret
   - Has resource requests and limits
   - Has a liveness probe and readiness probe on `/wp-login.php` port 80
3. Wait until both pods show `1/1 Running`

**Verify:** Are both WordPress pods running and ready?
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8230e353-49ee-4d60-a09e-c6cd711cca96" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b8439fbd-b633-4d2a-9c8b-658b8979f9cc" />

---

### Task 4: Expose WordPress (Day 53)
1. Create a NodePort Service on port 30080 targeting the WordPress pods
2. Access WordPress in your browser:
   - Minikube: `minikube service wordpress -n capstone`
   - Kind: `kubectl port-forward svc/wordpress 8080:80 -n capstone`
3. Complete the setup wizard and create a blog post

**Verify:** Can you see the WordPress setup page?
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/56229231-1cf3-445c-ac19-cdf7b69bb49f" />

---

### Task 5: Test Self-Healing and Persistence
1. Delete a WordPress pod — watch the Deployment recreate it within seconds. Refresh the site.
2. Delete the MySQL pod: `kubectl delete pod mysql-0 -n capstone` — watch the StatefulSet recreate it
3. After MySQL recovers, refresh WordPress — your blog post should still be there

**Verify:** After deleting both pods, is your blog post still there?
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/47330132-38fc-4c01-bb68-ab7dae6977b3" />

---

### Task 6: Set Up HPA (Day 58)
1. Write an HPA manifest targeting the WordPress Deployment with CPU at 50%, min 2, max 10 replicas
2. Apply and check: `kubectl get hpa -n capstone`
3. Run `kubectl get all -n capstone` for the complete picture

**Verify:** Does the HPA show correct min/max and target?

---

### Task 7: (Bonus) Compare with Helm (Day 59)
1. Install WordPress using `helm install wp-helm bitnami/wordpress` in a separate namespace
2. Compare: how many resources did each approach create? Which gives more control?
3. Clean up the Helm deployment

---

### Task 8: Clean Up and Reflect
1. Take a final look: `kubectl get all -n capstone`
2. Count the concepts you used: Namespace, Secret, ConfigMap, PVC, StatefulSet, Headless Service, Deployment, NodePort Service, Resource Limits, Probes, HPA, Helm — twelve concepts in one deployment
3. Delete the namespace: `kubectl delete namespace capstone`
4. Reset default: `kubectl config set-context --current --namespace=default`

**Verify:** Did deleting the namespace remove everything?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c006922b-96cc-4318-ace4-b7a6f30e521c" />


---

'''
This is your **Day 60 Capstone** — the real DevOps milestone. I’ll give you:

1. ✅ All Kubernetes manifests
2. ✅ Commands to verify each step
3. ✅ A ready-to-submit `day-60-capstone.md` file

---

# 🚀 Step-by-Step Implementation

---

## 🔹 Task 1: Namespace

```bash
kubectl create namespace capstone
kubectl config set-context --current --namespace=capstone
```

---

## 🔹 Task 2: MySQL Setup

### ✅ Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
stringData:
  MYSQL_ROOT_PASSWORD: rootpass
  MYSQL_DATABASE: wordpress
  MYSQL_USER: wpuser
  MYSQL_PASSWORD: wppass
```

---

### ✅ Headless Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
    - port: 3306
```

---

### ✅ StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          envFrom:
            - secretRef:
                name: mysql-secret
          ports:
            - containerPort: 3306
          resources:
            requests:
              cpu: "250m"
              memory: "512Mi"
            limits:
              cpu: "500m"
              memory: "1Gi"
          volumeMounts:
            - name: mysql-data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

---

### ✅ Verify MySQL

```bash
kubectl exec -it mysql-0 -- mysql -u wpuser -pwppass -e "SHOW DATABASES;"
```

✔ You should see `wordpress`

---

## 🔹 Task 3: WordPress

---

### ✅ ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: wordpress-config
data:
  WORDPRESS_DB_HOST: mysql-0.mysql.capstone.svc.cluster.local:3306
  WORDPRESS_DB_NAME: wordpress
```

---

### ✅ Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
        - name: wordpress
          image: wordpress:latest
          envFrom:
            - configMapRef:
                name: wordpress-config
          env:
            - name: WORDPRESS_DB_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: MYSQL_USER
            - name: WORDPRESS_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: MYSQL_PASSWORD
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "200m"
              memory: "256Mi"
            limits:
              cpu: "500m"
              memory: "512Mi"
          readinessProbe:
            httpGet:
              path: /wp-login.php
              port: 80
            initialDelaySeconds: 10
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /wp-login.php
              port: 80
            initialDelaySeconds: 30
            periodSeconds: 10
```

---

### ✅ Verify

```bash
kubectl get pods
```

✔ Both pods → `1/1 Running`

---

## 🔹 Task 4: Expose WordPress

```yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress
spec:
  type: NodePort
  selector:
    app: wordpress
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

---

### ✅ Access

**Minikube:**

```bash
minikube service wordpress -n capstone
```

**Kind:**

```bash
kubectl port-forward svc/wordpress 8080:80 -n capstone
```

👉 Open: `http://localhost:8080`

✔ Complete WordPress setup

---

## 🔹 Task 5: Self-Healing & Persistence

```bash
kubectl delete pod <wordpress-pod>
kubectl delete pod mysql-0
```

✔ Verify:

* Pods auto-recreated
* Blog still exists ✅

---

## 🔹 Task 6: HPA

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: wordpress-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: wordpress
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

---

### ✅ Verify

```bash
kubectl get hpa
kubectl get all
```

---

## 🔹 Task 7: Helm (Bonus)

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install wp-helm bitnami/wordpress -n helm-test --create-namespace
```

Compare:

* Helm creates MANY resources automatically
* YAML gives **full control**

Cleanup:

```bash
helm uninstall wp-helm -n helm-test
kubectl delete namespace helm-test
```

---

## 🔹 Task 8: Cleanup

```bash
kubectl get all -n capstone
kubectl delete namespace capstone
kubectl config set-context --current --namespace=default
```

✔ Everything removed ✅

---

# 📄 Final Submission File

# Day 60 Capstone – Kubernetes WordPress Deployment

## Overview

Deployed a complete WordPress + MySQL application using Kubernetes core concepts.

## Concepts Used

* Namespace
* Secret
* ConfigMap
* PVC
* StatefulSet
* Headless Service
* Deployment
* NodePort Service
* Resource Limits
* Probes
* HPA
* Helm

## Implementation Summary

### MySQL

* StatefulSet with persistent storage
* Headless service for stable DNS
* Secrets for credentials

### WordPress

* Deployment with 2 replicas
* ConfigMap + Secret integration
* Liveness & readiness probes

### Exposure

* NodePort service for external access

### Scaling

* HPA configured (CPU 50%, min=2, max=10)

## Verification

### Self-Healing

* Deleted pods → auto recreated

### Persistence

* MySQL data retained after restart

### Scaling

* HPA visible and configured correctly

## Helm Comparison

* Helm created more resources automatically
* Manual YAML gives better control and learning

## Screenshots

* WordPress UI
* kubectl get all -n capstone

## Conclusion

Successfully deployed a production-like application using 12 Kubernetes concepts, validating real-world readiness.

---

# 🎯 What You Achieved

This is not just a lab anymore.

You now know how to:

* Deploy **stateful + stateless apps together**
* Handle **real production architecture**
* Use **every major Kubernetes primitive**

---

If you want next level:
👉 I can help you convert this into:

* CI/CD pipeline (GitLab/Jenkins)
* Helm chart from scratch
* Production-grade architecture (Ingress + TLS + DB backup)

Just tell me 👍

'''
