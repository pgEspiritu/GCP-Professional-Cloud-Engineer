# 👗 Dress4Win

Dress4Win is a **web-based company** that helps users organize and manage their personal wardrobe through a **web application and mobile app**. The company also operates a **social network** connecting users with designers and retailers.

Dress4Win monetizes its platform through:
- Advertising
- E-commerce
- Referrals
- A freemium application model

The application has grown from a few servers in the founder’s garage to **several hundred servers and appliances** in a colocated data center. However, the current infrastructure **cannot scale fast enough** to support rapid growth. To address scalability, innovation speed, and cost efficiency, Dress4Win is committing to a **full migration to the public cloud**.

> **Key point:** The current infrastructure is no longer able to scale to meet application growth.

---

## 💡 Solution Concept

For the **first phase** of cloud migration, Dress4Win plans to:

- Migrate **development and test environments** to the public cloud
- Build a **disaster recovery (DR) site**
- Keep the **production environment on-premises** initially
- Evaluate which components can be:
  - Lifted and shifted
  - Refactored before migration

> **Key point:** Development and test environments move first, while cloud is used as a **DR site** for the existing on-prem production system.

---

## 🧑‍💼 Executive Statement

> Our investors are concerned about our ability to scale and control costs with our current infrastructure. A competitor could leverage the public cloud to reduce up-front investment and focus on faster innovation.

Traffic patterns show:
- Peak usage in **mornings and weekend evenings**
- **80% of capacity idle** during off-peak hours

Key financial concerns:
- Capital expenditures exceed quarterly projections
- Cloud migration may initially increase costs
- Full transition is planned before the next hardware refresh cycle
- **5-year TCO analysis** shows a **30–50% cost reduction** with a public cloud strategy

> **Key focus areas:** scalability, efficiency, reduction of idle resources, lower CAPEX, and improved long-term TCO.

---

## 🧱 Existing Technical Environment

### Infrastructure
- Single on-premises data center
- All servers run **Ubuntu LTS 16.04**

---

### 🗄️ Databases

#### MySQL
- Used for:
  - User data
  - Inventory
  - Static data
- Configuration:
  - MySQL 5.8
  - 8 CPU cores
  - 128 GB RAM
  - 2 × 5 TB HDD (RAID 1)

**Cloud Migration Option**
- Can be migrated directly to **Cloud SQL for MySQL**
- Fully managed relational database service on GCP

---

#### Redis Cluster
- 3-node Redis 3.2 cluster
- Used for:
  - Metadata
  - Social graph
  - Caching
- Each node:
  - 4 CPU cores
  - 32 GB RAM

**Cloud Migration Option**
- Replace with **Cloud Memorystore for Redis**
- Fully managed in-memory data store
- No application changes required

---

### 🖥️ Compute

- **40 web application servers**
- Provide:
  - Microservices-based APIs
  - Static content
- Technologies:
  - Java (Tomcat)
  - Nginx

---

## 🎯 Key Migration Drivers (Summary)

- Inability of current infrastructure to scale
- Excess idle capacity during off-peak hours
- High capital expenditure
- Desire for faster innovation
- Long-term cost efficiency via public cloud
- Improved disaster recovery posture

---

# 1. Secure Operations Access Without External SSH

## Question

The Dress4Win security team has disabled external SSH access into production virtual machines (VMs) on Google Cloud Platform (GCP).  
The operations team needs to remotely manage the VMs, build and push Docker containers, and manage Google Cloud Storage objects.  
What can they do?

A. Grant the operations engineer access to use Google Cloud Shell.  
B. Configure a VPN connection to GCP to allow SSH access to the cloud VMs.  
C. Develop a new access request process that grants temporary SSH access to cloud VMs when an operations engineer needs to perform a task.  
D. Have the development team build an API service that allows the operations team to execute specific remote procedure calls to accomplish their tasks.  

---

## ✅ Correct Answer

**A. Grant the operations engineer access to use Google Cloud Shell.**

---

## Explanation

Google Cloud Shell is the recommended secure solution for managing Google Cloud resources **without enabling external SSH access**.

Cloud Shell provides:
- Browser-based access secured by IAM  
- Pre-installed tools such as `gcloud`, `docker`, and `kubectl`  
- Ability to manage VMs, build and push containers, and manage Cloud Storage  
- No need to expose production VMs to external networks  

This approach fully satisfies the security requirement while enabling all operational tasks.

---

## ❌ Why the other options are wrong

**B. Configure a VPN connection**  
- Reintroduces SSH access, which is explicitly disabled by security  

**C. Temporary SSH access process**  
- Still violates the intent of removing SSH access  

**D. Build a custom API service**  
- Unnecessary complexity and cost  
- Not aligned with Google-recommended best practices  

---

## ✅ Final Answer

**A**

---

# 2. Low-Cost Remote Archiving of Database Backup Files

## Question

At Dress4Win, an operations engineer wants to create a **low-cost solution** to remotely archive copies of database backup files.  
The database files are **compressed tar files** stored in their current data center.  
How should he proceed?

A. Create a cron script using gsutil to copy the files to a Coldline Storage bucket.  
B. Create a cron script using gsutil to copy the files to a Regional Storage bucket.  
C. Create a Cloud Storage Transfer Service Job to copy the files to a Coldline Storage bucket.  
D. Create a Cloud Storage Transfer Service job to copy the files to a Regional Storage bucket.  

---

## ✅ Correct Answer

**C. Create a Cloud Storage Transfer Service Job to copy the files to a Coldline Storage bucket.**

---

## Explanation

- **Cloud Storage Transfer Service** is the managed, reliable, and scalable way to move large amounts of data from on-premises to Cloud Storage.  
- **Coldline Storage** is optimized for **archival and infrequently accessed data**, making it the **lowest-cost option** for long-term backups.  
- Using Transfer Service avoids the need to manage cron jobs, network retries, or manual scripting.  
- Ensures **automated, repeatable transfers** with monitoring and reporting.  

---

## ❌ Why the other options are wrong

**A. Cron script with gsutil to Coldline**  
- Works but requires manual maintenance and monitoring; not scalable for large datasets  

**B. Cron script to Regional Storage**  
- Regional Storage is more expensive; not optimized for archival  

**D. Transfer Service to Regional Storage**  
- Managed transfer is good, but Regional Storage is **not cost-optimal** for archival  

---

## ✅ Final Answer

**C**

---

# 3. Recommending Machine Types for Application Servers

## Question

Dress4Win has asked you to recommend **machine types** they should deploy their **application servers** to.  
How should you proceed?

A. Perform a mapping of the on-premises physical hardware cores and RAM to the nearest machine types in the cloud.  
B. Recommend that Dress4Win deploy application servers to machine types that offer the highest RAM to CPU ratio available.  
C. Recommend that Dress4Win deploy into production with the smallest instances available, monitor them over time, and scale the machine type up until the desired performance is reached.  
D. Identify the number of virtual cores and RAM associated with the application server virtual machines, align them to a **custom machine type** in the cloud, monitor performance, and scale the machine types up until the desired performance is reached.  

---

## ✅ Correct Answer

**D. Identify the number of virtual cores and RAM associated with the application server virtual machines, align them to a custom machine type in the cloud, monitor performance, and scale the machine types up until the desired performance is reached.**

---

## Explanation

- Google Cloud allows **custom machine types** to match exact CPU and RAM requirements.  
- Start with a configuration **aligned with current usage** for predictable performance.  
- **Monitoring and scaling** over time ensures optimal resource utilization and cost efficiency.  
- Avoids over-provisioning while providing flexibility to **adjust machine types** as needed.  

---

## ❌ Why the other options are wrong

**A. Map physical hardware to standard machine types**  
- Does not account for actual VM workload utilization and may lead to over/under-provisioning  

**B. Choose highest RAM-to-CPU ratio**  
- Arbitrary and may be **wasteful** if the application does not need it  

**C. Start with smallest instances**  
- May cause **performance issues** in production before scaling  

---

## ✅ Final Answer

**D**

---

# 4. Managed Logging and Monitoring for Dress4Win

## Question

As part of Dress4Win's plans to migrate to the cloud, they want to be able to set up a **managed logging and monitoring system** so they can handle **spikes in their traffic load**.  

They want to ensure that:  

- The infrastructure can be **notified when it needs to scale up and down** to handle the ebb and flow of usage throughout the day  
- Their administrators are **notified automatically** when their application reports errors  
- They can **filter aggregated logs** to debug one piece of the application across many hosts  

Which Google StackDriver features should they use?

A. Logging, Alerts, Insights, Debug  
B. Monitoring, Trace, Debug, Logging  
C. Monitoring, Logging, Alerts, Error Reporting  
D. Monitoring, Logging, Debug, Error Report  

---

## ✅ Correct Answer

**C. Monitoring, Logging, Alerts, Error Reporting**

---

## Explanation

- **Monitoring**: Tracks system and application metrics and can trigger autoscaling based on thresholds  
- **Logging**: Aggregates logs across hosts for filtering and debugging  
- **Alerts**: Sends notifications automatically when predefined conditions occur  
- **Error Reporting**: Notifies administrators automatically when applications report errors  

This combination ensures **visibility, automation, and proactive error handling**, fully supporting Dress4Win's requirements.

---

## ❌ Why the other options are wrong

**A. Logging, Alerts, Insights, Debug**  
- “Insights” is not a Google Cloud feature; missing **Monitoring**  

**B. Monitoring, Trace, Debug, Logging**  
- Trace is for latency profiling, not for automated error notification or alerts  

**D. Monitoring, Logging, Debug, Error Report**  
- “Error Report” is misnamed; proper service is **Error Reporting**  
- Missing **Alerts** for autoscaling and notifications  

---

## ✅ Final Answer

**C**

---

# 5. First Applications to Move to the Cloud for Dress4Win

## Question

Dress4Win would like to become familiar with **deploying applications to the cloud** by successfully deploying some applications quickly, **as is**. They have asked for your recommendation.  

What should you advise?

A. Identify self-contained applications with external dependencies as a first move to the cloud.  
B. Identify enterprise applications with internal dependencies and recommend these as a first move to the cloud.  
C. Suggest moving their in-house databases to the cloud and continue serving requests to on-premise applications.  
D. Recommend moving their message queuing servers to the cloud and continue handling requests to on-premise applications.  

---

## ✅ Correct Answer

**A. Identify self-contained applications with external dependencies as a first move to the cloud.**

---

## Explanation

- **Self-contained applications** are easier to migrate because they **do not rely on complex internal dependencies**  
- They can be **deployed quickly** to the cloud without requiring extensive refactoring or integration work  
- Provides a **fast learning experience** for the team and builds confidence in cloud deployments  
- Reduces the **risk of downtime or integration issues** with critical enterprise applications  

This approach follows **Google Cloud migration best practices** for initial application migration.

---

## ❌ Why the other options are wrong

**B. Enterprise applications with internal dependencies**  
- Highly coupled applications are **difficult to migrate** and may require redesign  

**C. Moving databases first**  
- Requires applications to remain on-premise, which **limits benefits** and increases complexity  

**D. Moving message queuing servers first**  
- Still requires integration with on-premise apps, increasing **operational complexity**  

---

## ✅ Final Answer

**A**

---

# 6. Migrating On-Premises MySQL to the Cloud for Dress4Win

## Question

Dress4Win has asked you for advice on how to **migrate their on-premises MySQL deployment to the cloud**.  
They want to **minimize downtime** and **performance impact** to their on-premises solution during the migration.  

Which approach should you recommend?

A. Create a dump of the on-premises MySQL master server, and then shut it down, upload it to the cloud environment, and load into a new MySQL cluster.  
B. Setup a MySQL replica server/slave in the cloud environment, and configure it for asynchronous replication from the MySQL master server on-premises until cutover.  
C. Create a new MySQL cluster in the cloud, configure applications to begin writing to both on-premises and cloud MySQL masters, and destroy the original cluster at cutover.  
D. Create a dump of the MySQL replica server into the cloud environment, load it into Google Cloud Datastore, and configure applications to read/write to Cloud Datastore at cutover.  

---

## ✅ Correct Answer

**B. Setup a MySQL replica server/slave in the cloud environment, and configure it for asynchronous replication from the MySQL master server on-premises until cutover.**

---

## Explanation

- Using **replication** allows the cloud MySQL instance to **stay synchronized** with the on-premises master.  
- Minimizes **downtime** because the master continues serving requests during migration.  
- **Asynchronous replication** ensures updates are continuously copied to the cloud, enabling a **smooth cutover**.  
- After verification, **cutover** is performed by switching the application to the cloud replica, then promoting it to master if necessary.  

This approach follows **Google Cloud best practices for minimal downtime database migration**.

---

## ❌ Why the other options are wrong

**A. Dump and upload after shutting down master**  
- Requires **downtime** while taking the dump and loading it to the cloud  
- Not suitable if minimizing service disruption is critical  

**C. Dual-write to both masters**  
- High risk of **data inconsistency** and conflicts  
- Requires application changes and careful conflict resolution  

**D. Dump to Cloud Datastore**  
- Cloud Datastore is **NoSQL**, incompatible with MySQL workloads  
- Would require significant **application refactoring**  

---

## ✅ Final Answer

**B**

---

# 7. Fixing Stackdriver Uptime Check for Legacy Services

## Question

Dress4Win has configured a new **uptime check with Google Stackdriver** for several of their legacy services.  
The Stackdriver dashboard is **not reporting the services as healthy**.  

What should they do?

A. Install the Stackdriver agent on all of the legacy web servers.  
B. In the Cloud Platform Console download the list of the uptime servers' IP addresses and create an inbound firewall rule.  
C. Configure their load balancer to pass through the User-Agent HTTP header when the value matches `GoogleStackdriverMonitoring-UptimeChecks` (https://cloud.google.com/monitoring).  
D. Configure their legacy web servers to allow requests that contain User-Agent HTTP header when the value matches `GoogleStackdriverMonitoring-UptimeChecks` (https://cloud.google.com/monitoring).  

---

## ✅ Correct Answer

**B. In the Cloud Platform Console download the list of the uptime servers' IP addresses and create an inbound firewall rule.**

---

## Explanation

- Google Stackdriver uptime checks **originate from specific IP addresses** that are publicly documented.  
- If the **firewall blocks requests** from these IPs, Stackdriver cannot reach your services, and uptime checks fail.  
- Allowing these IP addresses ensures **reliable and successful health checks**.  
- Installing agents or modifying headers is unnecessary for simple uptime checks.

---

## ❌ Why the other options are wrong

**A. Install Stackdriver agent**  
- The agent monitors VM metrics and logs, **not required for uptime checks**.  

**C. Pass through User-Agent via load balancer**  
- Not needed unless a load balancer **strips headers**, which is not indicated.  

**D. Allow User-Agent on legacy servers**  
- Header filtering is **not the root cause** here; the problem is **blocked IPs at the firewall**.  

---

## ✅ Final Answer

**B**

---

# 8. Customer Image Upload and Access Control

## Question

As part of their new application experience, Dress4Win allows customers to **upload images of themselves**.  
The customer has **exclusive control** over who may view these images.  
Customers should be able to **upload images with minimal latency** and also be shown their images quickly on the main application page when they log in.  

Which configuration should Dress4Win use?

A. Store image files in a Google Cloud Storage bucket. Use Google Cloud Datastore to maintain metadata that maps each customer's ID and their image files.  
B. Store image files in a Google Cloud Storage bucket. Add custom metadata to the uploaded images in Cloud Storage that contains the customer's unique ID.  
C. Use a distributed file system to store customers' images. As storage needs increase, add more persistent disks and/or nodes. Assign each customer a unique ID, which sets each file's owner attribute, ensuring privacy of images.  
D. Use a distributed file system to store customers' images. As storage needs increase, add more persistent disks and/or nodes. Use a Google Cloud SQL database to maintain metadata that maps each customer's ID to their image files.  

---

## ✅ Correct Answer

**A. Store image files in a Google Cloud Storage bucket. Use Google Cloud Datastore to maintain metadata that maps each customer's ID and their image files.**

---

## Explanation

- **Cloud Storage** is ideal for storing large, unstructured objects like images.  
- **Cloud Datastore (Firestore in Datastore mode)** provides fast, scalable access to metadata such as mapping **customer IDs to images**, enabling quick retrieval.  
- This architecture allows:
  - **Exclusive access control** via metadata and bucket/object ACLs or IAM policies  
  - **Low-latency uploads** and fast delivery for display in the app  
  - **Scalability** as the number of users and images grows  

---

## ❌ Why the other options are wrong

**B. Store custom metadata in Cloud Storage**  
- Metadata alone **cannot provide robust access control**, and querying for all images per user is **less efficient** than using a database.  

**C & D. Use a distributed file system**  
- Cloud Storage already provides a **fully managed, scalable, and highly available solution**  
- Managing your own distributed file system adds **operational overhead** and **complexity**, and is unnecessary in GCP  

---

## ✅ Final Answer

**A**

---

# 9. Additional Testing Methods for Cloud Migration

## Question

Dress4Win has **end-to-end tests covering 100% of their endpoints**.  
They want to ensure that the **move to the cloud** does not introduce any new bugs.  

Which **additional testing methods** should the developers employ to prevent an outage?

A. They should enable Google Stackdriver Debugger on the application code to show errors in the code.  
B. They should add additional unit tests and production scale load tests on their cloud staging environment.  
C. They should run the end-to-end tests in the cloud staging environment to determine if the code is working as intended.  
D. They should add canary tests so developers can measure how much of an impact the new release causes to latency.  

---

## ✅ Correct Answer

**B. They should add additional unit tests and production scale load tests on their cloud staging environment.**

---

## Explanation

- **Unit tests** ensure that individual components continue to function correctly in the new cloud environment.  
- **Production-scale load tests** simulate real-world usage to detect performance issues or scaling problems.  
- Running these in a **cloud staging environment** mirrors the production setup without impacting users.  
- This approach helps **prevent outages** by catching functional and performance issues **before deployment**.  
- Google Cloud best practices recommend **comprehensive testing in a staging environment** before production migration.

---

## ❌ Why the other options are wrong

**A. Stackdriver Debugger**  
- Useful for diagnosing runtime errors but **does not prevent bugs before deployment**.  

**C. Running end-to-end tests only**  
- End-to-end tests are important but **may not simulate production-scale load** or detect performance bottlenecks.  

**D. Canary tests**  
- Canary tests **measure impact in production**, but they do **not proactively prevent bugs** in staging.  

---

## ✅ Final Answer

**B**

---

# 10. Long-Term Storage for Audit Records

## Question

You want to ensure Dress4Win's **sales and tax records** remain available for **infrequent viewing by auditors for at least 10 years**.  
**Cost optimization** is your top priority.  

Which cloud services should you choose?

A. Google Cloud Storage Coldline to store the data, and gsutil to access the data.  
B. Google Cloud Storage Nearline to store the data, and gsutil to access the data.  
C. Google Bigtable with US or EU as location to store the data, and gcloud to access the data.  
D. BigQuery to store the data, and a web server cluster in a managed instance group to access the data. Google Cloud SQL mirrored across two distinct regions to store the data, and a Redis cluster in a managed instance group to access the data.  

---

## ✅ Correct Answer

**A. Google Cloud Storage Coldline to store the data, and gsutil to access the data.**

---

## Explanation

- **Coldline** is designed for **long-term archival storage** with **infrequent access** (once a year or less).  
- Provides **low storage cost**, which is ideal for audit records that are rarely accessed.  
- Can be accessed via **gsutil**, a simple and cost-effective method.  
- Meets **durability and retention requirements** (99.999999999% annual durability, configurable retention policies).  
- **Nearline** is cheaper than Standard but intended for data accessed **up to once a month**, so Coldline is more cost-efficient for 10-year retention.  
- Bigtable and BigQuery are **not cost-efficient** for long-term archival of rarely accessed data.  

---

## ❌ Why the other options are wrong

**B. Nearline Storage**  
- Suitable for monthly access, but not the **lowest cost for decade-long archival**.  

**C. Bigtable**  
- Designed for **high throughput, low-latency access**, not for archival; costly for long-term storage.  

**D. BigQuery + Cloud SQL + Redis**  
- Overly complex and expensive for **rarely accessed audit data**; not cost-optimized.  

---

## ✅ Final Answer

**A**

---

# 11. Distributing System Architecture for Performance

## Question

The current Dress4Win system architecture has **high latency to some customers** because it is located in **one data center**.  
As part of a future evaluation and **optimizing for performance in the cloud**, Dress4Win wants to **distribute its system architecture to multiple locations** on Google Cloud Platform.  

Which approach should they use?

A. Use regional managed instance groups and a global load balancer to increase performance because the regional managed instance group can grow instances in each region separately based on traffic.  
B. Use a global load balancer with a set of virtual machines that forward the requests to a closer group of virtual machines managed by your operations team.  
C. Use regional managed instance groups and a global load balancer to increase reliability by providing automatic failover between zones in different regions.  
D. Use a global load balancer with a set of virtual machines that forward the requests to a closer group of virtual machines as part of a separate managed instance groups.  

---

## ✅ Correct Answer

**A. Use regional managed instance groups and a global load balancer to increase performance because the regional managed instance group can grow instances in each region separately based on traffic.**

---

## Explanation

- **Regional Managed Instance Groups (MIGs)** allow you to **deploy VMs in multiple regions** and **autoscale** based on traffic per region.  
- **Global HTTP(S) Load Balancer** routes traffic to the **closest available region**, reducing latency for end users.  
- This architecture is **performance-focused**, not just for reliability.  
- Provides **automatic scaling** and **low-latency access** globally.  

---

## ❌ Why the other options are wrong

**B & D. Forwarding VMs manually**  
- Adds unnecessary complexity and operational overhead.  
- Not automated; does not take advantage of GCP autoscaling or global load balancing.

**C. Focuses on reliability**  
- While regional MIGs with global load balancing **do increase reliability**, this option emphasizes failover, not **performance optimization** (latency reduction).  

---

## ✅ Final Answer

**A**

---

# 13. Automating Deployment of Web and Transactional Layers

## Question

For this question, refer to the Dress4Win case study.  
Considering the given business requirements, how would you **automate the deployment** of web and transactional data layers?

A. Deploy Nginx and Tomcat using Cloud Deployment Manager to Compute Engine. Deploy a Cloud SQL server to replace MySQL. Deploy Jenkins using Cloud Deployment Manager.  
B. Deploy Nginx and Tomcat using Cloud Launcher. Deploy a MySQL server using Cloud Launcher. Deploy Jenkins to Compute Engine using Cloud Deployment Manager scripts.  
C. Migrate Nginx and Tomcat to App Engine. Deploy a Cloud Datastore server to replace the MySQL server in a high-availability configuration. Deploy Jenkins to Compute Engine using Cloud Launcher.  
D. Migrate Nginx and Tomcat to App Engine. Deploy a MySQL server using Cloud Launcher. Deploy Jenkins to Compute Engine using Cloud Launcher.  

---

## ✅ Correct Answer

**A. Deploy Nginx and Tomcat using Cloud Deployment Manager to Compute Engine. Deploy a Cloud SQL server to replace MySQL. Deploy Jenkins using Cloud Deployment Manager.**

---

## Explanation

- **Cloud Deployment Manager** enables **infrastructure as code** for reproducible and automated deployments.  
- **Compute Engine VMs** for Nginx and Tomcat provide **full control over web and application servers**.  
- **Cloud SQL** replaces on-premises MySQL with a **managed, high-availability relational database**.  
- Using Deployment Manager for **Jenkins** allows **automated CI/CD setup** consistent with the rest of the cloud deployment.  
- This approach ensures **repeatable deployments** and **minimizes human error**.

---

## ❌ Why the other options are wrong

**B. Cloud Launcher for Nginx/Tomcat and MySQL**  
- Cloud Launcher provides pre-built solutions, but **less control** and flexibility than Deployment Manager.  
- Mixing Cloud Launcher and Deployment Manager increases management complexity.  

**C & D. Migrate Nginx/Tomcat to App Engine**  
- App Engine imposes restrictions on server configurations, which may not meet legacy application requirements.  
- Using Cloud Datastore instead of Cloud SQL may require **major application changes**.  

---

## ✅ Final Answer

**A**

---

# 14. Compute Services Optimized for Cloud Performance

## Question

For this question, refer to the Dress4Win case study.  
Which of the compute services should be **migrated as-is** and would still be an **optimized architecture** for performance in the cloud?

A. Web applications deployed using App Engine standard environment  
B. RabbitMQ deployed using an unmanaged instance group  
C. Hadoop/Spark deployed using Cloud Dataproc Regional in High Availability mode  
D. Jenkins, monitoring, bastion hosts, security scanners services deployed on custom machine types  

---

## ✅ Correct Answer

**C. Hadoop/Spark deployed using Cloud Dataproc Regional in High Availability mode**

---

## Explanation

- **Cloud Dataproc Regional in HA mode** is a managed, **highly available cluster** for Hadoop and Spark workloads.  
- It provides **optimized cloud performance**, easy **scaling**, and **minimal operational overhead**.  
- The architecture can be migrated as-is without major changes and continues to meet performance requirements.

---

## ❌ Why the other options are wrong

**A. App Engine standard environment**  
- App Engine requires some **application modifications** and is not a direct “as-is” migration.  

**B. RabbitMQ on unmanaged instance groups**  
- Unmanaged instance groups **lack autoscaling and HA**, which is not optimized for cloud performance.  

**D. Jenkins, monitoring, bastion hosts on custom machine types**  
- These require **manual management**, which is not fully optimized in a cloud-native architecture.  

---

## ✅ Final Answer

**C**

---

# 15. Auditing Administrative Actions

## Question

For this question, refer to the Dress4Win case study.  
To be legally compliant during an audit, Dress4Win must be able to give **insights in all administrative actions** that modify the configuration or metadata of resources on Google Cloud.  
What should you do?

A. Use Stackdriver Trace to create a Trace list analysis.  
B. Use Stackdriver Monitoring to create a dashboard on the project's activity.  
C. Enable Cloud Identity-Aware Proxy in all projects, and add the group of Administrators as a member.  
D. Use the Activity page in the GCP Console and Stackdriver Logging to provide the required insight.  

---

## ✅ Correct Answer

**D. Use the Activity page in the GCP Console and Stackdriver Logging to provide the required insight.**

---

## Explanation

- **Cloud Audit Logs** (visible in Stackdriver Logging and the Activity page) provide **records of administrative and data access actions**.  
- This ensures compliance with audit requirements by showing **who modified what and when**.  
- The Activity page aggregates and filters changes across GCP resources, while Logging provides **long-term retention** and **querying capabilities**.

---

## ❌ Why the other options are wrong

**A. Stackdriver Trace**  
- Trace is for **request latency and performance analysis**, not administrative auditing.  

**B. Stackdriver Monitoring dashboard**  
- Monitoring shows metrics, not **resource configuration changes or admin actions**.  

**C. Cloud Identity-Aware Proxy**  
- IAP controls access to applications, not **tracking admin modifications**.  

---

## ✅ Final Answer

**D**

---

# 16. Securing Cloud Storage Data for Dress4Win

## Question

For this question, refer to the Dress4Win case study.  
You are responsible for the **security of data stored in Cloud Storage** for your company, Dress4Win. You have already created a set of Google Groups and assigned the appropriate users to those groups. You should use **Google best practices** and implement the **simplest design** to meet the requirements.  

Considering Dress4Win's business and technical requirements, what should you do?

A. Assign custom IAM roles to the Google Groups you created in order to enforce security requirements. Encrypt data with a customer-supplied encryption key when storing files in Cloud Storage.  
B. Assign custom IAM roles to the Google Groups you created in order to enforce security requirements. Enable default storage encryption before storing files in Cloud Storage.  
C. Assign predefined IAM roles to the Google Groups you created in order to enforce security requirements. Utilize Google's default encryption at rest when storing files in Cloud Storage.  
D. Assign predefined IAM roles to the Google Groups you created in order to enforce security requirements. Ensure that the default Cloud KMS key is set before storing files in Cloud Storage.  

---

## ✅ Correct Answer

**C. Assign predefined IAM roles to the Google Groups you created in order to enforce security requirements. Utilize Google's default encryption at rest when storing files in Cloud Storage.**

---

## Explanation

- **Predefined IAM roles** (like `roles/storage.objectViewer`, `roles/storage.objectAdmin`) provide sufficient granularity for **group-based access control**.  
- Using **Google-managed default encryption at rest** is the simplest and recommended approach for **data security in Cloud Storage**.  
- This avoids the complexity of managing **customer-supplied encryption keys** while still complying with security best practices.  
- Leveraging **Google Groups** simplifies user management and enforces access at the group level.

---

## ❌ Why the other options are wrong

**A. Custom IAM roles + customer-supplied keys**  
- Unnecessarily complex; managing keys increases operational overhead.  

**B. Custom IAM roles + default encryption**  
- Custom roles are only needed if predefined roles do not satisfy requirements; adds unnecessary complexity.  

**D. Predefined IAM roles + Cloud KMS default key**  
- Using a Cloud KMS key is optional and adds operational overhead; Google-managed encryption is sufficient in most cases.  

---

## ✅ Final Answer

**C**

---

# 17. Preparing On-Premises Architecture for Cloud Migration

## Question

For this question, refer to the Dress4Win case study.  
You want to ensure that your **on-premises architecture meets business requirements** before you migrate your solution.  

What change in the on-premises architecture should you make?

A. Replace RabbitMQ with Google Pub/Sub.  
B. Downgrade MySQL to v5.7, which is supported by Cloud SQL for MySQL.  
C. Resize compute resources to match predefined Compute Engine machine types.  
D. Containerize the micro-services and host them in Google Kubernetes Engine.  

---

## ✅ Correct Answer

**D. Containerize the micro-services and host them in Google Kubernetes Engine.**

---

## Explanation

- **Containerizing microservices** prepares the architecture for a **cloud-native deployment**, improving portability and scalability.  
- **Google Kubernetes Engine (GKE)** provides a managed environment to deploy, scale, and manage containerized applications.  
- This approach allows Dress4Win to **validate architecture and functionality on-premises** before cloud migration, ensuring **minimal disruption**.  
- It also facilitates **incremental migration** by decoupling services from monolithic on-premises dependencies.

---

## ❌ Why the other options are wrong

**A. Replace RabbitMQ with Pub/Sub**  
- Pub/Sub is a cloud service; cannot be tested fully on-premises.  

**B. Downgrade MySQL**  
- Downgrading versions may cause unnecessary compatibility issues; not required for migration validation.  

**C. Resize compute resources**  
- Matching GCP machine types is not sufficient for validating the architecture’s portability or scalability.  

---

## ✅ Final Answer

**D**
