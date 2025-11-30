# ☁️ Cloud Storage Features

Cloud Storage comes with several powerful features.  
We’ll cover them at a high level for now, and explore some of them in more detail later. 🚀

---

## 🔐 Customer-Supplied Encryption Keys (CSEK)

Earlier in the course, we discussed CSEK when attaching persistent disks to VMs.  
This feature is also available for Cloud Storage and allows you to:

- Provide your **own encryption keys** 🔑  
- Use them instead of Google-managed keys  

---

## 🔄 Object Lifecycle Management

Cloud Storage provides **Object Lifecycle Management**, which allows you to automatically:

- 🗑️ Delete objects  
- 📦 Archive objects  
- 🔽 Downgrade storage classes  
- ♻️ Retain only recent versions  

You will apply rules to the bucket, and when objects meet those rules, Cloud Storage automatically performs specified actions.

---

## 🗂️ Object Versioning

Object Versioning allows you to maintain **multiple versions** of objects in a bucket.  
Key details:

- Every new version counts as a **separate object** (charged as separate storage) 💰
- Archived versions have the **same object name** but unique **generation numbers** (e.g., `g1`)  
- You can:
  - 📜 List archived versions  
  - 🔁 Restore earlier versions  
  - ❌ Permanently delete archived versions  
- You can turn versioning **on or off anytime**  
- Turning versioning off **keeps existing versions** but stops creating new ones  

📌 **Recommendation:** Google advises using **Soft Delete** instead of Object Versioning for better protection.

---

## 🧽 Soft Delete (Recommended)

Soft Delete protects against accidental or malicious deletions by preserving deleted objects for a set duration.

- Enabled by default with **7-day retention** 🗓️  
- Can be increased up to **90 days**  
- Can be disabled by setting retention to **0**  
- Retains objects deleted by:
  - A delete command  
  - Being overwritten  

After the retention duration ends, objects are **permanently deleted**.

---

## 🔁 Object Lifecycle Management (Detailed)

Lifecycle rules help automate:

- 🥶 Downgrading storage class (e.g., to Coldline for objects older than 1 year)  
- 🗑️ Deleting objects created before a specific date  
- 🧹 Keeping only the **3 most recent versions** of each object  

⏱️ Notes:
- Object inspection is **asynchronous**  
- Rule updates may take **up to 24 hours** to apply  
- Old rules may still apply for up to 24 hours  

---

## 🔒 Object Retention Lock

Object Retention Lock allows you to set **retention configurations** on objects.

Features:

- Ensures objects must be retained for a specified duration  
- Option to make retention **permanent** (cannot be reduced or removed)  
- Helps you comply with regulatory standards like:
  - 🏦 FINRA  
  - 📈 SEC  
  - 📑 CFTC  

Supports immutable storage requirements for enterprise backup solutions.

---

## 📤 Large-Scale Data Upload Options

Uploading files via the Cloud Console works only for small datasets.  
For massive data (TB to PB), use:

### 📦 1. Transfer Appliance
- Physical hardware device  
- Migrate **hundreds of TB to 1 PB**  
- No disruption to business operations  

### 🚚 2. Storage Transfer Service
- High-performance online data transfer  
- Supports:
  - Cloud Storage buckets  
  - Amazon S3  
  - HTTP/HTTPS sources  

### 💽 3. Offline Media Import
- Third-party vendor uploads data from:
  - Hard drives  
  - Storage arrays  
  - Tapes  
  - USB drives  

---

## 🌍 Strong Global Consistency

Cloud Storage offers **strong, global consistency** for:

### 📥 Uploads
When an upload succeeds:
- Object is **immediately available** for download  
- Metadata is instantly consistent  
- No stale reads  
- No 404 errors after write  

### 🗑️ Deletes
After a successful delete:
- Any attempt to read/download returns **404 Not Found**  
- Object is guaranteed to be gone  

### 🪣 Bucket Listing
Creating a bucket → listing buckets immediately shows it.

### 📄 Object Listing
Uploading an object → listing objects immediately includes it.

---

## ✅ Summary

Cloud Storage provides:

- Encryption flexibility 🔐  
- Versioning & Soft Delete 🧽  
- Automated lifecycle workflows 🔄  
- Compliance-grade retention locks 📜  
- Massive-scale data ingestion tools 🚚  
- Strong global consistency 🌍  

