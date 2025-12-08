# 🔐 SSL Proxy Load Balancing

SSL Proxy Load Balancing is a **global load balancing service** for **encrypted non-HTTP traffic**. It terminates SSL connections at the load balancer and distributes traffic to backend instances efficiently.

---

## 1️⃣ Key Features

- **Global load balancing** for SSL-encrypted traffic 🌍  
- Terminates user **SSL connections at the load balancer**  
- Distributes traffic to backend instances across **multiple regions** based on capacity  
- Supports both **IPv4 and IPv6** client traffic  
- Provides **intelligent routing**, **certificate management**, **security patching**, and **SSL policies** 🔒  

---

## 2️⃣ How it Works

1. User traffic reaches the **global SSL proxy**  
2. Load balancer terminates SSL connections at the edge  
3. Traffic is routed to the **closest backend region** with available capacity  
4. Communication between the load balancer and backend can use **SSL or TCP**, with SSL recommended ✅  

> Example: Users in Boston reach the US-east backend, while users in Iowa reach the US-central backend if capacity is available.

---

## 3️⃣ Certificate Management

- Only need to update customer-facing certificates at the **load balancer**  
- Backend instances can use **self-signed certificates** to reduce management overhead  
- Google automatically **applies security patches** to SSL/TCP stack at the load balancer  

---

## 4️⃣ Benefits

- 🌐 **Global reach** with intelligent routing  
- 🔒 **Centralized certificate management**  
- ⚡ **Reduced operational overhead** for backend instances  
- 🛡️ **Automatic SSL/TCP patching** to maintain security  

> ⚡ For the full list of supported ports and detailed benefits, see Google Cloud documentation.
