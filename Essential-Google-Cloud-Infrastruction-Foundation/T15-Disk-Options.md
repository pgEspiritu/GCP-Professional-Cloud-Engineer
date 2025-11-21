# 💽 Disk Options in Google Cloud Compute Engine

## 🧱 Overview  
When you create a VM, the operating system is stored on a **disk**. Google Cloud offers multiple disk types with different performance, durability, and use cases.

---

# 🪛 Boot Disks

- Every VM includes **one root persistent disk** 📦.
- This disk:
  - 🥾 Is **bootable** (VM boots from it)
  - 🔒 Is **durable** (survives VM termination)
- To keep a boot disk after deleting a VM:
  - Disable **“Delete boot disk when instance is deleted”** ❌🗑️

---

# 💾 Persistent Disks (PD)

Persistent disks are **network-attached storage** — not physically tied to the VM.

### ⭐ Key Features  
- 🚀 Survive VM termination  
- 📸 Support **snapshots** (incremental backups)  
- 📏 Can be **resized dynamically**, even while attached  
- 🔁 Can be attached **read-only to multiple VMs**  
  - Useful for sharing static data  
- 🔐 Automatically encrypted

---

# 🗺️ Zonal vs. Regional Persistent Disks

### 📍 Zonal PD  
- Standard reliable block storage  
- Lives in **one zone**

### 🌍 Regional PD  
- Replicates data **across two zones** (active–active)  
- Great for:
  - Databases 🗄️
  - High-availability apps 💼  

---

# 🔧 Disk Types (Zonal & Regional PDs)

### 1️⃣ **Standard Persistent Disk (HDD) 🧲**  
- Best for:
  - Large data workloads  
  - Sequential I/O  
- Cheapest option 💸

### 2️⃣ **SSD Persistent Disk ⚡**  
- High performance  
- Ideal for:
  - Enterprise apps  
  - High-performance databases  

### 3️⃣ **Balanced Persistent Disk ⚖️**  
- SSD-backed  
- Cheaper than performance SSD  
- Same max IOPS, but lower IOPS per GB  
- Best for general-purpose apps 🖥️

### 4️⃣ **Extreme Persistent Disk 🔥**  
- Highest performance  
- You can **provision your own IOPS**  
- Ideal for:
  - High-end databases  
  - Intensive random-access workloads  

---

# 🔐 Disk Encryption

All persistent disks are encrypted by default 🔒.

You can also choose:
- **Customer-Managed Keys (CMEK)** via Cloud KMS 🗝️  
- **Customer-Supplied Keys (CSEK)** that you manage yourself 🔑  

---

# 💥 Local SSDs (Ephemeral)

Local SSDs are **physically attached** to the VM.

### ⚡ Characteristics  
- Ultra-high IOPS ⚡  
- Ephemeral (❌ does NOT survive stop/terminate)  
- Size: **375 GB each**  
- Up to **24 partitions (9TB total)**  
- Data survives **reset**, but not **stop/terminate**

---

# 🧠 RAM Disks (tmpfs)

- Store data **in memory** 🧠  
- Fastest option available ⚡⚡⚡  
- Volatile — data disappears on reboot  
- Use with:
  - High-memory VM  
  - Persistent disk for backup

---

# 🧮 Summary of Disk Options

| Disk Type | Durability | Performance | Use Case |
|----------|------------|-------------|----------|
| **Persistent HDD** 🧲 | High | Low | Capacity-focused workloads |
| **Persistent SSD** ⚡ | High | High | General & enterprise apps |
| **Balanced SSD** ⚖️ | High | Medium-High | General workloads with good performance |
| **Extreme PD** 🔥 | High | Very High | High-end DBs |
| **Local SSD** 🚀 | Low (ephemeral) | Extremely High | High IOPS, caches, scratch space |
| **RAM Disk** 🧠 | Very Low | Maximum | Small in-memory data structures |

---

# 📦 Disk Attachment Limits

Depends on **machine type**:

- 🧩 Shared-core: up to **16** disks  
- 🧱 Standard / High-Memory / High-CPU / Compute-Optimized: up to **128** disks  

---

# ⚠️ Performance Note  
Disk I/O **shares bandwidth** with network traffic 🌐  
➡️ High disk throughput can compete with network throughput.

---

# 🆚 Physical Disk vs. Cloud PD

### 🖥️ Physical Disk  
- Must partition manually  
- Must manage redundancy  
- Must handle encryption  
- Must resize and reformat manually

### ☁️ Cloud Persistent Disk  
- No partitioning complexity  
- Automatic redundancy  
- Auto-encryption  
- Easy resizing  
- Snapshots built in  
- Can bring your own encryption keys 🔐

---

