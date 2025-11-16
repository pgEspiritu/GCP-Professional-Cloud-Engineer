# ☁️ Cloud Storage: Storage Classes & Data Transfer

Google Cloud Storage offers four primary **storage classes**, each designed for different access patterns and cost needs.

---

## 🏷️ Storage Classes

### 🔥 1. Standard Storage
- Best for **frequently accessed ("hot") data**
- Ideal for:
  - Short-term data storage
  - Applications that access data often

---

### 📅 2. Nearline Storage
- Best for **infrequently accessed data**
- Access frequency: **about once a month or less**
- Use cases:
  - Backups
  - Long-tail multimedia content
  - Archival data

---

### ❄️ 3. Coldline Storage
- Low-cost storage for **very infrequently accessed data**
- Access frequency: **once every 90 days or less**
- Use cases:
  - Archival data
  - Backups accessed quarterly

---

### 🧊 4. Archive Storage
- **Lowest-cost** storage class
- Designed for data accessed **less than once a year**
- Use cases:
  - Long-term data archiving
  - Online backup
  - Disaster recovery
- Has:
  - Higher access costs  
  - **365-day minimum storage duration**

---

## 🎯 Shared Features Across All Storage Classes

Regardless of class, Cloud Storage offers:
- 📦 Unlimited storage (no minimum object size)
- 🌐 Worldwide accessibility
- ⚡ Low latency & high durability
- 🔐 Uniform security, tools, and API experience
- 🌍 Geo-redundancy (for multi-region or dual-region buckets)  
  - Protects against catastrophic events and natural disasters  
  - Provides load balancing for performance

---

## 🤖 Autoclass

**Autoclass** automatically moves objects between storage classes based on their **access patterns**.

- Moves inactive data → **colder storage** to reduce cost  
- Moves recently accessed data → **Standard Storage** for performance  
- Helps automate **cost optimization** with zero manual work  

---

## 💰 Pricing & Security

### 💵 Pricing
- No minimum fees  
- Pay only for what you use  
- No need to pre-provision capacity  

### 🔐 Security
- **Server-side encryption** by default (no extra charge)
- **TLS/HTTPS** encrypts data in transit from your device to Google Cloud

---

## 🚚 Data Transfer Options

### 🧑‍💻 1. Online Transfer via Cloud SDK
- Use `gcloud storage` commands  
- Most common method for regular uploads

---

### 🖱️ 2. Browser Upload (Drag & Drop)
- Available in the Google Cloud Console  
- Works with the **Google Chrome** browser

---

### 📦 3. Storage Transfer Service (STS)
Used for moving **large-scale online data**.

Supports:
- Transfers from:
  - Other cloud providers  
  - Different Cloud Storage regions  
  - HTTP(S) endpoints  
- Allows:
  - Batch scheduling  
  - Managed transfer jobs  
- Ideal for **terabytes to petabytes** of data

---

### 🏗️ 4. Transfer Appliance
- A **rackable, high-capacity server** leased from Google
- Steps:
  1. Connect it to your network
  2. Load it with data
  3. Ship it back to Google
- Google uploads it directly to Cloud Storage  
- Handles up to **1 petabyte** per appliance  

---

## 🔌 Integrations with Other Google Cloud Services

Cloud Storage integrates seamlessly with many GCP services, enabling various import/export capabilities:

### 📊 BigQuery
- Import and export tables via Cloud Storage

### 🗄️ Cloud SQL
- Import/export database dumps

### 🚀 App Engine
- Store:
  - App Engine logs  
  - Firestore backups  
  - Images & static assets  

### 🖥️ Compute Engine
- Store:
  - Startup scripts  
  - VM images  
  - Data objects used by applications  
