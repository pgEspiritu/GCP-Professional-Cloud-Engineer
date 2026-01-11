# 1. Load-Testing Requirements for Cloud Services

## Question

You are helping the **QA team roll out a new load-testing tool** to test the scalability of your primary cloud services that run on **Google Compute Engine** with **Cloud Bigtable**.

Which **three requirements** should they include? (Choose three.)

A. Ensure that the load tests validate the performance of Cloud Bigtable  
B. Create a separate Google Cloud project to use for the load-testing environment  
C. Schedule the load-testing tool to regularly run against the production environment  
D. Ensure all third-party systems your services use is capable of handling high load  
E. Instrument the production services to record every transaction for replay by the load-testing tool  
F. Instrument the load-testing tool and the target services with detailed logging and metrics collection  

---

## ✅ Correct Answers

**A, B, F**

---

## Explanation

### **A. Validate Cloud Bigtable performance** 📊
- Cloud Bigtable is a core dependency of the service  
- Load tests must confirm **latency, throughput, and scalability** under stress  

---

### **B. Use a separate Google Cloud project** 🏗️
- Isolates testing from production  
- Prevents accidental outages and unexpected billing impact  
- Follows Google-recommended best practices for testing environments  

---

### **F. Enable detailed logging and metrics** 📈
- Required to analyze:
  - Bottlenecks  
  - Error rates  
  - Resource saturation  
- Enables actionable insights from test results  

---

## ❌ Why the other options are not correct

### **C. Run load tests against production**
- Can cause outages and degrade user experience  
- Load testing should never stress live customer traffic  

---

### **D. Ensure third-party systems handle high load**
- Out of direct control of the QA team  
- Load tests should focus on **your own infrastructure**  

---

### **E. Record every production transaction**
- High overhead and unnecessary  
- Synthetic traffic is preferred for predictable, repeatable testing  

---

## ✅ Final Answer

**A, B, F**

---

# 2. Providing Organization-Wide Visibility for the Security Team

## Question

Your customer is moving their **corporate applications to Google Cloud Platform**.  
The **security team wants detailed visibility of all projects in the organization**.

You have:
- Provisioned **Google Cloud Resource Manager**
- Set yourself as the **Organization Admin**

What **Cloud IAM roles** should you grant to the security team?

A. Org viewer, project owner  
B. Org viewer, project viewer  
C. Org admin, project browser  
D. Project owner, network admin  

---

## ✅ Correct Answer

**B. Org viewer, project viewer**

---

## Explanation

### **Org Viewer** 👁️
- Provides **read-only visibility** across the entire organization
- Allows the security team to:
  - View folders, projects, and organizational structure
  - Audit configurations without making changes

### **Project Viewer** 📂
- Allows read-only access within individual projects
- Enables the team to:
  - Inspect resources
  - Review IAM policies
  - Monitor configurations for compliance and security risks

This combination gives the security team **full visibility with zero risk of accidental changes**, which aligns with **least-privilege and Google-recommended best practices**.

---

## ❌ Why the other options are not correct

### **A. Org viewer, project owner**
- Project Owner grants **full administrative control**
- Violates the principle of least privilege

---

### **C. Org admin, project browser**
- Org Admin is overly permissive and risky
- Security teams typically **audit**, not administer, organizations

---

### **D. Project owner, network admin**
- Too narrow in scope (project-level only)
- Excessive permissions
- Does not provide organization-wide visibility

---

## ✅ Final Answer

**B**

---

# 3. Reducing Security Errors While Maximizing Release Speed and Agility

## Question

Your company places a **high value on responsiveness and meeting customer needs quickly**.  
The **primary business objectives are release speed and agility**, but you also want to **reduce the risk of accidentally introducing security errors**.

Which **two actions** should you take? *(Choose two.)*

A. Ensure every code check-in is peer reviewed by a security SME  
B. Use source code security analyzers as part of the CI/CD pipeline  
C. Ensure you have stubs to unit test all interfaces between components  
D. Enable code signing and a trusted binary repository integrated with your CI/CD pipeline  
E. Run a vulnerability security scanner as part of your CI/CD pipeline  

---

## ✅ Correct Answers

**B. Use source code security analyzers as part of the CI/CD pipeline**  
**E. Run a vulnerability security scanner as part of your CI/CD pipeline**

---

## Explanation

To support **fast releases and agility**, security controls must be:

- Automated
- Integrated directly into CI/CD
- Low-friction for developers

### **B. Source code security analyzers (SAST)** 🔍
- Automatically scan source code for:
  - Insecure coding patterns
  - Hardcoded secrets
  - Common vulnerabilities
- Run on every commit or build
- Provide **early feedback without slowing developers down**

---

### **E. Vulnerability security scanners (DAST / dependency scanning)** 🛡️
- Detect:
  - Known vulnerabilities in dependencies
  - Runtime or configuration issues
- Catch problems **before deployment**
- Fully automated and CI/CD-friendly

Together, these tools support **DevSecOps best practices** by embedding security into fast-moving pipelines.

---

## ❌ Why the other options are not correct

### **A. Security SME review for every check-in**
- Strong security, but:
  - Does not scale
  - Slows down delivery
- Conflicts with speed and agility goals

---

### **C. Unit test stubs**
- Improves software quality
- Not focused on **security risk reduction**

---

### **D. Code signing and trusted binary repositories**
- Valuable for supply-chain security
- Higher setup and operational overhead
- Not the fastest or simplest first step for agility-focused teams

---

## ✅ Final Answer

**B and E**

---


# 4. Enabling Autoscaling for a Running GKE Cluster

## Question

You want to enable your **existing Google Kubernetes Engine (GKE) cluster** to **scale automatically as application demand changes**.

What should you do?

A. Add additional nodes manually using:  
`gcloud container clusters resize CLUSTER_NAME --size 10`  

B. Add a tag to the instances in the cluster using:  
`gcloud compute instances add-tags INSTANCE --tags enable-autoscaling,max-nodes-10`  

C. Update the existing Kubernetes Engine cluster using:  
`gcloud alpha container clusters update mycluster --enable-autoscaling --min-nodes=1 --max-nodes=10`  

D. Create a new Kubernetes Engine cluster with autoscaling enabled and redeploy the application  

---

## ✅ Correct Answer

**C. Update the existing Kubernetes Engine cluster to enable autoscaling**

---

## Explanation

### Why **Option C** is correct 🚀

- **Cluster autoscaling** allows GKE to:
  - Automatically add nodes when pods cannot be scheduled
  - Remove unused nodes when demand decreases
- You can **enable autoscaling on an existing cluster** without recreating it
- This approach:
  - Minimizes downtime
  - Requires minimal effort
  - Follows Google-recommended best practices

Once enabled, GKE automatically manages node count between the specified **minimum and maximum values**.

---

## ❌ Why the other options are not correct

### **A. Manually resizing the cluster**
- Works temporarily
- Does **not provide automatic scaling**
- Requires manual intervention for every demand change

---

### **B. Adding instance tags**
- Instance tags are used for:
  - Firewall rules
  - Network policies
- They **do not control autoscaling behavior**

---

### **D. Recreating the cluster**
- Unnecessary
- Causes additional effort and potential downtime
- Autoscaling can be enabled on an existing cluster instead

---

## ✅ Final Answer

**C**

---

# 5. Scalable and Low-Management Infrastructure for a Promotional Campaign

## Question

Your **marketing department** wants to send a promotional email campaign.  
The **development team wants to minimize operational management**.  

The expected traffic varies widely: **100 to 500,000 click-throughs per day**.  
The linked website will **collect user information and preferences**.  

Which infrastructure should you recommend? (Choose **two**.)

A. Use **Google App Engine** to serve the website and **Cloud Datastore** to store user data  
B. Use a **Google Kubernetes Engine (GKE) cluster** to serve the website and store data to persistent disk  
C. Use a **managed instance group** to serve the website and **Cloud Bigtable** to store user data  
D. Use a **single Compute Engine VM** to host a web server, backed by **Cloud SQL**

---

## ✅ Correct Answers

**A. Use App Engine and Cloud Datastore**  
**C. Use a managed instance group and Cloud Bigtable**

---

## Explanation

### Why **Option A** is correct 🌟

- **App Engine**:
  - Fully managed platform → minimal operational overhead
  - **Automatically scales** to handle spikes in traffic
- **Cloud Datastore** (Firestore in Datastore mode):
  - Fully managed NoSQL database
  - Automatically scales with the number of users
- Ideal for **variable and unpredictable traffic** like a promotional campaign

---

### Why **Option C** is correct 🌟

- **Managed instance groups**:
  - Can automatically scale the number of VMs based on load
  - Provide **high availability**
- **Cloud Bigtable**:
  - Excellent for **high-throughput writes**
  - Handles large numbers of concurrent users efficiently
- Suitable when workloads involve **massive click-throughs** with low latency needs

---

## ❌ Why the other options are not correct

### **B. GKE + persistent disk**
- Requires **more operational management** (cluster updates, node scaling)
- Persistent disk storage is **not ideal for high-concurrency writes**
- Overhead too high for a simple promotional website

### **D. Single Compute Engine VM + Cloud SQL**
- Single VM is a **single point of failure**
- Cannot scale automatically for spikes from 100 → 500,000 users
- Not suitable for **highly variable load**

---

## ✅ Final Answer

**A and C**

---

# 6. No-Ops, Auto-Scaling Compute on Google Cloud

## Question

Your company completed a **lift-and-shift to Compute Engine**.  
You now have **9 months to migrate to a more cloud-native platform** that is:

- **No-ops** (minimal infrastructure management)  
- **Auto-scaling**

Which **two compute products** should you choose?

A. Compute Engine with containers  
B. Google Kubernetes Engine (GKE) with containers  
C. Google App Engine Standard Environment  
D. Compute Engine with custom instance types  
E. Compute Engine with managed instance groups  

---

## ✅ Correct Answers

**B. Google Kubernetes Engine with containers**  
**C. Google App Engine Standard Environment**

---

## Explanation

### Why **GKE with containers (B)** ☸️

- Kubernetes provides:
  - Automatic scaling  
  - Self-healing  
  - Rolling deployments  
- Google manages the control plane  
- You only manage your containers, not the infrastructure  
- Ideal for cloud-native microservices  

---

### Why **App Engine Standard (C)** 🚀

- Fully serverless  
- Zero infrastructure management  
- Built-in auto-scaling  
- Handles load balancing, patching, and scaling automatically  

---

## ❌ Why the other options are not correct

### A. Compute Engine with containers
- Still requires VM management  
- Not no-ops  

### D. Compute Engine with custom instance types
- VM-based  
- No auto-scaling  

### E. Managed instance groups
- Provides auto-scaling  
- Still requires OS and VM management  

---

## ✅ Final Answer

**B and C**

---

# 7. Verifying Log Authenticity

## Question

One of your **primary business objectives** is to be able to **trust the data stored in your application**.  
You want to **log all changes to the application data** and be able to **verify that the logs are authentic and untampered**.

How should you design your logging system?

A. Write the log concurrently in the cloud and on premises  
B. Use a SQL database and limit who can modify the log table  
C. Digitally sign each timestamp and log entry and store the signature  
D. Create a JSON dump of each log entry and store it in Google Cloud Storage  

---

## ✅ Correct Answer

**C. Digitally sign each timestamp and log entry and store the signature**

---

## Explanation

To **verify log authenticity**, you must be able to prove that:

- The log entry was created by a trusted source  
- The log has **not been modified** since it was written  

### Why **digital signatures** work 🔐

Digitally signing each log entry provides:
- **Integrity** — any change breaks the signature  
- **Authenticity** — proves who created the log  
- **Non-repudiation** — the creator cannot deny it  

This is how financial, audit, and forensic logging systems are designed.

---

## ❌ Why the other options are wrong

### A. Writing logs in two places
- Provides redundancy  
- Does **not** prove logs weren’t altered  

### B. SQL with restricted access
- Protects against casual changes  
- Cannot detect **tampering by administrators or attackers**  

### D. JSON in Cloud Storage
- Storage location does not guarantee integrity  
- Files can still be modified  

---

## ✅ Final Answer

**C**

---

# 8. Designing a Secure and Flexible Google Cloud Organization

## Question

Your company has a Google Workspace account and Google Cloud Organization. Some developers in the company have created Google Cloud projects outside of the Google Cloud Organization.  
You want to create an Organization structure that allows developers to create projects, but prevents them from modifying production projects. You want to manage policies for all projects centrally and be able to set more restrictive policies for production projects.  
You want to minimize disruption to users and developers when business needs change in the future. You want to follow Google-recommended practices. Now should you design the Organization structure?

A.  
1. Create a second Google Workspace account and Organization.  
2. Grant all developers the Project Creator IAM role on the new Organization.  
3. Move the developer projects into the new Organization.  
4. Set the policies for all projects on both Organizations.  
5. Additionally, set the production policies on the original Organization.  

B.  
1. Create a folder under the Organization resource named “Production.”  
2. Grant all developers the Project Creator IAM role on the new Organization.  
3. Move the developer projects into the new Organization.  
4. Set the policies for all projects on the Organization.  
5. Additionally, set the production policies on the “Production” folder.  

C.  
1. Create folders under the Organization resource named “Development” and “Production.”  
2. Grant all developers the Project Creator IAM role on the “Development” folder.  
3. Move the developer projects into the “Development” folder.  
4. Set the policies for all projects on the Organization.  
5. Additionally, set the production policies on the “Production” folder.  

D.  
1. Designate the Organization for production projects only.  
2. Ensure that developers do not have the Project Creator IAM role on the Organization.  
3. Create development projects outside of the Organization using the developer Google Workspace accounts.  
4. Set the policies for all projects on the Organization.  
5. Additionally, set the production policies on the individual production projects.  

---

## ✅ Correct Answer

**C**

---

## Explanation

Google’s recommended enterprise structure uses **folders** to separate environments like Development and Production inside a single Organization.

By creating **Development** and **Production** folders:

- Developers can safely create and manage projects in **Development**
- Production projects are **protected by stricter IAM and policy controls**
- Policies can be applied:
  - Globally at the **Organization level**
  - More restrictively at the **Production folder**
- Projects can be **moved between folders later** without breaking billing, IAM, or networking

This design gives you:
- Strong security isolation
- Centralized governance
- Future flexibility
- Full alignment with Google Cloud best practices

---

## Why the other options are wrong

**A — Two organizations**  
Creates fragmented policy management, billing, and security. Google does not recommend using multiple organizations to separate environments.

**B — Only a Production folder**  
Developers could still create projects in the Organization root, bypassing the intended separation.

**D — Development outside the Organization**  
Removes central control and prevents enforcing security and compliance policies across all projects.

---

## Final Answer

**C**

---

# 9. Improving Performance for a Cloud Storage–Backed Streaming Application

## Question

Your company has an application running on **Compute Engine** that allows users to play their favorite music. There are a **fixed number of instances**.  
Files are stored in **Cloud Storage**, and data is **streamed directly to users**.  

Users are reporting that they sometimes need to attempt to play **popular songs multiple times** before they are successful.  
You need to **improve the performance of the application**.

What should you do?

A.  
1. Mount the Cloud Storage bucket using **gcsfuse** on all backend Compute Engine instances.  
2. Serve music files directly from the backend Compute Engine instance.  

B.  
1. Create a **Cloud Filestore NFS volume** and attach it to the backend Compute Engine instances.  
2. Download popular songs in Cloud Filestore.  
3. Serve music files directly from the backend Compute Engine instance.  

C.  
1. Copy popular songs into **Cloud SQL** as a blob.  
2. Update application code to retrieve data from Cloud SQL when Cloud Storage is overloaded.  

D.  
1. Create a **managed instance group** with Compute Engine instances.  
2. Create a **global load balancer** and configure it with two backends:  
   - Managed instance group  
   - Cloud Storage bucket  
3. **Enable Cloud CDN** on the bucket backend.  

---

## ✅ Correct Answer

**D**

---

## Explanation

The problem is caused by **many users requesting the same popular songs**, which creates:
- High load on Cloud Storage
- Increased latency
- Failed or slow streams

The best solution is to **offload repeated requests from Cloud Storage** by using **Cloud CDN**.

### Why this works

By configuring:

- A **global HTTP(S) load balancer**
- With a **Cloud Storage backend**
- And **Cloud CDN enabled**

Popular songs are:
- Cached at **Google edge locations**
- Served **close to users**
- Delivered without repeatedly hitting Cloud Storage

This dramatically:
- Reduces latency
- Eliminates repeated download failures
- Scales automatically
- Requires **no application changes**

---

## Why the other options are wrong

### **A — gcsfuse**
- gcsfuse is slow and not designed for high-throughput streaming
- Adds overhead instead of reducing it

---

### **B — Cloud Filestore**
- Filestore is not global
- Still limited by VM network bandwidth
- Does not provide edge caching

---

### **C — Cloud SQL**
- Databases are not designed to stream large media files
- This would be extremely slow and expensive

---

## Final Answer

**D**

---

# 10. Retaining Cloud VPN Logs for One Year

## Question

The operations team in your company wants to save **Cloud VPN log events for one year**.  
You need to configure the cloud infrastructure to save the logs.

What should you do?

A. Set up a filter in **Cloud Logging** and a **Cloud Storage bucket** as an export target for the logs you want to save.  
B. Enable the **Compute Engine API**, and then enable logging on the firewall rules that match the traffic you want to save.  
C. Set up a **Cloud Logging Dashboard** titled *Cloud VPN Logs*, and then add a chart that queries for the VPN metrics over a one-year time period.  
D. Set up a filter in **Cloud Logging** and a topic in **Pub/Sub** to publish the logs.  

---

## ✅ Correct Answer

**A**

---

## Explanation

Cloud Logging has **limited log retention** by default.  
To retain logs for **one year**, you must **export them to a long-term storage service**.

### Why **Cloud Storage** is the right destination

Cloud Storage provides:
- Low-cost long-term storage
- Lifecycle policies (e.g., auto-delete after 365 days)
- High durability
- Compliance and audit support

By creating:
- A **log sink** in Cloud Logging
- With a **filter** for Cloud VPN logs
- And exporting to a **Cloud Storage bucket**

You ensure:
- Logs are captured in real time
- Retained for the full year
- Available for audits, forensics, and compliance

---

## Why the other options are wrong

### **B — Firewall rule logging**
- Logs **network traffic**, not **Cloud VPN service events**
- Does not guarantee one-year retention

---

### **C — Dashboards**
- Dashboards only **visualize** logs
- They **do not store** logs
- Old data disappears when logs expire

---

### **D — Pub/Sub**
- Pub/Sub is for **stream processing**
- Messages are **not retained long-term**
- Not suitable for one-year compliance storage

---

## Final Answer

**A**
