# 📊 Comparing Storage Options

Now that we’ve covered Google Cloud’s core storage services, let’s compare them to help determine the most suitable option for specific applications or workflows.

---

## 🗄️ Cloud Storage
Use **Cloud Storage** when you need to store **immutable blobs** larger than **10 MB**, such as:

- 🎬 Large images  
- 🎞️ Movies  
- 📦 Binary large objects (BLOBs)

**Capacity:** Petabytes  
**Max object size:** 5 TB per object  

---

## 🧮 Cloud SQL & Spanner (Relational SQL Databases)

### 📘 Cloud SQL
Choose **Cloud SQL** when you need:

- Full SQL support  
- A traditional relational database  
- Online transaction processing (OLTP)  
- Compatibility with existing applications or frameworks  

**Typical uses:**  
- 🔐 User credentials  
- 🛒 Customer orders  

**Capacity:** Up to 64 TB depending on machine type  

---

### 🌍 Spanner
Choose **Spanner** when:

- You need **horizontal scalability**  
- Cloud SQL can’t meet your requirements  
- Strong consistency + global scalability are required  

**Capacity:** Petabytes  
**Best for:** large-scale SQL apps with global consistency  

---

## ⚡ Firestore (NoSQL Document Database)

Choose **Firestore** for:

- Massive scaling  
- Predictable performance  
- Real-time queries  
- Offline query support (mobile & web apps)  

**Capacity:** Terabytes  
**Max entity size:** 1 MB per document  

**Ideal for:**  
- 📱 Mobile applications  
- 🌐 Web applications  
- 🔄 Data syncing across devices  

---

## 🏢 Bigtable (NoSQL Wide-Column Database)

Choose **Bigtable** when:

- You need to store **large volumes of structured data**  
- You require **very high read/write throughput**  
- SQL queries or multi-row transactions are **not** required  

**Limitations:**  
- ❌ No SQL queries  
- ❌ No multi-row transactions  

**Capacity:** Petabytes  
**Max sizes:**  
- 10 MB per cell  
- 100 MB per row  

**Best for:**  
- 📈 AdTech  
- 💰 Financial analytics  
- 🌐 IoT telemetry data  
- ⚡ Heavy read-write analytical workloads  

---

## 🔍 A Note About BigQuery
You may notice **BigQuery** is not included in this comparison.

This is because **BigQuery sits between data storage and data processing**.  
Its primary purpose is:

- 🧠 Big data analysis  
- ⚡ Interactive querying at scale  

It is *not* purely a storage product, which is why it is covered in other courses.

---

## 🎯 Final Reminder
Depending on your application’s architecture, you might use **one** or even **multiple storage services** together to meet all your data needs.

