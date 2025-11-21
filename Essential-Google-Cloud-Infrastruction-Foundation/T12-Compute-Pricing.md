# 🖥️ Compute Pricing Overview

## 🛠️ VM Creation Methods
You can create and configure a VM using:

- 🌐 **Cloud Console**
- 💻 **Cloud Shell CLI**
- 🔗 **RESTful API**

💡 *Tip:* Configure the instance in the Console first, then get the equivalent REST or CLI commands to avoid errors and view all available configurations.

---

# 🧩 Machine Families Overview

When creating a VM, you choose a **machine type** from a **machine family**, which determines CPU and memory resources.

Google Cloud offers **four machine families**:

1. ⚙️ **General-purpose**  
2. 🚀 **Compute-optimized**  
3. 🧠 **Memory-optimized**  
4. 🎮 **Accelerator-optimized**

Each family contains various machine **series** and predefined or **custom** machine types.

---

# ⚙️ General-Purpose Machine Family

## 💸 E2 Machine Series
- Designed for **low-cost**, day-to-day workloads  
- No dependency on specific CPU architecture  
- **2–32 vCPUs**  
- **0.5–8 GB** memory per vCPU  
- Ideal for:
  - 🌐 Web servers  
  - 🗄️ Small/medium databases  
  - 🧪 Dev/test environments  
  - 📦 Apps without strict performance needs  

### 🔄 Shared-Core E2 Types
- **0.25–1 vCPU**
- Cost-effective for lightweight workloads  

---

## 🔧 N2 & N2D Machine Series

### 🟦 N2 (Intel-Based)
- Up to **128 vCPUs**  
- **0.5–8 GB** memory per vCPU  
- Suitable for:
  - 🏢 Enterprise applications  
  - 🗃️ Medium–large databases  
  - 🔗 Web/app-serving workloads  

### 🟥 N2D (AMD-Based)
- Up to **224 vCPUs**  
- Uses **AMD EPYC Milan/Rome** CPUs  

---

## 🏎️ Tau T2D Machine Series
- Optimized for **cost-effective scale-out workloads**
- Up to **60 vCPUs**
- Uses **AMD EPYC** processors  
- Great for:
  - 🌐 Web servers  
  - 🧩 Microservices  
  - 🎞️ Media transcoding  
  - ☕ Large Java applications  

Supports **GKE node pools**.

---

# 🚀 Compute-Optimized Machine Family

## 🔥 C2 Machine Series
- Highest **performance per core**
- Best for:
  - 🎮 Gaming  
  - 🔬 HPC simulations  
  - 🧬 Genomic analysis  
  - 📺 Media transcoding  
- **4–60 vCPUs**
- Up to **240 GB** memory  
- Optional **3 TB local storage**

## 🧮 C2D Machine Series
- Largest VM sizes for HPC workloads  
- **2–112 vCPUs**
- **4 GB** memory per vCPU  
- Large last-level cache  
- Up to **3 TB** local storage  
- Uses **AMD EPYC Milan** CPUs  

---

# 🧠 Memory-Optimized Machine Family

## 🟩 M1 Machine Series
- Up to **4 TB RAM**

## 🟦 M2 Machine Series
- Up to **12 TB RAM**

Ideal for:
- 🏛️ SAP HANA  
- 📊 In-memory databases  
- 📈 Large data analytics  

💵 Lowest cost per GB of memory  
💸 Supports sustained & committed-use discounts (up to **60%+**)  

---

# 🎮 Accelerator-Optimized Machine Family

## 🟧 A2 Machine Series
- Designed for **GPU-heavy workloads** like ML and HPC  
- **12–96 vCPUs**
- Up to **1.3 TB RAM**
- Includes **1–16 NVIDIA A100 GPUs**  
- Each A100 GPU has **40 GB** GPU memory  

---

# ⚙️ Custom Machine Types

If predefined machine types don’t fit your needs, you can define custom vCPU and memory values.

Custom machine rules:

- ✔️ 1 vCPU or an **even** number of vCPUs  
- ✔️ Memory between **0.9–6.5 GB per vCPU** (default limit)  
- ✔️ Total memory must be a multiple of **256 MB**  

### 🧩 Extended Memory
You can exceed the 6.5 GB/vCPU limit at additional cost (extended memory).

---

# 🌍 Regions and Zones

When choosing a region/zone, consider **geographical location** and performance needs.

- Regions include various CPU platforms:
  - 🟡 Ivy Bridge  
  - 🟠 Sandy Bridge  
  - 🔵 Haswell  
  - 🔴 Broadwell  
  - 🟣 Skylake  

Example:  
Creating an instance in **us-central1-a** uses a **Sandy Bridge** processor by default.

