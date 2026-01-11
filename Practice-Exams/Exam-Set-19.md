# 1. Processing External Data Without Storing PII

## Question

You are working with a **data warehousing team** that performs data analysis.  
The team needs to process data from **external partners**, but the data contains **personally identifiable information (PII)**.

You need to **process and store the data without storing any of the PII data**.  
What should you do?

A. Create a **Dataflow pipeline** to retrieve the data from the external sources. As part of the pipeline, use the **Cloud Data Loss Prevention (Cloud DLP) API** to remove any PII data. Store the result in **BigQuery**.  

B. Create a **Dataflow pipeline** to retrieve the data from the external sources. As part of the pipeline, store all **non-PII data in BigQuery** and store all **PII data in a Cloud Storage bucket** that has a retention policy set.  

C. Ask the external partners to upload all data on **Cloud Storage**. Configure **Bucket Lock** for the bucket. Create a **Dataflow pipeline** to read the data from the bucket. As part of the pipeline, use the **Cloud Data Loss Prevention (Cloud DLP) API** to remove any PII data. Store the result in **BigQuery**.  

D. Ask the external partners to import all data in your **BigQuery dataset**. Create a **Dataflow pipeline** to copy the data into a new table. As part of the Dataflow bucket, skip all data in columns that have PII data.  

---

## ✅ Correct Answer

**A**

---

## Explanation

### Why **A** is correct 🛡️

The requirement is very strict:

> **You must not store any PII data**

Using **Dataflow + Cloud DLP** allows you to:
- Inspect data **in transit**
- Detect and remove or mask **PII**
- Store **only sanitized data** in **BigQuery**

This ensures:
- No PII is ever written to storage
- Data is compliant before it is persisted
- Analytics can run safely on BigQuery

This is the **Google-recommended pattern** for handling sensitive data in data pipelines.

---

## ❌ Why the other options are wrong

### **B — Stores PII**
- PII is still stored in **Cloud Storage**
- Violates the requirement: *“without storing any of the PII data”*

---

### **C — PII is stored first**
- Raw data containing PII is stored in **Cloud Storage**
- Even if later removed, PII was already persisted → **compliance violation**

---

### **D — No reliable PII detection**
- Skipping columns assumes:
  - PII is perfectly labeled
  - PII never appears in free-text fields
- This is unsafe and unreliable

---

## ✅ Final Answer

**A**

---

# 2. Centralizing Logs for Production Projects

## Question

You want to allow your **operations team** to store logs from **all the production projects** in your **Organization**, **without including logs from other projects**.  

All of the **production projects are contained in a folder**.  
You want to ensure that **all logs for existing and new production projects are captured automatically**.  

What should you do?

A. Create an **aggregated export on the Production folder**. Set the log sink to be a **Cloud Storage bucket in an operations project**.  

B. Create an **aggregated export on the Organization resource**. Set the log sink to be a **Cloud Storage bucket in an operations project**.  

C. Create **log exports in the production projects**. Set the log sinks to be a **Cloud Storage bucket in an operations project**.  

D. Create **log exports in the production projects**. Set the log sinks to be **BigQuery datasets in the production projects**, and grant IAM access to the operations team to run queries on the datasets.  

---

## ✅ Correct Answer

**A**

---

## Explanation

### Why **A** is correct 🗂️

Google Cloud Logging supports **aggregated sinks** at:
- Organization level
- Folder level
- Project level  

Because:
- All production projects are already inside a **Production folder**
- You want **only production logs**
- You want **new production projects to be included automatically**

The best solution is to:
- Create a **log sink at the folder level**
- Point it to a **central Cloud Storage bucket**

This provides:
- Automatic inclusion of new projects
- Clean separation between production and non-production logs
- Centralized storage for operations

---

## ❌ Why the other options are wrong

### **B — Organization-level sink**
- Includes **all projects**, not just production
- Violates the requirement to exclude non-production logs

---

### **C — Per-project exports**
- Requires manual setup for every new project
- Does not scale
- Not automatically applied to future projects

---

### **D — Logs stored inside production projects**
- Operations team must access each project
- No centralized control
- Harder to manage and audit

---

## ✅ Final Answer

**A**

---

# 3. Long-Term Log Retention with Cost Optimization

## Question

Your company has an application that is running on **multiple instances of Compute Engine**. It generates **1 TB per day of logs**.  

**Requirements:**
- Logs must be kept for **at least two years** (compliance)
- Logs must be **actively queryable for 30 days**
- After 30 days, logs are only retained for **audit purposes**
- Solution must **minimize costs** and follow **Google-recommended practices**

What should you do?

A. 
1. Install a **Cloud Logging agent** on all instances  
2. Create a **sink to export logs into a regional Cloud Storage bucket**  
3. Create an **Object Lifecycle rule** to move files into a **Coldline Cloud Storage bucket after one month**  
4. Configure a **retention policy at the bucket level using bucket lock**  

B. 
1. Write a **daily cron job**, running on all instances, that uploads logs into a Cloud Storage bucket  
2. Create a sink to export logs into a regional Cloud Storage bucket  
3. Create an Object Lifecycle rule to move files into a Coldline Cloud Storage bucket after one month  

C. 
1. Install a **Cloud Logging agent** on all instances  
2. Create a **sink to export logs into a partitioned BigQuery table**  
3. Set a **time_partitioning_expiration of 30 days**  

D. 
1. Create a **daily cron job**, running on all instances, that uploads logs into a partitioned BigQuery table  
2. Set a **time_partitioning_expiration of 30 days**  

---

## ✅ Correct Answer

**A**

---

## Explanation

### Why **A** is correct ☁️

- **Cloud Logging agent**: Automatically collects logs from Compute Engine instances.
- **Export to Cloud Storage**: Cost-effective for large volumes of logs (1 TB/day), cheaper than BigQuery for long-term storage.
- **Object Lifecycle Rule**: Moves logs to **Coldline** (or Archive) storage after 30 days to **minimize costs**, since audit-only logs are rarely accessed.
- **Bucket Lock / Retention Policy**: Ensures compliance, prevents deletion before the required retention period.

This approach **balances query performance and cost**, following Google best practices:
- Active query period: Logs remain in **standard storage** for 30 days
- Long-term retention: Moved to **low-cost archival storage**
- Automated and compliant solution

---

## ❌ Why the other options are wrong

### **B — Cron job to upload logs**
- Manual process adds operational overhead
- Not Google-recommended; Cloud Logging agents and sinks are the preferred method

---

### **C — BigQuery sink with 30-day partition expiration**
- After 30 days, logs are deleted from BigQuery  
- Violates **compliance requirement** to retain logs for two years

---

### **D — Cron job to BigQuery**
- Manual process, operationally heavy  
- Same compliance issue as C; logs deleted after 30 days  

---

## ✅ Final Answer

**A**

---

# 4. Restricting IAM Access to Cloud Identity Domain

## Question

Your company has just recently activated **Cloud Identity** to manage users. The **Google Cloud Organization** has been configured as well.  

The **security team needs to secure projects** that will be part of the Organization. They want to **prohibit IAM users outside the domain from gaining permissions from now on**.  

What should they do?

A. Configure an **organization policy to restrict identities by domain**.  

B. Configure an organization policy to **block creation of service accounts**.  

C. Configure **Cloud Scheduler** to trigger a Cloud Function every hour that removes all users that don't belong to the Cloud Identity domain from all projects.  

D. Create a **technical user** (e.g., `crawler@yourdomain.com`), and give it the **project owner role** at the root organization level. Write a **bash script** that:  
- Lists all the IAM rules of all projects within the organization  
- Deletes all users that do not belong to the company domain  
Create a Compute Engine instance in a project within the Organization and configure `gcloud` to be executed with technical user credentials. Configure a **cron job** that executes the bash script every hour.  

---

## ✅ Correct Answer

**A. Configure an organization policy to restrict identities by domain**

---

## Explanation

### Why **Organization Policy Restricting Identities** 🔐

- **Org policies** allow you to enforce restrictions across all projects within a Google Cloud Organization.
- The **"Domain Restricted Sharing" policy** (`constraints/iam.allowedPolicyMemberDomains`) ensures that **IAM permissions can only be granted to members of your Cloud Identity domain**.
- This is a **proactive, declarative, and managed solution**:
  - Prevents external users from being added
  - Automatically applies to **existing and new projects**
  - Follows Google-recommended best practices
  - Reduces operational overhead

---

## ❌ Why the other options are not correct

### **B. Block creation of service accounts**
- Restricting service accounts does **not prevent IAM users outside the domain** from gaining access
- Does not meet the requirement

---

### **C. Cloud Function triggered hourly**
- Reactive approach: only removes external users after they are added
- Introduces **delays, complexity, and operational overhead**
- Less secure than using an org policy

---

### **D. Technical user with cron job**
- Highly manual and error-prone
- Not scalable for multiple projects or frequent IAM changes
- Introduces unnecessary complexity and security risk

---

## ✅ Final Answer

**A**

---

# 5. Resolving Cloud Bigtable Hotspotting

## Question

Your company has an application running on **Google Cloud** that is collecting data from **thousands of physical devices** that are globally distributed.  

Data is published to **Pub/Sub** and streamed in real time into an **SSD Cloud Bigtable cluster** via a **Dataflow pipeline**.  

The operations team informs you that your **Cloud Bigtable cluster has a hotspot**, and **queries are taking longer than expected**.  

You need to **resolve the problem and prevent it from happening in the future**.  

What should you do?

A. Advise your clients to use **HBase APIs** instead of NodeJS APIs.  

B. **Delete records older than 30 days**.  

C. **Review your RowKey strategy** and ensure that keys are evenly spread across the alphabet.  

D. **Double the number of nodes** you currently have.  

---

## ✅ Correct Answer

**C. Review your RowKey strategy and ensure that keys are evenly spread across the alphabet**

---

## Explanation

### Why **RowKey Design** Matters 🔑

- **Cloud Bigtable stores rows lexicographically by row key**.
- If your row keys are **sequential or clustered**, certain nodes will receive **disproportionate traffic**, causing a **hotspot**.
- Proper row key design ensures:
  - **Even distribution** of read and write requests across all nodes
  - Improved **query performance**
  - Prevention of **future hotspots**

**Best practices for row keys:**

- Add a **hashed prefix** or **salting** to sequential IDs
- Avoid monotonically increasing keys (timestamps, incremental IDs) as the first part of the key
- Use a composite key strategy: `[shard-prefix]#[device-id]#[timestamp]`

---

## ❌ Why the other options are not correct

### **A. HBase APIs instead of NodeJS APIs**
- Changing the client API **does not fix hotspotting**
- Hotspotting is due to **row key distribution**, not the programming interface

---

### **B. Delete records older than 30 days**
- Reducing data may temporarily reduce disk usage, but **does not address the root cause of uneven traffic distribution**
- Hotspots will persist with poorly designed row keys

---

### **D. Double the number of nodes**
- Scaling nodes can **temporarily reduce latency**, but hotspots **will continue** because traffic is still uneven
- Does not solve the **underlying row key issue**

---

## ✅ Final Answer

**C**

---

# 7. Migrating 10 TB of Data to Cloud Storage

## Question

Your operations team currently stores **10 TB of data** in an **object storage service from a third-party provider**.  

They want to move this data to a **Cloud Storage bucket** **as quickly as possible**, following **Google-recommended practices**, and they want to **minimize the cost** of the data migration.  

Which approach should they use?

A. Use the **gsutil mv** command to move the data.  

B. Use the **Storage Transfer Service** to move the data.  

C. Download the data to a **Transfer Appliance**, and ship it to Google.  

D. Download the data to the **on-premises data center**, and upload it to the Cloud Storage bucket.  

---

## ✅ Correct Answer

**B. Use the Storage Transfer Service to move the data.**

---

## Explanation

**Storage Transfer Service (STS)** is Google’s **recommended solution** for transferring data from **third-party object storage providers** (such as AWS S3, Azure Blob, or other S3-compatible services) to **Cloud Storage**.

It is ideal because:

- It performs **direct cloud-to-cloud transfers** — no local staging or servers needed  
- It is **high-performance and parallelized**, making it **fast**  
- It supports **large-scale data transfers** (10 TB is trivial for STS)  
- It is **cost-efficient**, avoiding egress, local storage, and compute costs  
- It is **managed and reliable**, with retry and verification built in  

---

### Why the other options are wrong

**A. gsutil mv** ❌  
- Requires a VM or local system to act as a middleman  
- Data must be **downloaded and re-uploaded**, increasing time and cost  

**C. Transfer Appliance** ❌  
- Designed for **very large datasets (hundreds of TB to PB)**  
- For only **10 TB**, shipping hardware is **slow and expensive**

**D. Download to on-prem then upload** ❌  
- **Slowest and most expensive** option  
- Requires extra bandwidth, storage, and operational work  

---

## ✅ Final Answer

**B**

---

# 8. Ensuring Reliable Cleanup When Compute Engine Instances Shut Down

## Question

You have a **Compute Engine managed instance group** that adds and removes instances based on application load.  
Each instance has a **shutdown script** that removes **REDIS database entries** associated with the instance.

However, you notice that **many database entries are not being removed**, and you suspect the shutdown script is unreliable.

You create a **Cloud Function** to remove the database entries.  
You need to ensure the cleanup runs **reliably every time** an instance is shut down.

What should you do next?

A. Modify the shutdown script to wait for 30 seconds before triggering the Cloud Function.  

B. Do not use the Cloud Function. Modify the shutdown script to restart if it has not completed in 30 seconds.  

C. Set up a Cloud Monitoring sink that triggers the Cloud Function after an instance removal log message arrives in Cloud Logging.  

D. Modify the shutdown script to wait for 30 seconds and then publish a message to a Pub/Sub queue.  

---

## ✅ Correct Answer

**C. Set up a Cloud Monitoring sink that triggers the Cloud Function after an instance removal log message arrives in Cloud Logging.**

---

## Explanation

The key issue is that **shutdown scripts are not guaranteed to finish** when instances in a **Managed Instance Group (MIG)** are deleted.  
Instances can be **terminated quickly** during autoscaling or updates, so cleanup logic inside the VM is **not reliable**.

The correct pattern is to make the cleanup **event-driven and external to the VM**.

By using:

**Compute Engine → Cloud Logging → Log-based sink → Cloud Function**

you ensure:

- The cleanup is triggered **after Google Cloud confirms the VM was deleted**
- It is **not affected** by how quickly the VM shuts down
- It is **reliable, scalable, and serverless**
- It follows **Google-recommended practices**

---

### Why the other options are wrong

**A. Wait 30 seconds before triggering the Cloud Function** ❌  
- The VM may be killed **before** the 30 seconds expires  
- Still unreliable  

**B. Restart the shutdown script** ❌  
- Shutdown scripts **do not restart** after the VM is terminated  
- MIG deletions are abrupt  

**D. Publish to Pub/Sub from the shutdown script** ❌  
- Same problem: the VM may be **terminated before publishing succeeds**

---

## ✅ Final Answer

**C**

---

# 9. Using gcloud Across Multiple Workstations

## Question

You are managing several projects on Google Cloud and need to interact on a daily basis with **BigQuery, Bigtable, and Kubernetes Engine** using the **gcloud CLI tool**.  
You travel a lot and work on different workstations during the week.  
You want to avoid having to manage the **gcloud CLI manually**.

What should you do?

A. Use Google Cloud Shell in the Google Cloud Console to interact with Google Cloud.  

B. Create a Compute Engine instance and install gcloud on the instance. Connect to this instance via SSH to always use the same gcloud installation when interacting with Google Cloud.  

C. Install gcloud on all of your workstations. Run the command `gcloud components auto-update` on each workstation.  

D. Use a package manager to install gcloud on your workstations instead of installing it manually.  

---

## ✅ Correct Answer

**A. Use Google Cloud Shell in the Google Cloud Console to interact with Google Cloud.**

---

## Explanation

**Google Cloud Shell** is a **browser-based, preconfigured environment** that includes:

- `gcloud`
- `kubectl`
- `bq`
- Git
- Common developer tools

Because Cloud Shell runs in Google Cloud:

- You get the **same environment everywhere**
- No local installation is needed
- It is always **up-to-date**
- It is already **authenticated** to your Google account
- It works from **any device** with a browser

This perfectly fits a user who:
- Travels frequently  
- Uses multiple machines  
- Wants **zero CLI management**

---

### Why the other options are wrong

**B. Compute Engine VM with gcloud installed** ❌  
- You must still **manage and update** the VM  
- More cost and operational overhead  

**C. Install gcloud everywhere with auto-update** ❌  
- Still requires installing, configuring, and maintaining gcloud on each machine  

**D. Use a package manager** ❌  
- Easier than manual installs, but still **local maintenance on every device**

---

## ✅ Final Answer

**A**

---

# 10. Connecting Two Organizations with Overlapping IP Ranges

## Question

Your company recently acquired a company that has infrastructure in Google Cloud.  
Each company has its own **Google Cloud organization**.  
Each company is using a **Shared Virtual Private Cloud (VPC)** to provide network connectivity for its applications.  

Some of the **subnets used by both companies overlap**.  
In order for both businesses to integrate, the applications need to have **private network connectivity**.  
These applications **are not on overlapping subnets**.  
You want to provide connectivity with **minimal re-engineering**.

What should you do?

A. Set up VPC peering and peer each Shared VPC together.  

B. Migrate the projects from the acquired company into your company's Google Cloud organization. Re-launch the instances in your companies Shared VPC.  

C. Set up a Cloud VPN gateway in each Shared VPC and peer Cloud VPNs.  

D. Configure SSH port forwarding on each application to provide connectivity between applications in the different Shared VPCs.  

---

## ✅ Correct Answer

**C. Set up a Cloud VPN gateway in each Shared VPC and peer Cloud VPNs.**

---

## Explanation

The key constraint is:

> **Some subnets overlap**

This immediately disqualifies **VPC Peering** because:
- VPC peering **does not support overlapping IP ranges**
- Even if only some subnets overlap, peering will fail

### Why Cloud VPN works

**Cloud VPN** creates an **encrypted tunnel** between VPCs and:
- Works **even if IP ranges overlap**
- Allows **selective routing**
- Requires **no re-IPing**
- Works **across organizations**
- Is the **lowest re-engineering option**

Because the **applications that must communicate are on non-overlapping subnets**, VPN routing can be configured to allow only those ranges to communicate.

---

### Why the other options are wrong

**A. VPC Peering** ❌  
- Fails because **overlapping IP ranges are not allowed**

**B. Migrate projects and re-launch instances** ❌  
- Requires **major re-engineering**
- Breaks the requirement of minimal change

**D. SSH port forwarding** ❌  
- Not scalable
- Not secure
- Not suitable for production network connectivity

---

## ✅ Final Answer

**C**
