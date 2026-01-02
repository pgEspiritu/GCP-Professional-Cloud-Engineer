# 1. Google Cloud Case Study — Authentication Strategy for Migrating Corporate Applications

## Question

Your customer is moving an existing corporate application to Google Cloud Platform from an on-premises data center. The business owners require **minimal user disruption**. There are **strict security team requirements for storing passwords**.  

What authentication strategy should they use?  

A. Use G Suite Password Sync to replicate passwords into Google  
B. Federate authentication via SAML 2.0 to the existing Identity Provider  
C. Provision users in Google using the Google Cloud Directory Sync tool  
D. Ask users to set their Google password to match their corporate password  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Federate authentication via SAML 2.0 to the existing Identity Provider**
- **SAML 2.0 federation** allows Google Cloud to **delegate authentication** to the company’s existing Identity Provider (IdP), such as Active Directory, ADFS, or another enterprise IdP.  
- This approach:
  - Provides **Single Sign-On (SSO)** with **minimal user disruption**.
  - Ensures **passwords remain on-premises** and are never stored in Google Cloud.
  - Meets **strict security requirements** by avoiding password replication or duplication.
- This is a **Google-recommended best practice** for enterprise migrations.

---

## ❌ Why the other options are not correct

### **A. Use G Suite Password Sync to replicate passwords into Google**
- This **replicates password hashes into Google**, which violates strict password storage policies.
- It increases security risk and operational complexity.

### **C. Provision users in Google using the Google Cloud Directory Sync tool**
- Directory Sync only **creates and manages user accounts**, not authentication.
- Passwords still need to be stored or managed separately.

### **D. Ask users to set their Google password to match their corporate password**
- This creates **security risk** and **password reuse issues**.
- It relies on users to manually manage passwords, which does not meet enterprise security standards.

---

## ✅ Final Answer

**B. Federate authentication via SAML 2.0 to the existing Identity Provider**

---

# 2. Google Cloud Case Study — Analyzing Batch and Streaming Data

## Question

Your company has successfully migrated to the cloud and wants to **analyze their data stream to optimize operations**. They do not have any existing code for this analysis, so they are exploring all their options. These options include a **mix of batch and stream processing**, as they are running some hourly jobs and live-processing some data as it comes in.  

Which technology should they use for this?  

A. Google Cloud Dataproc  
B. Google Cloud Dataflow  
C. Google Container Engine with Bigtable  
D. Google Compute Engine with Google BigQuery  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Google Cloud Dataflow**
- **Cloud Dataflow** is a **fully managed, serverless data processing service** built on **Apache Beam**.
- It natively supports:
  - **Streaming pipelines** for real-time data processing.
  - **Batch pipelines** for scheduled or hourly jobs.
- Since the team has **no existing code**, Dataflow is ideal because:
  - Apache Beam provides a **unified programming model** for both batch and streaming.
  - Google manages **scaling, resource provisioning, and fault tolerance** automatically.
- This makes Dataflow the **best fit for mixed workloads** with minimal operational overhead.

---

## ❌ Why the other options are not correct

### **A. Google Cloud Dataproc**
- Dataproc is a managed **Hadoop/Spark** service.
- Best suited when you already have **existing Spark or Hadoop jobs**.
- Requires more **cluster management** than Dataflow.

### **C. Google Container Engine with Bigtable**
- Kubernetes with Bigtable requires:
  - **Custom application development**
  - Ongoing **infrastructure and scaling management**
- Not optimized for analytics pipelines out of the box.

### **D. Google Compute Engine with Google BigQuery**
- BigQuery is excellent for **SQL-based analytics**, but it is not a **stream and batch processing engine**.
- Compute Engine requires **manual scaling and orchestration**, increasing complexity.

---

## ✅ Final Answer

**B. Google Cloud Dataflow**

---

# 3. Google Cloud Case Study — Diagnosing App Engine Performance Issues

## Question

Your customer is receiving reports that their recently updated **Google App Engine application** is taking approximately **30 seconds to load** for some users.  
This behavior was **not reported before the update**.  

What strategy should you take?  

A. Work with your ISP to diagnose the problem  
B. Open a support ticket to ask for network capture and flow data to diagnose the problem, then roll back your application  
C. Roll back to an earlier known good release initially, then use Stackdriver Trace and Logging to diagnose the problem in a development/test/staging environment  
D. Roll back to an earlier known good release, then push the release again at a quieter period to investigate. Then use Stackdriver Trace and Logging to diagnose the problem  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Roll back to an earlier known good release initially, then use Stackdriver Trace and Logging to diagnose the problem in a development/test/staging environment**
- Since the issue **appeared after a recent update**, the safest immediate action is to:
  - **Roll back to a known good version** to restore acceptable user experience.
- After stabilizing production:
  - Use **Cloud Trace** to identify slow requests and latency bottlenecks.
  - Use **Cloud Logging** to analyze errors or regressions introduced by the update.
- Performing analysis in a **development/test/staging environment** avoids further impact to users.
- This follows **Google Site Reliability Engineering (SRE) best practices**:
  - Restore service first
  - Investigate and fix safely afterward

---

## ❌ Why the other options are not correct

### **A. Work with your ISP to diagnose the problem**
- The issue is **application-specific** and correlated with a recent deployment.
- ISP involvement is unlikely to identify application-level performance regressions.

### **B. Open a support ticket to ask for network capture and flow data to diagnose the problem, then roll back your application**
- Network captures are **not the first step** for App Engine performance issues.
- Rolling back **after** lengthy diagnosis increases user impact.

### **D. Roll back to an earlier known good release, then push the release again at a quieter period to investigate**
- Re-deploying a **known problematic release** risks reintroducing the issue.
- Investigation should occur in **non-production environments**, not during live traffic.

---

## ✅ Final Answer

**C. Roll back to an earlier known good release initially, then use Stackdriver Trace and Logging to diagnose the problem in a development/test/staging environment**

---

# 4. Google Cloud Case Study — Expanding Persistent Disk with Minimal Downtime

## Question

A production database virtual machine on Google Compute Engine has an **ext4-formatted persistent disk** for data files. The database is about to **run out of storage space**.  

How can you remediate the problem with the **least amount of downtime**?  

A. In the Cloud Platform Console, increase the size of the persistent disk and use the `resize2fs` command in Linux.  
B. Shut down the virtual machine, use the Cloud Platform Console to increase the persistent disk size, then restart the virtual machine.  
C. In the Cloud Platform Console, increase the size of the persistent disk and verify the new space is ready to use with the `fdisk` command in Linux.  
D. In the Cloud Platform Console, create a new persistent disk attached to the virtual machine, format and mount it, and configure the database service to move the files to the new disk.  
E. In the Cloud Platform Console, create a snapshot of the persistent disk, restore the snapshot to a new larger disk, unmount the old disk, mount the new disk, and restart the database service.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### **A. Increase the disk size and use `resize2fs`**
- Google Cloud **persistent disks can be resized online** while attached to a running VM.
- For **ext4 file systems**, you can:
  1. Increase the disk size in the **Cloud Console**
  2. Run `resize2fs` on the mounted filesystem
- This approach:
  - Requires **no VM shutdown**
  - Results in **minimal or no downtime**
  - Is the **recommended and fastest remediation** for running out of disk space

---

## ❌ Why the other options are not correct

### **B. Shut down the VM and resize the disk**
- Works technically, but requires **full VM downtime**
- Not the least disruptive option

### **C. Use `fdisk` to verify space**
- `fdisk` is for partition management, not filesystem resizing
- Does not actually make the space usable by the filesystem

### **D. Create and migrate to a new disk**
- Requires **data migration**, configuration changes, and service interruption
- Significantly more downtime and operational risk

### **E. Snapshot and restore to a new disk**
- Snapshot creation and disk replacement introduce **extended downtime**
- Unnecessary when online disk resizing is supported

---

## ✅ Final Answer

**A. In the Cloud Platform Console, increase the size of the persistent disk and use the `resize2fs` command in Linux.**

---

# 5. Google Cloud Case Study — Designing for Minimal PCI Compliance Scope

## Question

Your application needs to **process credit card transactions**. You want the **smallest scope of Payment Card Industry (PCI) compliance** without compromising the ability to **analyze transactional data and trends** relating to which payment methods are used.  

How should you design your architecture?  

A. Create a tokenizer service and store only tokenized data  
B. Create separate projects that only process credit card data  
C. Create separate subnetworks and isolate the components that process credit card data  
D. Streamline the audit discovery phase by labeling all of the virtual machines (VMs) that process PCI data  
E. Enable Logging export to Google BigQuery and use ACLs and views to scope the data shared with the auditor  

---

## ✅ **Correct Answer: A**

---

## Explanation

### **A. Create a tokenizer service and store only tokenized data**
- **Tokenization** replaces sensitive credit card data (PAN) with non-sensitive tokens.
- Only the **tokenization service** handles real credit card numbers, which:
  - **Minimizes the PCI compliance scope**
  - Limits audits to a **very small, isolated component**
- Downstream systems store and analyze **tokens instead of real card data**, allowing:
  - Trend analysis
  - Reporting on payment methods
  - Analytics without exposing PCI-sensitive information
- This is a **PCI DSS best practice** and widely used in payment architectures.

---

## ❌ Why the other options are not correct

### **B. Create separate projects that only process credit card data**
- Helps with isolation, but **credit card data still exists** in those projects.
- PCI scope remains **large**, increasing audit and compliance burden.

### **C. Create separate subnetworks and isolate the components**
- Network isolation improves security, but **does not remove PCI data exposure**.
- Compliance scope is still broad.

### **D. Label VMs that process PCI data**
- Labels help with organization and auditing, but **do not reduce PCI scope**.
- This is an administrative aid, not a security control.

### **E. Export logs to BigQuery with scoped access**
- Useful for audits and reporting, but **logs may still contain sensitive data**.
- Does not prevent PCI data from being processed or stored elsewhere.

---

## ✅ Final Answer

**A. Create a tokenizer service and store only tokenized data**

---

# 6. Google Cloud Case Study — Selecting Storage for High-Volume Clickstream Data

## Question

You have been asked to select the **storage system for the click-data** of your company's large portfolio of websites.  
This data is streamed in from a custom website analytics package at a typical rate of **6,000 clicks per minute**, with **bursts of up to 8,500 clicks per second**.  
The data must be **stored for future analysis** by your data science and user experience teams.

Which storage infrastructure should you choose?  

A. Google Cloud SQL  
B. Google Cloud Bigtable  
C. Google Cloud Storage  
D. Google Cloud Datastore  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Google Cloud Bigtable**
- **Cloud Bigtable** is a **highly scalable NoSQL wide-column database** designed for:
  - **High-throughput writes** (thousands to millions of writes per second)
  - **Low-latency ingestion** of streaming data
  - Massive datasets used for **analytics and time-series data**
- It is commonly used for:
  - Clickstream analytics
  - User behavior tracking
  - Event and time-series data
- Bigtable integrates well with:
  - **Dataflow** for stream processing
  - **BigQuery** for analytical querying
- This makes it an ideal backend for **bursty, high-velocity click data**.

---

## ❌ Why the other options are not correct

### **A. Google Cloud SQL**
- Designed for **transactional workloads** (OLTP).
- Does not scale efficiently for **high write throughput and bursty traffic**.

### **C. Google Cloud Storage**
- Optimized for **object storage** (files, logs, backups).
- Not suitable for **real-time ingestion or low-latency analytics queries**.

### **D. Google Cloud Datastore**
- Suitable for application data and key-value workloads.
- Not optimized for **very high write rates and analytical scan workloads** like clickstream data.

---

## ✅ Final Answer

**B. Google Cloud Bigtable**

---

# 7. Google Cloud Case Study — Automating Backup Retention in Cloud Storage

## Question

You are creating a solution to **remove backup files older than 90 days** from your **Cloud Storage bucket**. You want to **optimize ongoing Cloud Storage spend**.  

What should you do?  

A. Write a lifecycle management rule in XML and push it to the bucket with `gsutil`  
B. Write a lifecycle management rule in JSON and push it to the bucket with `gsutil`  
C. Schedule a cron script using `gsutil ls -lr gs://backups/**` to find and remove items older than 90 days  
D. Schedule a cron script using `gsutil ls -l gs://backups/**` to find and remove items older than 90 days and schedule it with cron  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Write a lifecycle management rule in JSON and push it to the bucket with `gsutil`**
- **Cloud Storage Lifecycle Management** is the **recommended and most cost-effective** way to manage object retention.
- Lifecycle rules:
  - Automatically delete objects **older than a specified age**
  - Run **server-side**, requiring no VM, cron job, or custom scripts
  - Reduce operational complexity and failure risk
- **JSON** is the **current and preferred format** for Cloud Storage lifecycle configuration.
- Example use cases include:
  - Deleting old backups
  - Transitioning objects to colder storage classes
  - Cost optimization

---

## ❌ Why the other options are not correct

### **A. Write a lifecycle management rule in XML**
- XML is **deprecated** for Cloud Storage lifecycle management.
- JSON is the supported and recommended format.

### **C. Schedule a cron script using `gsutil ls -lr`**
- Requires:
  - A VM or server to run the cron job
  - Ongoing maintenance and monitoring
- Less reliable and more costly than native lifecycle rules.

### **D. Schedule a cron script using `gsutil ls -l`**
- Same drawbacks as option C.
- Manual scripts increase **operational overhead** and risk of errors.

---

## ✅ Final Answer

**B. Write a lifecycle management rule in JSON and push it to the bucket with `gsutil`**

---

# 8. Google Cloud Case Study — Scaling Spark and Hadoop Workloads

## Question

Your company is forecasting a **sharp increase in the number and size of Apache Spark and Hadoop jobs** being run on your local datacenter. You want to **utilize the cloud to help you scale** this upcoming demand with the **least amount of operations work and code change**.  

Which product should you use?  

A. Google Cloud Dataflow  
B. Google Cloud Dataproc  
C. Google Compute Engine  
D. Google Kubernetes Engine  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Google Cloud Dataproc**
- **Cloud Dataproc** is a **fully managed Spark and Hadoop service**.
- It is designed specifically for:
  - Running **existing Spark and Hadoop jobs**
  - Minimal or **no code changes**
  - Rapid cluster provisioning (in minutes)
- Operational benefits:
  - Google manages **cluster setup, configuration, and scaling**
  - You can create **ephemeral clusters** for jobs and shut them down after use to save costs
- This makes Dataproc ideal for **burst workloads** and hybrid scaling from on-premises environments.

---

## ❌ Why the other options are not correct

### **A. Google Cloud Dataflow**
- Dataflow uses **Apache Beam**, not native Spark or Hadoop.
- Requires **rewriting jobs**, which increases effort.

### **C. Google Compute Engine**
- Running Spark/Hadoop directly on VMs requires:
  - Manual cluster management
  - More operational overhead
- Not the least-ops option.

### **D. Google Kubernetes Engine**
- Kubernetes is powerful but requires:
  - Containerization
  - Additional configuration and management
- Not suitable for minimal code change Spark/Hadoop migrations.

---

## ✅ Final Answer

**B. Google Cloud Dataproc**

---

# 9. Google Cloud Case Study — Improving Database Performance on Compute Engine

## Question

The database administration team has asked you to help them **improve the performance** of their new database server running on **Google Compute Engine**.  
The database is used for **importing and normalizing performance statistics** and is built with **MySQL running on Debian Linux**.  

They currently have:
- **n1-standard-8 VM**
- **80 GB SSD persistent disk**

What should they change to get **better performance** from this system?

A. Increase the virtual machine's memory to 64 GB  
B. Create a new virtual machine running PostgreSQL  
C. Dynamically resize the SSD persistent disk to 500 GB  
D. Migrate their performance metrics warehouse to BigQuery  
E. Modify all of their batch jobs to use bulk inserts into the database  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Dynamically resize the SSD persistent disk to 500 GB**
- In Google Cloud, **persistent disk performance (IOPS and throughput) scales with disk size**.
- By increasing the SSD persistent disk from **80 GB to 500 GB**, you automatically gain:
  - Higher **IOPS**
  - Higher **disk throughput**
- This change:
  - Requires **no application changes**
  - Can be done **online**
  - Is **cost-effective** compared to increasing VM size
- Since the workload involves **importing and normalizing data**, disk I/O is likely the main bottleneck.

---

## ❌ Why the other options are not correct

### **A. Increase the virtual machine's memory to 64 GB**
- Increasing memory may help caching, but **disk I/O remains the bottleneck**.
- Much more expensive than increasing disk size.

### **B. Create a new virtual machine running PostgreSQL**
- Requires **database migration**, testing, and downtime.
- No guarantee of performance improvement for this workload.

### **D. Migrate their performance metrics warehouse to BigQuery**
- BigQuery is excellent for analytics, but this represents a **major architectural change**.
- Not an immediate or minimal-change performance fix.

### **E. Modify all batch jobs to use bulk inserts**
- Bulk inserts improve performance, but require **code changes and testing**.
- Does not address underlying **disk throughput limitations**.

---

## ✅ Final Answer

**C. Dynamically resize the SSD persistent disk to 500 GB**

---

# 10. Google Cloud Case Study — Storing High-Velocity Time-Series Sensor Data

## Question

You want to optimize the performance of an **accurate, real-time weather-charting application**.  
The data comes from **50,000 sensors**, each sending **10 readings per second**, in the format of a **timestamp and sensor reading**.  

Where should you store the data?  

A. Google BigQuery  
B. Google Cloud SQL  
C. Google Cloud Bigtable  
D. Google Cloud Storage  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Google Cloud Bigtable**
- **Cloud Bigtable** is designed for **massive-scale, low-latency, high-throughput** workloads.
- It is ideal for:
  - **Time-series data**
  - **Real-time analytics**
  - **Very high write rates** (50,000 × 10 = **500,000 writes per second**)
- Bigtable provides:
  - Horizontal scalability
  - Millisecond-level read/write latency
  - Seamless integration with **Dataflow** and **BigQuery** for analytics
- This makes it the **best choice** for real-time weather sensor data ingestion and querying.

---

## ❌ Why the other options are not correct

### **A. Google BigQuery**
- Optimized for **batch analytics**, not real-time ingestion at very high velocity.
- Higher latency for real-time charting use cases.

### **B. Google Cloud SQL**
- Relational databases are not designed to handle **hundreds of thousands of writes per second**.
- Scaling would be complex and expensive.

### **D. Google Cloud Storage**
- Designed for **object storage**, not real-time reads/writes or time-series queries.
- Poor fit for low-latency charting applications.

---

## ✅ Final Answer

**C. Google Cloud Bigtable**
