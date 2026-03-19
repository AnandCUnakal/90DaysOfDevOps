Task 1: Deploy the Application
First, create a Deployment that you will expose with Services. Create app-deployment.yaml:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
kubectl apply -f app-deployment.yaml
kubectl get pods -o wide
Note the individual Pod IPs. These will change if pods restart — that is the problem Services fix.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/398dbf19-8ab5-4350-a7f3-e282c60cb748" />

Verify: Are all 3 pods running? Note down their IP addresses.

Task 2: ClusterIP Service (Internal Access)
ClusterIP is the default Service type. It gives your Pods a stable internal IP that is only reachable from within the cluster.

Create clusterip-service.yaml:

apiVersion: v1
kind: Service
metadata:
  name: web-app-clusterip
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80

Key fields:
selector.app: web-app — this Service routes traffic to all Pods with the label app: web-app
port: 80 — the port the Service listens on
targetPort: 80 — the port on the Pod to forward traffic to
**kubectl apply -f clusterip-service.yaml
kubectl get services**
You should see web-app-clusterip with a CLUSTER-IP address. This IP is stable — it will not change even if Pods restart.

Now test it from inside the cluster:

# Run a temporary pod to test connectivity
kubectl run test-client --image=busybox:latest --rm -it --restart=Never -- sh

# Inside the test pod, run:
wget -qO- http://web-app-clusterip
exit
You should see the Nginx welcome page. The Service load-balanced your request to one of the 3 Pods.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/29f125dc-5d8a-4947-b079-2f8230a3bc2e" />


Verify: Does the Service respond? Try running the wget command multiple times — the Service distributes traffic across all healthy Pods.

Task 3: Discover Services with DNS
Kubernetes has a built-in DNS server. Every Service gets a DNS entry automatically:

Ex: 
anand@DESKTOP-H50FBNH MINGW64 /d/ANDYtws/twstask/kubernetes/day53
$ kubectl get svc
NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes          ClusterIP   10.96.0.1      <none>        443/TCP   4d5h
web-app-clusterip   ClusterIP   10.96.252.96   <none>        80/TCP    3m16s



<service-name>.<namespace>.svc.cluster.local
Test this:

web-app-clusterip.default.svc.cluster.local

kubectl run dns-test --image=busybox:latest --rm -it --restart=Never -- sh

# Inside the pod:
# Short name (works within the same namespace)
wget -qO- http://web-app-clusterip

# Full DNS name
wget -qO- http://web-app-clusterip.default.svc.cluster.local

Ex: 
wget -qO- http://web-app-clusterip.default.svc.cluster.local
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/095384c3-1999-49ed-b0c5-b9f72e6e19c8" />

# Look up the DNS entry
nslookup web-app-clusterip
exit
Both the short name and the full DNS name resolve to the same ClusterIP. In practice, you use the short name when communicating within the same namespace and the full name when reaching across namespaces.

Verify: What IP does nslookup return? Does it match the CLUSTER-IP from kubectl get services?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c48d176-63cd-4838-917e-2004c8d34fd1" />


Task 4: NodePort Service (External Access via Node)
A NodePort Service exposes your application on a port on every node in the cluster. This lets you access the Service from outside the cluster.

Create nodeport-service.yaml:

apiVersion: v1
kind: Service
metadata:
  name: web-app-nodeport
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
nodePort: 30080 — the port opened on every node (must be in range 30000-32767)
Traffic flow: <NodeIP>:30080 -> Service -> Pod:80
kubectl apply -f nodeport-service.yaml
kubectl get services
Access the service:

# If using Minikube
minikube service web-app-nodeport --url

# If using Kind, get the node IP first
kubectl get nodes -o wide
# Then curl <node-internal-ip>:30080

# If using Docker Desktop
curl http://localhost:30080
Verify: Can you see the Nginx welcome page from your browser or terminal using the NodePort?

Task 5: LoadBalancer Service (Cloud External Access)
In a cloud environment (AWS, GCP, Azure), a LoadBalancer Service provisions a real external load balancer that routes traffic to your nodes.

Create loadbalancer-service.yaml:

apiVersion: v1
kind: Service
metadata:
  name: web-app-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
kubectl apply -f loadbalancer-service.yaml
kubectl get services
On a local cluster (Minikube, Kind, Docker Desktop), the EXTERNAL-IP will show <pending> because there is no cloud provider to create a real load balancer. This is expected.

If you are using Minikube:

# Minikube can simulate a LoadBalancer
minikube tunnel
# In another terminal, check again:
kubectl get services
In a real cloud cluster, the EXTERNAL-IP would be a public IP address or hostname provisioned by the cloud provider.

Verify: What does the EXTERNAL-IP column show? Why is it <pending> on a local cluster?

Task 6: Understand the Service Types Side by Side
Check all three services:

kubectl get services -o wide
Compare them:

Type	Accessible From	Use Case
ClusterIP	Inside the cluster only	Internal communication between services
NodePort	Outside via <NodeIP>:<NodePort>	Development, testing, direct node access
LoadBalancer	Outside via cloud load balancer	Production traffic in cloud environments
Each type builds on the previous one:

LoadBalancer creates a NodePort, which creates a ClusterIP
So a LoadBalancer service also has a ClusterIP and a NodePort
Verify this:

kubectl describe service web-app-loadbalancer
You should see all three: a ClusterIP, a NodePort, and the LoadBalancer configuration.

Verify: Does the LoadBalancer service also have a ClusterIP and NodePort assigned?

Task 7: Clean Up
kubectl delete -f app-deployment.yaml
kubectl delete -f clusterip-service.yaml
kubectl delete -f nodeport-service.yaml
kubectl delete -f loadbalancer-service.yaml

kubectl get pods
kubectl get services
Only the built-in kubernetes service in the default namespace should remain.

Verify: Is everything cleaned up?


'''
Excellent—this is where Kubernetes **clicks** 🔥
I’ll walk through each task with **what you should see + exact verification answers**.

---

# ✅ Task 1: Deploy the Application

After:

```bash
kubectl apply -f app-deployment.yaml
kubectl get pods -o wide
```

### Expected Output (example)

```text
NAME                         READY   STATUS    IP           NODE
web-app-xxxxx-abcde          1/1     Running   10.244.0.5   node1
web-app-xxxxx-fghij          1/1     Running   10.244.0.6   node1
web-app-xxxxx-klmno          1/1     Running   10.244.0.7   node1
```

---

### ✅ Verify Answer

✔ Are all 3 pods running?

```text
YES (3/3 Running)
```

✔ Pod IPs:

```text
Each pod has a DIFFERENT IP
```

👉 Important:

```text
Pod IPs are dynamic → change on restart
```

---

# ✅ Task 2: ClusterIP Service

After:

```bash
kubectl apply -f clusterip-service.yaml
kubectl get services
```

### Output

```text
NAME                 TYPE        CLUSTER-IP     PORT(S)
web-app-clusterip    ClusterIP   10.96.x.x      80/TCP
```

---

### Test inside cluster

```bash
kubectl run test-client --image=busybox --rm -it --restart=Never -- sh
```

Inside:

```sh
wget -qO- http://web-app-clusterip
```

---

### ✅ Verify Answer

✔ Does service respond?

```text
YES → Nginx welcome page
```

✔ Load balancing?

```text
YES → Requests hit different pods
```

👉 Kubernetes uses **round-robin load balancing**

---

# ✅ Task 3: DNS Discovery

Inside test pod:

```sh
nslookup web-app-clusterip
```

---

### Output

```text
Name: web-app-clusterip
Address: 10.96.x.x
```

---

### ✅ Verify Answer

✔ Does DNS match ClusterIP?

```text
YES → nslookup IP = CLUSTER-IP
```

---

### Key Insight

```text
Service Name = DNS Name
```

---

# ✅ Task 4: NodePort Service

After:

```bash
kubectl apply -f nodeport-service.yaml
kubectl get services
```

---

### Output

```text
NAME               TYPE       CLUSTER-IP     PORT(S)
web-app-nodeport   NodePort   10.96.x.x      80:30080/TCP
```

---

### Access

#### Docker Desktop:

```bash
curl http://localhost:30080
```

#### Minikube:

```bash
minikube service web-app-nodeport --url
```

---

### ✅ Verify Answer

✔ Can you access app?

```text
YES → Nginx welcome page visible
```

---

### Traffic Flow

```text
Client → NodeIP:30080 → Service → Pod:80
```

---

# ✅ Task 5: LoadBalancer Service

After:

```bash
kubectl apply -f loadbalancer-service.yaml
kubectl get services
```

---

### Output (local cluster)

```text
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)
web-app-loadbalancer     LoadBalancer   10.96.x.x      <pending>     80:xxxxx/TCP
```

---

### ✅ Verify Answer

✔ EXTERNAL-IP shows:

```text
<pending>
```

✔ Why?

```text
No cloud provider → cannot create real load balancer
```

---

### If using Minikube:

```bash
minikube tunnel
```

Then:

```text
EXTERNAL-IP gets assigned
```

---

# ✅ Task 6: Compare Services

Run:

```bash
kubectl get services -o wide
```

---

### Table (Important)

| Type         | Access                | Use Case           |
| ------------ | --------------------- | ------------------ |
| ClusterIP    | Internal only         | Service-to-service |
| NodePort     | External via node     | Dev/testing        |
| LoadBalancer | External via cloud LB | Production         |

---

### Verify LoadBalancer internals

```bash
kubectl describe service web-app-loadbalancer
```

---

### ✅ Verify Answer

✔ Does it include all?

```text
YES:
- ClusterIP ✔
- NodePort ✔
- LoadBalancer config ✔
```

---

### Key Insight

```text
LoadBalancer → builds on NodePort → builds on ClusterIP
```

---

# ✅ Task 7: Cleanup

Run:

```bash
kubectl delete -f app-deployment.yaml
kubectl delete -f clusterip-service.yaml
kubectl delete -f nodeport-service.yaml
kubectl delete -f loadbalancer-service.yaml
```

---

### Verify

```bash
kubectl get pods
kubectl get services
```

---

### Expected Output

```text
No resources found
```

Services:

```text
kubernetes (default service only)
```

---

### ✅ Final Verify Answer

✔ Is everything cleaned?

```text
YES (only default kubernetes service remains)
```

---

# 🔥 What You Just Learned (Very Important)

You now understand **core Kubernetes networking**:

```text
Pod → unstable IP
Service → stable access
DNS → service discovery
```

---

# 🧠 Real DevOps Mapping

What happens in production:

```text
User → LoadBalancer → NodePort → ClusterIP → Pod
```

---

# 💡 Interview Questions You Can Now Answer

### ❓ Why not use Pod IP directly?

```text
Pods are ephemeral → IP changes
```

---

### ❓ What does Service solve?

```text
Stable networking + load balancing
```

---

### ❓ Difference between ClusterIP & NodePort?

```text
ClusterIP → internal
NodePort → external via node
```

---

### ❓ Why LoadBalancer in cloud?

```text
Provides public entry point
```

---

# 🚀 Next Step (Critical)

Now you’re ready for:

```text
Ingress Controller (real production routing)
```

👉 This replaces NodePort/LoadBalancer with:

```text
domain → path → service
```

---

If you want, I’ll guide you next:

🔥 **Deploy your app with Ingress + domain-style routing (like real production systems)**

'''
