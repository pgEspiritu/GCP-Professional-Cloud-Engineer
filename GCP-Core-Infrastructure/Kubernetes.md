# ☸️ Introduction to Kubernetes

Kubernetes helps manage and scale **containerized applications**.  
You can bootstrap it using **Google Kubernetes Engine (GKE)** to save time and effort when scaling workloads.

---

## 🛠 What is Kubernetes?

- Kubernetes is an **open-source platform** for managing containerized workloads and services.  
- It **orchestrates many containers across many hosts**, scaling them as microservices.  
- Supports **rollouts and rollbacks** for deployments.  

**Core concept:** Kubernetes exposes a set of **APIs** to deploy containers on a set of nodes called a **cluster**.  

- **Control plane:** Runs the primary components  
- **Nodes:** Run containers (a node represents a computing instance; in GKE, nodes are VMs in Compute Engine)  

---

## 📦 Pods

- A **Pod** is the smallest deployable unit in Kubernetes.  
- Represents a **running process** in the cluster, either part of an app or the entire app.  
- Usually contains **one container**, but can contain multiple containers with a hard dependency.  
- Provides:
  - A **unique network IP**
  - Configurable **ports**
  - Options governing container behavior  

**Example commands:**

- Run a container in a Pod:  
  ```bash
  kubectl run <name> --image=<image>
  ```

---

## 🔄 Deployments

- A Deployment represents a group of replicas of the same Pod.
- Ensures Pods keep running even if nodes fail.
- Can represent a component of an app or the entire app.
- Supports scaling and updates.

Example commands:
  Scale a Deployment:
  ```bash
  kubectl scale deployment <deployment-name> --replicas=3
  ```

  List Deployments:
  ```bash
  kubectl get deployments
  kubectl describe deployments
  ```

---

## 🌐 Services

- A Service provides a stable endpoint (fixed IP) for a set of Pods.
- Abstracts Pods behind a load balancer.
- Supports both internal and external access.

Example: 
- Backend Pods change dynamically
- Frontend Pods refer to the backend Service, not the individual Pods

---

## ⚡ Autoscaling

- Kubernetes can scale Pods automatically based on parameters like CPU utilization.
- Command-line autoscaling example:
  ```bash
  kubectl autoscale deployment <deployment-name> --min=3 --max=10 --cpu-percent=80
  ```

---

## 📝 Declarative Management

- Instead of issuing imperative commands, you can use Deployment config files.
- Define your desired state, and Kubernetes ensures the cluster matches it.

Example workflow:
1. Update Deployment config file to change the number of replicas.
2. Apply the config:
```bash
kubectl apply -f deployment.yaml
```

3. Check status:
```bash
kubectl get services
kubectl get deployments
```

---

## 🚀 Rolling Updates

- To safely update your app version:
  - Use kubectl rollout or update the Deployment config file
  - Kubernetes creates new Pods individuall
  - Waits for new Pods to be ready before removing old Pods

This ensures zero downtime and controlled rollout of new versions.
