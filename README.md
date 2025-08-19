# kubernetes-nginx-minikube
Set up a local Kubernetes cluster using Minikube + Docker, deploy an NGINX application 

Things I Did :

- Installed Minikube and Docker Desktop
- Set up Minikube cluster locally using Docker as the container runtime
- Installed and configured `kubectl`
- Wrote a Kubernetes YAML manifest (`Deployment + Service`) for NGINX and created 2 identical replicas.
- Deployed it using `kubectl apply`
- Exposed the service via `NodePort` and accessed the app in browser
- Resolved a real-world Kubernetes error (immutable field: `spec.selector`)
- Documented the learning experience as a DevOps practice project

---

##  Technologies Used :

- Kubernetes (v1.33.1 via Minikube)
- Docker Desktop (for container runtime)
- kubectl (Kubernetes CLI)
- wrote code and stored it as a YAML file in the local machine  and ran commands in VScode
- Git Bash 

---

##  Commands Used :

```bash
# Start the cluster
minikube start --driver=docker

# Check cluster health
minikube status
kubectl get nodes

# Apply Deployment + Service
kubectl apply -f nginx-deployment.yaml

# Fix immutable selector issue
kubectl delete deployment my-nginx
kubectl apply -f nginx-deployment.yaml

# Access the app
minikube service my-nginx
kubectl get svc



nginx-deployment.yaml file

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  replicas: 2
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
          image: nginx:latest
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080

Result

App successfully deployed and accessed at:

http://127.0.0.1:30080


or via forwarded port:

minikube service my-nginx
# -> http://127.0.0.1:63638 (auto port forwarding)

