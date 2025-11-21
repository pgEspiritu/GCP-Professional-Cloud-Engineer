# 💻 Compute Engine in Google Cloud

---

## 🔹 Overview

- Compute Engine provides **traditional virtual machine (VM) instances** in GCP.
- It is an **Infrastructure as a Service (IaaS)** offering:
  - Full control over OS, software, and network
  - Configure **autoscaling**, VM size, and resource allocation

---

## ⚙️ Flexibility & Workloads

- Run **any language** or application as on a traditional server.
- Best suited for **generic workloads** or **enterprise applications**.
- More **portable** than containerized services like Google Kubernetes Engine (GKE).

---

## 🖥️ VM Configuration Options

- **Machine types**:
  - Predefined or **custom** CPU & memory
- **Disks**:
  - Standard HDD 🟤
  - SSD ⚡
  - Local SSD (ephemeral, high throughput)
- **Networking**:
  - Multiple NICs
  - Internal & external IPs
  - Firewall rules & tags
- OS options: Linux and Windows

---

## ⚡ Accelerated Compute: TPUs

- **Tensor Processing Units (TPUs)** are custom ASICs for **Machine Learning (ML)**.
- Faster and more energy-efficient than CPUs and GPUs for AI workloads.
- Best for **long training jobs** and **large batch models**.
- Integrated across Google products and available in Compute Engine.

---

## 🖥️ CPU & Networking

- vCPUs are **hardware hyperthreads** on physical CPU platforms.
- Network throughput:
  - 2 Gbps per vCPU (except 2/4 CPU VMs → 10 Gbps)
  - Max 200 Gbps for 176 vCPUs (C3 machine series)
- CPU platform choice affects **network bandwidth**.

---

## 💾 Disk Options & Considerations

- **Persistent Disks**:
  - Standard HDD → lower cost, higher capacity
  - SSD → higher IOPS per dollar
- **Local SSD**:
  - High throughput, low latency
  - Data persists only while VM is running
  - Ideal for **swap disks or temporary storage**
- Maximum disk size: 257 TB per instance
- Disk performance scales with allocated space

---

## 🌐 Networking Integration

- Compute Engine integrates with **VPC networks**.
- Supports:
  - Regional HTTPS load balancing
  - Network load balancing
- Load balancers use **traffic engineering rules**; no pre-warming needed.
- Future courses cover **advanced load balancing and network configurations**.

---

💡 **Key Takeaways**

- Compute Engine = flexible, portable VMs with custom CPU, memory, and disk.
- TPUs = high-performance ML accelerators.
- Disk choice = trade-off between cost, performance, and persistence.
- Networking = integrates fully with VPC, internal/external IPs, and load balancers.
