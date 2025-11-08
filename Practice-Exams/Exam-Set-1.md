# 1. Google Cloud Case Study: Public Health Information Website Migration

## Question

Anonymous users from all over the world access a public health information website hosted in an on-premises EHR data center. The servers that host this website are older, and users are complaining about sluggish response times. There has also been a recent increase of distributed denial-of-service attacks toward the website. The attacks always come from the same IP address ranges. EHR management has identified the public health information website as an easy, low risk application to migrate to Google Cloud. You need to improve access latency and provide a security solution that will prevent the denial-of-service traffic from entering your Virtual Private Cloud (VPC) network. What should you do?

A. Deploy an external HTTP(S) load balancer, configure VPC firewall rules, and move the applications onto Compute Engine virtual machines.  
B. Deploy an external HTTP(S) load balancer, configure Google Cloud Armor, and move the application onto Compute Engine virtual machines.  
C. Containerize the application and move it into Google Kubernetes Engine (GKE). Create a GKE service to expose the pods within the cluster, and set up a GKE network policy.  
D. Containerize the application and move it into Google Kubernetes Engine (GKE). Create an internal load balancer to expose the pods outside the cluster, and configure Identity-Aware Proxy (IAP) for access.  

---

## ✅ **Correct Answer:** B  
**Deploy an external HTTP(S) load balancer, configure Google Cloud Armor, and move the application onto Compute Engine virtual machines.**

---

## 🧠 Explanation

### 1. **External HTTP(S) Load Balancer**
- The **External HTTP(S) Load Balancer** is a **global**, fully-distributed load balancing service that provides **low latency** and **high availability**.
- It operates at the **edge of Google’s network**, meaning user traffic is terminated **before** it reaches the VPC.
- This helps reduce latency for users across the world by directing traffic to the nearest Google edge location.

### 2. **Google Cloud Armor**
- **Cloud Armor** integrates with the external HTTP(S) load balancer to provide **DDoS protection** and **WAF (Web Application Firewall)** capabilities.
- It can filter requests **before** they reach your backend services.
- You can configure **IP-based rules** to **block known malicious IP ranges** identified from the previous DDoS attacks.
- This ensures the **malicious traffic never enters the VPC**, improving both **security** and **performance**.

### 3. **Compute Engine Virtual Machines**
- Migrating the web application to **Compute Engine VMs** is a straightforward way to lift and shift from on-premises servers.
- You can still achieve scalability by using **managed instance groups** behind the load balancer.
- This option fits the “easy, low-risk” migration criteria stated in the scenario.

---

## ❌ Why Not the Other Options?

### **A. VPC Firewall Rules**
- VPC firewall rules act **inside the VPC**, meaning DDoS traffic would still reach your network before being filtered.
- This doesn’t protect your infrastructure from **high-volume external attacks**.

### **C. GKE with Network Policies**
- While **GKE** offers scalability and flexibility, it adds unnecessary **complexity** for a “low-risk” public site.
- Network policies in GKE control **pod-to-pod communication**, not **external DDoS traffic**.

### **D. Internal Load Balancer with IAP**
- **Internal Load Balancers** are for **private** applications, not public-facing websites.
- **IAP (Identity-Aware Proxy)** is used for authenticated access, which doesn’t fit a **public health information** site meant for **anonymous users**.

---

## 🏁 Summary
The best solution provides:
- 🌐 **Global load balancing** for low latency  
- 🛡️ **DDoS protection at the edge** via Cloud Armor  
- ☁️ **Simple migration** path using Compute Engine  

✅ **Final Answer:** **B**

---

# 2. Google Cloud Networking: Connectivity Recommendation

## Question

EHR wants to connect one of their data centers to Google Cloud. The data center is in a remote location over 100 kilometers from a Google-owned point of presence. They can’t afford new hardware, but their existing data center can accomodate future throughput growth. Tey also shared these data points:

- Servers in their on-premises data center need to talk to Google Kubernetes Engine (GKE) resources in the cloud.  
- Both on-premises servers and cloud resources are configured with private RFC 1918 IP addresses.  
- The service provider has informed the customer that basic Internet connectivity is a best-effort service with no SLA.

You need to recommend a connectivity option. What should you recommend?

A. Provision Carrier Peering.  
B. Provision a new Internet connection.  
C. Provision a Partner Interconnect connection.  
D. Provision a Dedicated Interconnect connection.  

---

## ✅ **Correct Answer: C**  
**Provision a Partner Interconnect connection.**

---

## Explanation

**Why Partner Interconnect (C) is the right choice**

- **Distance constraint:** The data center is **>100 km from a Google PoP**, so a Dedicated Interconnect (which requires colocating at a Google edge/colo and direct physical cross-connect) is impractical or impossible without moving or adding hardware near a Google facility.  
- **No new hardware budget:** Partner Interconnect lets you connect via a **supported service provider/partner** that already has connectivity into Google’s network — you don’t need to install new Google-facing colo hardware yourself.  
- **Private RFC1918 connectivity:** Partner Interconnect supports **private connectivity** to your VPC (using VLAN attachments and Cloud Router with BGP), so on-prem RFC1918 addresses can route to GKE/private cloud resources securely and privately.  
- **Better than best-effort Internet:** The service provider partner can offer **SLA-backed connectivity** options (and typically better performance and predictability than “basic Internet”), addressing the customer’s concern that basic Internet is best-effort with no SLA.  
- **Scalability:** Partner Interconnect is designed to scale (you can order multiple VLAN attachments/partner circuits) to accommodate future throughput growth without the upfront capital expense of Dedicated Interconnect.

**Why the other options are not appropriate**

- **A — Carrier Peering:** Carrier (or Public) Peering is for exchanging **public Internet** traffic with Google (reachability to Google’s public IPs and services). It does **not** provide private RFC1918 connectivity into a VPC and is therefore not suitable for on-prem → GKE private IP communication.  
- **B — New Internet connection:** A plain Internet connection is **best-effort** and has no guaranteed SLA; it also exposes traffic to the public Internet (unless you build VPN on top). The scenario specifically calls out that the provider labels Internet as best-effort, so this fails the reliability/security requirement.  
- **D — Dedicated Interconnect:** Dedicated Interconnect provides a private, high-capacity connection but **requires colocation at a Google edge/colo** and physical cross-connects; because the site is remote (>100 km from a PoP) and they can’t afford new hardware/colocation, Dedicated Interconnect is not feasible.

---

## Implementation notes (practical next steps)
- Engage a **Partner Interconnect** service provider that has presence reachable from the EHR data center.  
- Configure **VLAN attachments** from the provider into your Google Cloud project and attach them to the appropriate VPC.  
- Use **Cloud Router + BGP** to exchange routes dynamically between on-prem RFC1918 subnets and the VPC (GKE private clusters or node pools).  
- Consider **redundant partner circuits** (different providers or diverse paths) for high availability.  
- If encryption is required and the provider does not encrypt by default, consider IPsec tunnels on top of the partner link or ensure provider offers an encrypted service.

---

**Final Answer:** **C — Provision a Partner Interconnect connection.**
