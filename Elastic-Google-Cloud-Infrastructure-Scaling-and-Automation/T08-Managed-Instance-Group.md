# 🖥️ Managed Instance Groups (MIGs)

A **Managed Instance Group (MIG)** is a collection of **identical VM instances** controlled as a **single entity** using an **instance template**.

---

## 1️⃣ Key Features

- **Rolling Updates:** Easily update all instances by specifying a new **instance template** 🔄  
- **Autoscaling:** Automatically adjust the number of instances based on application demand 📈  
- **Load Balancing:** Works with load balancers to distribute traffic across instances ⚖️  
- **Self-Healing:**  
  - Automatically recreates instances that **stop, crash, or are deleted**  
  - Recreated instances use the **same name** and **same instance template**  
- **Health Management:** Identifies and replaces unhealthy instances to ensure optimal performance ❤️  

---

## 2️⃣ Regional vs Zonal Managed Instance Groups

| Feature                    | Zonal MIG                  | Regional MIG                               |
|-----------------------------|---------------------------|-------------------------------------------|
| Zones covered               | Single zone               | Multiple zones within a region 🌐         |
| Load distribution           | Confined to one zone      | Spread across zones for reliability       |
| Fault tolerance             | Lower                     | Higher, protects against zonal failures ✅ |

> **Recommendation:** Prefer **Regional MIGs** for better fault tolerance and load distribution.  

---

## 3️⃣ Creating a Managed Instance Group

### Step 1: Create an Instance Template
- Define VM configurations (machine type, OS, disk, network, etc.)  
- Templates can be created via **Cloud Console**  
- Saved templates allow easy replication across instances

### Step 2: Create the Managed Instance Group
- Specify **number of instances (N)**  
- Select **instance template**  
- Configure **instance group rules**

### Step 3: Define Instance Group Rules

1. **Type of workload:**  
   - **Stateless:** Website front-end, image processing queue  
   - **Stateful:** Databases, legacy applications  
2. **Name:** Give your instance group a unique name  
3. **Scope:**  
   - Single-zone or multi-zone  
   - Specify the zones  
4. **Port mappings:** Optional configuration  
5. **Autoscaling:** Enable or configure based on metrics  
6. **Health checks:** Identify healthy instances that should receive traffic  

> Essentially, a MIG is like **creating multiple virtual machines** with **centralized rules and automation** applied to the group.
