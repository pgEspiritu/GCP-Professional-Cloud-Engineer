# ☁️ Cloud Storage

Cloud Storage is Google’s **durable, highly available object storage service** designed for developers and IT organizations. It allows you to store any amount of data and retrieve it whenever needed.

---

## 🧱 What Is Object Storage?

Object storage is a data storage architecture that manages data as **objects**, not:
- 📁 Files in folders (file storage)
- 💾 Blocks on disks (block storage)

Each object contains:
- 🔹 The binary data itself  
- 🔹 Metadata (e.g., creation date, author, resource type, permissions)  
- 🔹 A **globally unique identifier** (usually a URL)

Because of these URLs, object storage works very well with web technologies.

Common object types:
- 🎥 Video  
- 🖼️ Images  
- 🎧 Audio files  

---

## ⭐ What Cloud Storage Provides

Cloud Storage is Google’s object storage solution, offering:
- Infinite scalability  
- High durability  
- High availability  
- Fully managed operations  

### 💡 Common Uses
- Hosting website content  
- Archiving & disaster recovery  
- Storing photos, videos, and media  
- Direct download distribution  
- Storage for intermediate processing results (e.g., workflows, pipelines)

---

## 🪣 Buckets: How Data Is Organized

All files (objects) live inside **buckets**.

Each bucket requires:
- ✔️ A **globally unique name**
- 🌍 A **geographic location**  
  (Choose where latency will be lowest for your users)

Example:  
If your users are in Europe → choose a European region or EU multi-region.

---

## 🔄 Object Immutability & Versioning

Cloud Storage objects are **immutable** — you don’t edit them directly.  
When changes happen, a **new version** of the object is created.

Two options:

### 1️⃣ **No Versioning (default)**
- New versions overwrite old ones permanently.

### 2️⃣ **Versioning Enabled**
- Every version is stored.
- You can:
  - 📜 View object history  
  - 🔙 Restore old versions  
  - 🗑️ Permanently delete specific versions  

---

## 🔐 Access Control & Security

Cloud Storage integrates with:
- **IAM roles** (recommended for most use cases)  
- **Access Control Lists (ACLs)** for fine-grained control  

### 🔑 IAM
- Permissions flow: **Project → Bucket → Object**
- Enforces least-privilege access

### 🔑 ACLs
Each ACL contains:
- **Scope** — who (user, group, etc.)
- **Permission** — what actions (read/write)

Use ACLs only when IAM cannot meet the required level of granularity.

---

## ♻️ Lifecycle Management Policies

To control storage costs, Cloud Storage lets you automate object management.  
Examples:
- 🗑️ Delete objects older than **365 days**
- 🕰️ Delete objects created before **a specific date**
- 🔂 Keep only the **3 most recent versions** when versioning is enabled

These policies ensure you **don’t pay for unnecessary storage**.

---
