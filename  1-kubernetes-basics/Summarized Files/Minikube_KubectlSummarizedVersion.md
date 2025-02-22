# Local Kubernetes Development with Minikube

## Minikube Overview
- **Single-node Kubernetes cluster** for local development/testing
- **Combined master + worker node** in one machine
- Runs in **virtualized environment** (HyperKit/VirtualBox)
- Pre-configured with:
  - Docker container runtime
  - Core Kubernetes components
  - kubectl CLI tool

![Minikube Architecture](/%201-kubernetes-basics/k8s-basics-images/image-22.png)

### Production vs Local Cluster
| Feature          | Production Cluster       | Minikube Cluster         |
|------------------|--------------------------|--------------------------|
| Nodes            | Multiple masters/workers | Single node              |
| Resource Usage   | High                     | Optimized for local      |
| Setup Complexity | High                     | Single-command install   |
| Use Case         | Production workloads     | Development/Testing      |

---

## Key Components

### kubectl CLI Tool
- **Primary Kubernetes management interface**
- Communicates with **API Server** (cluster gateway)
- Common operations:
  - Cluster configuration
  - Pod management
  - Service creation
  - Debugging

![kubectl Workflow](/%201-kubernetes-basics/k8s-basics-images/image-25.png)

---

## Installation Guide

### Prerequisites
1. Install Hypervisor:
   ```bash
   brew update
   brew install hyperkit
   ```

2. Install Minikube (includes kubectl):
   ```bash
   brew install minikube
   ```

### Verification
Check installed tools:
```bash
kubectl version --client
minikube version
```

---

## Cluster Management

### Start Cluster
```bash
minikube start --vm-driver=hyperkit
```

### Check Cluster Status
```bash
minikube status
```
**Example Output:**
```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### Access Cluster Nodes
```bash
kubectl get nodes
```
**Expected Output:**
```
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   5m    v1.23.3
```

### Open Kubernetes Dashboard
```bash
minikube dashboard
```

![Cluster Dashboard](/%201-kubernetes-basics/k8s-basics-images/image-28.png)

---

## Essential Commands

### Minikube Commands
| Command                      | Description                          |
|------------------------------|--------------------------------------|
| `minikube start`             | Start the local Kubernetes cluster  |
| `minikube stop`              | Stop the running cluster            |
| `minikube pause`             | Freeze cluster state                |
| `minikube unpause`           | Resume cluster operations           |
| `minikube delete --all`      | Remove all cluster resources        |
| `minikube status`            | Check cluster status                |
| `minikube dashboard`         | Open Kubernetes dashboard           |
| `minikube ssh`               | SSH into the Minikube VM            |
| `minikube tunnel`            | Expose LoadBalancer services        |

### kubectl Commands
| Command                      | Description                          |
|------------------------------|--------------------------------------|
| `kubectl get pods -A`        | List all pods in all namespaces     |
| `kubectl get nodes`          | List cluster nodes                  |
| `kubectl cluster-info`       | Show cluster information            |
| `kubectl apply -f <file>`    | Deploy resources from YAML file     |
| `kubectl delete -f <file>`   | Delete resources from YAML file     |
| `kubectl describe pod <pod>` | Get detailed pod information        |
| `kubectl logs <pod>`         | View pod logs                       |
| `kubectl exec -it <pod> -- sh` | Open shell in a running pod        |

---

## Workflow Diagram
![Local Development Workflow](/%201-kubernetes-basics/k8s-basics-images/image-24.png)

---

## Example Workflow

1. Start Minikube Cluster:
   ```bash
   minikube start --vm-driver=hyperkit
   ```

2. Deploy a Sample Application:
   ```bash
   kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
   ```

3. Expose the Deployment:
   ```bash
   kubectl expose deployment hello-minikube --type=NodePort --port=8080
   ```

4. Access the Application:
   ```bash
   minikube service hello-minikube
   ```

5. Check Pod Status:
   ```bash
   kubectl get pods
   ```

6. View Application Logs:
   ```bash
   kubectl logs <pod-name>
   ```

7. Stop the Cluster:
   ```bash
   minikube stop
   ```

---

## Troubleshooting

### Common Issues
1. **Minikube Fails to Start**:
   - Check hypervisor logs:
     ```bash
     minikube logs
     ```
   - Increase resources:
     ```bash
     minikube start --memory=4096 --cpus=2
     ```

2. **kubectl Connection Issues**:
   - Verify cluster context:
     ```bash
     kubectl config get-contexts
     ```
   - Switch to Minikube context:
     ```bash
     kubectl config use-context minikube
     ```

---

> **Pro Tip:** Use `minikube tunnel` to access LoadBalancer services locally and `minikube ssh` to access the node directly.