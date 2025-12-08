# 🌐 Google Cloud VPN Gateways

Google Cloud offers **two types of Cloud VPN gateways**:

- **Classic VPN**  
- **HA VPN (High Availability VPN)**

---

## 1️⃣ Classic VPN

- **Definition:** Connects your on-premises network to a Google Cloud VPC network through an IPsec VPN tunnel.  
- **Traffic:** Encrypted by one VPN gateway, decrypted by the other, ensuring security over the public internet.  
- **Use case:** Best for **low-volume data connections**.  
- **Managed service SLA:** 99.9% service availability ✅  
- **Supported features:**
  - Site-to-site VPN 🌉  
  - Static and dynamic routes  
  - IKEv1 and IKEv2 ciphers  
- **Limitations:**  
  - Does **not support client VPN software** for individual computers.  
  - Dynamic routes require **Cloud Router**.  

### 📌 Example: Classic VPN Setup

- VPC network with subnets in **us-east1** and **us-west1**  
- Google Cloud resources communicate internally via internal IPs (firewall rules permitting)  
- Required components:  
  - Cloud VPN gateway (regional resource with external IP)  
  - On-premises VPN gateway (physical or software-based)  
  - **Two VPN tunnels** connecting both gateways  

> ⚠️ MTU Consideration: On-premises VPN gateway MTU cannot exceed **1460 bytes** due to encryption and encapsulation.

---

## 2️⃣ HA VPN

- **Definition:** High availability Cloud VPN connecting on-premises network to VPC in a single region.  
- **SLA:** 99.99% service availability ✅✅  
- **Tunnels:** Two or four tunnels required for guaranteed SLA  
- **IP Addresses:** Two external IPs automatically chosen from unique pools, one per interface  
- **Interfaces:** Each supports multiple tunnels; multiple HA VPN gateways can be created  

### ⚠️ Notes

- Configuring only one interface and IP will **not provide 99.99% SLA**  
- VPN tunnels must use **dynamic (BGP) routing**  
- Route priorities allow **active/active** or **active/passive** configurations  

### 🔹 Recommended HA VPN Topologies

1. **HA VPN to peer VPN devices**  
2. **HA VPN to AWS virtual private gateway**  
3. **Two HA VPN gateways connected to each other**  

#### Example 1: HA VPN to Two Peer Devices

- Each peer device: one interface & external IP  
- HA VPN gateway: two tunnels (one per peer)  
- Provides **redundancy & failover**  
- Google Cloud `REDUNDANCY_TYPE`: `TWO_IPS_REDUNDANCY`  
- SLA: 99.99%  

#### Example 2: HA VPN to AWS

- Components:  
  - HA VPN gateway in Google Cloud with 2 interfaces  
  - 2 AWS virtual private gateways  
  - External VPN gateway resource in Google Cloud  
- Total tunnels: 4 (2 per AWS gateway to HA VPN interfaces)  
- Supports **ECMP routing** (equal-cost multipath) for traffic distribution  

#### Example 3: HA VPN Between Two GCP VPCs

- Each HA VPN gateway creates **2 tunnels**  
- Connect interface 0 → interface 0, interface 1 → interface 1  
- SLA: 99.99%  

> 📖 More info: Refer to course documentation for HA VPN migration.

---

## 3️⃣ Routes and Cloud Router

- Cloud VPN supports **static and dynamic routes**  
- **Dynamic routes** require **Cloud Router**  
- Cloud Router manages routes using **BGP** (Border Gateway Protocol)  
- Allows automatic route updates **without changing tunnel config**  

### 🔹 Example: Adding New Subnets

- VPC subnets: Test & Prod  
- On-premises: 29 subnets  
- Add new “Staging” subnet in VPC and `10.0.30.0/24` subnet on-premises  
- **BGP session** automatically advertises new subnets  
- New instances can immediately communicate  

### ⚠️ BGP Setup

- Assign **link-local IP addresses** to each VPN tunnel endpoint  
- IP range: `169.254.0.0/16`  
- Used exclusively for BGP session, not part of either network’s IP space
