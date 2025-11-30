# 🔥 Firestore (Firetable) – Highly Scalable NoSQL Database

## 🌐 Overview
**Firestore** is a **fast, fully managed, serverless, cloud-native NoSQL document database** designed for **mobile, web, and IoT applications** that need to scale globally.

It simplifies:
- 🔄 Storing data  
- 🔗 Syncing data  
- 🔍 Querying data  

And it integrates tightly with **Firebase** and **Google Cloud**.

---

## ⚡ Key Capabilities

### 📱 Real-Time Sync + Offline Support
Firestore's client libraries provide:
- 🔴 Live data synchronization  
- 📵 Offline support for mobile & web  
- 🔐 Built-in security rules  

Great for reactive or collaborative apps.

---

### 🧪 ACID Transactions
Firestore supports **ACID transactions**:
- If any operation fails and cannot be retried ➝ ❌ the entire transaction fails
- Ensures consistent, reliable operations

---

### 🌍 Global Availability + Disaster Safety
- 🌎 Automatic **multi-region replication**  
- 🧱 **Strong consistency** across regions  
- ☁️ Data remains safe and available even during disasters  

---

### 🔍 Powerful NoSQL Querying
Firestore allows complex, efficient queries with **no performance degradation**, giving flexibility for how you structure data.

---

## 🔁 Firestore vs Datastore

Firestore is the **next generation** of Datastore. It has two modes:

### 🟦 Firestore in Datastore Mode
Backwards compatible with Datastore but includes:
- Strong consistency for all queries  
- No 25-entity-group transaction limit  
- No 1-write-per-second limitation  

Use for **server-side** projects needing Datastore behavior.

---

### 🟩 Firestore in Native Mode
Provides the *full feature set*:
- 🚀 New strongly consistent storage layer  
- 📄 Collection/document model  
- 🔔 Real-time updates  
- 📱 Mobile & web client libraries  

Use for **mobile and web apps**.

---

## 🧭 When to Choose Firestore
Use Firestore if:
- 🔄 Your schema may change (needs flexibility)  
- ➖ You want to **scale to zero**  
- 📚 You want minimal maintenance with seamless scaling to terabytes  
- 📡 You need global, real-time data synchronization  

If you do **not** need transactional consistency and want lower cost for huge datasets ➝ consider **Bigtable**.

---

## 📌 Summary Decision Tree
Choose **Firestore** if:
- You need a flexible, scalable NoSQL database  
- You need real-time sync or offline capabilities  
- You prefer a serverless, low-management solution  

Otherwise, evaluate Cloud SQL, Spanner, or Bigtable depending on workload type.

For deeper details, refer to Google Cloud's documentation.  
