# 🔹 Internal Load Balancing (TCP/UDP & HTTPS)

Internal Load Balancing in Google Cloud is a **regional, private load balancing service** designed for traffic that stays within your VPC network.

---

## 1️⃣ Internal TCP/UDP Load Balancing

- **Scope:** Regional, private (internal IP only)  
- **Traffic type:** TCP and UDP-based traffic  
- **Frontend:** Configured with an **internal IP address**  
- **Accessibility:** Only accessible to VMs in the same region  
- **Latency & efficiency:** Traffic remains within Google’s network → lower latency and simpler configuration  

### How it works:

- **Software-defined, fully distributed load balancing** (not device/VM-based)  
- Uses **Andromeda**, Google’s network virtualization stack, to deliver traffic directly from client to backend instance  
- Eliminates the traditional two-hop proxy model:  
  1. Client → Load Balancer  
  2. Load Balancer → Backend  
- Traffic flows directly to the backend instance for efficiency  

---

## 2️⃣ Internal HTTPS Load Balancing

- **Scope:** Regional, Layer 7 (application layer)  
- **Traffic type:** HTTP, HTTPS, HTTP/2  
- **Backend:** Supports **Envoy-based proxies** for rich traffic control  
- **Managed service:** Automatically allocates proxies based on traffic needs  
- **Use case:** Run scalable internal services behind private IPs  

---

## 3️⃣ Use Case Example: Three-Tier Web Application

1. **Web Tier:** External HTTPS load balancer with a global IP → serves users worldwide  
2. **Application/Internal Tier:** Internal load balancers in each region → serves the application tier  
3. **Database Tier:** Internal database instances in each zone  

### Benefits:

- Data and application tiers remain **internal** (not exposed externally)  
- Simplified **security** and **network pricing**  
- Supports traditional multi-tier architectures with internal-only access for sensitive layers  

> ⚡ More on Andromeda: see course resources.
