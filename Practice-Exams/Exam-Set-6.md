# 1. Pre-emptible Compute Engine

## Question

You have created several **preemptible Linux virtual machine instances** using Google Compute Engine. You want to properly shut down your application before the virtual machines are preempted.  
What should you do?

A. Create a shutdown script named `k99.shutdown` in the `/etc/rc.6.d/` directory  
B. Create a shutdown script registered as a xinetd service in Linux and configure a Stackdriver endpoint check to call the service  
C. Create a shutdown script and use it as the value for a new metadata entry with the key `shutdown-script` in the Cloud Platform Console when you create the new virtual machine instance  
D. Create a shutdown script, registered as a xinetd service in Linux, and use the `gcloud compute instances add-metadata` command to specify the service URL as the value for a new metadata entry with the key `shutdown-script-url`  

---

## ✅ Correct Answer

**C. Create a shutdown script and use it as the value for a new metadata entry with the key `shutdown-script` in the Cloud Platform Console when you create the new virtual machine instance**

---

## Explanation

- **Preemptible VMs** can be terminated at any time by Google Cloud.
- Google Compute Engine provides a built-in mechanism to handle this gracefully using **shutdown scripts**.
- A script defined in instance metadata with the key **`shutdown-script`** is:
  - Automatically executed when the VM is shutting down (including preemption)
  - The **recommended and supported best practice**
  - Simple to configure and reliable

This allows your application to:
- Flush logs
- Close network connections
- Save state
- Gracefully terminate processes

---

## ❌ Why the other options are not correct

- **A. `/etc/rc.6.d/` scripts**
  - Not reliable for preemptible VM termination
  - Not the recommended Google Cloud mechanism

- **B. Stackdriver endpoint checks**
  - Endpoint checks are for monitoring, not shutdown handling
  - Adds unnecessary complexity and latency

- **D. `shutdown-script-url`**
  - Valid feature, but overengineered for this use case
  - `shutdown-script` inline metadata is simpler and preferred unless you need centralized script hosting

---

## ✅ Final Answer

**C**

---

# 2. Google Cloud Networking Design — 3-Tier Application Security

## Question

Your organization has a **3-tier web application** deployed in the same network on Google Cloud Platform. Each tier (**web, API, and database**) scales independently of the others.

- Network traffic **should flow**: web → API → database  
- Network traffic **should NOT flow**: web → database  

How should you configure the network?

A. Add each tier to a different subnetwork  
B. Set up software-based firewalls on individual VMs  
C. Add tags to each tier and set up routes to allow the desired traffic flow  
D. Add tags to each tier and set up firewall rules to allow the desired traffic flow  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Add tags to each tier and set up firewall rules to allow the desired traffic flow**

- **VPC firewall rules** are the **recommended and native mechanism** in Google Cloud for controlling network traffic.
- By assigning **network tags** to instances in each tier (e.g., `web-tier`, `api-tier`, `db-tier`), you can:
  - Allow traffic from **web → API**
  - Allow traffic from **API → database**
  - Explicitly **deny traffic from web → database**
- Firewall rules automatically apply as instances scale up or down, making this approach **scalable, secure, and low maintenance**.
- This aligns with **Google best practices** for layered security and least-privilege network access.

---

## ❌ Why the other options are not correct

### **A. Add each tier to a different subnetwork**
- Subnetworks alone do **not enforce traffic restrictions**.
- Without firewall rules, all subnetworks in the same VPC can still communicate freely.

### **B. Set up software-based firewalls on individual VMs**
- Not scalable or manageable in autoscaling environments.
- Increases operational complexity and risk of misconfiguration.
- Google recommends **VPC firewall rules over host-based firewalls**.

### **C. Add tags to each tier and set up routes to allow the desired traffic flow**
- **Routes control where traffic goes**, not whether it is allowed.
- Routes cannot restrict communication between tiers based on security requirements.

---

## ✅ Final Answer

**D. Add tags to each tier and set up firewall rules to allow the desired traffic flow**

---

# 3. Google Cloud Operations & Troubleshooting — Kernel Module Failure on GCE

## Question

Your development team installed a **new Linux kernel module** on batch servers running on **Google Compute Engine (GCE) VMs** to speed up a nightly batch process. Two days later, **50% of the batch servers failed** during the nightly run.

You want to **collect detailed failure information** to provide back to the development team.

Which three actions should you take? (Choose three.)

A. Use Stackdriver Logging to search for the module log entries  
B. Read the debug GCE Activity log using the API or Cloud Console  
C. Use gcloud or Cloud Console to connect to the serial console and observe the logs  
D. Identify whether a live migration event of the failed server occurred, using the activity log  
E. Adjust the Google Stackdriver timeline to match the failure time, and observe the batch server metrics  
F. Export a debug VM into an image, and run the image on a local server where kernel log messages will be displayed on the native screen  

---

## ✅ **Correct Answers: A, C, and E**

---

## Explanation

### **A. Use Stackdriver Logging to search for the module log entries**
- Kernel modules typically write logs to **system logs** (e.g., `dmesg`, `/var/log/syslog`, `/var/log/kern.log`).
- **Cloud Logging (formerly Stackdriver Logging)** aggregates these logs centrally.
- This allows you to:
  - Identify kernel panics, warnings, or module load failures.
  - Compare logs between successful and failed batch servers.
- This is the **fastest and most scalable** way to analyze failures across many VMs.

---

### **C. Use gcloud or Cloud Console to connect to the serial console and observe the logs**
- The **serial console** captures **early boot and kernel-level messages**, even if the VM fails before networking is available.
- This is especially important for:
  - Kernel module crashes
  - Driver initialization failures
  - System hangs during boot or batch execution
- Serial console access is a **best practice** for diagnosing low-level OS and kernel issues on GCE.

---

### **E. Adjust the Google Stackdriver timeline to match the failure time, and observe the batch server metrics**
- Aligning metrics (CPU, memory, disk I/O, kernel errors) with the **exact failure window** helps identify:
  - Resource exhaustion caused by the kernel module
  - Abnormal spikes or drops correlated with failures
- Stackdriver Monitoring provides **time-series visibility** needed for root-cause analysis.

---

## ❌ Why the other options are not correct

### **B. Read the debug GCE Activity log using the API or Cloud Console**
- Activity logs focus on **infrastructure-level events** (e.g., VM creation, deletion, permission changes).
- They do **not provide kernel-level or application-level failure details**.

### **D. Identify whether a live migration event occurred using the activity log**
- Live migration is designed to be **transparent to workloads**.
- Kernel modules should not fail simply due to live migration.
- While useful in rare edge cases, this is **not a primary troubleshooting step** here.

### **F. Export a debug VM into an image and run it locally**
- This is **time-consuming, operationally heavy**, and not representative of GCE’s environment.
- Kernel behavior may differ outside Google Cloud’s hypervisor.
- Not a recommended or practical troubleshooting approach.

---

## ✅ Final Answers

**A. Use Stackdriver Logging to search for the module log entries**  
**C. Use gcloud or Cloud Console to connect to the serial console and observe the logs**  
**E. Adjust the Google Stackdriver timeline to match the failure time, and observe the batch server metrics**

---

# 4. Google Cloud Storage & Analytics — Low-Risk Cloud Adoption

## Question

Your company wants to **try out the cloud with low risk**. They want to:

- Archive approximately **100 TB of log data** in the cloud  
- **Test analytics features** available in Google Cloud  
- Retain the data as a **long-term disaster recovery (DR) backup**

Which two steps should you take? (Choose two.)

A. Load logs into Google BigQuery  
B. Load logs into Google Cloud SQL  
C. Import logs into Google Stackdriver  
D. Insert logs into Google Cloud Bigtable  
E. Upload log files into Google Cloud Storage  

---

## ✅ **Correct Answers: A and E**

---

## Explanation

### **E. Upload log files into Google Cloud Storage**
- **Cloud Storage** is ideal for:
  - Large-scale, cost-effective **data archiving** (100 TB+).
  - **Durable and highly available** long-term storage.
  - Supporting **disaster recovery** requirements.
- It offers multiple storage classes (Standard, Nearline, Coldline, Archive) to optimize cost over time.
- This satisfies the **low-risk entry** and **long-term backup** requirement.

---

### **A. Load logs into Google BigQuery**
- **BigQuery** enables:
  - **Serverless analytics** without managing infrastructure.
  - Fast, SQL-based analysis — ideal for teams wanting to experiment with analytics.
  - Direct querying of large datasets, including logs stored in Cloud Storage.
- This allows the company to **test analytics features** with minimal operational overhead.

---

## ❌ Why the other options are not correct

### **B. Load logs into Google Cloud SQL**
- Cloud SQL is designed for **transactional (OLTP)** workloads.
- Not cost-effective or scalable for **100 TB of log data**.

### **C. Import logs into Google Stackdriver**
- Cloud Logging (Stackdriver) is optimized for **operational logging**, not long-term archival at massive scale.
- Retention limits and cost make it unsuitable as a **primary archive or DR solution**.

### **D. Insert logs into Google Cloud Bigtable**
- Bigtable is optimized for **low-latency, high-throughput access**, not long-term archival.
- Requires schema design and operational overhead, making it **higher risk** for an initial cloud trial.

---

## ✅ Final Answers

**A. Load logs into Google BigQuery**  
**E. Upload log files into Google Cloud Storage**

---

# 5. Google Cloud IAM & Resource Hierarchy — Centralized Control with Departmental Autonomy

## Question

Your organization wants to **control IAM policies for different departments independently**, while still managing everything **centrally**.

Which approach should you take?

A. Multiple Organizations with multiple Folders  
B. Multiple Organizations, one for each department  
C. A single Organization with Folders for each department  
D. A single Organization with multiple projects, each with a central owner  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. A single Organization with Folders for each department**

- Google Cloud’s **resource hierarchy** is designed for centralized governance with delegated control:
  - **Organization** → **Folders** → **Projects**
- Using **one Organization** ensures:
  - Centralized visibility, billing, audit logging, and policy enforcement.
- Creating **Folders per department** allows:
  - Independent IAM policy management at the department level.
  - Policy inheritance from the Organization while still allowing customization.
  - Clear separation of responsibilities and access boundaries.
- This model aligns with **Google-recommended best practices** for large or multi-department enterprises.

---

## ❌ Why the other options are not correct

### **A. Multiple Organizations with multiple Folders**
- Organizations are meant to represent a **single company or legal entity**.
- Multiple Organizations increase complexity and fragment governance, billing, and auditing.

### **B. Multiple Organizations, one for each department**
- This breaks centralized control and makes:
  - Organization-wide policies
  - Central billing
  - Unified auditing  
  difficult to manage.
- Not a recommended design unless departments are **separate legal entities**.

### **D. A single Organization with multiple projects, each with a central owner**
- IAM at the **project level** lacks the structural grouping and inheritance benefits of Folders.
- Does not scale well for managing many projects or departments.
- Harder to enforce consistent policies across related projects.

---

## ✅ Final Answer

**C. A single Organization with Folders for each department**
