# 📡 Packet Mirroring

## 🔍 What Is Packet Mirroring?
Packet Mirroring **clones all ingress and egress VM traffic** (headers + payloads) in a VPC and sends it to a collector for deep inspection.  
Useful for **security**, **forensics**, and **network/application monitoring**.

---

## ⭐ Key Features
- Captures **full traffic**, not sampled traffic.
- Mirrors traffic **at the VM level** (not at the network level).
- Generates **significant bandwidth and data**, so consider collector capacity.
- Supports **up to 30 filters** to limit mirrored traffic.

---

## 🎯 Use Cases
### 🔐 Security & Compliance
- Supports **zero-trust** by inspecting traffic across boundaries.
- Useful for:
  - **Intrusion Detection Systems (IDS)**
  - **Deep Packet Inspection (DPI)**
  - **PCI DSS forensics**
  - Detecting anomalies, threats, and attack vectors.

### 🌐 Network & Application Monitoring
- Analyze packet loss, latency, reconnections.
- Investigate real-time traffic patterns.
- Maintain deployment integrity.

---

## 📈 Monitoring & Metrics
Packet Mirroring exports metrics to **Cloud Monitoring**, including:
- **Mirrored packets count**
- **Mirrored bytes count**
- **Dropped packets count**
- Metrics for both mirrored VMs and collector instances.

Useful for:
- Verifying if mirroring works correctly.
- Detecting unnecessary or unexpected mirroring configurations.

---

## ⚠️ Limitations & Considerations
- Consumes **egress bandwidth** of mirrored VMs.
- May **slow VM throughput**.
- Can **expose sensitive data** if not handled carefully.
- Generates **large volumes of data** requiring storage + processing.

---

## 🎛️ Optimization Tip
Use **filters** (by IP ranges, protocols, directions, etc.) to reduce mirrored traffic and control costs/performance impact.

