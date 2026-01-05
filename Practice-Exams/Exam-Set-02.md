# 11. Google Cloud Case Study — Designing a Future-Proof Hybrid Networking Environment

## Question

You are designing a **future-proof hybrid environment** that will require **network connectivity between Google Cloud and your on-premises environment**. You want to ensure that the Google Cloud environment you are designing is **compatible with your on-premises networking environment**. What should you do?

A. Use the default VPC in your Google Cloud project. Use a Cloud VPN connection between your on-premises environment and Google Cloud.  
B. Create a custom VPC in Google Cloud in auto mode. Use a Cloud VPN connection between your on-premises environment and Google Cloud.  
C. Create a network plan for your VPC in Google Cloud that uses CIDR ranges that overlap with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud.  
D. Create a network plan for your VPC in Google Cloud that uses non-overlapping CIDR ranges with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Create a network plan for your VPC in Google Cloud that uses non-overlapping CIDR ranges with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud.**
- **Non-overlapping CIDR ranges** are a strict requirement for reliable **hybrid networking**.
- Overlapping IP ranges prevent proper routing and make hybrid connectivity complex or impossible without NAT.
- **Cloud Interconnect** provides:
  - High-bandwidth, low-latency, and highly reliable connectivity.
  - A scalable, long-term solution suitable for **future growth**.
- Careful IP planning combined with Interconnect ensures:
  - Seamless integration with existing on-premises networks.
  - Minimal rework as the hybrid environment scales.
- This approach follows Google Cloud best practices for **enterprise-grade hybrid architecture**.

---

## ❌ Why the other options are not correct

### **A. Use the default VPC in your Google Cloud project. Use a Cloud VPN connection between your on-premises environment and Google Cloud.**
- The **default VPC** uses preconfigured IP ranges that may conflict with on-premises networks.
- Cloud VPN is suitable for **temporary or low-throughput** connectivity, not future-proof hybrid designs.

### **B. Create a custom VPC in Google Cloud in auto mode. Use a Cloud VPN connection between your on-premises environment and Google Cloud.**
- **Auto mode VPCs** automatically allocate subnets that can unintentionally overlap with on-premises CIDR ranges.
- Cloud VPN does not provide the scalability or performance expected for long-term hybrid environments.

### **C. Create a network plan for your VPC in Google Cloud that uses CIDR ranges that overlap with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud.**
- **Overlapping CIDR ranges** break routing and are explicitly discouraged.
- Even with Cloud Interconnect, overlapping IP ranges cause network conflicts and operational complexity.

---

## ✅ Final Answer
**D. Create a network plan for your VPC in Google Cloud that uses non-overlapping CIDR ranges with your on-premises environment. Use a Cloud Interconnect connection between your on-premises environment and Google Cloud.**

---

# 12. Google Cloud Case Study — Designing Scalable and Reliable IoT Data Ingestion

## Question

Your company wants to track whether someone is present in a meeting room reserved for a scheduled meeting. There are **1,000 meeting rooms across 5 offices on 3 continents**. Each room is equipped with a **motion sensor that reports its status every second**. You want to support the **data ingestion needs** of this sensor network. The receiving infrastructure needs to account for the possibility that the devices may have **inconsistent connectivity**. Which solution should you design?

A. Have each device create a persistent connection to a Compute Engine instance and write messages to a custom application.  
B. Have devices poll for connectivity to Cloud SQL and insert the latest messages on a regular interval to a device-specific table.  
C. Have devices poll for connectivity to Pub/Sub and publish the latest messages on a regular interval to a shared topic for all devices.  
D. Have devices create a persistent connection to an App Engine application fronted by Cloud Endpoints, which ingest messages and write them to Datastore.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Have devices poll for connectivity to Pub/Sub and publish the latest messages on a regular interval to a shared topic for all devices.**
- **Pub/Sub** is designed for **high-throughput, globally distributed, event-driven ingestion**.
- It naturally supports **bursty traffic** and **millions of messages per second**, which fits:
  - 1,000 devices
  - Reporting every second
- Pub/Sub handles **intermittent connectivity** well:
  - Devices can retry publishing when connectivity is restored.
  - Messages are durably stored once accepted by Pub/Sub.
- Using a **shared topic** simplifies architecture and scales automatically across regions.
- This is a Google-recommended pattern for **IoT and telemetry ingestion**.

---

## ❌ Why the other options are not correct

### **A. Have each device create a persistent connection to a Compute Engine instance and write messages to a custom application.**
- Maintaining **1,000 persistent connections** is operationally complex.
- Compute Engine does not automatically scale to handle intermittent connections.
- This design is fragile and not optimized for global, unreliable networks.

### **B. Have devices poll for connectivity to Cloud SQL and insert the latest messages on a regular interval to a device-specific table.**
- **Cloud SQL** is not designed for high-frequency, write-heavy ingestion workloads.
- Constant polling and inserts would quickly exhaust database connections.
- This approach does not scale and introduces performance bottlenecks.

### **D. Have devices create a persistent connection to an App Engine application fronted by Cloud Endpoints, which ingest messages and write them to Datastore.**
- Persistent connections are still problematic with **unreliable connectivity**.
- App Engine and Datastore add unnecessary complexity and latency.
- This architecture does not scale as efficiently as Pub/Sub for streaming telemetry.

---

## ✅ Final Answer
**C. Have devices poll for connectivity to Pub/Sub and publish the latest messages on a regular interval to a shared topic for all devices.**

---

# 13. Google Cloud Case Study — Low-Risk Cloud Adoption with Log Archiving and Analytics

## Question

Your company wants to **try out the cloud with low risk**. They want to **archive approximately 100 TB of log data** to the cloud and **test the serverless analytics features** available there, while also **retaining that data as a long-term disaster recovery backup**. Which two steps should they take? *(Choose two)*

A. Load logs into BigQuery.  
B. Load logs into Cloud SQL.  
C. Import logs into Cloud Logging.  
D. Insert logs into Cloud Bigtable.  
E. Upload log files into Cloud Storage.  

---

## ✅ **Correct Answers: A and E**

---

## Explanation

### **E. Upload log files into Cloud Storage.**
- **Cloud Storage** is ideal for **large-scale, low-cost data archiving**.
- It supports multiple storage classes (Nearline, Coldline, Archive) suitable for **long-term disaster recovery**.
- Storing raw log files in Cloud Storage:
  - Minimizes risk and cost.
  - Preserves the original data for future reprocessing.
  - Meets the requirement for a **long-term backup**.

### **A. Load logs into BigQuery.**
- **BigQuery** is a fully **serverless analytics platform** designed for analyzing large datasets.
- It allows the company to:
  - Run ad-hoc SQL queries on log data.
  - Test cloud-native, serverless analytics without managing infrastructure.
- BigQuery can directly analyze data stored in Cloud Storage or imported from it, making it a natural complement to Cloud Storage.

---

## ❌ Why the other options are not correct

### **B. Load logs into Cloud SQL.**
- **Cloud SQL** is a relational database intended for transactional workloads.
- It does not scale cost-effectively for **100 TB of log data**.
- This would introduce unnecessary operational complexity and cost.

### **C. Import logs into Cloud Logging.**
- **Cloud Logging** is optimized for real-time log ingestion and short- to medium-term retention.
- It is not designed to serve as a **long-term archival or disaster recovery solution** for massive datasets.

### **D. Insert logs into Cloud Bigtable.**
- **Cloud Bigtable** is designed for low-latency, key-value access patterns.
- It is not well suited for ad-hoc analytics or long-term archival storage.
- Operational overhead is higher compared to serverless analytics solutions.

---

## ✅ Final Answers
**A. Load logs into BigQuery.**  
**E. Upload log files into Cloud Storage.**

---

# 14. Google Cloud Case Study — Troubleshooting Load Balancer Health Checks

## Question

You set up an **autoscaling managed instance group** to serve web traffic for an upcoming launch. After configuring the instance group as a backend service to an **HTTP(S) load balancer**, you notice that **VM instances are being terminated and re-launched every minute**. The instances do **not have a public IP address**. You have verified that the appropriate web response is coming from each instance using the `curl` command. You want to ensure that the backend is configured correctly. What should you do?

A. Ensure that a firewall rule exists to allow source traffic on HTTP/HTTPS to reach the load balancer.  
B. Assign a public IP to each instance, and configure a firewall rule to allow the load balancer to reach the instance public IP.  
C. Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group.  
D. Create a tag on each instance with the name of the load balancer. Configure a firewall rule with the name of the load balancer as the source and the instance tag as the destination.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group.**
- **HTTP(S) load balancers** rely on **health checks** to determine whether backend instances are healthy.
- If health check traffic is **blocked by firewall rules**, the load balancer marks instances as **unhealthy**.
- Autoscaling managed instance groups automatically **terminate and recreate unhealthy instances**, which explains the constant recycling.
- Since the instances **do not have public IP addresses**, health checks must reach them via **internal networking**, which still requires an explicit firewall rule.
- Google Cloud requires allowing health check source IP ranges (such as `130.211.0.0/22` and `35.191.0.0/16`) to the instance group.

---

## ❌ Why the other options are not correct

### **A. Ensure that a firewall rule exists to allow source traffic on HTTP/HTTPS to reach the load balancer.**
- Client traffic reaching the load balancer is **not the issue**.
- The load balancer itself is functioning; the problem is backend health check failures.

### **B. Assign a public IP to each instance, and configure a firewall rule to allow the load balancer to reach the instance public IP.**
- Backend instances behind an HTTP(S) load balancer **do not require public IPs**.
- This approach increases attack surface and violates Google Cloud best practices.

### **D. Create a tag on each instance with the name of the load balancer. Configure a firewall rule with the name of the load balancer as the source and the instance tag as the destination.**
- Load balancers are **not valid firewall rule sources**.
- Firewall rules must reference **IP ranges, service accounts, or network tags**, not load balancer names.

---

## ✅ Final Answer
**C. Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group.**

---

# 15. Google Cloud Case Study — Securing Network Traffic in a 3-Tier Application

## Question

Your organization has a **3-tier web application** deployed in the same **Google Cloud Virtual Private Cloud (VPC)**. Each tier (**web, API, and database**) scales independently of the others.  
Network traffic should flow **from the web tier to the API tier, and then to the database tier**.  
Traffic should **not** flow directly between the **web tier and the database tier**.  
How should you configure the network with **minimal steps**?

A. Add each tier to a different subnetwork.  
B. Set up software-based firewalls on individual VMs.  
C. Add tags to each tier and set up routes to allow the desired traffic flow.  
D. Add tags to each tier and set up firewall rules to allow the desired traffic flow.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Add tags to each tier and set up firewall rules to allow the desired traffic flow.**
- **VPC firewall rules** are the native and recommended way to control traffic flow between resources in the same VPC.
- **Network tags** allow firewall rules to dynamically apply to instances, even as each tier scales independently.
- With firewall rules, you can explicitly:
  - Allow traffic from **web → API**.
  - Allow traffic from **API → database**.
  - Deny traffic from **web → database**.
- This approach:
  - Requires **minimal configuration steps**.
  - Avoids restructuring the network.
  - Scales automatically with autoscaling instance groups.

---

## ❌ Why the other options are not correct

### **A. Add each tier to a different subnetwork.**
- Subnetworks alone do not enforce traffic restrictions.
- You would still need firewall rules to control communication between tiers.
- This introduces unnecessary complexity without meeting the requirement by itself.

### **B. Set up software-based firewalls on individual VMs.**
- Managing firewall rules at the OS level does not scale well.
- It increases operational overhead and is not recommended when **VPC firewalls** are available.

### **C. Add tags to each tier and set up routes to allow the desired traffic flow.**
- **Routes** control how traffic is forwarded, not whether it is allowed or denied.
- They cannot enforce security boundaries between tiers.

---

## ✅ Final Answer
**D. Add tags to each tier and set up firewall rules to allow the desired traffic flow.**

---

# 16. Google Cloud Case Study — Secure Credential Management for Microservices

## Question

You are designing a **large distributed application with 30 microservices**. Each of your distributed microservices needs to **connect to a database backend**. You want to **store the credentials securely**. Where should you store the credentials?

A. In the source code  
B. In an environment variable  
C. In a secret management system  
D. In a config file that has restricted access through ACLs  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. In a secret management system**
- A **secret management system** (such as **Google Cloud Secret Manager**) is designed specifically to store and manage **sensitive information** like database credentials.
- It provides:
  - **Encryption at rest and in transit**
  - **Fine-grained IAM access control**
  - **Audit logging** to track access
  - **Secret rotation** without redeploying applications
- This approach scales well for **many microservices** and follows Google Cloud and industry **security best practices**.
- Microservices can securely retrieve secrets at runtime instead of embedding them in code or configuration.

---

## ❌ Why the other options are not correct

### **A. In the source code**
- Storing credentials in source code is a **critical security risk**.
- Secrets can be accidentally exposed through:
  - Version control systems
  - Code reviews
  - CI/CD pipelines
- This violates basic security principles and compliance requirements.

### **B. In an environment variable**
- Environment variables are better than hardcoding secrets, but they are **not the most secure option**.
- They can be exposed through:
  - Process listings
  - Debug logs
  - Misconfigured access
- They also lack built-in auditing and rotation capabilities.

### **D. In a config file that has restricted access through ACLs**
- Configuration files are not designed for secret management.
- Managing ACLs across many microservices is **error-prone and hard to scale**.
- Secrets stored this way are more difficult to rotate and audit compared to a dedicated secret manager.

---

## ✅ Final Answer
**C. In a secret management system**

---

# 17. Google Cloud Case Study — Granting Security Visibility with IAM Best Practices

## Question

Your customer is moving their **corporate applications to Google Cloud**. The **security team wants detailed visibility of all resources in the organization**. You use **Resource Manager** to set yourself up as the **Organization Administrator**.  
Which **Identity and Access Management (IAM) roles** should you give to the security team while following **Google recommended practices**?

A. Organization viewer, Project owner  
B. Organization viewer, Project viewer  
C. Organization administrator, Project browser  
D. Project owner, Network administrator  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Organization viewer, Project viewer**
- The **Organization Viewer** role provides **read-only visibility** across the entire Google Cloud organization:
  - Folders
  - Projects
  - Policies
- The **Project Viewer** role allows the security team to:
  - View project-level resources and configurations
  - Inspect logs, metadata, and settings
- This combination:
  - Meets the requirement for **detailed visibility**
  - Follows the **principle of least privilege**
  - Avoids granting unnecessary administrative or write permissions
- Google best practices recommend giving security teams **read-only access** unless they explicitly need to make changes.

---

## ❌ Why the other options are not correct

### **A. Organization viewer, Project owner**
- **Project Owner** grants full administrative access, including the ability to modify IAM policies and delete resources.
- This exceeds visibility needs and violates **least privilege** principles.

### **C. Organization administrator, Project browser**
- **Organization Administrator** is a highly privileged role with full control over the organization.
- Granting this role to the security team is unnecessary and risky when only visibility is required.

### **D. Project owner, Network administrator**
- These roles are **project-scoped**, not organization-wide.
- They grant excessive permissions and do not provide centralized visibility across the organization.

---

## ✅ Final Answer
**B. Organization viewer, Project viewer**

---

# 17. Google Cloud Case Study — Cost-Effective Development Environments with Cost Visibility

## Question

To reduce costs, the Director of Engineering has required all developers to move their **development infrastructure resources** from on-premises virtual machines (VMs) to **Google Cloud**. These resources go through **multiple start/stop events during the day** and require **state to persist**.  
You have been asked to design the process of running a development environment in Google Cloud while providing **cost visibility to the finance department**. Which two steps should you take? *(Choose two)*

A. Use persistent disks to store the state. Start and stop the VM as needed.  
B. Use the `gcloud --auto-delete` flag on all persistent disks before stopping the VM.  
C. Apply VM CPU utilization label and include it in the BigQuery billing export.  
D. Use BigQuery billing export and labels to relate cost to groups.  
E. Store all state in a Local SSD, snapshot the persistent disks, and terminate the VM.  

---

## ✅ **Correct Answers: A and D**

---

## Explanation

### **A. Use persistent disks to store the state. Start and stop the VM as needed.**
- **Persistent disks** retain data even when a VM is stopped.
- This allows developers to:
  - Stop VMs when not in use to save costs.
  - Restart them later without losing their development state.
- This is a standard and cost-effective pattern for **development environments** with frequent start/stop cycles.

### **D. Use BigQuery billing export and labels to relate cost to groups.**
- **BigQuery billing export** provides detailed, queryable cost data.
- **Labels** (e.g., team, project, environment) allow costs to be attributed to:
  - Development teams
  - Projects
  - Cost centers
- This gives the finance department **clear visibility** into cloud spending and aligns with Google’s recommended approach for **cost allocation and chargeback**.

---

## ❌ Why the other options are not correct

### **B. Use the `gcloud --auto-delete` flag on all persistent disks before stopping the VM.**
- Auto-deleting disks would **remove the stored state** when the VM stops.
- This contradicts the requirement that state must persist.

### **C. Apply VM CPU utilization label and include it in the BigQuery billing export.**
- **CPU utilization** is a metric, not a label that can be used directly for billing attribution.
- Labels should represent organizational context (team, project), not performance metrics.

### **E. Store all state in a Local SSD, snapshot the persistent disks, and terminate the VM.**
- **Local SSDs do not persist data** after VM termination.
- This adds unnecessary operational complexity and is not cost-efficient for frequent start/stop scenarios.

---

## ✅ Final Answers
**A. Use persistent disks to store the state. Start and stop the VM as needed.**  
**D. Use BigQuery billing export and labels to relate cost to groups.**

---

# 18. Google Cloud Case Study — Improving Database Performance on Compute Engine

## Question

The database administration team has asked you to help them **improve the performance** of their new database server running on **Compute Engine**.  
The database is used for **importing and normalizing performance statistics**. It is built with **MySQL on Debian Linux**.  
They have an **n1-standard-8 VM** with **80 GB of SSD zonal persistent disk**, which they **cannot restart until the next maintenance event**.  
What should they change to get **better performance as soon as possible** and in a **cost-effective manner**?

A. Increase the virtual machine’s memory to 64 GB.  
B. Create a new virtual machine running PostgreSQL.  
C. Dynamically resize the SSD persistent disk to 500 GB.  
D. Migrate their performance metrics warehouse to BigQuery.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Dynamically resize the SSD persistent disk to 500 GB.**
- **Persistent Disk SSD performance scales with disk size**.
- Increasing the disk size immediately increases:
  - **IOPS**
  - **Throughput**
- **Persistent disks can be resized online**, without restarting the VM.
- This provides an **immediate performance boost** with minimal risk.
- It is also **cost-effective**, since it avoids:
  - VM downtime
  - Re-architecture
  - Database migration

This is the fastest and safest way to improve database I/O performance under the given constraints.

---

## ❌ Why the other options are not correct

### **A. Increase the virtual machine’s memory to 64 GB.**
- Changing machine type requires a **VM restart**, which is not possible until the next maintenance window.
- This does not meet the “as soon as possible” requirement.

### **B. Create a new virtual machine running PostgreSQL.**
- Migrating to PostgreSQL is a **major architectural change**.
- It involves data migration, testing, and downtime.
- This is neither immediate nor cost-effective.

### **D. Migrate their performance metrics warehouse to BigQuery.**
- BigQuery is an analytics data warehouse, not a transactional database.
- Migration would require significant redesign and is not an immediate fix.
- This does not address short-term performance needs.

---

## ✅ Final Answer
**C. Dynamically resize the SSD persistent disk to 500 GB.**
