# 🏥 EHR Healthcare

EHR Healthcare is a leading provider of **electronic health record (EHR) software** to the medical industry. EHR Healthcare provides its software as a service to **multi-national medical offices, hospitals, and insurance providers**.

Due to rapid changes in the healthcare and insurance industry, EHR Healthcare’s business has been growing exponentially year over year. They need to scale their environment, adapt their **disaster recovery plan**, and roll out **continuous deployment** capabilities to update their software at a fast pace. **Google Cloud** has been chosen to replace their current colocation facilities, and the lease on one of the data centers is about to expire.

---

## 🏢 Company Overview

- Software is hosted in **multiple colocation facilities**
- One data center lease is expiring
- Customer-facing applications are **web-based**
- Many applications are **containerized** and run on **Kubernetes clusters**
- Data is stored using:
  - **MySQL**
  - **Microsoft SQL Server**
  - **Redis**
  - **MongoDB**

---

## 💡 Solution Concept

Migrate EHR Healthcare’s infrastructure to **Google Cloud** to support:

- High availability
- Continuous deployment
- Disaster recovery
- Scalable infrastructure
- Advanced analytics and predictive insights

---

## 🧱 Existing Technical Environment

- Hosted in **on-premises colocation facilities**
- Applications running on **Kubernetes clusters**
- Data stored in a mix of **relational and NoSQL databases**
- **Legacy file- and API-based integrations** with insurance providers remain on-premises
- No current plan to migrate or modernize legacy systems
- Users managed via **Microsoft Active Directory**
- Monitoring uses **open-source tools**
- Alerts sent by **email**, often ignored

---

## 📋 Business Requirements

- Onboard new insurance providers quickly
- Provide at least **99.9% availability**
- Centralized system monitoring and visibility
- Gain insights into healthcare trends
- Reduce customer latency
- Maintain regulatory compliance
- Reduce infrastructure administration costs
- Generate predictions and reports on industry trends

---

## ⚙️ Technical Requirements

- Maintain **legacy insurance provider interfaces**
- Support **container-based applications**
- Secure and high-performance connectivity between **on-premises and Google Cloud**
- Centralized logging, monitoring, alerting, and retention
- Manage multiple Kubernetes environments
- Enable **dynamic scaling and provisioning**
- Create interfaces to ingest and process data from new providers

---

## 🧑‍💼 Executive Statement

> Our on-premises strategy has worked for years but has required a major investment of time and money in training our team on different systems, managing separate environments, and responding to outages. Many outages were caused by misconfigured systems, inadequate capacity, and inconsistent monitoring.  
>  
> We want to use **Google Cloud** to leverage a scalable and resilient platform that spans multiple environments and provides a consistent and stable user experience that supports future growth.

---

# 1. Privacy Compliance for EHR Healthcare (Case Study)

## Question

For this question, refer to the **EHR Healthcare** case study. You are responsible for ensuring that EHR's use of **Google Cloud** will pass an upcoming **privacy compliance audit**. What should you do? (**Choose two.**)

a. Verify EHR's product usage against the list of compliant products on the Google Cloud compliance page.  
b. Advise EHR to execute a **Business Associate Agreement (BAA)** with Google Cloud.  
c. Use **Firebase Authentication** for EHR's user facing applications.  
d. Implement **Prometheus** to detect and prevent security breaches on EHR's web-based applications.  
e. Use **GKE private clusters** for all Kubernetes workloads.  

---

## ✅ Correct Answers

**a** and **b**

---

## Explanation

Healthcare data handled by EHR is subject to **HIPAA** and other privacy regulations. To pass a privacy compliance audit, two critical requirements must be met:

- **Google Cloud services used must be HIPAA-compliant**  
- **A legal agreement must exist allowing Google to process protected health information (PHI)**  

**A. Verify compliant services**  
Google Cloud only permits PHI on specific **HIPAA-compliant products**. Verifying usage against the compliance list ensures EHR is not using unsupported services.

**B. Execute a Business Associate Agreement (BAA)**  
A **BAA** is a **legal requirement** under HIPAA. Without it, EHR is not allowed to store or process PHI on Google Cloud.

---

## ❌ Why the other options are wrong

**c. Firebase Authentication**  
Helps with login management but does **not ensure HIPAA compliance**.

**d. Prometheus**  
A monitoring tool, not a **privacy or compliance control**.

**e. GKE private clusters**  
Improves security, but **does not satisfy legal or compliance requirements** by itself.

---

## ✅ Final Answer

**a and b**

---

# 2. Secure Container Deployment Architecture (EHR Healthcare Case Study)

## Question

For this question, refer to the **EHR Healthcare** case study. You need to define the **technical architecture** for securely deploying workloads to **Google Cloud**. You also need to ensure that **only verified containers are deployed** using Google Cloud services. What should you do? (**Choose two.**)

a. Enable **Binary Authorization** on GKE, and **sign containers** as part of a CI/CD pipeline.  
b. Configure **Jenkins** to utilize **Kritis** to cryptographically sign a container as part of a CI/CD pipeline.  
c. Configure **Container Registry** to only allow trusted service accounts to create and deploy containers from the registry.  
d. Configure **Container Registry** to use **vulnerability scanning** to confirm that there are no vulnerabilities before deploying the workload.  

---

## ✅ Correct Answers

**a** and **b**

---

## Explanation

To ensure **only verified containers** are deployed, HRL must enforce **cryptographic verification** of container images before they run.

### **A. Enable Binary Authorization**
**Binary Authorization** is a **GKE admission controller** that:
- Blocks unsigned or untrusted containers  
- Only allows images that have been **cryptographically signed**  
This enforces **trust at deploy time**.

### **B. Use Kritis to sign images**
**Kritis** is the tool used to:
- **Cryptographically sign container images**
- Integrate signing into a **CI/CD pipeline** (e.g., Jenkins)

Together, **Kritis + Binary Authorization** ensure:
> *Only containers built and signed by the trusted pipeline can run.*

---

## ❌ Why the other options are wrong

**c. Restricting Container Registry access**  
- Controls **who can push images**, not **which images can be deployed**
- Does not prevent a compromised or malicious image from running

**d. Vulnerability scanning**  
- Identifies vulnerabilities but **does not block deployment**
- Does not verify **image integrity or provenance**

---

## ✅ Final Answer

**a and b**

---

# 3. Upgrading EHR Network Connectivity (EHR Healthcare Case Study)

## Question

You need to **upgrade the EHR connection** to comply with their requirements. The new connection design must support **business-critical needs** and meet the **same network and security policy requirements**. What should you do?

a. Add a new **Dedicated Interconnect** connection.  
b. Upgrade the **bandwidth** on the Dedicated Interconnect connection to **100 G**.  
c. Add **three new Cloud VPN connections**.  
d. Add a new **Carrier Peering** connection.  

---

## ✅ Correct Answer

**a. Add a new Dedicated Interconnect connection.**

---

## Explanation

EHR requires a connection that is:

- **Business-critical**  
- **Highly available**  
- **Compliant with strict security and network policies**

A **Dedicated Interconnect** provides:
- **Private, direct connectivity** to Google Cloud  
- **High throughput and low latency**  
- **Enterprise-grade SLA**  
- Ability to build **redundant links** for high availability  

Adding another **Dedicated Interconnect**:
- Improves **capacity**
- Increases **fault tolerance**
- Maintains the **same security posture**

This is exactly what EHR requires.

---

## ❌ Why the other options are wrong

**b. Upgrade to 100 G**  
- Increases bandwidth but **does not improve redundancy or availability**
- A single large link is still a **single point of failure**

**c. Cloud VPN connections**  
- VPN is **not suitable for business-critical healthcare traffic**
- Lower performance and no enterprise SLA

**d. Carrier Peering**  
- Provides **public internet access**, not private networking
- Not suitable for **HIPAA-grade workloads**

---

## ✅ Final Answer

**a**

---

# 4. Hybrid Connectivity Architecture for EHR Healthcare

## Question

For this question, refer to the **EHR Healthcare** case study. You need to define the **technical architecture for hybrid connectivity** between **EHR's on-premises systems** and **Google Cloud**. You want to follow **Google's recommended practices for production-level applications**. Considering the **EHR Healthcare business and technical requirements**, what should you do?

a. Configure **two Partner Interconnect connections in one metro (City)**, and make sure the Interconnect connections are placed in **different metro zones**.  
b. Configure **two VPN connections** from on-premises to Google Cloud, and make sure the VPN devices on-premises are in **separate racks**.  
c. Configure **Direct Peering** between EHR Healthcare and Google Cloud, and make sure you are peering at least **two Google locations**.  
d. Configure **two Dedicated Interconnect connections in one metro (City)** and **two connections in another metro**, and make sure the Interconnect connections are placed in **different metro zones**.  

---

## ✅ Correct Answer

**d**

---

## Explanation

EHR Healthcare requires **production-grade**, **highly available**, and **secure hybrid connectivity** for **healthcare workloads**.

Google’s best practice for **mission-critical workloads** is:

> **Four Interconnect links across two metros and two zones**

This provides:
- **Redundancy across physical locations**
- **Protection against metro-level outages**
- **Highest availability SLA**
- **Consistent private connectivity**

Option **D** follows this architecture exactly:
- Two links in **Metro A**
- Two links in **Metro B**
- Each in **separate metro zones**

This design meets **HIPAA-grade reliability and security requirements**.

---

## ❌ Why the other options are wrong

**a. Two Partner Interconnects in one metro**  
- A **single metro failure** would take down connectivity  

**b. Two VPN connections**  
- VPN is not suitable for **production healthcare workloads**  

**c. Direct Peering**  
- Uses **public IPs**, not private networking  
- Not appropriate for sensitive healthcare data  

---

## ✅ Final Answer

**d**

---

# 5. Improving Pub/Sub Publishing Latency (EHR Healthcare Case Study)

## Question

For this question, refer to the **EHR Healthcare** case study. You are a **developer on the EHR customer portal team**. Your team recently migrated the **customer portal application** to **Google Cloud**. The **load has increased** on the application servers, and now the application is logging many **timeout errors**. You recently incorporated **Pub/Sub** into the application architecture, and the application is **not logging any Pub/Sub publishing errors**. You want to **improve publishing latency**. What should you do?

a. Increase the **Pub/Sub Total Timeout** retry value.  
b. Move from a **Pub/Sub subscriber pull model** to a **push model**.  
c. Turn off **Pub/Sub message batching**.  
d. Create a **backup Pub/Sub message queue**.  

---

## ✅ Correct Answer

**c. Turn off Pub/Sub message batching.**

---

## Explanation

Pub/Sub **batching** groups messages before sending them to improve throughput. However, batching **adds delay** because Pub/Sub waits to accumulate messages before publishing.

Since the problem is **high latency and timeouts** (not message loss), the correct solution is to:

> **Disable batching so messages are published immediately**

This reduces:
- End-to-end latency  
- Application timeouts  

While throughput may decrease slightly, **latency improves**, which is what the application needs.

---

## ❌ Why the other options are wrong

**a. Increase retry timeout**  
- Masks the problem instead of reducing latency  
- Makes failures take longer to surface  

**b. Pull vs push subscribers**  
- Subscriber models do **not affect publishing latency**  

**d. Backup queue**  
- Helps with reliability, not performance  

---

## ✅ Final Answer

**c**

---

# 6. Preventing External IP Misconfiguration (EHR Healthcare Case Study)

## Question

For this question, refer to the **EHR Healthcare** case study. In the past, **configuration errors** put **public IP addresses** on backend servers that should **not have been accessible from the Internet**. You need to ensure that **no one can put external IP addresses on backend Compute Engine instances** and that **external IP addresses can only be configured on frontend Compute Engine instances**. What should you do?

a. Create an **Organizational Policy** with a constraint to allow **external IP addresses only on the frontend Compute Engine instances**.  
b. Revoke the **compute.networkAdmin** role from all users in the project with front end instances.  
c. Create an **Identity and Access Management (IAM) policy** that maps the IT staff to the **compute.networkAdmin** role for the organization.  
d. Create a **custom Identity and Access Management (IAM) role** named **GCE_FRONTEND** with the **compute.addresses.create** permission.  

---

## ✅ Correct Answer

**a**

---

## Explanation

The requirement is to **enforce a hard rule** that:

- Backend VMs **can never** have public IPs  
- Frontend VMs **can** have public IPs  

This must be enforced **centrally and automatically**, not by human permissions.

**Organization Policies**:
- Apply **guardrails across projects**
- Prevent configuration drift
- Block disallowed resource configurations at creation time  

Using an **Org Policy constraint for External IP access** ensures backend instances cannot be misconfigured, even by admins.

---

## ❌ Why the other options are wrong

**b. Revoking roles**  
- People can still create VMs through other roles  
- Does not guarantee backend VMs won’t get public IPs  

**c. Granting compute.networkAdmin**  
- Increases risk instead of controlling it  

**d. Custom IAM role**  
- Controls *who* can create IPs  
- Does not enforce *where* they can be attached  

Only **Organization Policy** enforces **resource-level constraints**.

---

## ✅ Final Answer

**a**

---

# 7. Reducing GKE Attack Surface (EHR Healthcare Case Study)

## Question

For this question, refer to the **EHR Healthcare** case study. You are responsible for designing the **Google Cloud network architecture for Google Kubernetes Engine (GKE)**. You want to follow **Google best practices**. Considering the **EHR Healthcare business and technical requirements**, what should you do to **reduce the attack surface**?

a. Use a **private cluster with a private endpoint** with **master authorized networks** configured.  
b. Use a **public cluster** with **firewall rules** and **Virtual Private Cloud (VPC) routes**.  
c. Use a **private cluster with a public endpoint** with **master authorized networks** configured.  
d. Use a **public cluster** with **master authorized networks** enabled and **firewall rules**.  

---

## ✅ Correct Answer

**a**

---

## Explanation

Healthcare workloads require **maximum isolation and minimum exposure**.

**Private GKE clusters with private endpoints**:
- The Kubernetes control plane is **not exposed to the public Internet**
- Nodes do **not have public IP addresses**
- Access to the control plane is only possible **inside the VPC or through private connectivity**

Adding **Master Authorized Networks** further restricts:
- Which internal IP ranges can access the Kubernetes API

This configuration provides:
- **Smallest possible attack surface**
- **HIPAA-grade network isolation**
- **Google’s recommended best practice** for regulated workloads

---

## ❌ Why the other options are wrong

**b. Public cluster with firewall rules**  
- Control plane is still **publicly accessible**

**c. Private cluster with public endpoint**  
- Kubernetes API is still **exposed to the Internet**

**d. Public cluster with MAN and firewall rules**  
- Still **Internet-facing**, increasing attack surface  

---

## ✅ Final Answer

**a**
