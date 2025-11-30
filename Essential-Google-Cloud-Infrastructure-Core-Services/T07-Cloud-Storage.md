# ☁️ Cloud Storage

Cloud Storage is Google Cloud’s **object storage service**, enabling worldwide storage and retrieval of **any amount of data at any time**.  

You can use it for:
- 🌐 Serving website content  
- 🗄️ Archival & disaster recovery  
- 📦 Distributing large files via direct download  

## ⭐ Key Features
- 📈 Scales to **exabytes**
- ⚡ Millisecond **time to first byte**
- ♻️ High **availability** across all storage classes
- 🔄 Single **unified API** for all classes

---

# 🪣 Buckets & Objects

Cloud Storage is **not** a file system.  
Instead, it consists of **buckets** that contain **objects**.

- Buckets have **globally unique names**
- Buckets **cannot be nested**
- "Directories" are simulated using object prefixes
- Objects inherit the **storage class** of the bucket unless specified

You can access objects using:
- `gcloud storage`
- JSON API
- XML API

---

# 🏷️ Storage Classes & Location Types

Cloud Storage has **4 storage classes** and **3 location types**.

## 📦 Storage Classes
1. 🚀 **Standard** – Hot data, high frequency access  
   - No minimum duration  
   - No retrieval cost  

2. 📉 **Nearline** – Infrequently accessed (once/month)  
   - 30-day minimum  
   - Lower storage cost  

3. ❄️ **Coldline** – Rarely accessed (once/quarter)  
   - 90-day minimum  

4. 🥶 **Archive** – Access less than once/year  
   - 365-day minimum  
   - Lowest storage cost  
   - Still millisecond access (unlike other cloud providers)

---

## 🌍 Location Types
- 🌎 **Multi-region** (ex: US) – geo-redundant across >2 regions  
- 🌏 **Dual-region** (ex: Finland + Netherlands)  
- 📍 **Region** (ex: London)

Objects in multi-region and dual-region storage are **geo-redundant**.

---

# 🔒 Durability vs Availability

All storage classes offer **11 nines (99.999999999%) durability**.  
This means:
- ✅ You **won’t lose** your data  
- ❌ But access availability varies by class & location  

Analogy: Your money in the bank is durable,  
but the bank may be closed — that's **availability**.

---

# 🧰 Managing Storage Classes

- Objects inherit the **bucket’s storage class**
- You **can change**:
  - Bucket **default storage class**
  - Object **storage class** (without changing URL)
- You **cannot change**:
  - Bucket **location type** (regional ⟷ multi/dual-region)

---

# 🔄 Object Lifecycle Management

Cloud Storage can **automatically transition** objects between classes.  
Useful for:
- Cost optimization  
- Archival workflows  
- Automatic deletion rules  

---

# 🔐 Access Control Options

### 1️⃣ IAM (project level)
Controls:
- Who can see buckets  
- Who can list objects  
- Who can create buckets  
- Roles inherited **project → bucket → object**

IAM is sufficient for most cases.

### 2️⃣ ACLs (bucket/object level)
Provide finer control.  
- Maximum **100** entries  
- Each entry includes:
  - **Scope** (who)  
  - **Permission** (what: read/write)

Special identifiers:
- 🌍 `allUsers` – anyone on the internet  
- 🔑 `allAuthenticatedUsers` – anyone with Google account auth  

### 3️⃣ Signed URLs (temporary access)
Use when:
- You don’t want users to have a Google account  
- You want time-limited object access  

A signed URL:
- Grants temporary **read/write** access  
- Is signed using a **service account private key**  
- Becomes **out of your control** once shared  
- Should have a **short expiration time**

### 4️⃣ Signed Policy Documents
Allow granular control over:
- What file types can be uploaded  
- Under what conditions  
- Using a signed URL

---

# 📝 Summary

Cloud Storage provides:
- Flexible storage classes  
- Global access  
- Granular security controls  
- Millisecond access even for Archive class  
- Strong durability & lifecycle management  

