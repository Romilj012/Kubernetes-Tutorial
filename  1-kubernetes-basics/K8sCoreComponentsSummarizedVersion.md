# Kubernetes Core Concepts Overview

## Pod
- **Smallest unit** of Kubernetes (K8s)
- **Abstraction over container(s)**
- **1 application per Pod** (typical use case)
  - Can run multiple containers (e.g., main app + helper containers)
- **Each Pod gets unique IP address**
- Lifecycle considerations:
  - Pods are ephemeral (can crash/restart)
  - New IP assigned after restart → requires Service component

![Pod Diagram](k8s-basics-images/image-1.png)

## Service
- **Static IP address** attached to Pod(s)
- **Decoupled lifecycle** from Pods
- Two main types:
  - **External Service**: Public-facing (browser accessible)
  - **Internal Service**: Private (e.g., database access)
- Acts as load balancer for Pod replicas

![Service Diagram](k8s-basics-images/image.png)
![Service Types](k8s-basics-images/image-2.png)

## Ingress
- **Route external requests** to internal services
- Enables:
  - **Custom domains** (e.g., `https://my-app.com`)
  - **SSL termination**
  - **Path-based routing**

![Ingress Diagram](k8s-basics-images/image-3.png)

## Configuration Management
### ConfigMap
- **External configuration** store
- Stores non-sensitive data:
  - Database URLs
  - Service endpoints
- Avoids need to rebuild k8s-basics-images/images for config changes

![ConfigMap Diagram](k8s-basics-images/image-4.png)
![ConfigMap Usage](k8s-basics-images/image-5.png)

### Secret
- **Secure credential storage**
- Stores sensitive data:
  - Passwords
  - API keys
  - Certificates
- **Base64 encoded** (not encrypted!)

![Secret Diagram](k8s-basics-images/image-6.png)

## Data Persistence
### Volumes
- **Persistent storage** solution
- Types:
  - Local storage (node-specific)
  - Remote storage (cloud/NAS)
- Essential for stateful applications (e.g., databases)

![Volume Problem](k8s-basics-images/image-7.png)
![Volume Solution](k8s-basics-images/image-8.png)

## Scaling & High Availability
### Deployment
- **Pod blueprint** for stateless apps
- Features:
  - Replication
  - Rolling updates
  - Self-healing
- Manages Pod replicas behind Service

![Deployment Diagram](k8s-basics-images/image-9.png)
![Deployment Scaling](k8s-basics-images/image-10.png)

### StatefulSet
- **Database-specific** management
- Handles:
  - Ordered deployments
  - Stable network identifiers
  - Persistent storage
- Complex to manage → often hosted externally

![StatefulSet Diagram](k8s-basics-images/image-11.png)
![High Availability](k8s-basics-images/image-12.png)

## Architecture Summary
1. **Pods & Services**: Basic communication units
2. **Ingress**: External traffic routing
3. **ConfigMap/Secrets**: External configuration
4. **Volumes**: Data persistence
5. **Deployments/StatefulSets**: Scaling & reliability

> **Key Takeaway**: Use Deployments for stateless apps, StatefulSets for databases, and always pair with Services for stable networking.