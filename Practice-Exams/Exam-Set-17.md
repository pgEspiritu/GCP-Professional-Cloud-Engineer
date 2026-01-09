# 1. Optimizing CPU and Memory for Migrated Applications

## Question

You are migrating **third-party applications** from optimized **on-premises virtual machines** to **Google Cloud**.  
You are unsure about the **optimal CPU and memory configuration**.

The applications:
- Have a **consistent usage pattern** across multiple weeks
- Need to be optimized for the **lowest possible cost**

What should you do?

A. Create an instance template with the smallest available machine type, and use an image of the third-party application taken from a current on-premises virtual machine. Create a managed instance group that uses average CPU utilization to autoscale the number of instances in the group. Modify the average CPU utilization threshold to optimize the number of instances running.  

B. Create an App Engine flexible environment, and deploy the third-party application using a Dockerfile and a custom runtime. Set CPU and memory options similar to your application's current on-premises virtual machine in the `app.yaml` file.  

C. Create multiple Compute Engine instances with varying CPU and memory options. Install the Cloud Monitoring agent, and deploy the third-party application on each of them. Run a load test with high traffic levels on the application, and use the results to determine the optimal settings.  

D. Create a Compute Engine instance with CPU and memory options similar to your application's current on-premises virtual machine. Install the Cloud Monitoring agent, and deploy the third-party application. Run a load test with normal traffic levels on the application, and follow the **Rightsizing Recommendations** in the Cloud Console.  

---

## ✅ Correct Answer

**D. Create a Compute Engine instance with CPU and memory options similar to your application's current on-premises virtual machine. Install the Cloud Monitoring agent, and deploy the third-party application. Run a load test with normal traffic levels on the application, and follow the Rightsizing Recommendations in the Cloud Console.**

---

## Explanation

### Why **Option D** is correct ✅

- **Cloud Rightsizing Recommendations** analyze real workload usage over time
- Provides **data-driven CPU and memory optimization**
- Minimizes manual effort and operational overhead
- Optimizes **cost without sacrificing performance**
- Follows **Google-recommended best practices**

This approach works best when workloads are:
- Stable
- Predictable
- Long-running

---

## ❌ Why the other options are not correct

### **A. Smallest machine + autoscaling**
- Autoscaling adjusts instance count, not **machine sizing**
- Risks under-provisioning and performance issues
- Not ideal for stable workloads

---

### **B. App Engine flexible**
- Requires replatforming
- Higher cost and complexity
- Not suitable for third-party applications without changes

---

### **C. Multiple instances with load testing**
- High operational overhead
- Manual analysis required
- Synthetic tests may not reflect real usage patterns

---

## ✅ Final Answer

**D**

---

# 2. Preventing Data Exfiltration from BigQuery

## Question

Your company has a **Google Cloud project that uses BigQuery for data warehousing**.  
There is a **VPN tunnel** between the on-premises environment and Google Cloud using **Cloud VPN**.

The **security team wants to prevent data exfiltration** caused by:
- Malicious insiders  
- Compromised code  
- Accidental data oversharing  

What should they do?

A. Configure Private Google Access for on-premises only.  
B. Perform the following tasks:  
   1. Create a service account.  
   2. Grant the BigQuery JobUser role and Storage Reader role to the service account.  
   3. Remove all other IAM access from the project.  
C. Configure VPC Service Controls and configure Private Google Access.  
D. Configure Private Google Access.  

---

## ✅ Correct Answer

**C. Configure VPC Service Controls and configure Private Google Access**

---

## Explanation

### Why **VPC Service Controls** 🔐

- **VPC Service Controls** provide a **security perimeter** around Google-managed services like **BigQuery**
- They protect against:
  - Data exfiltration by insiders
  - Accidental sharing
  - Compromised credentials or workloads
- Even if a user has valid IAM permissions, access is **blocked outside the perimeter**

### Why **Private Google Access** 🌐

- Ensures BigQuery traffic stays on **Google’s private network**
- Prevents data access over the public internet
- Works together with VPC Service Controls to provide **defense in depth**

This combination is the **Google-recommended best practice** for protecting sensitive data in managed services.

---

## ❌ Why the other options are not correct

### **A. Private Google Access for on-premises only**
- Private Google Access **does not prevent data exfiltration**
- It only controls network routing, not service-level access

---

### **B. Service account with restricted IAM**
- IAM controls **who** can access resources
- Does **not protect against compromised credentials**
- Does not prevent data from being accessed outside trusted networks

---

### **D. Private Google Access**
- Network-level protection only
- Does not enforce **service perimeters**
- Insufficient against insider threats and accidental oversharing

---

## ✅ Final Answer

**C**

---

# 3. Deploying Medical Workloads on Sole-Tenant Nodes

## Question

You are working at an institution that processes **medical data**. You are migrating several workloads onto **Google Cloud**.

Company policies require:
- All workloads must run on **physically separated hardware**
- Workloads from **different clients must also be separated**

You created a **sole-tenant node group** and added **one node for each client**.  
You need to deploy the workloads on these dedicated hosts.

What should you do?

A. Add the node group name as a network tag when creating Compute Engine instances in order to host each workload on the correct node group.  
B. Add the node name as a network tag when creating Compute Engine instances in order to host each workload on the correct node.  
C. Use node affinity labels based on the node group name when creating Compute Engine instances in order to host each workload on the correct node group.  
D. Use node affinity labels based on the node name when creating Compute Engine instances in order to host each workload on the correct node.  

---

## ✅ Correct Answer

**D. Use node affinity labels based on the node name when creating Compute Engine instances in order to host each workload on the correct node**

---

## Explanation

### Why **Node Affinity (Node Name)** 🧷

- **Sole-tenant nodes** provide physical isolation
- Since **each client has its own dedicated node**, workloads must be pinned to a **specific node**, not just the node group
- **Node affinity labels** ensure a VM is scheduled on:
  - The **exact physical host** assigned to that client

Google Compute Engine automatically assigns default affinity labels:
- **Node group label**
  - `compute.googleapis.com/node-group-name`
- **Node name label**
  - `compute.googleapis.com/node-name`

To guarantee **one client per physical host**, you must use the **node name label**.

---

## ❌ Why the other options are not correct

### **A. Node group name as a network tag**
- Network tags are used for **firewall rules**
- They do **not control VM placement**
- Cannot enforce physical isolation

---

### **B. Node name as a network tag**
- Same issue as A
- Network tags have **no scheduling effect**

---

### **C. Node affinity based on node group name**
- This only guarantees placement **somewhere within the group**
- Does **not ensure isolation per client**
- Multiple workloads could land on the same node

---

## ✅ Final Answer

**D**

---

# 4. Speeding Up a C++ Test Suite Using Google Cloud

## Question

Your company’s **test suite is a custom C++ application** that:
- Runs throughout the day
- Executes on **Linux virtual machines**
- Takes **several hours** to complete
- Currently runs on a **limited number of on-premises servers**

The company wants to:
- Move the testing infrastructure to the **cloud**
- **Reduce total test execution time**
- **Change the tests as little as possible**

Which cloud infrastructure should you recommend?

A. Google Compute Engine unmanaged instance groups and Network Load Balancer  
B. Google Compute Engine managed instance groups with auto-scaling  
C. Google Cloud Dataproc to run Apache Hadoop jobs to process each test  
D. Google App Engine with Google Stackdriver for logging  

---

## ✅ Correct Answer

**B. Google Compute Engine managed instance groups with auto-scaling**

---

## Explanation

### Why **Managed Instance Groups (MIGs) with Auto-scaling** ⚙️

- Your tests already run on **Linux VMs**, so Compute Engine is a **natural lift-and-shift**
- **Managed Instance Groups** allow you to:
  - Run many identical VM instances in parallel
  - Automatically **scale out** when more tests need to run
  - **Reduce total execution time** by running tests concurrently
- Requires **minimal to no changes** to your existing C++ test code

This directly addresses:
- Faster test execution 🚀  
- Elastic scaling 🧩  
- Minimal refactoring 🛠️  

---

## ❌ Why the other options are not correct

### **A. Unmanaged instance groups + Network Load Balancer**
- Unmanaged groups **do not support auto-scaling**
- Requires manual VM lifecycle management
- Adds unnecessary operational overhead

---

### **C. Google Cloud Dataproc**
- Designed for **Hadoop/Spark big data workloads**
- Requires rewriting tests as distributed data-processing jobs
- Violates the requirement to **change tests as little as possible**

---

### **D. Google App Engine**
- Best suited for **web applications**
- Not ideal for long-running, CPU-intensive C++ binaries
- Limited control over OS-level configuration

---

## ✅ Final Answer

**B**

---

# 5. Supporting WebSockets and Non-Distributed HTTP Sessions on Google Cloud

## Question

A lead software engineer explains that the new application design:
- Uses **WebSockets**
- Uses **HTTP sessions that are not distributed** across web servers

You want to help ensure the application will **run properly on Google Cloud Platform**.

What should you do?

A. Help the engineer convert the WebSocket code to use HTTP streaming  
B. Review the encryption requirements for WebSocket connections with the security team  
C. Meet with the cloud operations team and the engineer to discuss load balancer options  
D. Help the engineer redesign the application to use a distributed user session service that does not rely on WebSockets and HTTP sessions  

---

## ✅ Correct Answer

**C. Meet with the cloud operations team and the engineer to discuss load balancer options**

---

## Explanation

### Why **Load Balancer Selection** matters ⚖️

- Applications using **WebSockets** and **non-distributed HTTP sessions** require:
  - **Session affinity (sticky sessions)**
  - Load balancers that explicitly **support WebSockets**
- Google Cloud provides multiple load balancers (HTTP(S), TCP, internal/external), each with different capabilities

By discussing load balancer options:
- You ensure WebSocket connections remain stable 🔌
- You preserve session affinity so users are routed to the same backend instance
- You **avoid unnecessary application redesign**

This approach aligns with the goal of **making the application work correctly on GCP with minimal changes**.

---

## ❌ Why the other options are not correct

### **A. Convert WebSockets to HTTP streaming**
- Requires **application-level changes**
- Not necessary, since Google Cloud load balancers support WebSockets

---

### **B. Review encryption requirements**
- Security is important, but **does not solve session affinity or WebSocket routing issues**
- Does not address application functionality

---

### **D. Redesign the application**
- Distributed sessions are a good long-term practice
- However, this **violates the intent of helping the existing design run properly**
- More invasive and unnecessary at this stage

---

## ✅ Final Answer

**C**

---

# 6. Minimizing Data Loss When Writing High-Volume Events to Cloud Storage

## Question

The application reliability team added a **debug feature** that sends **all server events** to **Google Cloud Storage** for later analysis.

**Event characteristics:**
- Size: **50 KB – 15 MB**
- Peak rate: **3,000 events per second**
- Goal: **Minimize data loss**

Which process should you implement?

A.  
- Append metadata to file body  
- Compress individual files  
- Name files with `serverName-Timestamp`  
- Create a new bucket if the bucket is older than 1 hour  

B.  
- Batch every 10,000 events with a manifest file  
- Compress events + manifest into one archive  
- Name files with `serverName-EventSequence`  
- Create a new bucket if the bucket is older than 1 day  

C.  
- Compress individual files  
- Name files with `serverName-EventSequence`  
- Save files to **one bucket**  
- Set custom metadata headers for each object after saving  

D.  
- Append metadata to file body  
- Compress individual files  
- Name files with a **random prefix pattern**  
- Save files to one bucket  

---

## ✅ Correct Answer

**C**

---

## Explanation

To **minimize data loss** at very high write rates, Google Cloud Storage best practices emphasize:

### ✅ Independent object writes
- Writing **one event per object** avoids cascading failures
- Failed uploads only affect individual events

### ✅ Deterministic, unique object names
- `serverName-EventSequence` guarantees:
  - No overwrites
  - Easy traceability
  - Correct ordering per server

### ✅ Single bucket design
- Cloud Storage scales automatically
- Creating or rotating buckets frequently increases operational risk
- Buckets are **not a throughput bottleneck**

### ✅ Compression
- Reduces storage cost and upload time
- Improves overall reliability under peak load

Even though setting metadata after upload causes an extra API call, it **does not increase the risk of losing event data**, making this option the safest overall.

---

## ❌ Why the other options are not correct

### **A. Frequent bucket creation**
- Buckets are heavyweight resources
- Rotating buckets hourly increases:
  - Configuration errors
  - Upload failures
- Timestamp-based naming risks collisions at high throughput

---

### **B. Large batch archives**
- Batching 10,000 events creates **large blast radius**
- If one upload fails, **thousands of events are lost**
- Increases memory pressure and retry complexity

---

### **D. Random prefix naming**
- Makes debugging and analysis difficult
- No event ordering or server correlation
- Metadata embedded in the body complicates downstream processing

---

## ✅ Final Answer

**C**

---

# 7. Identifying the Origin of a Newly Created Network with Open SSH Ports

## Question

A recent **audit revealed a new network** in your GCP project.  
In this network, a **Compute Engine instance has SSH (port 22) open to the world**.  

You want to **discover the origin of this network**.

A. Search for **Create VM** entry in the Stackdriver alerting console  
B. Navigate to the **Activity page** in the Home section. Set category to **Data Access** and search for **Create VM** entry  
C. In the **Logging** section of the console, specify **GCE Network** as the logging section. Search for the **Create Insert** entry  
D. Connect to the GCE instance using project SSH keys. Identify previous logins in system logs, and match these with the project owners list  

---

## ✅ Correct Answer

**B. Navigate to the Activity page in the Home section. Set category to Data Access and search for Create VM entry**

---

## Explanation

### Why **Activity page + Data Access logs** 🔍

- **Cloud Audit Logs** record all **administrative actions** on Google Cloud resources:
  - `Admin Activity` logs are **always enabled** and cannot be disabled  
  - `Data Access` logs are required for tracking **resource creation/modification**  

- To find the origin of the network and the VM:
  1. Go to the **Activity page** in the GCP console  
  2. Filter by **Data Access logs**  
  3. Search for **Create VM** or **Create Network** events  
- These logs show **who performed the action**, **when**, and **from which IP address**  

This is the **Google-recommended way** to trace resource creation and ensure accountability.

---

## ❌ Why the other options are not correct

### **A. Stackdriver alerting console**
- Stackdriver (Cloud Monitoring) alerts trigger **after events occur**, but they **do not track historical creation events**
- Cannot reliably discover who created the network

---

### **C. Logging section, GCE Network**
- GCE Network itself does not generate direct **creation logs**
- Admin Activity logs are required; searching only network logs may **miss the responsible actor**

---

### **D. SSH into instance**
- System logs only record **instance-level logins**
- Cannot reliably link **network creation** to a user account
- Violates principle of least privilege and auditing best practices

---

## ✅ Final Answer

**B**
---

# 8. Copying a Production Linux VM Across Regions and Projects

## Question

You want to make a **copy of a production Linux VM** in **US-Central**.  
Requirements:

- The copy should be **easy to manage and replace** if the production VM changes  
- The copy will be deployed as a **new instance in a different project in US-East**  

What steps should you take?

A. Use Linux `dd` and `netcat` commands to copy and stream the root disk contents to a new VM instance in US-East  
B. Create a **snapshot** of the root disk and select the snapshot as the root disk when creating a new VM instance in US-East  
C. Create an **image file** from the root disk with Linux `dd`, create a new VM in US-East  
D. Create a **snapshot** of the root disk, create an **image file in Cloud Storage** from the snapshot, and create a new VM instance in US-East using that image  

---

## ✅ Correct Answer

**D. Create a snapshot of the root disk, create an image file in Cloud Storage from the snapshot, and create a new VM instance in US-East using that image**

---

## Explanation

### Why **Snapshot → Image → New VM** 🖼️

1. **Snapshot the root disk**  
   - Captures a **point-in-time copy** of the VM disk  
   - Snapshots are incremental and efficient for ongoing changes  

2. **Create an image from the snapshot**  
   - Images are **project-independent** and can be shared across projects  
   - Can be stored in **Cloud Storage** to **cross regions**  

3. **Create a VM in the new project/region using the image**  
   - Ensures that the VM is a **faithful copy** of production  
   - Simplifies **future updates**: update the image and redeploy  

This approach follows **Google Cloud best practices** for VM migration across projects and regions while maintaining manageability.

---

## ❌ Why the other options are not correct

### **A. dd + netcat**
- Manual, error-prone, not scalable  
- Hard to manage updates or redeploy  

### **B. Snapshot directly as root disk**
- Snapshots are **regional** by default  
- Cannot use a snapshot directly **across projects/regions**  

### **C. Linux dd image**
- Requires manual handling of disk image files  
- More complex than using **Cloud snapshots and images**  
- Not integrated with Google Cloud tools for cross-project deployment  

---

## ✅ Final Answer

**D**
---

# 9. Efficient Database Backups Without Impacting Disk Performance

## Question

Your company runs **multiple databases on a single MySQL instance**.  

Requirements for backups:

- Only **specific databases** need to be backed up regularly  
- Backups must **complete quickly**  
- Backups **cannot impact disk performance**  

How should you configure storage?

A. Configure a cron job to use the `gcloud` tool to take regular backups using persistent disk snapshots  
B. Mount a **Local SSD** volume as the backup location. After the backup is complete, use `gsutil` to move the backup to Cloud Storage  
C. Use **gcsfuse** to mount a Cloud Storage bucket as a volume and write backups directly using `mysqldump`  
D. Mount additional **persistent disk volumes** on each VM in a **RAID10 array** and use **LVM snapshots** to send to Cloud Storage  

---

## ✅ Correct Answer

**D. Mount additional persistent disk volumes onto each VM instance in a RAID10 array and use LVM snapshots to send to Cloud Storage**

---

## Explanation

### Why **RAID10 + LVM snapshots** ⚡

- **RAID10 with additional persistent disks**:
  - Provides **high performance for backup writes**  
  - Reduces risk of disk I/O contention with the main MySQL data disk  

- **LVM snapshots**:
  - Allows **point-in-time copies** of the backup volume  
  - Minimizes **impact on running database performance**  
  - Snapshots can be **exported to Cloud Storage** asynchronously  

- This approach ensures **fast backups without slowing down the MySQL instance**  

---

## ❌ Why the other options are not correct

### **A. gcloud persistent disk snapshots**
- Snapshots capture **entire persistent disks**  
- Cannot efficiently backup a **single database**  
- Snapshotting the main disk may **impact disk performance**  

### **B. Local SSD + gsutil**
- Local SSDs are **ephemeral**: data is lost if VM is stopped or crashes  
- Writing directly to Local SSD can **impact disk performance**  
- Requires extra steps to move data to Cloud Storage  

### **C. gcsfuse + mysqldump**
- Writing directly to Cloud Storage through gcsfuse is **slow**  
- High latency for large databases  
- Can **significantly impact MySQL performance**  

---

## ✅ Final Answer

**D**
---

You are helping the QA team to roll out a new load-testing tool to test the scalability of your primary cloud services that run on Google Compute Engine with Cloud
Bigtable.
Which three requirements should they include? (Choose three.)

A. Ensure that the load tests validate the performance of Cloud Bigtable
B. Create a separate Google Cloud project to use for the load-testing environment
C. Schedule the load-testing tool to regularly run against the production environment
D. Ensure all third-party systems your services use is capable of handling high load
E. Instrument the production services to record every transaction for replay by the load-testing tool
F. Instrument the load-testing tool and the target services with detailed logging and metrics collection
