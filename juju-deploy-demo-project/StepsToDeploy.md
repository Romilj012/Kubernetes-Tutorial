Here's a step-by-step guide to **use Juju with Minikube** (without MicroK8s) to deploy applications like WordPress:

---

### **Step 1: Install Minikube**
Install Minikube and start a Kubernetes cluster:
```bash
# Install Minikube (macOS)
brew install minikube

# Start Minikube with Docker driver (lightweight)
minikube start --driver=docker
```

---

### **Step 2: Configure `kubectl`**
Ensure `kubectl` points to Minikube:
```bash
kubectl config use-context minikube
```

Verify the cluster is running:
```bash
kubectl cluster-info
# Should show Minikube API server is running
```

---

### **Step 3: Install Juju**
Install Juju using Homebrew:
```bash
brew install juju
```

---

### **Step 4: Bootstrap Juju to Minikube**
Tell Juju to use Minikube’s Kubernetes cluster:
```bash
juju bootstrap minikube
```
This creates a Juju controller (management layer) on your MicroK8s cluster.

---

### **Step 5: Deploy Applications with Juju Charms**
Deploy WordPress and MySQL:
```bash
# Create a Juju model (workspace)
juju add-model demo

# Deploy WordPress and MySQL
juju deploy wordpress
juju deploy mysql

# Connect WordPress to MySQL
juju relate wordpress mysql

# Expose WordPress to the network
juju expose wordpress
```

---

### **Step 6: Get the WordPress URL**
Find the IP/port to access WordPress:
```bash
juju status
```
Look for the `wordpress` unit’s IP and port under `Exposed endpoints`.

---

### **Step 7: Access WordPress**
If Minikube uses `NodePort`, forward the port:
```bash
# Get the NodePort of the WordPress service
kubectl get svc wordpress -o jsonpath='{.spec.ports[0].nodePort}'

# Port-forward to localhost
kubectl port-forward svc/wordpress 8080:80
```
Open `http://localhost:8080` in your browser.

---

### **Cleanup**
Delete everything when done:
```bash
# Destroy the Juju model
juju destroy-model demo --destroy-storage --force

# Remove the Juju controller
juju destroy-controller minikube --destroy-all-models --force

# Stop Minikube
minikube stop
```

---

### **Troubleshooting**
#### **1. "Connection refused" errors**:
   - Ensure Minikube is running:
     ```bash
     minikube status
     ```
   - Restart Minikube:
     ```bash
     minikube stop && minikube start
     ```

#### **2. Juju fails to bootstrap**:
   - Reset Minikube’s Kubernetes context:
     ```bash
     minikube update-context
     ```

#### **3. Exposing WordPress**:
   If `juju expose` doesn’t work, use Minikube’s ingress:
   ```bash
   minikube addons enable ingress
   minikube service wordpress --url
   ```

---

### **Key Commands Summary**
| Command | Purpose |
|---------|---------|
| `minikube start --driver=docker` | Start Minikube cluster |
| `juju bootstrap minikube` | Link Juju to Minikube |
| `juju deploy wordpress` | Deploy WordPress |
| `juju relate wordpress mysql` | Connect WordPress to MySQL |
| `kubectl port-forward svc/wordpress 8080:80` | Access WordPress locally |

---

This setup lets you use Juju with Minikube instead of MicroK8s. Let me know if you hit any snags! 🛠️