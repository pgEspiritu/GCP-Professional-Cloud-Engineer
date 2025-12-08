# ⚡ TCP Proxy Load Balancing

TCP Proxy Load Balancing is a **global load balancing service** for **unencrypted, non-HTTP traffic**. It terminates TCP sessions at the load balancer and forwards traffic efficiently to backend instances.

---

## 1️⃣ Key Features

- **Global load balancing** for TCP traffic 🌍  
- Terminates user **TCP connections at the load balancer**  
- Forwards traffic to backend instances using **TCP or SSL**  
- Supports **IPv4 and IPv6** client traffic  
- Provides **intelligent routing** and **automatic security patching**  

---

## 2️⃣ How it Works

1. User traffic reaches the **global TCP proxy**  
2. Load balancer terminates TCP connections at the edge  
3. Traffic is routed to the **closest backend region** with available capacity  
4. Communication between the load balancer and backend can use **TCP or SSL**, with SSL recommended ✅  

> Example: Users in Boston reach the US-east backend, while users in Iowa reach the US-central backend if capacity is available.

---

## 3️⃣ Benefits

- 🌐 **Global reach** with intelligent routing  
- ⚡ **Efficient traffic distribution** across multiple regions  
- 🛡️ **Security patching** applied automatically at the load balancer  
- 🔄 **Centralized connection termination** reduces management overhead on backend instances  

> ⚡ For a full list of supported ports and detailed benefits, see Google Cloud documentation.
