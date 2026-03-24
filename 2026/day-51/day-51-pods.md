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


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/60932914-739d-4d92-b42d-c5f5e7be3561" />



  482  kubectl get nodes


  485  kubectl run test-pod --image=nginx --dry-run=client -o yaml

  487  cat nginx-pod.yml
  488  kubectl run test-pod --image=nginx
  489  kubectl get po

  491  kubectl logs test-pod
  492  docker ps

  494  docker exec -it kind-worker2 bash
 
  498  kubectl describe po test-pod
  499  docker pull nginx

  504  kubectl exec -it test-pod -- bash
  505  vi busybox.yaml
  506  kubectl apply -f busybox.yaml


  509  cat busybox.yaml
  510  docker pull busybox
  511  kubectl get po -w

  515  kubectl exec -it busybox-pod -- bash
  516  kubectl get po -w
  517  kubectl exec -it busybox-pod -- sh
  518  kubectl logs busybox-pod
  
  520  # Create a pod without a YAML file
  521  kubectl run redis-pod --image=redis:latest
  522  kubectl get po
  523  docker pull redis

  525  kubectl get pod redis-pod -o yaml

  532  kubectl apply -f nginx-pod.yml --dry-run=server

  534  kubectl get pods -l app=test-pod
  535  kubectl get pods -l run=test-pod
  536  kubectl get pods -l environment=dev
  537  kubectl lable pod test-pod run=test-pod

  539  kubectl label pod test-pod app=nginxpod
  540  kubectl get po

  542  kubectl get po --show-labels
  543  kubectl label pod test-pod run-

  545  kubectl get all
  546  history
  547  history | awk '{print $2}' | sort | uniq -c | sort -rn
'''
