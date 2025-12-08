# 🌐 Overview of HTTP(S) Load Balancing

Google Cloud's **HTTP(S) Load Balancing** operates at **Layer 7 (Application Layer)** of the OSI model, allowing routing decisions based on **content and URL**.

---

## 1️⃣ Key Features

- **Global Load Balancing:** Single **anycast IP address** for your application 🌎  
- **Traffic Types:** Balances **HTTP (port 80/8080)** and **HTTPS (port 443)**  
- **Cross-Regional Support:** Distributes traffic across multiple regions  
- **Scalable & No Pre-Warming Required** ⚡  
- **IPv4 and IPv6 Support**  
- **Content-Based Routing:** URL maps allow routing different URLs to different backends  

---

## 2️⃣ Request Routing

1. **Incoming requests** hit the **Global Forwarding Rule**  
2. Requests are sent to a **Target HTTP(S) Proxy**  
3. The **HTTP(S) Proxy** checks the **URL map** to determine the proper **Backend Service**  
4. Example:  
   - `www.example.com/audio` → Audio backend service  
   - `www.example.com/video` → Video backend service  

---

## 3️⃣ Backend Services

- **Components:**  
  - **Instance Group:** Contains VM instances (managed or unmanaged, with or without autoscaling)  
  - **Health Check:** Determines instance availability ✅  
  - **Session Affinity:** Optional; ensures requests from the same client go to the same VM  
  - **Timeout Setting:** Default 30 seconds; waits before marking a request as failed ⏱️  
  - **Balancing Mode:** Determines when backend is considered full  
  - **Capacity Scaler:** Adjusts backend usage relative to balancing mode  

- **Balancing Modes:**  
  - **CPU Utilization** (e.g., 80% max)  
  - **Requests Per Second (RPS)**  
- **Capacity Example:**  
  - Balancing mode = 80% CPU, Capacity = 100% → full usage at 80% CPU  
  - Capacity = 50% → reduces instance utilization by half  

---

## 4️⃣ Health Checks

- Poll instances at configured intervals  
- **Healthy instances:** Receive requests ✅  
- **Unhealthy instances:** Do not receive requests until recovery ❌  

---

## 5️⃣ Traffic Management

- **Round-Robin:** Default distribution of requests  
- **Session Affinity:** Overrides round-robin to maintain client session consistency  
- **Cross-Region Failover:** If all backends in a region are full, traffic routes to nearest region with available capacity  

> ⚡ **Note:** Changes to backend services are **not instantaneous**; propagation can take several minutes.
