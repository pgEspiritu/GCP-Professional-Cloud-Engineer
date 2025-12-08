# 🌐 Cloud Interconnect and Peering

Google Cloud offers **Cloud Interconnect and Peering services** to connect your infrastructure to Google’s network.

---

## 1️⃣ Types of Connections

Connections can be classified as:

- **Dedicated vs Shared**  
  - **Dedicated:** Direct connection to Google’s network 🔗  
  - **Shared:** Connection via a partner network 🤝  

- **Layer 2 vs Layer 3**  
  - **Layer 2:** Uses a VLAN that connects directly to your GCP environment, providing access to **internal IP addresses** (RFC 1918) 🏠  
  - **Layer 3:** Provides access to **Google Workspace, YouTube, and Cloud APIs** using **public IP addresses** 🌍  

---

## 2️⃣ Available Services

- **Direct Peering** – Directly peers your network with Google’s network  
- **Carrier Peering** – Connect via a service provider  
- **Dedicated Interconnect** – Direct physical connection to Google’s network  
- **Partner Interconnect** – Shared connection through a partner, supports Layer 2 and Layer 3  

---

## 3️⃣ Relation to Cloud VPN

- **Cloud VPN:** Uses the **public internet**, encrypts traffic, and provides access to **internal IP addresses** 🔐  
- Cloud VPN complements **Direct Peering** and **Carrier Peering** for secure connectivity.  
