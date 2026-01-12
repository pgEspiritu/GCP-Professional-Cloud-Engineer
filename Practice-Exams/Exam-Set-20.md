# 1. Troubleshooting a Slow Application on Compute Engine

## Question

You are managing several internal applications that are deployed on **Compute Engine**.  
Business users inform you that an application has become **very slow over the past few days**.  
You want to find the **underlying cause** in order to solve the problem.

What should you do **first**?

A. Inspect the logs and metrics from the instances in Cloud Logging and Cloud Monitoring.  

B. Change the Compute Engine Instances behind the application to a machine type with more CPU and memory.  

C. Restore a backup of the application database from a time before the application became slow.  

D. Deploy the applications on a managed instance group with autoscaling enabled. Add a load balancer in front of the managed instance group, and have the users connect to the IP of the load balancer.  

---

## ✅ Correct Answer

**A. Inspect the logs and metrics from the instances in Cloud Logging and Cloud Monitoring.**

---

## Explanation

Before making any changes, you must **identify the root cause** of the slowdown.  
Cloud Monitoring and Cloud Logging provide:

- CPU, memory, disk, and network usage  
- Application error rates and latency  
- Resource saturation and bottlenecks  

This data tells you whether the issue is caused by:
- Resource exhaustion
- Application errors
- Network or disk I/O
- Database or backend latency

Without this information, any change would be **guesswork**.

---

### Why the other options are wrong

**B. Increase machine size** ❌  
You should not scale blindly without knowing the bottleneck. This wastes money and may not fix the issue.

**C. Restore a backup** ❌  
There is no evidence of data corruption or bad data.

**D. Add autoscaling and a load balancer** ❌  
This is a **design change**, not a **diagnostic step**. You must first understand why the application is slow.

---

## 📌 Final Answer

**A**


---

# 2. Preventing Outages During GKE Rolling Deployments

## Question

Your company has an application running as a **Deployment in a Google Kubernetes Engine (GKE) cluster**.  
When releasing new versions of the application via a **rolling deployment**, the team has been causing **outages**.  
The root cause of the outages is **misconfigurations with parameters that are only used in production**.

You want to put **preventive measures in the platform** to prevent outages.  
What should you do?

A. Configure liveness and readiness probes in the Pod specification.  

B. Configure health checks on the managed instance group.  

C. Create a Scheduled Task to check whether the application is available.  

D. Configure an uptime alert in Cloud Monitoring.  

---

## ✅ Correct Answer

**A. Configure liveness and readiness probes in the Pod specification.**

---

## Explanation

In Kubernetes, **rolling deployments depend on readiness probes** to decide whether a new Pod is ready to receive traffic.

If production-only configuration is wrong:
- The container may start
- But the application is not actually ready to serve traffic

With a **readiness probe**:
- GKE will **not send traffic** to the new Pod until it is healthy
- The rollout will **pause or stop** if Pods fail to become ready
- This prevents broken versions from taking production traffic

With a **liveness probe**:
- GKE can automatically **restart** crashed or deadlocked containers

Together, they provide **automatic protection during deployments**.

---

### Why the other options are wrong

**B. Managed instance group health checks** ❌  
GKE does not use MIG health checks for Pod-level traffic routing.

**C. Scheduled task** ❌  
This detects problems after they happen — it does not prevent outages.

**D. Uptime alert** ❌  
Alerts notify you after failure; they do not stop bad deployments.

---

## 📌 Final Answer

**A**

---

# 3. Reducing Cost in a Large GKE Cluster Without Losing Availability

## Question

Your company uses **Google Kubernetes Engine (GKE)** as a platform for all workloads.  
Your company has a **single large GKE cluster** that contains **batch, stateful, and stateless workloads**.  
The GKE cluster is configured with a **single node pool with 200 nodes**.

Your company needs to **reduce the cost of this cluster** but does **not want to compromise availability**.  
What should you do?

A. Create a second GKE cluster for the batch workloads only. Allocate the 200 original nodes across both clusters.  

B. Configure CPU and memory limits on the namespaces in the cluster. Configure all Pods to have CPU and memory limits.  

C. Configure a HorizontalPodAutoscaler for all stateless workloads and for all compatible stateful workloads. Configure the cluster to use node auto scaling.  

D. Change the node pool to use preemptible VMs.  

---

## ✅ Correct Answer

**C. Configure a HorizontalPodAutoscaler for all stateless workloads and for all compatible stateful workloads. Configure the cluster to use node auto scaling.**

---

## Explanation

The biggest cost driver in GKE is **unused compute capacity**.  
Right now, you have **200 nodes running all the time**, regardless of how much load exists.

By enabling:

### 1. Horizontal Pod Autoscaler (HPA)
- Automatically scales Pods up and down based on CPU/memory or custom metrics
- Reduces wasted Pod capacity

### 2. Cluster Autoscaler
- Automatically adds or removes nodes
- If fewer Pods are needed, **nodes are removed**
- If more Pods are needed, **nodes are added**

Together, they ensure:
- You only pay for the resources you actually need
- High availability is preserved because scaling is automatic

---

### Why the other options are wrong

**A. Split into two clusters** ❌  
This adds operational complexity and does not reduce total node count.

**B. CPU and memory limits** ❌  
Limits prevent resource abuse, but they do **not reduce the number of nodes**.

**D. Preemptible VMs** ❌  
Preemptible VMs are cheaper but **can be terminated at any time**, which compromises availability — especially for stateful workloads.

---

## 📌 Final Answer

**C**

---

# 4. Monitoring Costly BigQuery Queries in Real Time

## Question

Your company has a Google Cloud project that uses **BigQuery for data warehousing on a pay-per-use basis**.  
You want to **monitor queries in real time** to discover:

- The **most costly queries**
- Which **users spend the most**

What should you do?

A.  
1. In the BigQuery dataset that contains all the tables to be queried, add a label for each user that can launch a query.  
2. Open the Billing page of the project.  
3. Select Reports.  
4. Select BigQuery as the product and filter by the user you want to check.  

B.  
1. Create a Cloud Logging sink to export BigQuery **data access logs** to BigQuery.  
2. Perform a BigQuery query on the generated table to extract the information you need.  

C.  
1. Create a Cloud Logging sink to export BigQuery data access logs to Cloud Storage.  
2. Develop a Dataflow pipeline to compute the cost of queries split by users.  

D.  
1. Activate **billing export into BigQuery**.  
2. Perform a BigQuery query on the billing table to extract the information you need.  

---

## ✅ Correct Answer

**B. Create a Cloud Logging sink to export BigQuery data access logs to BigQuery and query them.**

---

## Explanation

BigQuery writes **Data Access Logs** to **Cloud Logging** that contain:

- Who ran the query
- Which project and dataset
- Bytes processed
- Job ID
- Query text
- Start and end times

These logs are generated **in near real time**.

By exporting them to **BigQuery**:
- You can query them instantly
- You can calculate cost using:
  
```math
cost = bytes_processed × BigQuery price per TB
```


This lets you:
- Identify the most expensive queries
- Identify which users run them
- Do it **in near real time**

---

### Why the other options are wrong

**A. Billing reports** ❌  
Billing reports are **delayed** and **aggregated**, not real-time.

**C. Dataflow pipeline** ❌  
This is overly complex. BigQuery can directly query logs.

**D. Billing export** ❌  
Billing export data is:
- Delayed (hours)
- Aggregated by service, not by individual query

You cannot see **individual queries or users**.

---

## 📌 Final Answer

**B**

---

# 5. Private Connectivity Between Projects in Different Organizations

## Question

Your company and one of its partners each have a Google Cloud project in **separate organizations**.  

Your company’s project (**prj-a**) runs in **vpc-a**.  
The partner’s project (**prj-b**) runs in **vpc-b**.  

There are:
- **Two instances** in **vpc-a**
- **One instance** in **vpc-b**

The subnets in both VPCs **do not overlap**.

You need to ensure that **all instances communicate with each other using internal IPs**, while **minimizing latency and maximizing throughput**.

What should you do?

A. Set up a network peering between **vpc-a** and **vpc-b**.  
B. Set up a VPN between **vpc-a** and **vpc-b** using **Cloud VPN**.  
C. Configure IAP TCP forwarding on the instance in **vpc-b**, and then run:  
   `gcloud compute start-iap-tunnel INSTANCE_NAME_IN_VPC_B 22 --local-host-port=localhost:22`  
D.  
1. Create an additional instance in **vpc-a**.  
2. Create an additional instance in **vpc-b**.  
3. Install OpenVPN on the new instances.  
4. Configure a VPN tunnel between **vpc-a** and **vpc-b** using OpenVPN.  

---

## ✅ Correct Answer

**A. Set up a network peering between vpc-a and vpc-b**

---

## Explanation

### Why **VPC Network Peering** 🛣️

VPC Network Peering is the **best solution** when:
- Networks are in **different projects**
- Even in **different organizations**
- IP ranges **do not overlap**
- You need **high throughput** and **low latency**

With VPC peering:
- Traffic flows **directly over Google’s private backbone**
- No encryption overhead
- No VPN gateways
- Uses **internal IP addresses**
- No bandwidth bottleneck

This provides the **highest performance and lowest latency**.

---

### Why the other options are wrong

**B. Cloud VPN** ❌  
- Adds **encryption overhead**
- Lower throughput
- Higher latency than VPC peering

**C. IAP TCP forwarding** ❌  
- Designed for **administrative access**
- Not for production service-to-service communication

**D. OpenVPN on VMs** ❌  
- Adds:
  - Operational overhead
  - Lower performance
  - Higher cost
- Not Google-recommended

---

## 📌 Final Answer

**A**

---

# 6. Versioning and Recovery for Cloud Storage Data

## Question

You want to store **critical business information** in **Cloud Storage buckets**.  

The information is:
- **Regularly changed**
- **Older versions must be referenced**
- **All changes must be recorded**
- **Accidental deletions or edits must be easily rolled back**

Which feature should you enable?

A. Bucket Lock  
B. Object Versioning  
C. Object change notification  
D. Object Lifecycle Management  

---

## ✅ Correct Answer

**B. Object Versioning**

---

## Explanation

### Why **Object Versioning** 🕒

Object Versioning keeps:
- **Every previous version** of an object
- A full **history of changes**
- The ability to:
  - Recover deleted files
  - Roll back overwritten files
  - Audit changes over time

When enabled:
- Deleting a file only creates a **delete marker**
- Uploading a new file creates a **new version**
- Older versions remain retrievable

This perfectly satisfies:
- Audit requirements  
- Rollback capability  
- Change tracking  

---

### ❌ Why the other options are wrong

**A. Bucket Lock**  
- Prevents deletion or modification  
- Does **not** track versions or changes  

**C. Object change notification**  
- Sends event messages when objects change  
- Does **not** store previous versions  

**D. Object Lifecycle Management**  
- Automates deletion or storage class changes  
- Does **not** preserve history  

---

## 📌 Final Answer

**B**

---

# 7. Autoscaling Based on Memory Usage in Compute Engine

## Question

You have a **Compute Engine application** that you want to **autoscale when total memory usage exceeds 80%**.  

You installed the **Cloud Monitoring agent** and configured autoscaling as:

- **Metric identifier:** `agent.googleapis.com/memory/percent_used`  
- **Filter:** `metric.label.state = 'used'`  
- **Target utilization level:** `80`  
- **Target type:** `GAUGE`  

However, the application **does not scale under high load**.

What should you do?

A. Change the Target type to `DELTA_PER_MINUTE`.  
B. Change the Metric identifier to `agent.googleapis.com/memory/bytes_used`.  
C. Change the filter to  
`metric.label.state = 'used' AND metric.label.state = 'buffered' AND metric.label.state = 'cached' AND metric.label.state = 'slab'`.  
D. Change the filter to `metric.label.state = 'free'` and the Target utilization to `20`.  

---

## ✅ Correct Answer

**D. Change the filter to `metric.label.state = 'free'` and the Target utilization to 20.**

---

## Explanation

### How Linux memory really works 🧠

Linux aggressively uses memory for:
- Disk **cache**
- **Buffers**
- **Slab** allocations

This means:
- Memory marked as `used` is often still **reclaimable**
- Even under heavy load, `used` may stay high
- Autoscaler never sees a meaningful signal → **no scaling**

---

### Why monitoring **free memory** works better

Instead of watching what is used, you should watch what is **left**:

If:
- `free` memory falls below **20%**
→ real memory pressure exists  
→ scaling should occur

This is exactly equivalent to:
> **Used memory > 80%**

but gives a **much more accurate signal**.

---

### ❌ Why the other options are wrong

**A. DELTA_PER_MINUTE**  
Memory usage is a **GAUGE**, not a rate. Changing to DELTA breaks autoscaling.

**B. bytes_used**  
Autoscaler expects a **percentage-based utilization** metric. Bytes cannot be normalized.

**C. used + buffered + cached + slab**  
Linux already includes these in memory accounting. This makes the signal worse, not better.

---

## 📌 Final Answer

**D**

---

# 8. Highly Available and Cost-Optimized Hybrid Connectivity

## Question

You are deploying an application to **Google Cloud** that must communicate over a **private network** with applications in a **non-Google Cloud environment**.

The expected **average throughput** is **200 kbps**.

The business requires:
- **As close to 100% availability as possible**
- **Cost optimization**

What should you provision?

A. An **HA Cloud VPN gateway** connected with **two tunnels** to an on-premises VPN gateway  
B. **Two Classic Cloud VPN gateways** connected to **two on-premises VPN gateways**. Configure each Classic Cloud VPN gateway to have **two tunnels**, each connected to different on-premises VPN gateways  
C. **Two HA Cloud VPN gateways** connected to **two on-premises VPN gateways**. Configure each HA Cloud VPN gateway to have **two tunnels**, each connected to different on-premises VPN gateways  
D. A **single Cloud VPN gateway** connected to an on-premises VPN gateway  

---

## ✅ Correct Answer

**A. An HA Cloud VPN gateway connected with two tunnels to an on-premises VPN gateway**

---

## Explanation

### Why **HA Cloud VPN** is the right choice 🔐

**HA Cloud VPN** provides:
- **99.99% availability SLA**
- **Active-active tunnels**
- Automatic **failover**
- Lower operational complexity

With **two tunnels** connected to the on-premises gateway, traffic automatically switches if:
- One tunnel fails
- One Google endpoint becomes unavailable

This achieves **near-100% availability** at the **lowest cost** that still meets SLA.

---

### Why the other options are wrong

**B. Two Classic Cloud VPN gateways**
- **Classic Cloud VPN is deprecated**
- No SLA
- More fragile and complex

**C. Two HA Cloud VPN gateways**
- Extremely high availability  
- But **unnecessarily expensive** for only **200 kbps**
- Over-engineered for the requirement

**D. Single Cloud VPN gateway**
- No redundancy
- No SLA
- Does **not meet availability requirement**

---

## 🧠 Design principle

> Use **HA Cloud VPN with two tunnels** when you need **high availability** but **low to moderate bandwidth**.

This gives:
- SLA-backed reliability  
- Minimal cost  
- Minimal complexity  

---

## ✅ Final Answer

**A**

---

# 9. Direct Browser Uploads to Cloud Storage from App Engine

## Question

Your company has an application running on **App Engine** that allows users to **upload music files** and share them with other people.  
You want to allow users to **upload files directly into Cloud Storage from their browser session**.  
The **payload must not pass through the backend**.

What should you do?

A.  
1. Set a **CORS configuration** in the target Cloud Storage bucket where the **base URL of the App Engine application** is an allowed origin.  
2. Use the **Cloud Storage Signed URL** feature to generate a **POST URL**.

B.  
1. Set a **CORS configuration** in the target Cloud Storage bucket where the base URL of the App Engine application is an allowed origin.  
2. Assign the **Cloud Storage WRITER** role to users who upload files.

C.  
1. Use the **Cloud Storage Signed URL** feature to generate a POST URL.  
2. Use **App Engine default credentials** to sign requests against Cloud Storage.

D.  
1. Assign the **Cloud Storage WRITER** role to users who upload files.  
2. Use **App Engine default credentials** to sign requests against Cloud Storage.

---

## ✅ Correct Answer

**A**

---

## Explanation

The requirement is:

- Files must be uploaded **directly from the browser to Cloud Storage**
- The **backend must not receive the file**
- The solution must be **secure**

This is exactly what **Signed URLs (or Signed POST policies)** are designed for.

### How the correct solution works

1. **Your App Engine backend** generates a **Signed POST URL** for Cloud Storage.
2. That URL:
   - Is valid for a limited time
   - Allows upload only to a specific bucket/object
3. The **browser uploads directly to Cloud Storage** using that signed URL.
4. The file **never passes through App Engine**.

Because the browser is making a **cross-origin request** to Cloud Storage, **CORS must be configured** on the bucket to allow the App Engine domain.

That is why **both steps in A are required**.

---

## ❌ Why the other options are wrong

### **B. Granting Cloud Storage WRITER to users**
This would require:
- Every user to have a Google identity
- IAM permissions on the bucket  

This is **not scalable or secure** for end users on a public website.

---

### **C. Signed URL but no CORS**
Without CORS, the browser will **block the upload**, even if the URL is valid.

---

### **D. IAM + default credentials**
This gives users **direct write access to your bucket**, which is:
- Insecure
- Not scalable
- Violates least-privilege principles

---

## 🧠 Key principle

> **Signed URLs + CORS = secure, scalable, backend-free browser uploads**

---

## ✅ Final Answer

**A**

---

# 10. Enabling Internet Access for Private Subnet Instances

## Question

You are configuring the cloud network architecture for a newly created project in **Google Cloud** that will host applications in **Compute Engine**.  
Compute Engine virtual machine instances will be created in two different subnets (**sub-a** and **sub-b**) within a single region:

- Instances in **sub-a** will have **public IP addresses**.  
- Instances in **sub-b** will have **only private IP addresses**.

To **download updated packages**, instances must connect to a **public repository outside Google Cloud**.  
You need to allow **sub-b** to access the external repository.

What should you do?

A. Enable **Private Google Access** on sub-b.  
B. Configure **Cloud NAT** and select **sub-b** in the NAT mapping section.  
C. Configure a **bastion host** instance in sub-a to connect to instances in sub-b.  
D. Enable **Identity-Aware Proxy** for TCP forwarding for instances in sub-b.

---

## ✅ Correct Answer

**B. Configure Cloud NAT and select sub-b in the NAT mapping section.**

---

## Explanation

Instances in **sub-b** have **only private IP addresses**, but they must reach the **public internet** to download OS packages and updates.

This is exactly what **Cloud NAT** is designed for:

> **Cloud NAT allows private instances to initiate outbound connections to the public internet without having public IPs.**

When Cloud NAT is configured:
- Traffic from sub-b goes out to the internet
- Responses are translated back to the private IP
- No inbound connections from the internet are allowed

This keeps sub-b:
- **Private**
- **Secure**
- **Internet-enabled for outbound traffic**

---

## ❌ Why the other options are wrong

### **A. Private Google Access**
Private Google Access allows VMs without public IPs to reach **Google APIs only** (like BigQuery, Cloud Storage, etc).  
It **does NOT** allow access to:
- Linux repositories
- GitHub
- OS update servers
- Any public internet site

---

### **C. Bastion host**
A bastion host allows **SSH access**, not **internet access**.  
It does not solve outbound package downloads.

---

### **D. Identity-Aware Proxy (IAP)**
IAP is for:
- Secure admin access (SSH/RDP)
- Application access

It does **not provide internet egress** for instances.

---

## 🧠 Key Principle

> **Private VMs that need outbound internet access → Cloud NAT**

---

## ✅ Final Answer

**B**
