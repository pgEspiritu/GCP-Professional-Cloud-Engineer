# ⚙️ Compute Options in Google Cloud  
A deeper look into CPU, memory, and VM machine families.

---

## 🖥️ Ways to Create a VM

You can create/configure a VM using:
1. **Cloud Console** (UI)
2. **Cloud Shell CLI**
3. **RESTful API**

### 🧠 Pro Tip  
If using CLI or REST:
- First configure in **Console**
- Then view the **equivalent REST/CLI command**  
  → Helps avoid typos and shows all CPU/memory options.

---

# 🧩 Machine Types Overview

When creating a VM, you select a **machine family → series → predefined or custom machine type**.

### 🏷️ Four Machine Families
1. **General-purpose**
2. **Compute-optimized**
3. **Memory-optimized**
4. **Accelerator-optimized**

Each family is optimized for specific workload types.

---

# 🟦 General-Purpose Machine Family

Balanced price-performance, flexible CPU:RAM ratios.

## 🔹 E2 Series (Lowest cost)
- Best for day-to-day workloads
- No dependency on specific CPU architecture
- **2–32 vCPUs**, **0.5–8 GB per vCPU**

### 💡 Ideal for:
- Web servers  
- Small/medium databases  
- Dev/test environments  
- Apps without strict performance needs

### 🌀 Shared-Core E2 Machine Types
- **0.25–1 vCPU**, **0.5–8 GB memory**
- Uses context switching for cost efficiency  
- Good for lightweight apps

---

## 🔹 N2 & N2D Series (Next-gen performance)
### N2
- Intel Scalable Processors (Cascade Lake / Ice Lake)
- Up to **128 vCPUs**, **0.5–8 GB per vCPU**

### N2D
- AMD EPYC Milan/Rome processors  
- Up to **224 vCPUs**

### 💡 Good for:
- Enterprise applications  
- Medium–large databases  
- App-serving workloads  
- Eligible for committed & sustained use discounts

---

## 🔹 Tau Series (T2D, T2A)

### T2D (AMD EPYC)
- Optimized for **scale-out workloads**
- Up to **60 vCPUs**, **4 GB per vCPU**
- Great for:
  - Web servers  
  - Microservices  
  - Media transcoding  
  - Large-scale Java apps

### T2A (ARM - Ampere Altra)
- 64-core ARM processor at 3 GHz
- GKE-optimized for container workloads

---

# 🟥 Compute-Optimized Machine Family  
Highest per-core performance.

## 🔹 C2 Series (Intel)
- Best for compute-intensive tasks
- Up to **3.8 GHz** sustained all-core turbo
- **4–60 vCPUs**, up to **240 GB RAM**
- Supports up to **3 TB local SSD**

### 💡 Ideal for:
- AAA gaming  
- Electronic design automation  
- HPC simulations  
- Genomic analysis  
- Expensive per-core licensed applications

---

## 🔹 C2D Series (AMD EPYC Milan)
- Largest VM sizes
- **2–112 vCPUs**, **4 GB RAM per vCPU**
- Up to **3 TB local SSD**
- Largest last-level cache per core

---

## 🔹 H3 Series (Intel Sapphire Rapids)
- 88 cores  
- 352 GB DDR5 memory  
- Uses custom Google Intel IPU  
- Great for HPC, simulation-heavy workloads

---

# 🟩 Memory-Optimized Machine Family  
Highest memory configurations.

## 🔹 M1 Series
- Up to **4 TB RAM**

## 🔹 M2 Series
- Up to **12 TB RAM**

## 🔹 M3 Series
- Up to **128 vCPUs**
- Up to **30.5 GB per vCPU**
- Great for:
  - SAP HANA  
  - Large in-memory DBs  
  - In-memory analytics  
  - Genomic modeling  
  - HPC workloads

### 💰 Cost Optimization
- Lowest **cost per GB** of memory
- Up to **30% sustained-use discount**
- Eligible for **committed-use discounts**  
  → Up to **60% savings**

---

# 🟪 Accelerator-Optimized Machine Family  
Designed for GPU-based, massively parallel workloads.

## 🔹 A2 Series
- **12–96 vCPUs**, up to **1360 GB RAM**
- Up to **16× NVIDIA A100 GPUs**  
- A100 GPU = **40 GB GPU memory**

### 💡 Ideal for:
- Large language model training  
- HPC  
- Scientific computing  
- DB acceleration  

---

## 🔹 G2 Series
- **4–96 vCPUs**, up to **432 GB RAM**
- NVIDIA L4 GPUs (CUDA optimized)
- Great for:
  - ML training/inference  
  - Video transcoding  
  - Visualization/workstations  

---

# 🛠️ Custom Machine Types  
When predefined shapes don't fit your workload.

### ✔️ Choose your own:
- Number of vCPUs  
- Amount of memory  

### 📌 Limitations
- vCPUs must be **1 or even numbers**
- Memory must be:
  - **1–8 GB per vCPU**
  - A **multiple of 256 MB**
- Extended memory available (extra cost)  
  → More than 8 GB per vCPU

### 💡 Ideal for:
- Workloads not fitting predefined shapes  
- Needing just slightly more CPU or memory  
- Avoiding unnecessary cost from scaling to next VM size

---

# 🌍 Choosing Regions & Zones

### Consider:
- User proximity  
- Latency  
- Data sovereignty  
- Available hardware/processor types  

### 🔎 Platform examples
Zones may support combinations of:
- Ivy Bridge  
- Sandy Bridge  
- Haswell  
- Broadwell  
- Skylake  

### Example:
- Creating a VM in **us-central1-a** → defaults to **Sandy Bridge** processor  
