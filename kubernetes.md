# Kubernetes Teaching Notes

## 1. What is Kubernetes?

Kubernetes (often called **K8s**) is an **open-source system** for **automating** the **deployment**, **scaling**, and **management** of containerized applications.

**Analogy:**  
> Docker creates containers. Kubernetes is like a **container orchestra conductor**, telling containers when and where to run.

---

## 2. Why Use Kubernetes?

- **Automated Deployment**: No need to manually start/stop containers.
- **Scaling**: Automatically adjust the number of containers based on traffic.
- **Load Balancing**: Distribute traffic across containers.
- **Self-healing**: Restart containers if they crash.
- **Declarative Configuration**: Manage everything via YAML files.

---

## 3. Key Kubernetes Concepts

| Concept | Meaning |
|:--------|:--------|
| **Cluster** | Group of machines running Kubernetes (1 Master + Worker nodes) |
| **Node** | Single machine (VM or physical) inside the cluster |
| **Pod** | Smallest deployable unit — a wrapper for one or more containers |
| **Deployment** | Controller that manages Pods — makes sure the correct number are running |
| **Service** | Exposes Pods as a network service (IP or DNS) |
| **Namespace** | Virtual cluster inside Kubernetes — for organizing resources |

---

## 4. Basic Kubernetes Architecture

```plaintext
[ Client (kubectl) ] ---> [ Master Node (Control Plane) ] ---> [ Worker Nodes ] ---> [ Pods (containers) ]
```

- **kubectl**: CLI to talk to Kubernetes cluster.
- **Master Node**: Makes decisions (scheduling, scaling).
- **Worker Nodes**: Actually run the containers inside Pods.

---

## 5. Basic Kubernetes Workflow

### Step 1: Define your app in a YAML file

Example: `deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: myusername/myapp:latest
        ports:
        - containerPort: 8080
```

---

### Step 2: Apply the YAML to the cluster
```bash
kubectl apply -f deployment.yaml
```

---

### Step 3: Create a Service to expose your app
Example: `service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

```bash
kubectl apply -f service.yaml
```

---

## 6. Important kubectl Commands

| Command | Description |
|:--------|:------------|
| `kubectl get pods` | List all Pods |
| `kubectl get deployments` | List all Deployments |
| `kubectl get services` | List all Services |
| `kubectl describe pod pod_name` | Detailed info about a Pod |
| `kubectl logs pod_name` | View logs from a Pod |
| `kubectl delete pod pod_name` | Delete a Pod |
| `kubectl apply -f file.yaml` | Apply YAML config to the cluster |
| `kubectl delete -f file.yaml` | Delete resources defined in YAML |

---

## 7. Practical Demo

### Run your first Kubernetes Deployment
```bash
kubectl create deployment nginx-deployment --image=nginx
```

---

### Scale the Deployment (increase pods)
```bash
kubectl scale deployment nginx-deployment --replicas=5
```

---

### Expose Deployment as a Service
```bash
kubectl expose deployment nginx-deployment --type=LoadBalancer --port=80
```

---

### Check your running resources
```bash
kubectl get all
```

---

### Delete everything
```bash
kubectl delete service nginx-deployment
kubectl delete deployment nginx-deployment
```

---

## 8. Quick Tips

- **Always use YAML files** for production — more reliable than CLI commands.
- **Pods are short-lived** — they can die anytime. Always use Deployments to manage Pods.
- **Services** are the stable network endpoints for your apps.

---

## 🎯 Key Takeaways

- **Kubernetes = container orchestration** — managing containers automatically.
- **Pod = smallest unit** (contains container(s)).
- **Deployment = manages Pods** for high availability and scaling.
- **Service = exposes Pods** to the outside world.

---