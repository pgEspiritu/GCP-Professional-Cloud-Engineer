# 🌐 Important VPC Compatibilities

## 🛣️ Built-In Routing Tables
Google Cloud **Virtual Private Cloud (VPC)** networks include **built-in routing tables**, meaning:
- No need to provision or manage physical routers.
- Routes automatically forward traffic:
  - Between instances in the same network
  - Across different subnetworks
  - Across Google Cloud zones  
- And all of this works **without requiring external IP addresses**. 🚫🌐

---

## 🔥 Global Distributed Firewall
Google Cloud automatically provides a **global, distributed firewall**, so you don’t need to deploy one yourself.

### Firewall Features:
- Controls **incoming and outgoing** traffic 🔄
- Rules can be defined using **network tags**, making access management simpler and more scalable.

### Example:
You can:
- Tag all web servers as `WEB`  
- Create a firewall rule allowing traffic on **ports 80 and 443** to any VM with the `WEB` tag  
This works regardless of each VM’s individual IP address. 🌍

---

## 🔗 Multi-Project Communication
VPCs belong to **Google Cloud projects**, but many companies use several projects. When VPCs need to communicate, there are two major options:

### 1️⃣ **VPC Peering**
- Creates a **direct relationship** between two VPCs  
- Allows them to **exchange traffic privately and efficiently**

### 2️⃣ **IAM-Controlled Shared VPC**
- Lets organizations use **Identity and Access Management (IAM)**  
- Allows one project’s resources to securely interact with a VPC hosted in another project  
- Provides centralized control and better governance 🔐
