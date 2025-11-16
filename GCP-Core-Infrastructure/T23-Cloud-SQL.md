# 🗄️ Cloud SQL

Cloud SQL is Google Cloud’s fully managed **relational database** service. It supports **MySQL**, **PostgreSQL**, and **SQL Server**, providing a powerful, scalable, and secure database solution without the operational overhead.

---

## 🌟 What Cloud SQL Offers

### 🛠️ Fully Managed Relational Databases
Cloud SQL automates and manages:
- 🔧 Patches and updates  
- 💾 Backups  
- 🔁 Replication  
- 🧰 Routine maintenance  

This allows developers to focus on building applications instead of managing database infrastructure.

---

## 📈 Performance & Scalability

Cloud SQL can scale up to:
- ⚙️ **128 vCPUs**
- 🧠 **864 GB of RAM**
- 💽 **64 TB of storage**

---

## 🔁 Replication Support

Cloud SQL supports **automatic replication**, including:
- Cloud SQL primary → read replicas  
- External primary instance → Cloud SQL replica  
- External MySQL instances → Cloud SQL replica  

This enables high availability, disaster recovery, and workload distribution.

---

## 🔒 Security & Backup Features

### 💾 Managed Backups
- Automatically performs and stores backups  
- Instance cost includes **7 backups**  
- Ensures recoverability when needed  

### 🔐 Encryption
- Encrypts customer data:
  - Within Google’s internal networks  
  - In database tables  
  - In temporary files  
  - In backups  

### 🚧 Network Firewall
- Controls access to each database instance  
- Ensures only authorized sources can connect  

---

## 🔗 Integration with Other Services

### 🚀 App Engine
- Works with standard drivers:
  - Connector/J (Java)  
  - MySQLdb (Python)  

### 🖥️ Compute Engine
- VMs can be authorized to access Cloud SQL  
- SQL instance can be deployed in the **same zone** as the VM to reduce latency  

### 🌐 External Tools & Applications
Cloud SQL supports standard MySQL/PostgreSQL/SQL Server drivers and can be used with:
- SQL Workbench  
- Toad  
- Third-party applications and tools  
