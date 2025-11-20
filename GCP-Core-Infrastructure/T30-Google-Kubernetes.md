# ☸️ Google Kubernetes Engine (GKE)

## 🌥 What is GKE?

- GKE is a **Google-hosted managed Kubernetes service**.
- A GKE cluster is made up of **multiple machines (Compute Engine instances)** grouped together.
- You can create clusters through:
  - **Google Cloud Console**
  - **gcloud CLI (Cloud SDK)**

---

## 🔍 How GKE Differs from Kubernetes

From a user’s perspective, **GKE is much simpler** than self-managed Kubernetes.

### ✔ Managed Control Plane
- GKE fully manages the **control plane** for you.
- It provides an **API endpoint** for Kubernetes API requests.
- GKE handles:
  - Control plane provisioning  
  - Scaling  
  - Security patches  
  - Upgrades  
  - Availability  

You **no longer need to deploy, secure, or maintain master nodes**.

---

## 🧭 GKE Modes: Autopilot vs Standard

GKE offers two modes for running clusters:

---

### 🚀 Autopilot Mode (Recommended)

Autopilot manages the **entire infrastructure layer** so you focus only on workloads.

**Autopilot automatically handles:**
- Node provisioning and configuration  
- Autoscaling  
- Auto-upgrades  
- Baseline security configuration  
- Networking configuration  

**Benefits of Autopilot:**
- ✔ Production-optimized  
- ✔ Strong default security posture  
- ✔ Operational efficiency  
- ✔ Lower management overhead  

Use Autopilot unless you need full control of the nodes.

---

### 🖥 Standard Mode

Standard mode gives you:
- Full control of **node configuration**  
- Responsibility for:
  - Node management  
  - Node upgrades  
  - Node security  
  - Infrastructure optimization  

Use Standard only when you need deep customization or special configurations.

---

## ⚙️ Core Capabilities of GKE

GKE provides advanced cluster management features:

### 🌀 1. Google Cloud Load Balancing
Automatic load balancing for Pods running on nodes.

### 🧩 2. Node Pools
Group nodes with different machine types or purposes.

### 📈 3. Cluster Autoscaling
Automatically add or remove nodes based on workload demand.

### 🔄 4. Node Auto-Upgrades
Ensures node software stays up-to-date and secure.

### ❤️ 5. Node Auto-Repair
Automatically recreates unhealthy nodes.

### 📊 6. Logging & Monitoring
Integrated with **Google Cloud Observability** for:
- Metrics  
- Logs  
- Traces  

---

## ▶️ Starting Kubernetes on GKE

To create your first cluster with the Cloud SDK:

```bash
gcloud container clusters create k1
```

This provisions a fully functioning Kubernetes cluster on Google Cloud.
