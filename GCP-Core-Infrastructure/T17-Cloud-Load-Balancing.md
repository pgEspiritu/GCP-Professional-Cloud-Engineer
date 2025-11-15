# ⚖️ Cloud Load Balancing

## 🚀 Why Load Balancing Matters
Earlier, we learned how virtual machines can **autoscale** to meet changing workloads.  
But how do customers reach your application when it might run on **4 VMs now** and **40 VMs later**?  
👉 **Cloud Load Balancing** solves this.

A load balancer’s job is to **distribute user traffic** across multiple application instances.  
This ensures:
- Better performance ⚡
- Higher reliability ✔️
- No single VM becomes overloaded ❌💥

---

## ☁️ What Is Cloud Load Balancing?
Google Cloud Load Balancing is:
- **Fully distributed**  
- **Software-defined**  
- **Completely managed** (no VMs to configure or scale!)

You can use it for **all** traffic types:
- 🌐 HTTP / HTTPS  
- 🔒 TCP / SSL  
- 📡 UDP  

It also supports **cross-region load balancing**, including **automatic multi-region failover**, smoothly shifting traffic if backends become unhealthy.

### ⚡ Fast & Responsive
Cloud Load Balancing reacts quickly to:
- Traffic spikes  
- User location changes  
- Backend health  
- Network conditions  

And the best part?  
👉 **No pre-warming required** — even if your app suddenly goes viral! 🎮🔥

---

# 🧩 Types of Load Balancers in Google Cloud

Google Cloud offers multiple load balancing solutions categorized by **OSI layer** and **functionality**.

---

## 🟦 1. Application Load Balancers (Layer 7)
Application Load Balancers:
- Handle **HTTP/HTTPS** traffic 🌐  
- Ideal for modern web apps  
- Support advanced features:
  - Content-based routing  
  - SSL/TLS termination  
  - URL/path-based rules  
- Work as **reverse proxies**  
- Can be **external or internal**

Perfect for:  
✔️ Websites  
✔️ APIs  
✔️ Microservices architectures

---

## 🟩 2. Network Load Balancers (Layer 4)
Operate at the **transport layer**, handling:
- TCP  
- UDP  
- Other IP protocols

There are *two types*:

### 🔷 a. Proxy Network Load Balancers
- Act as **reverse proxies**  
- Terminate client connections and open new ones to the backend  
- Support:
  - Traffic management  
  - Hybrid/on-prem backends  
- Can work across multiple environments 🌍

### 🔶 b. Passthrough Network Load Balancers
- Do **not** modify or terminate connections  
- Forward traffic directly to backend instances  
- Preserve the **original client IP**  
- Ideal for:
  - Direct server return  
  - Apps needing full IP protocol support
