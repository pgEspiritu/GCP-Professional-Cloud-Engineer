# 🔥 Firestore

Firestore is Google Cloud’s fourth core storage option — a **flexible, scalable NoSQL cloud database** designed for mobile, web, and server applications.

---

## 📄 How Firestore Stores Data

Firestore uses a NoSQL document model:

- 📁 **Documents** → store data as key–value pairs  
- 📚 **Collections** → groups of documents  
- 🌿 **Subcollections** → collections nested inside documents  
- 🧩 **Nested objects** are fully supported

Example structure for a user document:
```text
{
firstname: "Alex",
lastname: "Cruz"
}
```


---

## 🔍 Querying Data

Firestore provides powerful NoSQL querying capabilities:

- 🧠 Retrieve **specific documents** or **all documents** that match filters  
- 🔗 Apply **multiple chained filters**  
- 🗂️ Combine **filtering and sorting**  
- ⚡ **Indexed by default** → query performance depends on result size, not total dataset size  

---

## 🔄 Real-Time Sync & Offline Features

Firestore is built for real-time applications:

- 📡 **Automatic data synchronization** across connected devices  
- 📥 Efficient **one-time fetch queries**  
- 💾 **Local caching** allows the app to read/write/query offline  
- 🌐 When online again, Firestore **syncs local changes** automatically  

---

## 🏗️ Powered by Google Cloud Infrastructure

Firestore includes:

- 🌍 **Automatic multi-region replication**  
- 🔒 **Strong consistency guarantees**  
- 🧾 **Atomic batch operations**  
- 🔁 **Full transaction support**  

Together, these make Firestore ideal for apps needing reliability, real-time updates, and seamless scaling.
