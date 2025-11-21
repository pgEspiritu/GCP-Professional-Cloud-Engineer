# 🌐 Common Network Design in Google Cloud

---

## 🔹 Overview

- Using what we've learned, let's explore **common network designs**.
- "Common" is relative; these are key designs relevant to this module.

---

## 🏗️ Availability Design

- Place **two VMs in multiple zones** within the **same subnet** to improve availability.
- **Single subnet** allows a single firewall rule to protect all VMs in that subnet (`10.2.0.0/16`) 🔒
- **Benefits:**
  - Increased availability ✅
  - Reduced security complexity 🔐
- **Regional Managed Instance Group (MIG):**
  - Instances span multiple zones within a region
  - Provides **high availability** across zones 🌍

---

## 🌍 Globalization Design

- Placing resources in **different regions** provides **failure independence**.
- Use **global load balancers** (e.g., HTTP LB) to route traffic to the **closest region**:
  - Better **latency** for users ⚡
  - Lower **network traffic costs** 💸
- Helps design **robust, fault-tolerant systems**.

---

## 🔐 Security Best Practices

### Internal IPs & Cloud NAT

- Assign **internal IP addresses** to VM instances whenever possible.
- **Cloud NAT**:
  - Managed Network Address Translation service
  - Lets private instances access the internet for:
    - Updates
    - Patching
    - Configuration management
  - Provides **outbound NAT**
  - Does **not allow inbound NAT** → private instances stay isolated 🛡️

---

### Private Google Access

- Allows VM instances **with only internal IPs** to reach Google APIs and services 🌐
- Enable **per subnet**:
  - **Subnet A** → private Google access enabled ✅
  - **Subnet B** → private Google access disabled ❌
- Only instances in **subnets with private access** can use Google APIs without external IPs.
- **Instances with external IPs** are unaffected.

---

💡 **Key Takeaways**

- Use **multi-zone subnets** for higher availability.
- Use **multi-region placement** for global fault tolerance.
- Use **internal IPs + Cloud NAT** to enhance security while allowing internet access.
- Enable **private Google access** for private VMs needing Google API connectivity.
