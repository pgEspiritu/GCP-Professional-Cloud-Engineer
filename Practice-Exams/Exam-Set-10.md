# 1. Disaster Recovery Connectivity and Resilience Verification

## Question

You need to develop procedures to **verify resilience of disaster recovery for remote recovery using GCP**.  
Your **production environment is hosted on-premises**. You need to establish a **secure, redundant connection** between your on-premises network and the GCP network.

What should you do?

A. Verify that Dedicated Interconnect can replicate files to GCP. Verify that direct peering can establish a secure connection between your networks if Dedicated Interconnect fails.  
B. Verify that Dedicated Interconnect can replicate files to GCP. Verify that Cloud VPN can establish a secure connection between your networks if Dedicated Interconnect fails.  
C. Verify that the Transfer Appliance can replicate files to GCP. Verify that direct peering can establish a secure connection between your networks if the Transfer Appliance fails.  
D. Verify that the Transfer Appliance can replicate files to GCP. Verify that Cloud VPN can establish a secure connection between your networks if the Transfer Appliance fails.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Dedicated Interconnect**
  - Provides **high-bandwidth, low-latency, private connectivity**
  - Commonly used for **primary production and disaster recovery links**
- **Cloud VPN**
  - Provides **encrypted, secure connectivity over the public internet**
  - Commonly used as a **backup/failover connection** for Dedicated Interconnect

👉 **Google-recommended best practice** is:
- Use **Dedicated Interconnect as primary**
- Use **Cloud VPN as a redundant backup** for resilience and disaster recovery

This ensures:
- Secure connectivity
- High availability
- Continuity if the primary link fails

---

## ❌ Why the other options are incorrect

### **A. Direct peering as a backup**
❌ Incorrect  
- Direct peering **does not provide encryption**
- Not recommended for secure disaster recovery fallback

---

### **C. Transfer Appliance + direct peering**
❌ Incorrect  
- Transfer Appliance is for **one-time or batch data migration**
- It is **not suitable for ongoing replication**
- Direct peering is not secure

---

### **D. Transfer Appliance + Cloud VPN**
❌ Incorrect  
- Transfer Appliance is **not designed for continuous DR replication**
- It is used for **offline data transfer**, not live resilience testing

---

## ✅ Final Answer

**B. Verify that Dedicated Interconnect can replicate files to GCP. Verify that Cloud VPN can establish a secure connection between your networks if Dedicated Interconnect fails.**

---

# 2. Cost-Optimized and HIPAA-Compliant GCP Design

## Question

Your company operates nationally and plans to use GCP for **multiple batch workloads**, including some that are **not time-critical**.  
You also need to use **GCP services that are HIPAA-certified** and **manage service costs**.

How should you design to meet **Google best practices**?

A. Provision preemptible VMs to reduce cost. Discontinue use of all GCP services and APIs that are not HIPAA-compliant.  
B. Provision preemptible VMs to reduce cost. Disable and then discontinue use of all GCP services and APIs that are not HIPAA-compliant.  
C. Provision standard VMs in the same region to reduce cost. Discontinue use of all GCP services and APIs that are not HIPAA-compliant.  
D. Provision standard VMs to the same region to reduce cost. Disable and then discontinue use of all GCP services and APIs that are not HIPAA-compliant.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

#### 🔹 Cost optimization
- **Preemptible VMs** are recommended by Google for:
  - Batch workloads
  - Fault-tolerant workloads
  - Non–time-critical processing
- They offer **significant cost savings** compared to standard VMs

#### 🔹 HIPAA compliance best practice
- Google recommends:
  - **Disabling** unused and non-compliant services first
  - Then **discontinuing** their use to reduce risk and attack surface
- This aligns with:
  - Principle of least privilege
  - Compliance and security best practices

---

## ❌ Why the other options are incorrect

### **A. Discontinue services without disabling**
❌ Not best practice  
- Services should be **disabled first** before discontinuation
- Disabling helps ensure no accidental usage or dependency issues

---

### **C. Standard VMs instead of preemptible**
❌ Cost-inefficient  
- Standard VMs are more expensive
- Not optimal for non-time-critical batch workloads

---

### **D. Standard VMs + disable/discontinue**
❌ Partially correct but inefficient  
- HIPAA handling is correct
- But **standard VMs are not cost-optimized** for batch workloads

---

## ✅ Final Answer

**B. Provision preemptible VMs to reduce cost. Disable and then discontinue use of all GCP services and APIs that are not HIPAA-compliant.**

---

# 3. Resilience Testing for Authentication Layer

## Question

Your customer wants to do **resilience testing** of their **authentication layer**.  
This consists of:
- A **regional managed instance group (MIG)** serving a **public REST API**
- The API **reads from and writes to a Cloud SQL instance**

What should you do?

A. Engage with a security company to run web scrapers that look for users' authentication data on malicious websites and notify you if any is found.  
B. Deploy intrusion detection software to your virtual machines to detect and log unauthorized access.  
C. Schedule a disaster simulation exercise during which you can shut off all VMs in a zone to see how your application behaves.  
D. Configure a read replica for your Cloud SQL instance in a different zone than the master, and then manually trigger a failover while monitoring KPIs for your REST API.

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct

- **Resilience testing** focuses on how systems behave during **failures**
- Cloud SQL is a **critical dependency** of the authentication layer
- Configuring a **read replica in a different zone** and **triggering a failover**:
  - Simulates a realistic database failure
  - Tests application behavior under disruption
  - Validates:
    - Application availability
    - Error handling
    - Recovery time
    - KPI impact (latency, errors, availability)

This aligns with **Google-recommended disaster recovery and resilience testing practices**.

---

## ❌ Why the other options are incorrect

### **A. Web scraping for leaked credentials**
❌ Security monitoring, not resilience testing  
- Detects data exposure
- Does not test system availability or fault tolerance

---

### **B. Intrusion detection software**
❌ Security-focused, not resilience-focused  
- Helps detect attacks
- Does not simulate infrastructure or service failures

---

### **C. Shutting off all VMs in a zone**
❌ Incomplete and risky  
- Tests compute resilience only
- Does **not validate Cloud SQL failover behavior**
- Could cause unnecessary downtime without testing the actual bottleneck (database)

---

## ✅ Final Answer

**D. Configure a read replica for your Cloud SQL instance in a different zone than the master, and then manually trigger a failover while monitoring KPIs for your REST API.**

---

# 4. Auditing BigQuery Queries by User

## Question

Your **BigQuery project** has several users. For **audit purposes**, you need to see how many queries each user ran in the **last month**.  

What should you do?

A. Connect Google Data Studio to BigQuery. Create a dimension for the users and a metric for the amount of queries per user.  
B. In the BigQuery interface, execute a query on the JOBS table to get the required information.  
C. Use `bq show` to list all jobs. Per job, use `bq ls` to list job information and get the required information.  
D. Use **Cloud Audit Logging** to view Cloud Audit Logs, and create a filter on the **query operation** to get the required information.

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct

- BigQuery automatically logs **all query jobs** to **Cloud Audit Logs**.  
- Audit logs provide:
  - Who ran the query (`protoPayload.authenticationInfo.principalEmail`)
  - When the query ran
  - The query text and job metadata
- You can filter **Cloud Audit Logs** for:
  - **Query operations**
  - A specific **time range** (e.g., last month)
- This is the **Google-recommended way** to perform auditing for BigQuery usage.

---

## ❌ Why the other options are incorrect

### **A. Use Google Data Studio**
❌ Incorrect  
- Data Studio visualizes **existing BigQuery data**
- It **cannot audit historical query usage by users**

---

### **B. Query the JOBS table**
❌ Incorrect  
- BigQuery does **not expose a built-in JOBS table**
- Jobs info is accessible via **Cloud Audit Logs** or the **BigQuery Jobs API**

---

### **C. Use `bq show` and `bq ls`**
❌ Incorrect  
- This approach is **manual, incomplete, and inefficient** for auditing multiple users and jobs
- Does not scale for monthly reports

---

## ✅ Final Answer

**D. Use Cloud Audit Logging to view Cloud Audit Logs, and create a filter on the query operation to get the required information.**

---

# 5. Automating Managed Instance Group with Preinstalled OS Packages

## Question

You want to **automate the creation of a managed instance group**. The VMs have **many OS package dependencies**. You want to **minimize the startup time** for new VMs in the instance group.  

What should you do?

A. Use Terraform to create the managed instance group and a startup script to install the OS package dependencies.  
B. Create a **custom VM image** with all OS package dependencies. Use Deployment Manager to create the managed instance group with the VM image.  
C. Use Puppet to create the managed instance group and install the OS package dependencies.  
D. Use Deployment Manager to create the managed instance group and Ansible to install the OS package dependencies.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Custom VM images** allow you to **pre-install all OS package dependencies**.  
- Benefits:
  - **Minimizes startup time**: new VMs don’t need to install packages at boot.  
  - **Consistency**: all instances are identical with the required software.  
  - Can be combined with **Deployment Manager** to automate the creation of the managed instance group.  
- This approach follows **Google best practices** for managing VM dependencies in managed instance groups.

---

## ❌ Why the other options are incorrect

### **A. Terraform + startup script**
❌ Incorrect  
- Startup scripts run **at boot time**, increasing VM startup time.  
- Not ideal when VMs have **many dependencies**.

---

### **C. Puppet**
❌ Incorrect  
- Puppet automates configuration but requires **installation at boot or post-provisioning**.  
- Startup time is not optimized compared to a prebuilt VM image.

---

### **D. Deployment Manager + Ansible**
❌ Incorrect  
- Ansible provisions software **after the VM boots**.  
- This does not minimize startup time for the managed instance group.

---

## ✅ Final Answer

**B. Create a custom VM image with all OS package dependencies. Use Deployment Manager to create the managed instance group with the VM image.**

---

# 6. BigQuery Country-Specific Access for Analysts

## Question

Your company captures all web traffic data in **Google Analytics 360** and stores it in **BigQuery**. Each country has its own **dataset**, and each dataset has multiple tables.  

You want **analysts from each country** to be able to see and query **only the data for their respective countries**.  

How should you configure the access rights?

A. Create a group per country. Add analysts to their respective country-groups. Create a single group 'all_analysts', and add all country-groups as members. Grant the 'all_analysts' group the IAM role of BigQuery jobUser. Share the appropriate dataset with view access with each respective analyst country-group.  

B. Create a group per country. Add analysts to their respective country-groups. Create a single group 'all_analysts', and add all country-groups as members. Grant the 'all_analysts' group the IAM role of BigQuery jobUser. Share the appropriate tables with view access with each respective analyst country-group.  

C. Create a group per country. Add analysts to their respective country-groups. Create a single group 'all_analysts', and add all country-groups as members. Grant the 'all_analysts' group the IAM role of BigQuery dataViewer. Share the appropriate dataset with view access with each respective analyst country-group.  

D. Create a group per country. Add analysts to their respective country-groups. Create a single group 'all_analysts', and add all country-groups as members. Grant the 'all_analysts' group the IAM role of BigQuery dataViewer. Share the appropriate table with view access with each respective analyst country-group.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- **Groups per country** ensure analysts can be managed efficiently.
- **Dataset-level sharing** allows all tables in the dataset to be queried **without granting access to other countries’ data**.
- **BigQuery jobUser IAM role** allows analysts to **run queries**, but **does not grant access to datasets** by itself.
- Sharing the dataset with **view access** to the respective country-group enforces **row/table isolation by country**.
- Follows Google **best practices** for dataset-level access control in BigQuery.

---

## ❌ Why the other options are incorrect

### **B. Grant access at table level**
❌ Incorrect  
- Table-level sharing is **more granular and harder to maintain** than dataset-level sharing when multiple tables exist.  
- Dataset-level sharing is simpler and preferred for country-specific datasets.

---

### **C. Use BigQuery dataViewer IAM role**
❌ Incorrect  
- **dataViewer at project level** grants **read access to all datasets** in the project, which violates the requirement to isolate data per country.  
- You only want dataset-specific access, not project-wide.

---

### **D. Table-level + dataViewer role**
❌ Incorrect  
- Same issues as B and C combined: **too granular** and **dataViewer may overgrant access** beyond intended datasets.

---

## ✅ Final Answer

**A. Create a group per country. Add analysts to their respective country-groups. Create a single group 'all_analysts', and add all country-groups as members. Grant the 'all_analysts' group the IAM role of BigQuery jobUser. Share the appropriate dataset with view access with each respective analyst country-group.**

---

# 7. Cost-Effective Storage Allocation for GCP Migration

## Question

You have been engaged by your client to lead the **migration of their application infrastructure to GCP**. Their current problem is that their **on-premises high-performance SAN** requires frequent and expensive upgrades. Current workloads are:

- **20 TB** of log archives retained for legal reasons  
- **500 GB** of VM boot/data volumes and templates  
- **500 GB** of image thumbnails  
- **200 GB** of customer session state data (allows customers to restart sessions even if offline for several days)  

Which of the following best reflects a **cost-effective storage allocation**?

A. Local SSD for customer session state data. Lifecycle-managed Cloud Storage for log archives, thumbnails, and VM boot/data volumes.  

B. Memcache backed by Cloud Datastore for the customer session state data. Lifecycle-managed Cloud Storage for log archives, thumbnails, and VM boot/data volumes.  

C. Memcache backed by Cloud SQL for customer session state data. Assorted local SSD-backed instances for VM boot/data volumes. Cloud Storage for log archives and thumbnails.  

D. Memcache backed by Persistent Disk SSD storage for customer session state data. Assorted local SSD-backed instances for VM boot/data volumes. Cloud Storage for log archives and thumbnails.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

- **Customer session state data (200 GB):**  
  - Memcache **alone is in-memory and ephemeral**, so we need **durable backing** to survive VM restarts.  
  - **Cloud Datastore** provides a **durable, highly available NoSQL database**, ideal for session state that needs persistence over days.  

- **Log archives, thumbnails, VM boot/data volumes:**  
  - Use **Cloud Storage with Object Lifecycle Management** to manage retention and deletion policies efficiently.  
  - This is cost-effective for **large, infrequently accessed data** like log archives and thumbnails.  

- **Cost-effectiveness:**  
  - Avoids over-provisioning local SSDs for large static datasets.  
  - Avoids expensive high-performance SANs by leveraging **managed, durable, and scalable GCP storage services**.  

---

## ❌ Why the other options are incorrect

### **A. Local SSD for session state**
❌ Incorrect  
- Local SSD is **ephemeral**, so data is lost on VM stop/restart.  
- Not suitable for session state that must persist for several days.

---

### **C. Memcache backed by Cloud SQL**
❌ Incorrect  
- Cloud SQL is **more expensive** and better suited for relational data, not ephemeral session states.  
- Local SSD-backed VMs for VM boot/data volumes adds unnecessary operational complexity.

---

### **D. Memcache backed by Persistent Disk SSD**
❌ Incorrect  
- Persistent Disk SSD is **more expensive** than Cloud Datastore for this use case.  
- Again, overkill for session data that requires durability but not extremely high IOPS.

---

## ✅ Final Answer

**B. Memcache backed by Cloud Datastore for the customer session state data. Lifecycle-managed Cloud Storage for log archives, thumbnails, and VM boot/data volumes.**

---

# 8. Consistent Hostnames in GKE Workloads

## Question

Your web application uses **Google Kubernetes Engine (GKE)** to manage several workloads. One workload requires a **consistent set of hostnames** even after **pod scaling and relaunches**.  

Which feature of Kubernetes should you use to accomplish this?

A. StatefulSets  
B. Role-based access control  
C. Container environment variables  
D. Persistent Volumes  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

- **StatefulSets** in Kubernetes are designed for workloads that require:

  - **Stable, unique network identifiers (hostnames)**  
  - **Stable, persistent storage**  
  - **Ordered, graceful deployment and scaling**

- Each pod in a StatefulSet:

  - Gets a **unique, predictable hostname** (e.g., `myapp-0`, `myapp-1`)  
  - Retains this hostname even after scaling or restart  
  - Can be associated with **persistent storage** if needed  

- Common use cases:

  - Databases (MySQL, PostgreSQL, MongoDB)  
  - Distributed systems (Kafka, Zookeeper, Elasticsearch)  

---

## ❌ Why the other options are incorrect

### **B. Role-based access control (RBAC)**
❌ Incorrect  
- RBAC is for **access control and permissions**, not for managing pod hostnames.

### **C. Container environment variables**
❌ Incorrect  
- Environment variables can configure applications but **do not enforce stable hostnames** for pods.

### **D. Persistent Volumes**
❌ Incorrect  
- Persistent Volumes provide **durable storage**, but **do not assign stable hostnames** to pods.

---

## ✅ Final Answer

**A. StatefulSets**

---

# 9. Improving Cache Hit Ratio with Cloud CDN

## Question

You are using **Cloud CDN** to deliver static HTTP(S) website content hosted on a **Compute Engine instance group**. You want to **improve the cache hit ratio**.  

Which action should you take?

A. Customize the cache keys to omit the protocol from the key.  
B. Shorten the expiration time of the cached objects.  
C. Make sure the HTTP(S) header `Cache-Region` points to the closest region of your users.  
D. Replicate the static content in a Cloud Storage bucket. Point Cloud CDN toward a load balancer on that bucket.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct

- **Cloud CDN** works best with **origin servers that are highly available and can serve cached content efficiently**.
- Hosting **static content in Cloud Storage** provides:

  - **High availability**  
  - **Automatic scaling**  
  - **Optimized performance for caching at edge locations**

- Pointing **Cloud CDN** to a **load balancer backed by Cloud Storage** ensures that:

  - Content is **uniform** across all edge locations  
  - The cache hit ratio improves because the origin consistently serves cacheable content

---

## ❌ Why the other options are incorrect

### **A. Customize the cache keys to omit the protocol**
❌ Incorrect  
- Omitting protocol from the cache key may **combine HTTP and HTTPS requests**, but this **does not inherently improve cache hit ratio** for static content.

### **B. Shorten the expiration time**
❌ Incorrect  
- Shorter expiration reduces **cache lifetime**, which **decreases** cache hit ratio, not improves it.

### **C. Use a `Cache-Region` header**
❌ Incorrect  
- Cloud CDN **does not use a `Cache-Region` header**. Cache location is **determined automatically by edge nodes and proximity to users**.

---

## ✅ Final Answer

**D. Replicate the static content in a Cloud Storage bucket. Point Cloud CDN toward a load balancer on that bucket.**

---

# 10. Centralized Collection of Admin Activity and VM System Logs

## Question

Your architecture calls for the **centralized collection** of all **admin activity** and **VM system logs** within your GCP project.  

How should you collect these logs from both **VMs** and **services**?

A. All admin and VM system logs are automatically collected by Stackdriver.  
B. Stackdriver automatically collects admin activity logs for most services. The Stackdriver Logging agent must be installed on each instance to collect system logs.  
C. Launch a custom `syslogd` Compute Engine instance and configure your GCP project and VMs to forward all logs to it.  
D. Install the Stackdriver Logging agent on a single Compute Engine instance and let it collect all audit and access logs for your environment.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

- **Stackdriver Logging (Cloud Logging)** automatically collects **admin activity logs** (audit logs) for most GCP services.  
- **VM system logs** (e.g., `/var/log/syslog` on Linux, Windows Event logs) are **not automatically collected**.  
- To collect VM system logs:

  - Install the **Stackdriver Logging agent** (Fluentd-based) on each VM instance.  
  - This allows logs to be sent to Cloud Logging alongside admin activity logs.  

- This approach ensures **centralized collection** of both **GCP service activity** and **VM-level logs**.

---

## ❌ Why the other options are incorrect

### **A. All admin and VM system logs are automatically collected**
❌ Incorrect  
- Admin activity logs are collected automatically.  
- **VM system logs require the Logging agent**; they are not automatically collected.

### **C. Launch a custom `syslogd` instance**
❌ Incorrect  
- Manually forwarding logs to a custom syslogd is **complex, error-prone, and not recommended**.  
- Cloud Logging provides **managed, centralized logging** out-of-the-box.

### **D. Install the Logging agent on a single VM**
❌ Incorrect  
- The Logging agent must be installed **on each VM** to collect its system logs.  
- A single VM cannot collect logs from other VMs automatically.

---

## ✅ Final Answer

**B. Stackdriver automatically collects admin activity logs for most services. The Stackdriver Logging agent must be installed on each instance to collect system logs.**
