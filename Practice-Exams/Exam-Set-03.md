# 1. Google Cloud Case Study — API Versioning and Traffic Management

## Question

Your company has decided to make a major revision of their API in order to create better experiences for their developers. They need to keep the old version of the API available and deployable, while allowing new customers and testers to try out the new API. They want to keep the same SSL and DNS records in place to serve both APIs.  

What should they do?  

A. Configure a new load balancer for the new version of the API  
B. Reconfigure old clients to use a new endpoint for the new API  
C. Have the old API forward traffic to the new API based on the path  
D. Use separate backend pools for each API path behind the load balancer  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Use separate backend pools for each API path behind the load balancer**
- Google Cloud **HTTP(S) Load Balancers** support **path-based routing**.  
- You can configure **different backend services (pools) for different URL paths**, e.g., `/v1/*` goes to the old API and `/v2/*` goes to the new API.  
- This approach allows both APIs to **share the same SSL certificate and DNS record**, while keeping them **independently deployable**.  
- Path-based routing is **Google-recommended** for API versioning and traffic management.

---

## ❌ Why the other options are not correct

### **A. Configure a new load balancer for the new version of the API**
- Using a new load balancer would require **different IP addresses, DNS records, or SSL certificates**.  
- This adds operational overhead and does **not meet the requirement** to keep the same SSL and DNS in place.

### **B. Reconfigure old clients to use a new endpoint for the new API**
- This would break backward compatibility for existing clients.  
- The requirement is to **keep the old API available for existing users** without forcing them to change endpoints.

### **C. Have the old API forward traffic to the new API based on the path**
- Forwarding from the old API introduces **unnecessary complexity**.  
- It may create **single points of failure** and **performance overhead**, instead of leveraging the load balancer for path-based routing.

---

## ✅ Final Answer

**D. Use separate backend pools for each API path behind the load balancer**

---

# 2. Google Cloud Case Study — Migrating Large Data Sets for Analysis

## Question

Your company plans to migrate a multi-petabyte data set to the cloud. The data set must be available 24hrs a day. Your business analysts have experience only with using a SQL interface.  

How should you store the data to optimize it for ease of analysis?  

A. Load data into Google BigQuery  
B. Insert data into Google Cloud SQL  
C. Put flat files into Google Cloud Storage  
D. Stream data into Google Cloud Datastore  

---

## ✅ **Correct Answer: A**

---

## Explanation

### **A. Load data into Google BigQuery**
- **BigQuery** is a fully managed, serverless, petabyte-scale **data warehouse**.  
- It allows users to query large datasets using **standard SQL**, which matches the skillset of the business analysts.  
- It is designed to handle **multi-petabyte datasets** with **24/7 availability**.  
- BigQuery provides features like **columnar storage**, **automatic scaling**, and **highly optimized querying**, making it ideal for large-scale analytics.

---

## ❌ Why the other options are not correct

### **B. Insert data into Google Cloud SQL**
- Cloud SQL is a managed relational database for **transactional workloads**, not optimized for **multi-petabyte analytical workloads**.  
- It would be **slow, expensive, and difficult to scale** for this use case.

### **C. Put flat files into Google Cloud Storage**
- Cloud Storage is excellent for storing **raw data** but does **not provide a SQL interface** for analytics.  
- Analysts would need an additional tool like BigQuery or Dataproc to query the data.

### **D. Stream data into Google Cloud Datastore**
- Cloud Datastore (now Firestore in Datastore mode) is a **NoSQL document database** designed for operational workloads.  
- It is **not optimized for large-scale analytics** or SQL-based querying.

---

## ✅ Final Answer

**A. Load data into Google BigQuery**

---

# 3. Google Cloud Case Study — Recommended Practices for Migrating a J2EE Application

## Question

The operations manager asks you for a list of recommended practices that she should consider when migrating a J2EE application to the cloud.  

Which three practices should you recommend? (Choose three.)  

A. Port the application code to run on Google App Engine  
B. Integrate Cloud Dataflow into the application to capture real-time metrics  
C. Instrument the application with a monitoring tool like Stackdriver Debugger  
D. Select an automation framework to reliably provision the cloud infrastructure  
E. Deploy a continuous integration tool with automated testing in a staging environment  
F. Migrate from MySQL to a managed NoSQL database like Google Cloud Datastore or Bigtable  

---

## ✅ **Correct Answers: C, D, E**

---

## Explanation

### **C. Instrument the application with a monitoring tool like Stackdriver Debugger**
- Monitoring and debugging tools like **Cloud Monitoring, Cloud Logging, and Stackdriver Debugger** provide insight into application performance and errors.  
- Instrumenting the application helps ensure **visibility, reliability, and proactive issue resolution** in the cloud.

### **D. Select an automation framework to reliably provision the cloud infrastructure**
- Using **Infrastructure as Code (IaC) tools** such as Terraform, Deployment Manager, or Ansible ensures that infrastructure is **consistently provisioned, version-controlled, and reproducible**.  
- Automation reduces human error and accelerates migration and scaling processes.

### **E. Deploy a continuous integration tool with automated testing in a staging environment**
- Setting up a **CI/CD pipeline** with automated testing ensures that code changes are **tested, validated, and safely deployed**.  
- This is a **cloud best practice** to maintain application stability and reliability during migration and afterward.

---

## ❌ Why the other options are not correct

### **A. Port the application code to run on Google App Engine**
- While App Engine can host J2EE applications, **porting is optional** and not always necessary.  
- Many applications can be lifted-and-shifted to **Compute Engine or GKE** without major code changes.

### **B. Integrate Cloud Dataflow into the application to capture real-time metrics**
- Cloud Dataflow is for **streaming or batch data processing**, not application monitoring.  
- Using monitoring and logging tools is the correct approach for tracking application performance.

### **F. Migrate from MySQL to a managed NoSQL database like Google Cloud Datastore or Bigtable**
- Migrating to NoSQL is **not a mandatory best practice**.  
- Many J2EE applications can continue using **managed relational databases like Cloud SQL** without modification.

---

## ✅ Final Answers

**C. Instrument the application with a monitoring tool like Stackdriver Debugger**  
**D. Select an automation framework to reliably provision the cloud infrastructure**  
**E. Deploy a continuous integration tool with automated testing in a staging environment**

---

# 4. Google Cloud Case Study — Session Management in App Engine

## Question

A news feed web service has the following code running on Google App Engine. During peak load, users report that they can see news articles they already viewed.  

What is the most likely cause of this problem?  

A. The session variable is local to just a single instance  
B. The session variable is being overwritten in Cloud Datastore  
C. The URL of the API needs to be modified to prevent caching  
D. The HTTP Expires header needs to be set to -1 stop caching  

---

## ✅ **Correct Answer: A**

---

## Explanation

### **A. The session variable is local to just a single instance**
- In **Google App Engine**, the standard environment can **scale horizontally**, creating multiple instances of the application during peak load.  
- If a **session variable is stored in memory on a single instance**, it **won’t be shared across other instances**.  
- Users routed to a different instance may **see stale or incorrect session data**, leading to them seeing news articles they already viewed.  
- To fix this, **use a shared session store** like **Memorystore, Cloud Datastore, or Firestore** instead of local instance memory.

---

## ❌ Why the other options are not correct

### **B. The session variable is being overwritten in Cloud Datastore**
- Overwriting in Datastore would affect **all users**, not just cause inconsistency under peak load.  
- The problem described is **instance-specific**, so local session storage is more likely the cause.

### **C. The URL of the API needs to be modified to prevent caching**
- URL modification prevents **browser or CDN caching**, but the issue is **server-side session state**, not caching of API responses.

### **D. The HTTP Expires header needs to be set to -1 stop caching**
- Changing the Expires header only affects **browser caching**, not **App Engine instance memory session variables**.  
- The problem occurs **even without caching headers**, because it’s due to instance-local memory.

---

## ✅ Final Answer

**A. The session variable is local to just a single instance**

---

# 5. Google Cloud Case Study — Selecting a Logging Solution

## Question

An application development team believes their current logging tool will not meet their needs for their new cloud-based product. They want a better tool to capture errors and help them analyze their historical log data. You want to help them find a solution that meets their needs.  

What should you do?  

A. Direct them to download and install the Google StackDriver logging agent  
B. Send them a list of online resources about logging best practices  
C. Help them define their requirements and assess viable logging tools  
D. Help them upgrade their current tool to take advantage of any new features  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Help them define their requirements and assess viable logging tools**
- Before selecting a logging solution, it is essential to **understand the team's requirements**, such as:  
  - Error capturing and alerting  
  - Historical log analysis  
  - Integration with other tools  
  - Scalability and retention  
- Once requirements are clear, you can **evaluate and select the most suitable tool**, e.g., **Google Cloud Logging (Stackdriver), BigQuery for log analysis, or third-party logging solutions**.  
- This approach ensures the solution **matches the business and technical needs**, rather than blindly implementing a tool.

---

## ❌ Why the other options are not correct

### **A. Direct them to download and install the Google StackDriver logging agent**
- Installing a tool without assessing requirements may **not meet the team’s needs**.  
- They may require features that the agent alone does not provide.

### **B. Send them a list of online resources about logging best practices**
- Resources provide guidance but **do not solve the team’s immediate problem** or address their specific requirements.

### **D. Help them upgrade their current tool to take advantage of any new features**
- Upgrading the current tool may still **not meet requirements** for error capturing or historical log analysis.  
- A proper assessment is needed before deciding whether to upgrade or switch.

---

## ✅ Final Answer

**C. Help them define their requirements and assess viable logging tools**

---

# 6. Google Cloud Case Study — Reducing Production Deployment Rollbacks

## Question

You need to reduce the number of unplanned rollbacks of erroneous production deployments in your company's web hosting platform. Improvements to the QA/Test processes accomplished an 80% reduction.  

Which additional two approaches can you take to further reduce the rollbacks? (Choose two.)  

A. Introduce a green-blue deployment model  
B. Replace the QA environment with canary releases  
C. Fragment the monolithic platform into microservices  
D. Reduce the platform's dependency on relational database systems  
E. Replace the platform's relational database systems with a NoSQL database  

---

## ✅ **Correct Answers: A and C**

---

## Explanation

### **A. Introduce a green-blue deployment model**
- **Blue-green deployments** reduce the risk of faulty deployments by maintaining **two identical production environments**.  
- One environment serves live traffic while the other is updated with the new release.  
- Traffic is switched to the new version **only after validation**, minimizing the chance of rollbacks.

### **C. Fragment the monolithic platform into microservices**
- Breaking a **monolithic application into microservices** isolates failures to individual services.  
- This reduces the impact of erroneous deployments because **issues in one service do not require rolling back the entire platform**.  
- Microservices also enable **smaller, safer, and more frequent deployments**, improving overall reliability.

---

## ❌ Why the other options are not correct

### **B. Replace the QA environment with canary releases**
- Canary releases **complement QA**, but do not replace it.  
- Skipping QA entirely increases risk, which can **increase rollbacks**, not reduce them.

### **D. Reduce the platform's dependency on relational database systems**
- Dependency on relational databases is not a direct cause of rollbacks.  
- Changing dependencies may improve scalability but does **not directly reduce deployment errors**.

### **E. Replace the platform's relational database systems with a NoSQL database**
- Switching to NoSQL may help with scaling certain workloads, but it **does not address deployment safety** or rollback risk.  
- This is **not a practical approach** to reduce production rollbacks.

---

## ✅ Final Answers

**A. Introduce a green-blue deployment model**  
**C. Fragment the monolithic platform into microservices**

---

# 7. Google Cloud Case Study — Cost-Effective Development Environment Design

## Question

To reduce costs, the Director of Engineering has required all developers to move their development infrastructure resources from on-premises virtual machines (VMs) to Google Cloud Platform. These resources go through multiple start/stop events during the day and require state to persist. You have been asked to design the process of running a development environment in Google Cloud while providing cost visibility to the finance department.  

Which two steps should you take? (Choose two.)  

A. Use the --no-auto-delete flag on all persistent disks and stop the VM  
B. Use the --auto-delete flag on all persistent disks and terminate the VM  
C. Apply VM CPU utilization label and include it in the BigQuery billing export  
D. Use Google BigQuery billing export and labels to associate cost to groups  
E. Store all state into local SSD, snapshot the persistent disks, and terminate the VM  
F. Store all state in Google Cloud Storage, snapshot the persistent disks, and terminate the VM  

---

## ✅ **Correct Answers: A and D**

---

## Explanation

### **A. Use the --no-auto-delete flag on all persistent disks and stop the VM**
- Using **persistent disks with `--no-auto-delete`** ensures that **data persists** even when the VM is stopped.  
- This allows developers to **start and stop VMs as needed** without losing state, reducing ongoing costs compared to running VMs 24/7.  
- Local SSDs or auto-deleted disks would **lose data** when the VM is stopped or terminated, which is not suitable for persistent state.

### **D. Use Google BigQuery billing export and labels to associate cost to groups**
- Applying **labels to VMs** (e.g., by developer, team, or project) and exporting **billing data to BigQuery** allows the finance department to **analyze and attribute costs** effectively.  
- This provides transparency for **resource usage and cost control** while supporting multiple start/stop events.

---

## ❌ Why the other options are not correct

### **B. Use the --auto-delete flag on all persistent disks and terminate the VM**
- Auto-deleting disks **removes the persistent state**, which violates the requirement to **retain state**.

### **C. Apply VM CPU utilization label and include it in the BigQuery billing export**
- Labels should track **ownership or cost center**, not metrics like CPU utilization.  
- CPU metrics help with performance monitoring, but not with **cost attribution**.

### **E. Store all state into local SSD, snapshot the persistent disks, and terminate the VM**
- **Local SSDs are ephemeral**; they cannot store persistent data reliably. Snapshots could work but are more complex and slower for frequent start/stop cycles.

### **F. Store all state in Google Cloud Storage, snapshot the persistent disks, and terminate the VM**
- Cloud Storage could store state, but persistent disks are simpler for running VMs directly.  
- The combination of Cloud Storage plus snapshots is **more complex** than using persistent disks with `--no-auto-delete`.

---

## ✅ Final Answers

**A. Use the --no-auto-delete flag on all persistent disks and stop the VM**  
**D. Use Google BigQuery billing export and labels to associate cost to groups**

---

# 8. Google Cloud Case Study — Selecting a Database for Sensor Data

## Question

Your company wants to track whether someone is present in a meeting room reserved for a scheduled meeting. There are 1000 meeting rooms across 5 offices on 3 continents. Each room is equipped with a motion sensor that reports its status every second. The data from the motion detector includes only a sensor ID and several different discrete items of information. Analysts will use this data, together with information about account owners and office locations.  

Which database type should you use?  

A. Flat file  
B. NoSQL  
C. Relational  
D. Blobstore  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. NoSQL**
- The sensor data is **high-velocity, semi-structured, and time-series in nature**, arriving every second from 1000 rooms across multiple locations.  
- **NoSQL databases** (like **Cloud Bigtable or Firestore**) are designed to handle:
  - **High write throughput** for frequent updates from sensors.  
  - **Scalability** across multiple regions and continents.  
  - Flexibility to **store discrete sensor readings** without rigid schema constraints.  
- Analysts can join this data later with relational datasets (e.g., account owners, office locations) for analysis.

---

## ❌ Why the other options are not correct

### **A. Flat file**
- Flat files cannot handle **real-time ingestion** efficiently for high-frequency sensor data.  
- They lack query flexibility and scaling.

### **C. Relational**
- Relational databases are good for structured, transactional data, but may struggle with:
  - **High ingestion rates** (1 reading per second × 1000 sensors × multiple offices)  
  - **Global scaling** across continents without complex sharding

### **D. Blobstore**
- Blobstore is meant for **large unstructured objects** like images or videos.  
- It is **not suitable for high-frequency sensor data** or for analytics queries.

---

## ✅ Final Answer

**B. NoSQL**

---

# 9. Google Cloud Case Study — Autoscaling Instance Group and Load Balancer Health Checks

## Question

You set up an autoscaling instance group to serve web traffic for an upcoming launch. After configuring the instance group as a backend service to an HTTP(S) load balancer, you notice that virtual machine (VM) instances are being terminated and re-launched every minute. The instances do not have a public IP address.  

You have verified the appropriate web response is coming from each instance using the curl command. You want to ensure the backend is configured correctly.  

What should you do?  

A. Ensure that a firewall rule exists to allow source traffic on HTTP/HTTPS to reach the load balancer.  
B. Assign a public IP to each instance and configure a firewall rule to allow the load balancer to reach the instance public IP.  
C. Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group.  
D. Create a tag on each instance with the name of the load balancer. Configure a firewall rule with the name of the load balancer as the source and the instance tag as the destination.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group**
- The behavior of VM instances being **terminated and re-launched every minute** is caused by **failed health checks**.  
- Google Cloud HTTP(S) load balancers rely on **health checks** to determine if a backend VM is healthy.  
- If **health checks cannot reach the instance** due to firewall restrictions, the load balancer marks it unhealthy, triggering **autoscaler to replace the instance**.  
- To fix this:
  - Configure a **firewall rule allowing traffic from the load balancer's health check IP ranges** to the backend instances on the health check port (usually HTTP/HTTPS).

---

## ❌ Why the other options are not correct

### **A. Ensure that a firewall rule exists to allow source traffic on HTTP/HTTPS to reach the load balancer**
- This allows **client traffic** to reach the load balancer, but the issue is **health check traffic**, not user traffic.

### **B. Assign a public IP to each instance and configure a firewall rule to allow the load balancer to reach the instance public IP**
- Backend VMs **do not require public IPs** to serve traffic behind a Google Cloud load balancer.  
- Load balancer traffic is **routed through Google’s internal network**; public IPs are unnecessary.

### **D. Create a tag on each instance with the name of the load balancer. Configure a firewall rule with the name of the load balancer as the source and the instance tag as the destination**
- While tagging can organize firewall rules, the key issue is **allowing health check traffic**, which is correctly handled by a specific firewall rule.  
- Adding tags alone does not fix misconfigured health check access.

---

## ✅ Final Answer

**C. Ensure that a firewall rule exists to allow load balancer health checks to reach the instances in the instance group**

---

# 10. Google Cloud Case Study — BigQuery Access from Compute Engine

## Question

You write a Python script to connect to Google BigQuery from a Google Compute Engine virtual machine. The script is printing errors that it cannot connect to BigQuery.  

What should you do to fix the script?  

A. Install the latest BigQuery API client library for Python  
B. Run your script on a new virtual machine with the BigQuery access scope enabled  
C. Create a new service account with BigQuery access and execute your script with that user  
D. Install the bq component for gcloud with the command gcloud components install bq  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Create a new service account with BigQuery access and execute your script with that user**
- **Compute Engine VMs use service accounts** to authenticate with Google Cloud APIs.  
- If the VM's service account **does not have BigQuery permissions**, Python scripts will fail to connect.  
- The correct approach:
  - Create a **service account with the `BigQuery User` role** (or more specific permissions).  
  - Assign this service account to the VM (or use a **JSON key file** to authenticate).  
- This ensures your Python script can **authenticate and access BigQuery** securely.

---

## ❌ Why the other options are not correct

### **A. Install the latest BigQuery API client library for Python**
- Installing the client library is **necessary** but not sufficient.  
- The error is due to **authentication/permissions**, not missing libraries.

### **B. Run your script on a new virtual machine with the BigQuery access scope enabled**
- Access scopes are **deprecated** in favor of **service account IAM roles**.  
- Using a new VM does not guarantee the VM has proper service account permissions.

### **D. Install the bq component for gcloud with the command gcloud components install bq**
- The `bq` command-line tool is **not required** for Python scripts.  
- Python scripts use the **BigQuery client library**, not the CLI.

---

## ✅ Final Answer

**C. Create a new service account with BigQuery access and execute your script with that user**
