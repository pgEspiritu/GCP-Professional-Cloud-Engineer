# 1. Inspecting Logs for Restarting Pods in Google Kubernetes Engine (GKE)

## Question

You are running a **cluster on Google Kubernetes Engine (GKE)** to serve a web application. Users are reporting that a **specific part of the application is not responding**.  

You notice that **all pods of your deployment keep restarting after 2 seconds**. The application writes logs to **standard output**. You want to inspect the logs to find the cause of the issue.  

Which approach can you take?

A. Review the **Stackdriver logs** for each Compute Engine instance that is serving as a node in the cluster.  

B. Review the **Stackdriver logs** for the **specific GKE container** that is serving the unresponsive part of the application.  

C. Connect to the cluster using **gcloud credentials** and connect to a container in one of the pods to read the logs.  

D. Review the **Serial Port logs** for each Compute Engine instance that is serving as a node in the cluster.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **GKE automatically streams container stdout and stderr to Google Cloud Logging (formerly Stackdriver)**.  
- You can view logs for a **specific container or pod** directly in the **Cloud Console**:
  - Navigate to **Kubernetes Engine → Workloads → Select Deployment → View Logs**
- This allows you to **investigate application-specific errors** causing pod restarts.
- It is the **recommended approach** for debugging GKE workloads.

---

### Why the other options are incorrect

#### **A. Stackdriver logs for the nodes**
❌ Incorrect  
- Logs for the underlying **Compute Engine nodes** include system-level information (kubelet, system logs)  
- They **do not provide detailed container stdout logs**, so application errors may not be visible.

#### **C. Connect to the container directly**
❌ Incorrect  
- If pods **restart every 2 seconds**, you may **not have enough time** to connect to the container.  
- Logs from Cloud Logging are **persistent** and more reliable in this scenario.

#### **D. Serial Port logs for nodes**
❌ Incorrect  
- Serial port logs capture **VM boot and system-level messages**  
- They do **not contain application container logs**.

---

## ✅ Final Answer

**B. Review the Stackdriver logs for the specific GKE container that is serving the unresponsive part of the application**

---

# 2. Configuring High Availability for Cloud SQL

## Question

You are using a single **Cloud SQL instance** to serve your application from a specific zone. You want to introduce **high availability (HA)**.  

What should you do?

A. Create a **read replica** instance in a **different region**  

B. Create a **failover replica** instance in a **different region**  

C. Create a **read replica** instance in the **same region**, but in a **different zone**  

D. Create a **failover replica** instance in the **same region**, but in a **different zone**  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct
- **High availability** in Cloud SQL is achieved by creating a **failover replica**.  
- The failover replica is **kept in a different zone** within the **same region**.  
- Features of HA Cloud SQL:
  - Automatic **failover** if the primary instance becomes unavailable
  - Synchronous replication between primary and failover instance
  - **No application downtime** during zone failures
- This setup **does not require a separate region**, which helps reduce latency and cost.

---

### Why the other options are incorrect

#### **A. Read replica in a different region**
❌ Incorrect  
- Read replicas are intended for **scaling read operations**, not HA.  
- Cross-region read replicas **do not provide automatic failover**.

#### **B. Failover replica in a different region**
❌ Incorrect  
- Cloud SQL **failover replicas must be in the same region** as the primary instance.  
- Cross-region replicas cannot be used for HA failover.

#### **C. Read replica in the same region but different zone**
❌ Incorrect  
- Read replicas **do not support automatic failover**  
- They are **asynchronous** and primarily used to **scale read traffic**, not HA.

---

## ✅ Final Answer

**D. Create a failover replica instance in the same region, but in a different zone**

---

# 3. Autoscaling a Compute Engine Application for Peak Hours

## Question

Your company is running a **stateless application on a Compute Engine instance**. The application is used heavily during **regular business hours** and lightly outside of business hours. Users report that the application is **slow during peak hours**.  

You need to **optimize the application's performance**.  

Which approach should you take?

A. Create a **snapshot** of the existing disk. Create an **instance template** from the snapshot. Create an **autoscaled managed instance group** from the instance template.  

B. Create a **snapshot** of the existing disk. Create a **custom image** from the snapshot. Create an **autoscaled managed instance group** from the custom image.  

C. Create a **custom image** from the existing disk. Create an **instance template** from the custom image. Create an **autoscaled managed instance group** from the instance template.  

D. Create an **instance template** from the existing disk. Create a **custom image** from the instance template. Create an **autoscaled managed instance group** from the custom image.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **Stateless applications** can benefit from **autoscaling managed instance groups (MIGs)**.  
- Steps to enable autoscaling for an existing instance:
  1. **Create a custom image** from the existing disk (captures the OS, app, and configuration).  
  2. **Create an instance template** from the custom image.  
     - Instance templates define the VM configuration for the MIG.  
  3. **Create an autoscaled managed instance group** using the instance template.  
     - Allows **automatic scaling up during peak hours** and **scaling down during low usage**.  
- This approach **optimizes performance without manual intervention**.

---

### Why the other options are incorrect

#### **A. Snapshot → instance template → MIG**
❌ Incorrect  
- You **cannot create an instance template directly from a snapshot**.  
- Snapshots are used to create disks or images, not templates.

#### **B. Snapshot → custom image → MIG**
❌ Incorrect  
- While **custom image creation is correct**, starting from a snapshot is **unnecessary** if the instance exists.  
- Directly creating a **custom image from the existing disk** is simpler and recommended.

#### **D. Instance template → custom image → MIG**
❌ Incorrect  
- **Instance templates cannot be created first** from a disk that is not an image.  
- The order is incorrect: you must **create a custom image first**, then create an instance template from it.

---

## ✅ Final Answer

**C. Create a custom image from the existing disk. Create an instance template from the custom image. Create an autoscaled managed instance group from the instance template.**

---

# 4. Restricting Communication Between Autoscaling VMs in a VPC

## Question

Your web application has several VM instances running within a **VPC**. You want to **restrict communications between instances** to only the paths and ports you authorize.  

You **do not want to rely on static IP addresses or subnets** because the application can **autoscale**.  

Which approach should you take?

A. Use separate VPCs to restrict traffic  

B. Use **firewall rules based on network tags** attached to the compute instances  

C. Use Cloud DNS and only allow connections from authorized hostnames  

D. Use service accounts and configure the web application to authorize particular service accounts to have access  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Google Cloud firewall rules** can be configured using **network tags**.  
- Steps:
  1. **Assign network tags** to your VMs based on role or function (e.g., `web`, `api`, `db`).  
  2. **Create firewall rules** that allow or deny traffic **based on tags and ports**.  
- **Benefits**:
  - Works for **autoscaling groups** since new instances can inherit tags automatically.  
  - Does **not require static IPs or subnets**.  
  - Provides **fine-grained control** over allowed traffic between VM groups.

---

### Why the other options are incorrect

#### **A. Use separate VPCs**
❌ Incorrect  
- VPCs **isolate networks**, but managing multiple VPCs for autoscaling instances is **complex and unnecessary**.  
- Firewall rules within a single VPC are sufficient.

#### **C. Use Cloud DNS and hostnames**
❌ Incorrect  
- DNS cannot **enforce network-level access control**.  
- Hostnames do not guarantee security because **IP connections are still unrestricted**.

#### **D. Use service accounts**
❌ Incorrect  
- Service accounts control **API-level access**, not **network-level traffic**.  
- They cannot restrict TCP/UDP traffic between VMs.

---

## ✅ Final Answer

**B. Use firewall rules based on network tags attached to the compute instances**

---

# 5. Scaling and Monitoring Cloud SQL for a Large CRM Deployment

## Question

You are using **Cloud SQL** as the database backend for a large CRM deployment. You want to:

- Scale as usage increases  
- Ensure you **don’t run out of storage**  
- Maintain **≤ 75% CPU usage**  
- Keep **replication lag below 60 seconds**  

Which steps should you take?

A. 1. Enable automatic storage increase for the instance. 2. Create a Stackdriver alert when CPU usage exceeds 75%, and change the instance type to reduce CPU usage. 3. Create a Stackdriver alert for replication lag, and shard the database to reduce replication time.  

B. 1. Enable automatic storage increase for the instance. 2. Change the instance type to a 32-core machine type to keep CPU usage below 75%. 3. Create a Stackdriver alert for replication lag, and deploy memcache to reduce load on the master.  

C. 1. Create a Stackdriver alert when storage exceeds 75%, and increase the available storage on the instance to create more space. 2. Deploy memcached to reduce CPU load. 3. Change the instance type to a 32-core machine type to reduce replication lag.  

D. 1. Create a Stackdriver alert when storage exceeds 75%, and increase the available storage on the instance to create more space. 2. Deploy memcached to reduce CPU load. 3. Create a Stackdriver alert for replication lag, and change the instance type to a 32-core machine type to reduce replication lag.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Step 1: Enable automatic storage increase**
  - Ensures the database **does not run out of storage** as usage grows.
  - Cloud SQL can automatically increase storage when needed.
- **Step 2: Change instance type to a 32-core machine**
  - Increasing CPU cores helps **keep utilization below 75%** during peak load.
  - Selecting a sufficiently powerful machine type prevents CPU bottlenecks.
- **Step 3: Stackdriver alert for replication lag**
  - Monitoring replication lag ensures **lag stays under 60 seconds**.
  - Cloud SQL alerts can notify you if the primary is falling behind.

**Notes:**
- **Memcached** may reduce database load, but CPU and replication lag are better addressed by **scaling instance resources** rather than caching alone.
- **Sharding** is complex and usually a last-resort measure.
- **Alerts** are essential for proactive monitoring, but **automatic scaling and right-sizing** are primary steps.

---

### Why the other options are incorrect

#### **A. Use CPU alerts and shard the database**
❌ Incorrect  
- Sharding is unnecessary for typical scaling; changing instance type is more practical.  

#### **C & D. Use memcached**
❌ Incorrect  
- Memcached reduces database read load but does **not directly address replication lag**.
- Step order may lead to reactive scaling rather than proactive resource management.

---

## ✅ Final Answer

**B. Enable automatic storage increase, scale instance type to 32 cores, and create Stackdriver alert for replication lag**

---

# 6. OLAP Marketing Analytics and Reporting on Google Cloud

## Question

You are tasked with building an **online analytical processing (OLAP)** marketing analytics and reporting tool. This requires a **relational database** that can operate on **hundreds of terabytes of data**.

What is the **Google-recommended tool** for such applications?

A. Cloud Spanner, because it is globally distributed  
B. Cloud SQL, because it is a fully managed relational database  
C. Cloud Firestore, because it offers real-time synchronization across devices  
D. BigQuery, because it is designed for large-scale processing of tabular data  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D. BigQuery** is correct
- **BigQuery** is Google’s **fully managed, serverless data warehouse**
- It is specifically designed for:
  - **OLAP workloads**
  - **Hundreds of terabytes to petabytes of data**
  - Complex analytical queries using **SQL**
- Key advantages:
  - Columnar storage optimized for analytics
  - Massively parallel query execution
  - No infrastructure management
  - Built-in scalability and high performance

👉 BigQuery is the **standard Google-recommended solution** for large-scale analytics and reporting.

---

## ❌ Why the other options are incorrect

### **A. Cloud Spanner**
❌ Incorrect  
- Designed for **OLTP**, not OLAP
- Optimized for transactional workloads with strong consistency, not large-scale analytics

---

### **B. Cloud SQL**
❌ Incorrect  
- Suitable for small to medium relational databases
- **Not designed to scale to hundreds of terabytes**
- Performance degrades for analytical workloads

---

### **C. Cloud Firestore**
❌ Incorrect  
- NoSQL document database
- Optimized for real-time application data, **not analytical queries**

---

## ✅ Final Answer

**D. BigQuery, because it is designed for large-scale processing of tabular data**

---

# 7. Authenticating Application VMs to Cloud Pub/Sub

## Question

Your company pushes batches of **sensitive transaction data** from its **application server VMs** to **Cloud Pub/Sub** for processing and storage.  
What is the **Google-recommended** way for your application to authenticate to the required Google Cloud services?

A. Ensure that VM service accounts are granted the appropriate Cloud Pub/Sub IAM roles.  
B. Ensure that VM service accounts do not have access to Cloud Pub/Sub, and use VM access scopes to grant the appropriate Cloud Pub/Sub IAM roles.  
C. Generate an OAuth2 access token for accessing Cloud Pub/Sub, encrypt it, and store it in Cloud Storage for access from each VM.  
D. Create a gateway to Cloud Pub/Sub using a Cloud Function, and grant the Cloud Function service account the appropriate Cloud Pub/Sub IAM roles.

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- **Service accounts** are the **recommended and secure authentication mechanism** for applications running on Compute Engine
- By attaching a **service account** to the VM and granting it the appropriate **Cloud Pub/Sub IAM roles** (for example, `roles/pubsub.publisher`), the application:
  - Avoids hard-coded credentials
  - Uses short-lived, automatically rotated credentials
  - Follows the **principle of least privilege**
- Authentication is handled transparently by Google Cloud libraries using **Application Default Credentials (ADC)**

👉 This is the **standard, secure, and scalable approach** recommended by Google.

---

## ❌ Why the other options are incorrect

### **B. Use access scopes instead of IAM**
❌ Incorrect  
- **Access scopes do not grant permissions by themselves**
- IAM roles are still required
- Google recommends **IAM-first**, not scope-based access control

---

### **C. Generate and store OAuth tokens**
❌ Incorrect  
- Storing tokens is insecure and operationally complex
- Tokens expire and require manual rotation
- Violates Google security best practices

---

### **D. Use a Cloud Function as a gateway**
❌ Incorrect  
- Adds unnecessary complexity and latency
- Does not improve security compared to using VM service accounts
- Not required for standard Pub/Sub publishing

---

## ✅ Final Answer

**A. Ensure that VM service accounts are granted the appropriate Cloud Pub/Sub IAM roles**

---

# 8. Cloud VPN Design for a Multi-Region VPC

## Question

You want to establish a **Compute Engine application in a single VPC across two regions**.  
The application must **communicate over VPN to an on-premises network**.

How should you deploy the VPN?

A. Use VPC Network Peering between the VPC and the on-premises network.  
B. Expose the VPC to the on-premises network using IAM and VPC Sharing.  
C. Create a global Cloud VPN Gateway with VPN tunnels from each region to the on-premises peer gateway.  
D. Deploy Cloud VPN Gateway in each region. Ensure that each region has at least one VPN tunnel to the on-premises peer gateway.

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct
- **Cloud VPN gateways are regional resources**
- When your application spans **multiple regions**, you must:
  - Deploy a **Cloud VPN gateway in each region**
  - Create **at least one VPN tunnel per region** to the on-premises VPN gateway
- This design:
  - Ensures **regional resiliency**
  - Allows workloads in each region to access on-premises systems
  - Follows **Google-recommended best practices** for hybrid networking

👉 Each region operates independently, so each requires its **own VPN gateway and tunnel**.

---

## ❌ Why the other options are incorrect

### **A. VPC Network Peering**
❌ Incorrect  
- VPC Peering works **only between Google Cloud VPCs**
- It **cannot connect** to on-premises networks

---

### **B. IAM and VPC Sharing**
❌ Incorrect  
- IAM and VPC Sharing manage **access and project relationships**
- They do **not provide network connectivity**

---

### **C. Global Cloud VPN Gateway**
❌ Incorrect  
- **Cloud VPN gateways are not global**
- They are strictly **regional resources**

---

## ✅ Final Answer

**D. Deploy Cloud VPN Gateway in each region. Ensure that each region has at least one VPN tunnel to the on-premises peer gateway**

---

# 9. BigQuery Log Retention and Storage Optimization

## Question

Your applications will be writing their logs to **BigQuery** for analysis.  
Each application should have its **own table**.  
Any logs **older than 45 days must be removed**.

You want to **optimize storage** and **follow Google-recommended practices**.

What should you do?

A. Configure the expiration time for your tables at 45 days  
B. Make the tables time-partitioned, and configure the partition expiration at 45 days  
C. Rely on BigQuery's default behavior to prune application logs older than 45 days  
D. Create a script that uses the BigQuery command line tool (bq) to remove records older than 45 days  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Time-partitioned tables** are the **recommended approach** for log and event data in BigQuery
- Setting a **partition expiration**:
  - Automatically deletes data older than **45 days**
  - Reduces storage costs
  - Avoids manual cleanup jobs
- Improves:
  - Query performance (scans only relevant partitions)
  - Cost control
  - Operational simplicity

👉 This approach is **automatic, scalable, and cost-efficient**, aligning with Google best practices.

---

## ❌ Why the other options are incorrect

### **A. Table expiration**
❌ Suboptimal  
- Deletes the **entire table**, not just old data
- Not suitable for continuously written log tables

---

### **C. Default BigQuery behavior**
❌ Incorrect  
- BigQuery **does not automatically delete data**
- Data is retained indefinitely unless configured otherwise

---

### **D. Manual deletion with scripts**
❌ Not recommended  
- Requires ongoing maintenance
- Error-prone and inefficient
- Increases operational overhead

---

## ✅ Final Answer

**B. Make the tables time-partitioned, and configure the partition expiration at 45 days**

---

# 10. Google Kubernetes Engine Node Autoscaling Based on CPU Load

## Question

You want your **Google Kubernetes Engine (GKE)** cluster to **automatically add or remove nodes based on CPU load**.

What should you do?

A. Configure a HorizontalPodAutoscaler with a target CPU usage. Enable the Cluster Autoscaler from the GCP Console.  
B. Configure a HorizontalPodAutoscaler with a target CPU usage. Enable autoscaling on the managed instance group for the cluster using the gcloud command.  
C. Create a deployment and set the maxUnavailable and maxSurge properties. Enable the Cluster Autoscaler using the gcloud command.  
D. Create a deployment and set the maxUnavailable and maxSurge properties. Enable autoscaling on the cluster managed instance group from the GCP Console.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
To scale **nodes automatically based on CPU load**, you need **two components working together**:

1. **Horizontal Pod Autoscaler (HPA)**
   - Scales **pods** based on CPU (or memory) utilization
   - Example: increases pods when CPU exceeds a threshold

2. **Cluster Autoscaler**
   - Scales **nodes** when:
     - Pods cannot be scheduled due to insufficient resources
     - Nodes are underutilized and can be removed

By:
- Configuring **HPA** → pod-level scaling
- Enabling **Cluster Autoscaler** → node-level scaling

👉 This is the **Google-recommended architecture** for workload-driven autoscaling in GKE.

---

## ❌ Why the other options are incorrect

### **B. Enable autoscaling on the managed instance group**
❌ Incorrect  
- GKE **does not recommend managing node scaling directly via MIG autoscaling**
- GKE Cluster Autoscaler must be used instead

---

### **C. Using maxUnavailable and maxSurge**
❌ Incorrect  
- These settings control **rolling updates**, not autoscaling
- They do **not** respond to CPU load

---

### **D. Autoscaling MIG + rolling update settings**
❌ Incorrect  
- MIG autoscaling is not workload-aware in GKE
- Rolling update properties are unrelated to scaling decisions

---

## ✅ Final Answer

**A. Configure a HorizontalPodAutoscaler with a target CPU usage. Enable the Cluster Autoscaler from the GCP Console.**
