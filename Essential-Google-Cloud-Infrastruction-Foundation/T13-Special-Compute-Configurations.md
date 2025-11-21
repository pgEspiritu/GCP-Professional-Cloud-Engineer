# ⚡ Special Compute Configurations

## 🪫 Preemptible VMs
Preemptible VMs are **low-cost VM instances** offering **60–91% discounts** compared to standard VMs.

### 🔍 Key Characteristics
- ⏳ **Can be preempted at any time**
- 🕒 **Maximum lifetime: 24 hours**
- 🔔 **30-second preemption notification**
- 💸 **No charge if preempted within the first minute**
- ❌ No **live migration**
- ❌ No **automatic restarts**

### 🧰 How to Use Them Safely
- Use monitoring + load balancers to **automatically recreate** preemptible VMs after failures.
- Best for:
  - 🗂️ Batch processing  
  - 🎞️ Rendering jobs  
  - 🧮 Distributed compute workloads  

If some VMs terminate mid-processing, **the job slows but does not stop**, making them ideal for distributed tasks.

---

## 🌩️ Spot VMs
Spot VMs are the **modern replacement** for preemptible VMs.

### 🆚 Spot vs Preemptible
| Feature | Preemptible VM | Spot VM |
|--------|----------------|---------|
| Max Runtime | ⏱️ 24 hrs | ♾️ No limit |
| Pricing | 💲 Same model | 💲 Same model |
| Live Migration | ❌ No | ❌ No |
| Auto-Restart | ❌ No | ❌ No |
| Availability | Limited | Limited (varies per zone/day) |

### ⚙️ Best Practices for Spot VMs
- Use **smaller machine types** → easier to get capacity.
- Expect occasional preemption.
- Design workloads with **fault tolerance**.

---

# 🧍‍♂️🧍‍♀️ Sole-Tenant Nodes
Sole-Tenant Nodes are **physical servers** dedicated exclusively to **your project**.

### 🛡️ Why Use Them?
- 🔐 Physical isolation for **compliance** (e.g., payment systems)
- 🧩 Place multiple VM instances of various sizes on one node
- 🧾 Bring-your-own-license support (BYOL)
- 🔄 In-place restart to **minimize core usage**

### 📦 Deployment
A normal host runs VMs from **multiple customers**, while a sole-tenant node runs **only your project’s VMs**.

---

# 🛡️ Shielded VMs
Shielded VMs help ensure **verifiable integrity** and protect against:

- 🐛 Boot-level malware  
- 🧬 Kernel rootkits  
- 🛠️ Unauthorized firmware changes  

### 🔐 Features Include
- Verified boot  
- Measured boot  
- vTPM protection  
- Shielded images (required)

Part of the broader **Shielded Cloud Initiative**.

---

# 🔒 Confidential VMs
Confidential VMs protect **data in use** — meaning memory stays **encrypted even while being processed**.

### 🧠 How It Works
Uses **AMD Secure Encrypted Virtualization (SEV)** on **N2D machines (AMD Rome CPU)**.

### 🌟 Benefits
- 🔐 Data stays encrypted in RAM during computation
- 🚫 Google cannot access encryption keys
- ⚡ Minimal performance overhead
- 👥 Enables secure collaboration & multi-party workloads

### 🧬 Ideal For
- Enterprise workloads  
- High-memory applications  
- Multi-party compute  
- Data-sensitive analytics  

### 💻 How to Enable
Select **Confidential VM** when creating a VM via:
- Cloud Console  
- gcloud CLI  
- Compute Engine API  

