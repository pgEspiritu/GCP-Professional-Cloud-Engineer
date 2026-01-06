# 1. App Engine Versioning

## Question

Your company has announced that they will be **outsourcing operations functions**. You want to allow developers to **easily stage new versions** of a cloud-based application in the production environment and allow the outsourced operations team to **autonomously promote staged versions** to production. You want to **minimize the operational overhead** of the solution.  

A. App Engine  
B. GKE On-Prem  
C. Compute Engine  
D. Google Kubernetes Engine  

---

## ✅ Correct Answer

**A. App Engine**

---

## Explanation

- **App Engine** supports **native versioning** of applications.  
- Developers can deploy a **staged version** without affecting the current production traffic.  
- Operations teams can **split traffic** between versions or fully **promote staged versions** to production.  
- **Fully managed infrastructure** reduces operational overhead compared to managing VMs or Kubernetes clusters manually.  

This setup allows your company to:

- Stage new application versions safely
- Promote versions to production independently
- Reduce operational effort and complexity

---

## ❌ Why the other options are not correct

- **B. GKE On-Prem**
  - Requires **manual cluster management**
  - Operational overhead is high
  - Not ideal for minimizing operational complexity

- **C. Compute Engine**
  - VMs do not provide **native versioning or traffic splitting**
  - Promoting new versions requires **manual scripts and load balancer updates**

- **D. Google Kubernetes Engine (GKE)**
  - Supports **rolling updates and traffic management**, but still requires **cluster operations**
  - More complex than App Engine for version staging and promotion

---

## ✅ Final Answer

**A**

---

# 2. Cost Optimization for Non-Production Environments

## Question

Your company is running its application workloads on **Compute Engine**. The applications have been deployed in **production**, **acceptance**, and **development** environments.

- The **production** environment is business-critical and runs **24/7**
- The **acceptance** and **development** environments are only critical during **office hours**

Your CFO has asked you to **optimize costs during idle times**.

What should you do?

A. Create a shell script that uses the gcloud command to change the machine type of the development and acceptance instances to a smaller machine type outside of office hours. Schedule the shell script on one of the production instances to automate the task.  
B. Use Cloud Scheduler to trigger a Cloud Function that will stop the development and acceptance environments after office hours and start them just before office hours.  
C. Deploy the development and acceptance applications on a managed instance group and enable autoscaling.  
D. Use regular Compute Engine instances for the production environment, and use preemptible VMs for the acceptance and development environments.  

---

## ✅ Correct Answer

**B. Use Cloud Scheduler to trigger a Cloud Function that will stop the development and acceptance environments after office hours and start them just before office hours.**

---

## Explanation

- **Stopping VM instances** is the most effective way to reduce Compute Engine costs during idle periods.
- **Cloud Scheduler** provides reliable, cron-like scheduling.
- **Cloud Functions** can programmatically start and stop Compute Engine instances using the GCP APIs.
- This approach:
  - Eliminates compute costs outside office hours
  - Requires no manual intervention
  - Aligns with Google-recommended automation practices
  - Keeps production unaffected

---

## ❌ Why the other options are not correct

- **A. Resize machine types**
  - Still incurs compute costs
  - Adds operational risk by modifying production scripts
  - Not cost-optimal compared to stopping instances

- **C. Managed instance groups with autoscaling**
  - Autoscaling adjusts capacity, but does **not scale to zero**
  - Idle base costs still remain

- **D. Preemptible VMs**
  - Can be terminated at any time
  - Not suitable for predictable office-hour workloads
  - No guarantee of availability when needed

---

## ✅ Final Answer

**B**

---

# 3. Migrating MySQL to Cloud SQL with Minimal Downtime

## Question

You are moving an application that uses **MySQL** from on-premises to **Google Cloud**.  
The application will run on **Compute Engine** and use **Cloud SQL**.

You want to:
- Cut over with **minimal downtime**
- Ensure **no data loss**
- Migrate with **minimal application changes**
- Determine an appropriate **cutover strategy**

What should you do?

A.  
1. Set up Cloud VPN between Compute Engine and on-premises MySQL  
2. Stop the on-premises application  
3. Create a mysqldump  
4. Upload the dump to Cloud Storage  
5. Import the dump into Cloud SQL  
6. Modify the application to write to both databases and read locally  
7. Start the Compute Engine application  
8. Stop the on-premises application  

B.  
1. Set up Cloud SQL Proxy and MySQL proxy  
2. Create a mysqldump of the on-premises MySQL server  
3. Upload the dump to Cloud Storage  
4. Import the dump into Cloud SQL  
5. Stop the on-premises application  
6. Start the Compute Engine application  

C.  
1. Set up Cloud VPN between Compute Engine and the on-premises MySQL server  
2. Stop the on-premises application  
3. Start the Compute Engine application configured to read/write to the on-premises MySQL server  
4. Create a replication configuration in Cloud SQL  
5. Configure the source database to accept connections from Cloud SQL  
6. Finalize the Cloud SQL replica configuration  
7. When replication is complete, stop the Compute Engine application  
8. Promote the Cloud SQL replica to a standalone instance  
9. Restart the Compute Engine application, now pointing to Cloud SQL  

D.  
1. Stop the on-premises application  
2. Create a mysqldump of the on-premises MySQL server  
3. Upload the dump to Cloud Storage  
4. Import the dump into Cloud SQL  
5. Start the application on Compute Engine  

---

## ✅ Correct Answer

**C**

---

## Explanation

### Why **C** is correct

- This approach uses **Cloud SQL replication**, which is the **Google-recommended best practice** for:
  - **Minimal downtime**
  - **Zero or near-zero data loss**
- The application initially continues to use the **on-premises MySQL database**, avoiding code changes.
- **Replication keeps Cloud SQL in sync** with on-prem data until cutover.
- Final cutover only requires:
  - A brief application stop
  - Promotion of the Cloud SQL replica
  - Restart pointing to Cloud SQL

This achieves:
- ✅ Minimal downtime  
- ✅ No data loss  
- ✅ Minimal application modification  
- ✅ Clean and controlled cutover  

---

## ❌ Why the other options are not correct

### **A. Dual-write application logic**
❌ Incorrect  
- Requires **significant code changes**
- Dual-write patterns are complex and error-prone
- Not a recommended migration strategy

---

### **B. Dump and restore without replication**
❌ Incorrect  
- Causes **extended downtime**
- Any data written after the dump is lost
- Not suitable for production systems

---

### **D. Simple dump and restore**
❌ Incorrect  
- Requires full downtime during migration
- High risk of data loss
- Not acceptable for business-critical systems

---

## ✅ Final Answer

**C**

---

# 4. Firewall Insights – No Logs Displayed

## Question

Your company uses the **Firewall Insights** feature in the **Google Network Intelligence Center**.  
You have several firewall rules applied to **Compute Engine** instances.

You need to evaluate the efficiency of the applied firewall ruleset.  
When you open the **Firewall Insights** page in the Google Cloud Console, you notice that **no log rows are displayed**.

What should you do to troubleshoot the issue?

A. Enable Virtual Private Cloud (VPC) flow logging.  
B. Enable Firewall Rules Logging for the firewall rules you want to monitor.  
C. Verify that your user account is assigned the `compute.networkAdmin` IAM role.  
D. Install the Google Cloud SDK and verify that there are no firewall logs in the command-line output.  

---

## ✅ Correct Answer

**B. Enable Firewall Rules Logging for the firewall rules you want to monitor.**

---

## Explanation

### Why **B** is correct

- **Firewall Insights relies on firewall rule logging**
- If **firewall logging is disabled**, Firewall Insights has **no data to analyze**
- You must explicitly enable logging on each firewall rule you want to evaluate

Once logging is enabled:
- Firewall Insights can analyze:
  - Unused rules
  - Overly permissive rules
  - Rule hit counts
- Logs will begin appearing after traffic matches the rules

👉 This is a **common oversight** and the most frequent reason Firewall Insights shows no data.

---

## ❌ Why the other options are incorrect

### **A. Enable VPC flow logging**
❌ Incorrect  
- VPC Flow Logs capture **network traffic flows**, not firewall rule decisions
- Firewall Insights does **not** use VPC Flow Logs

---

### **C. Verify IAM role**
❌ Incorrect  
- Lack of permissions would prevent access to the page entirely
- It would not result in an empty dataset if logs exist

---

### **D. Check logs via Cloud SDK**
❌ Incorrect  
- Firewall Insights depends on **Logging configuration**, not CLI inspection
- This does not address the root cause

---

## ✅ Final Answer

**B**

----

# 5. Restrict Cloud Storage Access to Office Network

## Question

Your company has **sensitive data** stored in **Cloud Storage buckets**.  
Data analysts already have **IAM permissions** to read the buckets.

You want to **prevent data analysts from accessing bucket data from outside the office network**.

What should you do?

A.  
1. Create a **VPC Service Controls perimeter** that includes the projects with the buckets.  
2. Create an **access level** with the **CIDR of the office network**.

B.  
1. Create a **firewall rule** for all instances in the VPC network for a source range.  
2. Use the **CIDR of the office network**.

C.  
1. Create a **Cloud Function** to remove IAM permissions from the buckets, and another Cloud Function to add IAM permissions.  
2. Schedule the Cloud Functions with **Cloud Scheduler** to add permissions during business hours and remove them afterward.

D.  
1. Create a **Cloud VPN** to the office network.  
2. Configure **Private Google Access** for on-premises hosts.

---

## ✅ Correct Answer

**A**

---

## Explanation

### Why **A** is correct

- **VPC Service Controls** are designed specifically to **mitigate data exfiltration risks** from Google Cloud services like **Cloud Storage**
- By creating:
  - A **service perimeter** around the projects containing the buckets
  - An **access level** restricted to the **office network CIDR**
- You ensure that:
  - Even users with valid IAM permissions **cannot access data unless they are on the trusted network**
  - Access attempts from outside the office network are **blocked**

This approach:
- Works at the **service level**
- Requires **no application changes**
- Is the **recommended Google Cloud security best practice**

---

## ❌ Why the other options are incorrect

### **B. Firewall rules**
❌ Incorrect  
- Firewall rules only control traffic **to and from Compute Engine instances**
- They **do not apply to Cloud Storage**, which is a managed Google service

---

### **C. Scheduling IAM permission changes**
❌ Incorrect  
- Overly complex and fragile
- Does **not enforce network-based access**
- Increases operational risk and overhead

---

### **D. Cloud VPN + Private Google Access**
❌ Incorrect  
- Allows private connectivity but **does not restrict access**
- Users could still access Cloud Storage from outside the office using public endpoints
- Does not prevent data exfiltration

---

## ✅ Final Answer

**A**

---

# 6. Managed Instance Group Update Without Impacting Running Instances

## Question

You have developed a **non-critical update** to your application that is running in a **managed instance group (MIG)**.  
You have created a **new instance template** with the update that you want to release.

To prevent any possible impact to the application, you **do not want to update any running instances**.  
You want **only new instances** created by the managed instance group to contain the new update.

What should you do?

A. Start a new rolling restart operation.  
B. Start a new rolling replace operation.  
C. Start a new rolling update. Select the **Proactive** update mode.  
D. Start a new rolling update. Select the **Opportunistic** update mode.

---

## ✅ Correct Answer

**D. Start a new rolling update. Select the Opportunistic update mode.**

---

## Explanation

### Why **D** is correct

- **Opportunistic rolling updates**:
  - Apply the new instance template **only to new instances**
  - Do **not modify or restart existing running instances**
  - Are ideal when:
    - The update is non-critical
    - You want **zero impact** on currently running workloads
    - You want gradual adoption as instances are naturally recreated (autoscaling, repairs)

This perfectly matches the requirement:
> *“I don’t want to update any running instances, only future ones.”*

---

## ❌ Why the other options are incorrect

### **A. Rolling restart**
❌ Incorrect  
- Restarts **all existing instances**
- Causes unnecessary disruption

---

### **B. Rolling replace**
❌ Incorrect  
- Actively replaces existing instances
- Directly contradicts the requirement to avoid impacting running instances

---

### **C. Proactive rolling update**
❌ Incorrect  
- **Forces updates** to existing instances according to the update policy
- Designed for controlled rollouts, not zero-impact scenarios

---

## ✅ Final Answer

**D**

---

# 7. Zonal Outage Recovery with Latest Application Data

## Question

Your company is designing its application landscape on **Compute Engine**.  
Whenever a **zonal outage** occurs, the application should be restored in **another zone as quickly as possible with the latest application data**.

You need to design the solution to meet this requirement. What should you do?

A. Create a snapshot schedule for the disk containing the application data. Whenever a zonal outage occurs, use the latest snapshot to restore the disk in the same zone.  
B. Configure the Compute Engine instances with an instance template for the application, and use a **regional persistent disk** for the application data. Whenever a zonal outage occurs, use the instance template to spin up the application in **another zone in the same region**. Use the regional persistent disk for the application data.  
C. Create a snapshot schedule for the disk containing the application data. Whenever a zonal outage occurs, use the latest snapshot to restore the disk in another zone within the same region.  
D. Configure the Compute Engine instances with an instance template for the application, and use a regional persistent disk for the application data. Whenever a zonal outage occurs, use the instance template to spin up the application in **another region**. Use the regional persistent disk for the application data.

---

## ✅ Correct Answer

**B. Configure the Compute Engine instances with an instance template for the application, and use a regional persistent disk for the application data. Whenever a zonal outage occurs, use the instance template to spin up the application in another zone in the same region. Use the regional persistent disk for the application data.**

---

## Explanation

### Why **B** is correct

- **Regional Persistent Disks (PDs)**:
  - Synchronously replicate data across **two zones in the same region**
  - Ensure **minimal data loss (near-zero RPO)**
  - Are designed specifically for **zonal failure recovery**

- **Instance templates**:
  - Allow fast and consistent recreation of Compute Engine instances
  - Enable quick recovery by launching instances in another zone

This combination ensures:
- ✅ Fast recovery
- ✅ Latest application data
- ✅ Resilience against zonal outages

---

## ❌ Why the other options are not correct

### **A. Restore snapshot in the same zone**
❌ Incorrect  
- The zone is unavailable during a zonal outage
- Recovery in the same zone is not possible

---

### **C. Restore snapshot in another zone**
❌ Incorrect  
- Snapshots are **point-in-time backups**
- You may lose recent data (higher RPO)
- Restoration takes longer compared to regional disks

---

### **D. Restore in another region**
❌ Incorrect  
- **Regional persistent disks cannot span regions**
- Cross-region recovery adds unnecessary complexity and latency
- Overkill for a zonal outage scenario

---

## ✅ Final Answer

**B**

---

# 8. Resolving Overlapping IP Ranges Between VPC and On-Premises

## Question

Your company has just acquired another company, and you have been asked to integrate their existing **Google Cloud environment** into your company’s **data center**.

Upon investigation, you discover that some of the **RFC 1918 IP ranges** being used in the new company’s **VPC** overlap with your data center IP space.

What should you do to enable connectivity and make sure that there are **no routing conflicts** when connectivity is established?

A. Create a Cloud VPN connection from the new VPC to the data center, create a Cloud Router, and apply new IP addresses so there is no overlapping IP space.  
B. Create a Cloud VPN connection from the new VPC to the data center, and create a Cloud NAT instance to perform NAT on the overlapping IP space.  
C. Create a Cloud VPN connection from the new VPC to the data center, create a Cloud Router, and apply a custom route advertisement to block the overlapping IP space.  
D. Create a Cloud VPN connection from the new VPC to the data center, and apply a firewall rule that blocks the overlapping IP space.

---

## ✅ Correct Answer

**A. Create a Cloud VPN connection from the new VPC to the data center, create a Cloud Router, and apply new IP addresses so there is no overlapping IP space.**

---

## Explanation

### Why **A** is correct

- **Overlapping IP ranges cannot be routed** reliably between networks.
- Google’s **recommended best practice** is to **eliminate IP overlap** before establishing hybrid connectivity.
- This is done by:
  - **Renumbering** one of the environments (typically the smaller or newly acquired one)
  - Using **Cloud VPN** for secure connectivity
  - Using **Cloud Router** for dynamic route exchange (BGP)

This ensures:
- ✅ Clean routing tables
- ✅ No ambiguity in packet delivery
- ✅ Long-term, scalable hybrid connectivity

---

## ❌ Why the other options are not correct

### **B. Use Cloud NAT**
❌ Incorrect  
- Cloud NAT is for **outbound internet access**, not for VPC-to-on-prem connectivity
- It does **not solve overlapping IP ranges** for hybrid routing

---

### **C. Block overlapping IPs via route advertisements**
❌ Incorrect  
- Blocking routes does not resolve the root cause
- Applications still cannot communicate correctly
- Leads to partial connectivity and fragile designs

---

### **D. Use firewall rules**
❌ Incorrect  
- Firewall rules control traffic flow, **not routing**
- Routing conflicts will still exist even if traffic is blocked

---

## ✅ Final Answer

**A**

---

# 9. Migrating Hadoop Jobs with Minimal Cost and Management

## Question

You need to migrate **Hadoop jobs** for your company’s **Data Science team** **without modifying the underlying infrastructure**.  
You want to **minimize costs** and **reduce infrastructure management effort**.

What should you do?

A. Create a Dataproc cluster using standard worker instances.  
B. Create a Dataproc cluster using preemptible worker instances.  
C. Manually deploy a Hadoop cluster on Compute Engine using standard instances.  
D. Manually deploy a Hadoop cluster on Compute Engine using preemptible instances.

---

## ✅ Correct Answer

**B. Create a Dataproc cluster using preemptible worker instances.**

---

## Explanation

### Why **B** is correct ✅

- **Cloud Dataproc** is a **fully managed Hadoop and Spark service**, requiring:
  - No infrastructure setup
  - No cluster lifecycle management
- Dataproc supports **preemptible worker nodes**, which:
  - Are significantly **cheaper than standard instances**
  - Are ideal for **batch and fault-tolerant workloads** like Hadoop jobs
- Hadoop jobs can **retry tasks automatically**, making them well-suited for preemptible VMs

This approach provides:
- 💰 **Lower costs**
- ⚙️ **Minimal operational overhead**
- 🔁 **No changes to existing Hadoop jobs**

---

## ❌ Why the other options are not correct

### **A. Dataproc with standard workers**
- Works, but **more expensive**
- Does not optimize costs when preemptible workers are supported

---

### **C. Manual Hadoop on Compute Engine (standard instances)**
- High operational overhead
- Requires managing:
  - Cluster setup
  - Scaling
  - Failures
- Not aligned with the requirement to minimize management effort

---

### **D. Manual Hadoop on Compute Engine (preemptible instances)**
- Lower cost, but still:
  - Requires full cluster management
  - Loses the benefits of Dataproc automation

---

## ✅ Final Answer

**B**

---

# 10. Debian-Based Application with Automated OS Updates

## Question

You need to deploy an application on **Google Cloud** that must run on a **Debian Linux** environment.  
The application requires **extensive configuration** to operate correctly.

You want to ensure that you can **install Debian distribution updates with minimal manual intervention** whenever they become available.

What should you do?

A. Create a Compute Engine instance template using the most recent Debian image. Create an instance from this template, and install and configure the application as part of the startup script. Repeat this process whenever a new Google-managed Debian image becomes available.  
B. Create a Debian-based Compute Engine instance, install and configure the application, and use **OS patch management** to install available updates.  
C. Create an instance with the latest available Debian image. Connect to the instance via SSH, and install and configure the application on the instance. Repeat this process whenever a new Google-managed Debian image becomes available.  
D. Create a Docker container with Debian as the base image. Install and configure the application as part of the Docker image creation process. Host the container on Google Kubernetes Engine and restart the container whenever a new update is available.

---

## ✅ Correct Answer

**B. Create a Debian-based Compute Engine instance, install and configure the application, and use OS patch management to install available updates.**

---

## Explanation

### Why **B** is correct ✅

- **OS patch management** (part of VM Manager) is the **Google-recommended solution** for:
  - Applying **operating system updates automatically**
  - Reducing **manual intervention**
  - Maintaining **long-lived VMs** with complex configurations
- This approach allows you to:
  - Install and configure the application **once**
  - Continuously apply **Debian security and distribution updates**
  - Schedule patching during maintenance windows
- It avoids rebuilding or reconfiguring the application for every OS update

This best satisfies:
- 🐧 Debian OS requirement  
- ⚙️ Extensive application configuration  
- 🔄 Minimal manual update effort  

---

## ❌ Why the other options are not correct

### **A. Rebuilding instances from templates**
- Requires **recreating instances** for every Debian image update
- Causes unnecessary operational overhead
- Not ideal for heavily configured applications

---

### **C. Manual SSH-based updates**
- Fully manual and error-prone
- Does not scale
- Not aligned with automation best practices

---

### **D. Containerizing on GKE**
- Introduces **major architectural changes**
- OS updates are tied to image rebuilds
- Overkill when the requirement is simply OS patching

---

## ✅ Final Answer

**B**
