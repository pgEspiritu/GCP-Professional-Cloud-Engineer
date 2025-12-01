# 🌐 Network Intelligence Center

## 🧭 Overview
Network Intelligence Center (NIC) provides **centralized monitoring, visibility, troubleshooting, and security insights** for your Google Cloud network.  
It contains **five major modules** that help analyze performance, connectivity, and configuration issues.

---

# 🧩 Modules

## 1️⃣ **Network Topology**
- Visualizes your entire GCP network in a **graph view**.
- Helps troubleshoot quickly by showing:
  - Network entities
  - Communication lines with bandwidth
  - Filters, hierarchy views, time ranges

---

## 2️⃣ **Connectivity Tests**
- Diagnoses **connectivity issues** in real time.
- Tests:
  - VM-to-VM
  - GCP-to-external IP (including on-prem or other clouds)
- Helps verify:
  - If an outage is caused by Google Cloud or external factors
  - The impact of configuration changes
  - Network security and compliance

---

## 3️⃣ **Performance Dashboard**
Provides **performance visibility** for VPCs through:

### 📉 Packet Loss
- Uses workers on **physical hosts**, not VMs.
- Detects issues on the network path between zones.
- No impact on VM resources.

### 🚀 Latency
- Uses samples from **actual TCP traffic**.
- Measures RTT (Round Trip Time) based on SEQ/ACK timing.
- Requires ~1000 TCP packets/min to display metrics.

---

## 4️⃣ **Firewall Insights**
Improves firewall rule quality and security.

### 🔍 Provides:
- Rule usage metrics  
- Misconfiguration detection  
- Recommendations for tighter rules  

### 📊 Powered by:
- **Cloud Monitoring metrics**
- **Recommender insights**

### 💡 What you can do:
- Verify intended allow/deny behavior over time  
- Live-debug dropped connections  
- Detect malicious access attempts via alerts  

---

## 5️⃣ **Network Analyzer**
Continuously monitors your VPC for **misconfigurations and failures**.

### 🛠️ Detects:
- Firewall issues  
- Route errors  
- Load balancing issues  
- Unused external IPs  
- GKE network misconfigurations  
- Invalid next-hop routes  

### 🤖 Features:
- Runs continuously in near real-time  
- Correlates failures with recent config changes  
- Provides **root cause analysis** and **recommended fixes**

### 📝 Example:
- **Issue:** GKE node cannot reach control plane  
- **Root cause:** Ingress firewall rule blocking the connection  
- **Fix:**  
  - If the rule was deleted → recreate it  
  - If shadowed → increase priority  

---

# ✅ Final Summary
Network Intelligence Center helps you:
- Visualize your network  
- Diagnose connectivity failures  
- Monitor performance (packet loss, latency)  
- Improve firewall security  
- Automatically detect and fix network misconfigurations  

It is an essential toolkit for maintaining **secure, reliable, and high-performance** GCP networks.  
