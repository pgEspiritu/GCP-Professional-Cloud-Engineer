# 1. Bringing Windows Server 2022 Licenses to Google Cloud (BYOL)

## Question

Your company is planning to migrate their **Windows Server 2022** from their **on-premises data center** to **Google Cloud**.  
You need to **bring the licenses** that are currently in use in on-premises virtual machines into the target cloud environment.

What should you do?

A.  
1. Create an image of the on-premises virtual machines and upload into Cloud Storage.  
2. Import the image as a virtual disk on Compute Engine.  

B.  
1. Create standard instances on Compute Engine.  
2. Select as the OS the same Microsoft Windows version that is currently in use in the on-premises environment.  

C.  
1. Create an image of the on-premises virtual machine.  
2. Import the image as a virtual disk on Compute Engine.  
3. Create a standard instance on Compute Engine, selecting as the OS the same Microsoft Windows version that is currently in use in the on-premises environment.  
4. Attach a data disk that includes data that matches the created image.  

D.  
1. Create an image of the on-premises virtual machines.  
2. Import the image as a virtual disk on Compute Engine using `--os=windows-2022-dc-v`.  
3. Create a **sole-tenancy** instance on Compute Engine that uses the imported disk as a **boot disk**.  

---

## ✅ Correct Answer

**D**

---

## Explanation

To **bring your own Windows Server licenses (BYOL)** to Google Cloud, the VM **must run on physically dedicated hardware**.  
Google Cloud provides this through **Sole-Tenant Nodes**.

Microsoft licensing requires:
- Dedicated hosts
- No shared hypervisor
- Use of special BYOL Windows images (`*-dc-v`)

By importing the image with `--os=windows-2022-dc-v` and running it on a **sole-tenant node**, you remain **fully license-compliant** and avoid paying for Google-provided Windows licenses.

---

## ❌ Why the other options are incorrect

**A** – The VM would run on shared hardware → **BYOL not allowed**  
**B** – Uses **Google-provided Windows license**, not your own  
**C** – Mixing imported disks with Google-licensed OS violates BYOL rules  

---

## ✅ Final Answer

**D**

---


# 2. Designing High-Availability Hybrid Connectivity with Cloud VPN

## Question

You are deploying an application to **Google Cloud**. The application is part of a system.  
The application in Google Cloud must communicate over a **private network** with applications in a **non-Google Cloud environment**.

The expected average throughput is **200 kbps**.

The business requires:
- **99.99% system availability**
- **Cost optimization**

You need to design the connectivity between the locations to meet the business requirements.  
What should you provision?

A. An **HA Cloud VPN gateway** connected with **two tunnels** to an on-premises VPN gateway.  
B. A **Classic Cloud VPN gateway** connected with **two tunnels** to an on-premises VPN gateway.  
C. **Two HA Cloud VPN gateways** connected to **two on-premises VPN gateways**. Configure each HA Cloud VPN gateway to have **two tunnels**, each connected to different on-premises VPN gateways.  
D. A **Classic Cloud VPN gateway** connected with **one tunnel** to an on-premises VPN gateway.  

---

## ✅ Correct Answer

**A**

---

## Explanation

To achieve **99.99% availability** at the **lowest possible cost**, Google recommends using:

> **One HA Cloud VPN gateway with two tunnels**

### Why HA Cloud VPN?

HA Cloud VPN:
- Is **designed for 99.99% SLA**
- Uses **two interfaces** in different Google edge zones
- Supports **BGP with dynamic failover**
- Automatically reroutes traffic if one tunnel fails

Two tunnels to the on-premises VPN gateway provide **redundancy without unnecessary cost**.

The traffic requirement (**200 kbps**) is very small, so Cloud VPN is the **most cost-effective** option.

---

## ❌ Why the other options are incorrect

### **B. Classic VPN with two tunnels**
Classic VPN:
- Does **not** provide 99.99% SLA
- Manual failover
- Older architecture

Fails availability requirement.

---

### **C. Two HA VPNs + four tunnels**
This is **over-engineered and expensive** for:
- Only **200 kbps**
- Required SLA already met by a single HA VPN

Fails **cost optimization**.

---

### **D. Classic VPN with one tunnel**
- No redundancy
- No SLA
- Single point of failure

Fails **availability requirement**.

---

## ✅ Final Answer

**A**

---

# 3. Migrating a 10-TB On-Premises Database Export to Cloud Storage ☁️

## Question

Your company wants to migrate their **10-TB on-premises database export** into **Cloud Storage**.  
You want to **minimize both migration time and overall cost**.  
The network bandwidth between on-premises and Google Cloud is **1 Gbps**.  
You also want to follow **Google-recommended best practices**.

What should you do?

A. Develop a **Dataflow job** to read data directly from the database and write it into Cloud Storage.  
B. Use the **Data Transfer Appliance** to perform an offline migration.  
C. Use a **commercial ETL tool** to extract the data and upload it into Cloud Storage.  
D. Upload the data using **gcloud storage cp**.

---

## ✅ Correct Answer

**D**

---

## Explanation

At **1 Gbps**, your maximum theoretical throughput is:

```
1 Gbps = 125 MB/s ≈ 10.8 TB per day
```


So your **10-TB dataset** can be transferred in about **one day** over the network.

Google recommends using **online transfer tools** when:
- Data size is **under ~60 TB**
- Network bandwidth is **good (≥ 1 Gbps)**
- You want the **lowest cost**

The simplest and cheapest Google-recommended solution is:

> **Use `gcloud storage cp` to upload directly to Cloud Storage**

This:
- Uses Google’s **optimized parallel transfer**
- Has **no appliance cost**
- Avoids ETL or Dataflow compute charges
- Is **faster than shipping hardware**

---

## ❌ Why the other options are wrong

### **A. Dataflow**
- Designed for **transforming data**
- Adds **compute cost**
- Unnecessary for a simple database export file

---

### **B. Transfer Appliance**
Used only when:
- Data is **very large** (typically > 60 TB), or
- Network is **slow or unreliable**

For 10 TB over 1 Gbps, this is **slower and more expensive**.

---

### **C. Commercial ETL**
- Adds **licensing and compute cost**
- No advantage for a raw export upload

---

## ✅ Final Answer

**D — Upload the data using `gcloud storage cp`.** 🚀

---

# 4. Protecting Mortgage Approval Documents in Cloud Storage 🏦

## Question

A financial institution stores **mortgage loan approval documents** in **Cloud Storage**.  
Every change is uploaded as a **new approval file**, and **no file must be deleted or overwritten for 5 years**.

Which solution guarantees this?

A. Create a **retention policy** on the bucket for **5 years** and **lock** it.  
B. Create an **organization policy** `constraints/storage.retentionPolicySeconds` at the **organization level** for **5 years**.  
C. Use a **customer-managed encryption key** and rotate it after 5 years.  
D. Create an **organization policy** `constraints/storage.retentionPolicySeconds` at the **project level** for **5 years**.

---

## ✅ Correct Answer

**A**

---

## 🧠 Explanation

This is a classic **WORM (Write Once, Read Many)** compliance requirement, which Cloud Storage supports using:

> **Bucket Retention Policy + Retention Policy Lock**

### What this does:
Once locked:
- Objects **cannot be deleted**
- Objects **cannot be overwritten**
- Even **project owners and Google** cannot remove them
- Protection lasts until the **retention period expires**

This perfectly satisfies:
> “documents cannot be deleted or overwritten for the next 5 years.”

---

## ❌ Why the other options are wrong

### **B & D — Organization Policy**
These only **enforce that buckets must have a retention policy**.  
They **do NOT**:
- Prevent deletion
- Lock retention
- Provide WORM compliance

They are governance controls, not data protection.

---

### **C — Customer-managed keys**
Encryption keys:
- Protect **confidentiality**
- Do **NOT** prevent deletion or overwriting

Someone could still delete or replace the files.

---

## 🏆 Final Answer

**A — Create a 5-year bucket retention policy and lock it.** 🔐

---

# 5. GCP Organization, Projects, and Least Privilege 🏗️🔐

## Question

The **JencoMart security team** requires:

- **Least-privilege access**
- **Separation of duties**
- **Strict isolation between production and development**

Which **Google domain and project structure** should be used?

A. Create **two G Suite accounts** (one for dev/test/staging, one for production). Each account should contain **one project per application**.  
B. Create **two G Suite accounts**, each with **one project**: one for all dev apps and one for all prod apps.  
C. Create **a single G Suite account** to manage users with **each stage of each application in its own project**.  
D. Create **a single G Suite account** with **one project for dev/test/staging** and **one project for production**.

---

## ✅ Correct Answer

**C**

---

## 🧠 Explanation

Google Cloud best practice is:

> **One Organization (one Google Workspace / G Suite domain) with multiple projects**

and

> **Separate projects for each environment of each application**

This design provides:
- **Strong isolation** (IAM, billing, quotas, networking)
- **Least privilege** (users get access only to the projects they need)
- **Separation of duties** (prod admins don’t touch dev, and vice versa)

So the correct model is:
```mermaid
One Organization (G Suite / Google Workspace)
│
├── App1-dev (project)
├── App1-test (project)
├── App1-prod (project)
├── App2-dev (project)
├── App2-prod (project)
└── ...
```


This allows:
- Devs → access only dev/test
- Ops → access prod
- Security → audit and enforce centrally

---

## ❌ Why the others are wrong

### **A & B — Two G Suite accounts**
Google strongly discourages multiple organizations unless legally required.  
This creates:
- Billing fragmentation
- Policy duplication
- Harder auditing
- Worse security posture

---

### **D — One project for all dev, one for all prod**
This is too coarse:
- Dev teams see other apps
- IAM becomes messy
- One compromised dev app affects all others

This violates **least privilege**.

---

## 🏆 Final Answer

**C — One G Suite domain with a separate project for each stage of each application.** 🔐

---

# 6. JencoMart – Troubleshooting SSH Access to a Database VM 🛠️

## Question

A few days after JencoMart migrates the user credentials database to Google Cloud Platform and shuts down the old server, the new database server stops responding to **SSH connections**.  
It is still serving **database requests to the application servers correctly**.

What **three steps** should you take to **diagnose the problem**? (Choose three.)

A. Delete the virtual machine (VM) and disks and create a new one  
B. Delete the instance, attach the disk to a new VM, and investigate  
C. Take a snapshot of the disk and connect to a new machine to investigate  
D. Check inbound firewall rules for the network the machine is connected to  
E. Connect the machine to another network with very simple firewall rules and investigate  
F. Print the Serial Console output for the instance for troubleshooting, activate the interactive console, and investigate  

---

## ✅ Correct Answers

**B, D, F**

---

## 🧠 Explanation

Because the database is still working, the VM is running — only **SSH access is broken**.  
You should troubleshoot without destroying data.

### **D. Check inbound firewall rules**
SSH (TCP port 22) might be blocked. This is the **most common cause** of lost SSH access.

---

### **F. Use Serial Console**
The **serial console** works even if:
- SSH is broken  
- Firewall rules are wrong  
- The OS network stack is misconfigured  

It lets you log in and see boot errors, OS logs, or fix broken SSH services.

---

### **B. Attach the disk to another VM**
If the OS or SSH configuration is broken:
1. Delete only the VM (not the disk)
2. Attach the disk to a rescue VM
3. Fix `/etc/ssh`, users, permissions, or logs
4. Reattach it

This is a **safe forensic recovery method**.

---

## ❌ Why the others are wrong

### **A. Delete VM and disks**
This destroys your production database — **data loss**.

---

### **C. Snapshot and connect**
Snapshots are for **backup**, not for live troubleshooting.  
You still won’t see what broke SSH.

---

### **E. Move to another network**
This is risky and unnecessary.  
Firewall rules can be fixed directly.

---

## 🏆 Final Answer

**B, D, and F** ✔️

---

# 7. JencoMart – Service Account Key Management 🔐

## Question

JencoMart has decided to migrate user profile storage to **Google Cloud Datastore** and the application servers to **Google Compute Engine (GCE)**.  
During the migration, the **existing on-premises infrastructure** will need access to Datastore to upload the data.

What **service account key-management strategy** should you recommend?

A. Provision service account keys for the on-premises infrastructure and for the GCE virtual machines (VMs)  
B. Authenticate the on-premises infrastructure with a user account and provision service account keys for the VMs  
C. Provision service account keys for the on-premises infrastructure and use Google Cloud Platform (GCP) managed keys for the VMs  
D. Deploy a custom authentication service on GCE/Google Kubernetes Engine (GKE) for the on-premises infrastructure and use GCP managed keys for the VMs  

---

## ✅ Correct Answer

**C**

---

## 🧠 Explanation

Google Cloud best practice is:

| Environment | Authentication method |
|------------|----------------------|
| **Outside GCP (on-premises)** | **Service account key file** |
| **Inside GCP (GCE, GKE, App Engine)** | **GCP-managed service account (no keys)** |

---

### Why **C** is correct

- **On-premises systems** have no native identity in GCP → they must use **service account key files**
- **GCE VMs** automatically receive **short-lived, rotated credentials** through:
  - Metadata Server  
  - Workload Identity  
- This avoids storing long-lived private keys on VMs (best security practice)

---

### Why the others are wrong

**A — ❌**  
VMs should **not** use service account key files. This is insecure and violates GCP best practices.

**B — ❌**  
Using a **human user account** for production data access breaks:
- Least privilege  
- Auditability  
- Automation

**D — ❌**  
A custom authentication system is:
- Expensive  
- Hard to secure  
- Completely unnecessary when GCP already provides workload identity

---

## 🏆 Final Answer

**C** ✔️  
Use service account keys for on-premises systems, and GCP-managed identities for GCE VMs.

---

# 8. Measuring Success for Asia Traffic

## Question

JencoMart has built a version of their application on **Google Cloud Platform** that serves traffic to **Asia**.  
You want to **measure success** against their **business and technical goals**.

Which metrics should you track?

A. Error rates for requests from Asia  
B. Latency difference between US and Asia  
C. Total visits, error rates, and latency from Asia  
D. Total visits and average latency for users from Asia  
E. The number of character sets present in the database  

---

## ✅ Correct Answer

**C. Total visits, error rates, and latency from Asia**

---

## Explanation

To measure **both business success and technical performance**, you need **three key dimensions**:

### 1️⃣ Business success — **Total visits**
This tells you:
- Whether users in Asia are actually using the application
- Whether adoption and engagement are increasing

---

### 2️⃣ Reliability — **Error rates**
This tells you:
- Whether users can successfully use the application
- Whether failures are affecting customers

High traffic with high error rates is **not success**.

---

### 3️⃣ Performance — **Latency**
This tells you:
- How fast the application feels to users in Asia
- Whether your regional deployment is improving user experience

---

These three together map directly to **Google SRE golden signals**:
- **Traffic** → Total visits  
- **Errors** → Error rates  
- **Latency** → Latency  

This combination gives a **complete and accurate picture** of how well the Asia deployment is working.

---

## ❌ Why the other options are wrong

### **A. Error rates only**
Misses:
- Performance
- User adoption

---

### **B. Latency difference between US and Asia**
Does not tell:
- How many users are in Asia
- Whether errors are happening

---

### **D. Total visits and average latency**
Missing:
- Error rates (reliability)

---

### **E. Number of character sets**
Irrelevant to performance, reliability, or business success

---

## ✅ Final Answer

**C**

---

# 9. Choosing a Database for User Profiles

## Question

JencoMart wants to move their **User Profiles database** to Google Cloud Platform.  

Which **Google database** should they use?

A. Cloud Spanner  
B. Google BigQuery  
C. Google Cloud SQL  
D. Google Cloud Datastore  

---

## ✅ Correct Answer

**C. Google Cloud SQL**

---

## Explanation

User Profiles are typically **structured, relational data** that require:

- Consistency for user data (e.g., email, preferences)
- ACID transactions for updates
- Query flexibility for CRUD operations

### Why **Cloud SQL** fits:

- Managed relational database (MySQL, PostgreSQL, SQL Server)  
- Supports **transactions and joins**, which are common for user profiles  
- Fully managed: automatic backups, replication, scaling

---

### Why not the others?

- **A. Cloud Spanner**
  - Designed for **global-scale, highly available relational data**  
  - Overkill for typical user profiles unless global scale is required  

- **B. BigQuery**
  - Optimized for **analytics and large-scale reporting**  
  - Not suitable for OLTP workloads like user profile updates  

- **D. Cloud Datastore**
  - NoSQL, schemaless, suitable for **highly scalable applications**  
  - Less suitable for relational user profile data requiring transactional consistency  

---

## ✅ Final Answer

**C**
