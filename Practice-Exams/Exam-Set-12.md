# 1. Reliable Task Scheduling for Compute on GCP

## Question

You need to ensure **reliability** for your application and operations by supporting **reliable task scheduling** for compute on GCP.  
Leveraging **Google best practices**, what should you do?

A. Using the Cron service provided by App Engine, publish messages directly to a message-processing utility service running on Compute Engine instances.  
B. Using the Cron service provided by App Engine, publish messages to a **Cloud Pub/Sub topic**. Subscribe to that topic using a message-processing utility service running on Compute Engine instances.  
C. Using the Cron service provided by Google Kubernetes Engine (GKE), publish messages directly to a message-processing utility service running on Compute Engine instances.  
D. Using the Cron service provided by GKE, publish messages to a Cloud Pub/Sub topic. Subscribe to that topic using a message-processing utility service running on Compute Engine instances.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

- **App Engine Cron service** is a fully managed and reliable scheduler.
- **Publishing tasks to Cloud Pub/Sub** ensures:
  - **Durable delivery** of messages
  - **Decoupling** of scheduling from task execution
  - **Scalable processing** by multiple consumers
- **Compute Engine instances** subscribe to the Pub/Sub topic to process tasks asynchronously.
- Benefits:
  - Ensures **no tasks are lost** even if the compute layer is temporarily unavailable
  - Follows **Google best practices** for reliable task scheduling

Workflow:
1. App Engine Cron triggers the scheduled task.
2. Cron publishes a message to a Cloud Pub/Sub topic.
3. Compute Engine workers subscribe to the topic and process tasks reliably.

---

## ❌ Why the other options are incorrect

### **A. Directly publish messages to Compute Engine**
❌ Incorrect  
- No durability guarantee if the Compute Engine service is down
- Tightly couples scheduler and worker, reducing reliability

---

### **C. Cron in GKE publishing directly**
❌ Incorrect  
- GKE CronJobs are not as managed/reliable as App Engine Cron for distributed scheduling
- Direct delivery lacks reliability and durability

---

### **D. Cron in GKE publishing to Pub/Sub**
❌ Incorrect  
- Using GKE CronJobs adds unnecessary management overhead
- App Engine Cron is simpler and fully managed for scheduled tasks

---

## ✅ Final Answer

**B. Using the Cron service provided by App Engine, publish messages to a Cloud Pub/Sub topic. Subscribe to that topic using a message-processing utility service running on Compute Engine instances.**

---

# 2. Data Migration and Daily Load to GCP

## Question

Your company is building a new architecture to support its data-centric business focus. You are responsible for setting up the network. Your company's **mobile and web-facing applications will be deployed on-premises**, and all **data analysis will be conducted in GCP**.  

The plan is to:

- Process and load **7 years of archived .csv files totaling 900 TB** of data  
- Continue loading **10 TB of data daily**  

You currently have an existing **100-MB internet connection**.  

Which actions will meet your company's needs?

A. Compress and upload both archived files and files uploaded daily using the `gsutil -m` option.  
B. Lease a **Transfer Appliance**, upload archived files to it, and send it to Google to transfer archived data to Cloud Storage. Establish a connection with Google using a **Dedicated Interconnect or Direct Peering connection** and use it to upload files daily.  
C. Lease a Transfer Appliance, upload archived files to it, and send it to Google to transfer archived data to Cloud Storage. Establish **one Cloud VPN Tunnel** to VPC networks over the public internet, and compress and upload files daily using the `gsutil -m` option.  
D. Lease a Transfer Appliance, upload archived files to it, and send it to Google to transfer archived data to Cloud Storage. Establish a **Cloud VPN Tunnel** to VPC networks over the public internet, and compress and upload files daily.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

1. **Bulk Archive Transfer**
   - **900 TB** is too large for a 100-MB internet connection.  
     - At 100 Mbps, transferring 900 TB would take **years**.  
   - **Transfer Appliance** is designed for bulk offline data transfer.
     - Upload the 900 TB to the appliance on-premises.
     - Ship the appliance to Google; data is ingested into **Cloud Storage**.

2. **Daily Data Upload**
   - **10 TB daily** is still too much for the existing 100-MB link for reliable daily uploads.  
   - Using **Dedicated Interconnect or Direct Peering**:
     - Provides **high-bandwidth, low-latency, private connectivity** to GCP.
     - Ensures daily ingestion of large datasets reliably and securely.
   - Compressing files with `gsutil -m` is helpful but **not sufficient for 10 TB/day on a 100-MB connection**.

3. **Recommended Approach**
   - **Hybrid strategy**:
     - **Offline bulk transfer** → Transfer Appliance for the archive
     - **High-speed private network** → Dedicated Interconnect / Direct Peering for ongoing daily uploads

---

## ❌ Why the other options are incorrect

### **A. Upload all files over the internet**
❌ Incorrect  
- 900 TB archive + 10 TB daily via 100-MB internet is **impractical**  
- Would take years to complete the initial load  

---

### **C. Transfer Appliance + Cloud VPN**
❌ Incorrect  
- **Cloud VPN** is **limited in bandwidth (~1–3 Gbps per tunnel)**  
- 10 TB daily data transfer is not practical over VPN for large-scale ingestion

---

### **D. Transfer Appliance + Cloud VPN without gsutil -m**
❌ Incorrect  
- Bandwidth limitation still applies; adding `gsutil -m` only improves parallelism slightly  
- Daily 10 TB transfer would **fail to meet SLA**  

---

## ✅ Final Answer

**B. Lease a Transfer Appliance, upload archived files to it, and send it to Google to transfer archived data to Cloud Storage. Establish a connection with Google using a Dedicated Interconnect or Direct Peering connection and use it to upload files daily.**

---

# 3. Guaranteed-once FIFO Delivery for a Legacy Streaming API

## Question

You are developing a **globally scaled frontend** for a legacy streaming backend data API.  

The API requirements are:

- **Strict chronological order** for events  
- **No repeated data** (exactly-once processing)  

Which products should you deploy to ensure **guaranteed-once FIFO (first-in, first-out) delivery** of data?

A. Cloud Pub/Sub alone  
B. Cloud Pub/Sub to Cloud Dataflow  
C. Cloud Pub/Sub to Stackdriver  
D. Cloud Pub/Sub to Cloud SQL  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

1. **Cloud Pub/Sub**
   - Provides **at-least-once delivery** by default.  
   - Can **publish globally**, scale horizontally, and maintain ordering **within an ordering key** (if configured).

2. **Cloud Dataflow**
   - Fully managed **streaming and batch processing service**  
   - Supports **exactly-once processing semantics**  
   - Can **enforce event-time ordering** and handle **deduplication**  
   - Integrates directly with Cloud Pub/Sub for streaming ingestion

3. **Combined Approach**
   - **Cloud Pub/Sub** handles scalable global event ingestion  
   - **Cloud Dataflow** ensures:
     - FIFO ordering across events
     - Deduplication for exactly-once delivery
   - Delivers data to your legacy backend in the proper order with no repeats

---

## ❌ Why the other options are incorrect

### **A. Cloud Pub/Sub alone**
❌ Incorrect  
- Pub/Sub guarantees **at-least-once delivery**, **not exactly-once** by itself  
- FIFO ordering is only guaranteed **within a single ordering key**  
- Deduplication and global ordering for a streaming API would require additional processing

---

### **C. Cloud Pub/Sub to Stackdriver**
❌ Incorrect  
- Stackdriver (Logging/Monitoring) **does not process or enforce delivery semantics**  
- Cannot guarantee ordering or exactly-once delivery  

---

### **D. Cloud Pub/Sub to Cloud SQL**
❌ Incorrect  
- Cloud SQL is a relational database, not a streaming processing service  
- Would require custom application logic to handle ordering and deduplication, which is **not recommended** for global-scale streams  

---

## ✅ Final Answer

**B. Cloud Pub/Sub to Cloud Dataflow**

---

# 4. Lift-and-Shift Migration of Linux VMs to Compute Engine

## Question

Your company plans to **perform a lift-and-shift migration** of Linux RHEL 6.5+ VMs running in an **on-premises VMware environment**.  

You want to migrate them to **Compute Engine** following **Google-recommended practices**.  

Which approach should you follow?

A. Define a migration plan based on the list of applications and their dependencies. Migrate all VMs individually with Migrate for Compute Engine.  
B. Perform an assessment of VMs in VMware. Create images of all disks. Import disks to Compute Engine. Create standard VMs using the imported boot disks.  
C. Perform an assessment of VMs in VMware. Define a migration plan, prepare a **Migrate for Compute Engine** migration RunBook, and execute the migration.  
D. Perform an assessment of VMs in VMware. Install a third-party agent on all selected VMs. Migrate all VMs into Compute Engine.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct

- **Migrate for Compute Engine** is Google’s recommended tool for lift-and-shift migrations of **on-premises VMware VMs**.  
- Best practices include:
  1. **Assessment of existing VMs**:
     - Inventory applications and dependencies  
     - Identify VM configurations, storage, and network requirements
  2. **Define a migration plan**:
     - Determine migration phases
     - Identify downtime requirements
  3. **Prepare a migration RunBook**:
     - Document the steps for migration, cutover, validation, and rollback  
  4. **Execute migration** using **Migrate for Compute Engine**:
     - Supports **minimal downtime**
     - Handles **OS, application, and disk replication**
     - Ensures a repeatable, auditable process

- This approach follows Google’s **lift-and-shift migration methodology** and leverages **native tools** for reliability and repeatability.

---

## ❌ Why the other options are incorrect

### **A. Migrate all VMs individually without assessment or RunBook**
❌ Incorrect  
- Does not include an **assessment or proper migration planning**  
- Risks **application downtime** and **dependency issues**

---

### **B. Create images and import disks manually**
❌ Incorrect  
- Manual disk image import is **not recommended for large-scale VMware migrations**  
- Lacks orchestration, minimal downtime, and dependency handling provided by Migrate for Compute Engine

---

### **D. Install a third-party agent on all VMs**
❌ Incorrect  
- Not necessary with **Migrate for Compute Engine**, which **does not require agent installation** on Linux VMs  
- Adds unnecessary operational overhead

---

## ✅ Final Answer

**C. Perform an assessment of VMs in VMware, define a migration plan, prepare a Migrate for Compute Engine migration RunBook, and execute the migration.**

---

# 5. Deploying a Single-Instance, Stateful TCP Application on GCP

## Question

You need to deploy an application to Google Cloud with the following requirements:

- Receives **TCP traffic**  
- Reads and writes **data to the filesystem**  
- Does **not support horizontal scaling** (single-writer application)  
- Requires **full control over filesystem data** to prevent corruption  
- Downtime is acceptable for incidents, but must be available **24/7**  
- Must **handle failover** between zones  

Which architecture should you choose?

A. Managed instance group in multiple zones, Cloud Filestore, HTTP load balancer  
B. Managed instance group in multiple zones, Cloud Filestore, network load balancer  
C. Unmanaged instance group with active + standby in different zones, regional persistent disk, HTTP load balancer  
D. Unmanaged instance group with active + standby in different zones, regional persistent disk, network load balancer  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Key Considerations

1. **Application constraints**:
   - Single-writer: cannot scale horizontally → **no managed instance group with multiple active instances**  
   - Requires **exclusive filesystem access** → must use **regional persistent disk** or equivalent

2. **Traffic type**:
   - Receives **TCP traffic**, not HTTP → **network load balancer** is required instead of HTTP(S) LB

3. **High availability**:
   - Active + standby architecture across **zones** ensures **24/7 availability**  
   - Regional persistent disk can be **attached to only one instance at a time**, avoiding data corruption

---

### Why **D** is correct

- **Unmanaged instance group** allows manual control of **active/standby instances**  
- **Regional persistent disk** provides **multi-zone redundancy** while ensuring **single-writer access**  
- **Network load balancer** supports **TCP traffic** to the active instance  
- When active fails, the standby can attach the regional disk and take over  

This approach meets Google Cloud’s **high availability best practices for stateful single-writer workloads**.

---

## ❌ Why the other options are incorrect

### **A & B. Managed instance group with Cloud Filestore**
❌ Incorrect  
- Managed instance group **would launch multiple instances**, violating the single-writer constraint  
- Cloud Filestore allows **shared access**, but this could cause **data corruption** if multiple instances write concurrently  
- HTTP LB (A) or network LB (B) do not handle single-writer failover correctly in this scenario  

---

### **C. Unmanaged instance group + regional PD + HTTP LB**
❌ Incorrect  
- HTTP load balancer only supports **HTTP(S) traffic**, not generic TCP → cannot serve TCP application  

---

## ✅ Final Answer

**D. Use an unmanaged instance group with an active and standby instance in different zones, use a regional persistent disk, and use a network load balancer in front of the instances.**

---

# 6. High-Throughput, Low-Latency Connectivity to On-Premises from GCP

## Question

Your company has an application running on multiple Compute Engine instances. You need to ensure that the application can communicate with an on-premises service that requires **high throughput via internal IPs** while **minimizing latency**.

Which approach should you take?

A. Use OpenVPN to configure a VPN tunnel between the on-premises environment and Google Cloud  
B. Configure a direct peering connection between the on-premises environment and Google Cloud  
C. Use Cloud VPN to configure a VPN tunnel between the on-premises environment and Google Cloud  
D. Configure a Cloud Dedicated Interconnect connection between the on-premises environment and Google Cloud  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Requirements Analysis

1. **High throughput** → VPN tunnels over the public Internet have limited bandwidth (~1–3 Gbps per tunnel)  
2. **Internal IP communication** → Traffic must be routed through private/internal IPs, not public addresses  
3. **Low latency** → Minimize network hops and avoid Internet variability  

### Why **D** is correct

- **Cloud Dedicated Interconnect** provides a **direct, physical connection** between your on-premises network and Google Cloud  
- Offers **multi-ten Gbps throughput** per connection (up to 100 Gbps)  
- Provides **internal IP connectivity**, enabling private network access to Compute Engine instances and other GCP services  
- Reduces latency compared to VPN solutions over the public Internet  

This is the **Google-recommended solution for enterprise, latency-sensitive, high-throughput connectivity**.

---

## ❌ Why the other options are incorrect

### **A. OpenVPN**
❌ Incorrect  
- Runs over the public Internet → **limited throughput** and **higher latency**  
- Not recommended for production high-throughput workloads  

---

### **B. Direct Peering**
❌ Incorrect  
- Direct Peering only supports **public IP connectivity** → cannot use **internal IPs**  
- Not suitable for private, internal communication  

---

### **C. Cloud VPN**
❌ Incorrect  
- Supports internal IP routing with **IPsec tunnels**  
- **Limited throughput (up to a few Gbps per tunnel)** and higher latency compared to Dedicated Interconnect  
- Suitable for smaller workloads or temporary setups, not high-throughput production  

---

## ✅ Final Answer

**D. Configure a Cloud Dedicated Interconnect connection between the on-premises environment and Google Cloud**

---

#  7. Blue/Canary Deployment Strategy on Cloud Run for Anthos

## Question

You are managing an application deployed on **Cloud Run for Anthos**, and you need to define a strategy for deploying new versions of the application. You want to **evaluate the new code with a subset of production traffic** to decide whether to proceed with the rollout.

Which approach should you take?

A. Deploy a new revision to Cloud Run with the new version. Configure traffic percentage between revisions.  
B. Deploy a new service to Cloud Run with the new version. Add a Cloud Load Balancing instance in front of both services.  
C. In the Google Cloud Console page for Cloud Run, set up continuous deployment using Cloud Build for the development branch. As part of the Cloud Build trigger, configure the substitution variable TRAFFIC_PERCENTAGE with the percentage of traffic you want directed to a new version.  
D. In the Google Cloud Console, configure Traffic Director with a new Service that points to the new version of the application on Cloud Run. Configure Traffic Director to send a small percentage of traffic to the new version of the application.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

- **Cloud Run for Anthos** supports **revisions** of a service. Each deployment creates a new revision.  
- You can **split traffic** between revisions, allowing you to **send a percentage of production traffic** to the new version.  
- This approach implements **canary deployments**, letting you **evaluate the new code with a subset of users** before rolling it out fully.  
- Traffic splitting is **native to Cloud Run for Anthos**, requiring no additional infrastructure.  

---

## ❌ Why the other options are incorrect

### **B. Deploy a new service and add a load balancer**
❌ Incorrect  
- Creating a separate service and a load balancer is **unnecessary complexity**.  
- Cloud Run natively supports **revision-level traffic splitting**, eliminating the need for manual load balancing between services.

---

### **C. Use Cloud Build substitution variable TRAFFIC_PERCENTAGE**
❌ Incorrect  
- TRAFFIC_PERCENTAGE is **not a Cloud Build feature**.  
- Cloud Build handles CI/CD, but **traffic splitting is done at the Cloud Run service level**, not via build triggers.

---

### **D. Use Traffic Director**
❌ Incorrect  
- Traffic Director is a **service mesh and global load balancing solution**.  
- While possible, it is **overkill** for basic canary traffic splitting in Cloud Run for Anthos.  
- Native traffic splitting between revisions is simpler and recommended.

---

## ✅ Final Answer

**A. Deploy a new revision to Cloud Run with the new version. Configure traffic percentage between revisions.**

---

# 8. Monitoring GKE Clusters and Triaging Incidents

## Question

You are monitoring **Google Kubernetes Engine (GKE)** clusters in a **Cloud Monitoring workspace**. As a **Site Reliability Engineer (SRE)**, you need to **triage incidents quickly**.

Which approach should you take?

A. Navigate the predefined dashboards in the Cloud Monitoring workspace, and then add metrics and create alert policies.  
B. Navigate the predefined dashboards in the Cloud Monitoring workspace, create custom metrics, and install alerting software on a Compute Engine instance.  
C. Write a shell script that gathers metrics from GKE nodes, publish these metrics to a Pub/Sub topic, export the data to BigQuery, and make a Data Studio dashboard.  
D. Create a custom dashboard in the Cloud Monitoring workspace for each incident, and then add metrics and create alert policies.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

- **Cloud Monitoring** provides **predefined dashboards for GKE** that display key metrics such as CPU, memory, pod status, and cluster health.  
- You can **quickly triage incidents** by using these dashboards.  
- Alert policies can be configured for specific metrics to **notify you automatically** when thresholds are breached.  
- This method leverages **native GCP features** for monitoring and alerting, providing fast, reliable incident triage without unnecessary complexity.

---

## ❌ Why the other options are incorrect

### **B. Predefined dashboards + custom metrics + alerting software on Compute Engine**
❌ Incorrect  
- Installing additional alerting software on Compute Engine is **unnecessary**, since Cloud Monitoring already provides alerting capabilities.  
- Creating custom metrics may help in special cases, but for **quick triage**, the predefined metrics are sufficient.

---

### **C. Shell script + Pub/Sub + BigQuery + Data Studio**
❌ Incorrect  
- This approach is **too complex and slow** for real-time incident triage.  
- Building custom pipelines is not required when **native dashboards and alerting exist**.

---

### **D. Create a custom dashboard for each incident**
❌ Incorrect  
- Creating a new dashboard **per incident is inefficient** and does not support fast triage.  
- Predefined dashboards are designed for **real-time visibility** and are reusable across incidents.

---

## ✅ Final Answer

**A. Navigate the predefined dashboards in the Cloud Monitoring workspace, and then add metrics and create alert policies.**

---

# 9. Ensuring Data Protection for Cloud SQL MySQL

## Question

You are implementing a single **Cloud SQL MySQL second-generation** database that contains **business-critical transaction data**. You want to ensure that the **minimum amount of data is lost** in case of a catastrophic failure.  

Which **two features** should you implement? (Choose two.)

A. Sharding  
B. Read replicas  
C. Binary logging  
D. Automated backups  
E. Semisynchronous replication  

---

## ✅ **Correct Answers: C, D**

---

## Explanation

### **C. Binary logging** ✅
- **Binary logs** record all changes to the database (insert, update, delete operations).  
- They allow **point-in-time recovery (PITR)** by replaying events from a specific timestamp.  
- This minimizes **data loss** after a catastrophic failure.

### **D. Automated backups** ✅
- **Automated backups** create daily snapshots of the database.  
- Backups are essential for **restoring the database** in case of total failure.  
- Combined with binary logs, they allow restoring to any point-in-time.

---

## ❌ Why the other options are incorrect

### **A. Sharding** ❌
- Sharding **splits data across multiple databases**.  
- It improves **scalability** but does **not reduce data loss** in a single catastrophic event.

### **B. Read replicas** ❌
- Read replicas provide **horizontal scaling for read traffic** and **some disaster recovery**.  
- However, MySQL replicas are **asynchronous by default**, so they **may not guarantee zero data loss**.

### **E. Semisynchronous replication** ❌
- Semisynchronous replication ensures that **at least one replica confirms a transaction** before committing.  
- This is not natively available for a single Cloud SQL instance; it only applies when you have replicas.

---

## ✅ Final Answer

**C. Binary logging**  
**D. Automated backups**

---

# 10. Handling Deletion Requests for Personal Data in BigQuery

## Question

You are working at a sports association whose members range in age from 8 to 30. The association collects a large amount of **health data**, such as sustained injuries. You are storing this data in **BigQuery**. Current legislation requires you to **delete such information upon request** of the subject.  

You want to design a solution that can accommodate such a request.  

A. Use a unique identifier for each individual. Upon a deletion request, delete all rows from BigQuery with this identifier.  
B. When ingesting new data in BigQuery, run the data through the Data Loss Prevention (DLP) API to identify any personal information. As part of the DLP scan, save the result to Data Catalog. Upon a deletion request, query Data Catalog to find the column with personal information.  
C. Create a BigQuery view over the table that contains all data. Upon a deletion request, exclude the rows that affect the subject's data from this view. Use this view instead of the source table for all analysis tasks.  
D. Use a unique identifier for each individual. Upon a deletion request, overwrite the column with the unique identifier with a salted SHA256 of its value.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### **A. Use a unique identifier for each individual. Upon a deletion request, delete all rows from BigQuery with this identifier.** ✅
- Assigning a **unique identifier** (e.g., member ID) to each subject ensures **easy lookup** of all rows related to that individual.  
- Using SQL, you can **delete rows** from BigQuery to comply with legal deletion requests (`DELETE FROM table WHERE member_id = ...`).  
- This approach **fully removes personal data**, which satisfies GDPR or similar privacy requirements.  

---

### ❌ Why the other options are incorrect

**B. DLP + Data Catalog** ❌  
- DLP API can detect sensitive data but does **not directly automate deletions**.  
- Data Catalog tracks columns but **cannot remove rows for a specific subject**.  
- This approach is overly complex and does not satisfy the deletion requirement efficiently.

**C. BigQuery view excluding rows** ❌  
- Using a view only **hides data from queries**, it does **not delete the personal data**.  
- This is **not sufficient for legal compliance**, as the underlying data still exists.

**D. Overwrite unique identifiers with a salted SHA256** ❌  
- Hashing the identifier pseudonymizes the data but may **not remove other personal information** in the row.  
- Full deletion is still the recommended approach under GDPR-like requirements.

---

## ✅ Final Answer

**A. Use a unique identifier for each individual. Upon a deletion request, delete all rows from BigQuery with this identifier.**
