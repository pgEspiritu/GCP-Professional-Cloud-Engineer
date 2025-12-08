# ⚡ Network Load Balancing (Regional)

Network Load Balancing is a **regional, non-proxied load balancing service**. Unlike global load balancers, all traffic passes directly through the load balancer to backend instances in the **same region**.

---

## 1️⃣ Key Features

- **Regional scope** 🌍 – Traffic can only be distributed to VMs within the same region  
- **Non-proxied** – Traffic flows directly through the load balancer  
- Uses **forwarding rules** to route traffic based on IP protocol data (address, port, protocol)  
- Supports **TCP, UDP, and SSL traffic**, including ports not supported by TCP/SSL proxy load balancers  
- Integrates with **managed instance groups** for autoscaling and connection draining  

---

## 2️⃣ Architecture

Network load balancers can use either:

### a) **Back-end service-based NLB**
- Defines load balancer behavior and traffic distribution across backend instance groups  
- Supports advanced features:  
  - Non-legacy health checks  
  - TCP, SSL, HTTP, HTTPS, HTTP/2 auto-scaling  
  - Connection draining  
  - Configurable failover policy  

### b) **Target-pool-based NLB (legacy)**
- A **target pool** defines a group of instances receiving traffic from forwarding rules  
- Load balancer selects an instance based on a **hash of source/destination IP and port**  
- Supports **TCP and UDP traffic only**  
- Each project can have **up to 50 target pools**  
- Each target pool can only have **one health check**  
- All instances must be in the **same region**  

> ⚡ You can transition existing target-pool-based NLBs to use back-end services for more features.

---

## 3️⃣ Use Cases

- Regional applications requiring **high throughput and low latency**  
- Load balancing **TCP/UDP traffic** for protocols not supported by global proxy load balancers  
- Situations where **traffic must remain within a region**
