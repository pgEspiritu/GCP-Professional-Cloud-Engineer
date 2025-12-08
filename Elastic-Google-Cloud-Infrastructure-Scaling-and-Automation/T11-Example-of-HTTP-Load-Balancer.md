# 🌐 Example of HTTP Load Balancer in Action

This example demonstrates how **HTTP load balancing** routes user traffic to backend instances across multiple regions using a **single global IP address**.

---

## 1️⃣ Scenario

- **Global IP:** Single IP for the project 🌍  
- **User Locations:**  
  - North America  
  - EMEA (Europe, Middle East, Africa)  
- **Application:** Guestbook app with **one backend service**  

---

## 2️⃣ Backend Service Configuration

- **Backends:**  
  - **us-central1-a** → Managed Instance Group  
  - **europe-west1-d** → Managed Instance Group  
- **Managed Instance Groups** ensure scalability, autoscaling, and health check integration ✅  

---

## 3️⃣ Request Routing Logic

1. **Global Forwarding Rule** directs incoming requests to the **Target HTTP Proxy**  
2. **HTTP Proxy** checks the **URL map** to determine the backend service  
3. **Routing Decision Factors:**  
   - User’s approximate **geographic location** from source IP 🌎  
   - Location of **backend instances**  
   - **Capacity and current usage** of backend instances  
4. **Closest Available Backend:** Requests are sent to the nearest backend with available capacity  
   - North America users → `us-central1-a`  
   - EMEA users → `europe-west1-d`  

---

## 4️⃣ Traffic Distribution

- Requests within a region are **evenly distributed** across all healthy instances  
- If no healthy instance with capacity is available in the nearest region, traffic is sent to the **next closest region**  
  - This is called **cross-region load balancing** 🔄  

---

## 5️⃣ Content-Based Load Balancing

- **Multiple backend services** handle different types of traffic  
  - `/video` → Video backend service 🎥  
  - All other URLs → Web backend service 🌐  
- **Single global IP** manages all routing and traffic distribution  
- Achieved via **URL maps** on the HTTP(S) proxy  

> ⚡ **Key takeaway:** HTTP(S) Load Balancing provides **global, cross-region, content-based routing** with **high availability** and a **single IP address**.
