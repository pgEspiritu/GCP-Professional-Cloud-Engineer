## 🌐 VPC Flow Logs

### 🔎 Overview
**VPC Flow Logs** capture a **sampled record (≈1 in 10 packets)** of network flows **sent from or received by VM instances**, including **GKE nodes**.

They are useful for:
- 📡 Network monitoring  
- 📊 Traffic analysis  
- 🔐 Security & threat detection  
- 🕵️ Forensics  
- 💰 Cost optimization  

VPC Flow Logs are part of **Andromeda**, Google’s virtual network stack, and introduce **no performance impact** when enabled.

---

## 🚦 How VPC Flow Logs Work

### 🖥️ VM to External Traffic Example
VPC Flow Logs reveal traffic patterns, such as flows:
- Between VMs  
- VM → on-prem (via Cloud VPN / Interconnect)  
- VM → Internet  

The logs collect:
### **📥 Ingress Traffic**
- VM is the **destination**
- Example: `10.30.0.2 → 10.10.0.2`

### **📤 Egress Traffic**
- VM is the **source**
- Example: `10.10.0.2 → 10.30.0.2`

---

## 📌 Key Characteristics to Remember

- 🔍 **Sampling is from the VM’s perspective**  
- ❌ **Egress-denied packets are logged** (sampled before firewall evaluation)  
- ❌ **Ingress-denied packets are NOT logged** (sampling happens after firewall rules)
- 📦 Supports **TCP, UDP, ICMP, ESP, GRE**
- 🔄 Records **both inbound and outbound** flows for:
  - VM ↔ VM  
  - VM ↔ On-prem  
  - VM ↔ Internet  
- 🖧 Supports multiple NICs per VM  
- 🕹️ Enabled **at the subnet level**  

---

## 🔧 How to Enable VPC Flow Logs
When creating a subnet:
1. Go to **Flow Logs**
2. Set to **ON**
3. (Optional) Configure:
   - 📉 Sample rate  
   - 🧩 Metadata fields  
   - 🕒 Aggregation interval  

---

## 🧱 What’s Inside a Flow Log Entry?

Each entry includes a wide set of fields:

### **🔑 5-Tuple (Core Connection Info)**
- Source IP  
- Source port  
- Destination IP  
- Destination port  
- Protocol  

### **⏱️ Timestamps**
- Start time  
- End time  

### **📦 Traffic Counts**
- Packets sent  
- Bytes sent  

### **🖥️ VM Metadata**
- Instance name  
- Network tags  
- Subnet & VPC details  
- Region & zone  

(See Google documentation for full field list.)

---

## 🗂️ Accessing VPC Flow Logs

### 📜 Logs Explorer
- Navigate to **Logs Explorer**
- Look for entries under: `vpc_flows`
- Or simply search: `vpc_flows`

---

## 📊 Flow Log Analysis with Log Analytics & BigQuery

With **Log Analytics** powered by **BigQuery**, you can:

- ⚡ Run **ad-hoc queries** (no pre-processing needed)
- 🔗 Link buckets to a BigQuery dataset
- 🧮 Perform advanced aggregation & analysis
- 📈 Use curated sample queries for quick insights

This unlocks more powerful network analytics than standard Logs Explorer.

---

For deeper details, refer to Google Cloud’s official VPC Flow Logs documentation. 📘
