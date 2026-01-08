# 1. Designing a Flexible Data Lake for Unstructured Data

## Question

Your company is designing its **data lake on Google Cloud** and wants to build multiple **ingestion pipelines** to collect **unstructured data** from different sources.

The requirements are:

- Data structure from source systems can **change at any time**
- Data must be stored **exactly as retrieved** (raw format)
- Data must support **reprocessing** if pipelines become incompatible
- Data will later be processed to build a **recommendation engine**

What should you do **after retrieving the data**?

A. Send the data through the processing pipeline, and then store the processed data in a BigQuery table for reprocessing.  
B. Store the data in a BigQuery table. Design the processing pipelines to retrieve the data from the table.  
C. Send the data through the processing pipeline, and then store the processed data in a Cloud Storage bucket for reprocessing.  
D. Store the data in a Cloud Storage bucket. Design the processing pipelines to retrieve the data from the bucket.  

---

## ✅ Correct Answer

**D. Store the data in a Cloud Storage bucket. Design the processing pipelines to retrieve the data from the bucket.**

---

## Explanation

### Why **Cloud Storage** 🗂️

- Cloud Storage is ideal for **raw, unstructured, and semi-structured data**
- Supports **schema-on-read**, which is critical when data formats change
- Allows you to store data **exactly as ingested**, without transformation
- Enables **reprocessing** with new or updated pipelines
- Highly durable, scalable, and cost-effective for data lakes

This follows the **Google-recommended data lake pattern**:
- **Raw zone** → Cloud Storage  
- **Processing & analytics** → Dataflow / Dataproc / BigQuery  

---

## ❌ Why the other options are not correct

### **A. Process first, then store in BigQuery**
- Loses the **original raw data**
- Prevents reprocessing if schema changes or logic is updated

### **B. Store raw data directly in BigQuery**
- BigQuery requires a defined schema
- Poor fit for **rapidly changing or unstructured data**
- Higher cost and less flexibility for raw storage

### **C. Process first, then store in Cloud Storage**
- Still loses the **original raw representation**
- Violates the requirement to store data *exactly as retrieved*

---

## ✅ Final Answer

**D**

---

# 2. Structuring Google Cloud IAM for Multiple Departments

## Question

You are responsible for the **Google Cloud environment** in your company.  
Multiple departments need access to their **own projects**, and the members within each department share the **same project responsibilities**.

Your goals are:
- **Minimal maintenance**
- **Clear overview of IAM permissions**
- Easy onboarding/offboarding as **projects start and end**
- Follow **Google-recommended practices**

What should you do?

A. Grant all department members the required IAM permissions for their respective projects.  
B. Create a Google Group per department and add all department members to their respective groups. Create a folder per department and grant the respective group the required IAM permissions at the folder level. Add the projects under the respective folders.  
C. Create a folder per department and grant the respective members of the department the required IAM permissions at the folder level. Structure all projects for each department under the respective folders.  
D. Create a Google Group per department and add all department members to their respective groups. Grant each group the required IAM permissions for their respective projects.  

---

## ✅ Correct Answer

**B. Create a Google Group per department and add all department members to their respective groups. Create a folder per department and grant the respective group the required IAM permissions at the folder level. Add the projects under the respective folders.**

---

## Explanation

### Why this is the Google-recommended approach ✅

This solution combines **Google Groups** and **resource hierarchy (folders)**, which is the best practice for scalable IAM management:

- **Google Groups**:
  - Centralized user management
  - Easy onboarding and offboarding
  - No need to update IAM bindings per user

- **Folders per department**:
  - Logical separation of resources
  - IAM permissions are inherited by all projects under the folder
  - Clear and auditable permission structure

- **Minimal maintenance**:
  - User changes → update the group only
  - Project changes → move projects between folders

---

## ❌ Why the other options are not recommended

### **A. Direct user-to-project permissions**
- High operational overhead
- Difficult to maintain at scale

### **C. Direct user permissions at folder level**
- Better than A, but still lacks group-based management

### **D. Group permissions at project level**
- Still requires per-project IAM updates
- Does not scale well as projects grow

---

## ✅ Final Answer

**B**

---

# 3. Enforcing Deployment Order Across GKE Environments

## Question

Your company has an application running as a **Deployment in a Google Kubernetes Engine (GKE)** cluster.  
You have **separate clusters** for:
- Development
- Staging
- Production

You discovered that the team can **deploy a Docker image directly to production** without first deploying to development and staging.

Your goals are:
- Allow **team autonomy**
- **Prevent skipping environments**
- Use a **Google Cloud–native solution**
- **Quick to implement** with **minimal effort**

What should you do?

A. Configure a Kubernetes lifecycle hook to prevent the container from starting if it is not approved for usage in the given environment.  
B. Implement a corporate policy to prevent teams from deploying Docker images to an environment unless the Docker image was tested in an earlier environment.  
C. Configure binary authorization policies for the development, staging, and production clusters. Create attestations as part of the continuous integration pipeline.  
D. Create a Kubernetes admissions controller to prevent the container from starting if it is not approved for usage in the given environment.  

---

## ✅ Correct Answer

**C. Configure binary authorization policies for the development, staging, and production clusters. Create attestations as part of the continuous integration pipeline.**

---

## Explanation

### Why option C is correct ✅

**Binary Authorization** is a **Google Cloud–recommended security control** for GKE that:

- Ensures **only approved container images** can be deployed
- Enforces **promotion workflows** (dev → staging → prod)
- Integrates easily with **CI/CD pipelines**
- Requires **minimal custom code**

### How it works in this scenario:
- **Development cluster**: Allows unsigned images
- **Staging cluster**: Requires a test attestation
- **Production cluster**: Requires a staging + approval attestation
- CI/CD pipeline creates attestations after successful testing

This prevents bypassing environments while still allowing developers autonomy.

---

## ❌ Why the other options are incorrect

### **A. Kubernetes lifecycle hooks**
- Not designed for deployment governance
- Runs after scheduling, too late for enforcement

### **B. Corporate policy**
- Not technically enforceable
- Relies on human compliance

### **D. Custom admissions controller**
- Complex to implement and maintain
- Slower and higher effort than Binary Authorization

---

## ✅ Final Answer

**C**

---

# Data Migration to Cloud Storage (10-TB Database Export)

## Question

Your company wants to migrate a **10-TB on-premises database export** into **Cloud Storage**.  
Your goals are to:

- **Minimize migration time**
- **Minimize overall cost**
- **Minimize database load**
- Follow **Google-recommended practices**

The available **network bandwidth is 1 Gbps** between on-premises and Google Cloud.

What should you do?

A. Develop a Dataflow job to read data directly from the database and write it into Cloud Storage.  
B. Use the Data Transfer Appliance to perform an offline migration.  
C. Use a commercial partner ETL solution to extract the data from the on-premises database and upload it into Cloud Storage.  
D. Compress the data and upload it with `gsutil -m` to enable multi-threaded copy.

---

## ✅ Correct Answer

**D. Compress the data and upload it with `gsutil -m` to enable multi-threaded copy.**

---

## Explanation

### Why **D** is correct ✅

- **10 TB is not large enough** to justify offline transfer with a Data Transfer Appliance
- **1 Gbps bandwidth** is sufficient for online transfer
- `gsutil -m` enables **parallel uploads**, maximizing bandwidth usage
- **Compression reduces data size**, speeding up transfer and lowering costs
- Uploading an **export** (not live DB reads) keeps **database load minimal**
- This approach is **simple, low-cost, and Google-recommended**

Estimated transfer time at 1 Gbps:
- ~1.25 TB/hour (before compression)
- Faster with compression and parallelism

---

## ❌ Why the other options are incorrect

### **A. Dataflow job**
- Reads directly from the database
- Increases database load
- Adds unnecessary complexity

### **B. Data Transfer Appliance**
- Best for **very large datasets (tens to hundreds of TB or PB)**
- Higher cost and longer setup time
- Overkill for 10 TB

### **C. Commercial ETL solution**
- Higher cost
- Not necessary for a simple export upload
- Adds operational overhead

---

## ✅ Final Answer

**D**

---

# 4. High-Availability Stateful Application on Compute Engine

## Question

Your company has an **enterprise application running on Compute Engine** that requires:

- **High availability**
- **High performance**
- Deployment on **two instances in different zones** within the same region
- **Active-passive** configuration
- The application **writes data to a persistent disk**
- In case of a **single zone outage**, the data must be **immediately available** to the other instance

You want to **maximize performance** while **minimizing downtime and data loss**.

What should you do?

A.  
1. Attach a persistent SSD disk to the first instance.  
2. Create a snapshot every hour.  
3. In case of a zone outage, recreate a persistent SSD disk in the second instance from the snapshot.  

B.  
1. Create a Cloud Storage bucket.  
2. Mount the bucket to the first instance using `gcs-fuse`.  
3. In case of a zone outage, mount the bucket to the second instance using `gcs-fuse`.  

C.  
1. Attach a **regional SSD persistent disk** to the first instance.  
2. In case of a zone outage, **force-attach the disk** to the other instance.  

D.  
1. Attach a local SSD to the first instance.  
2. Run `rsync` every hour to copy data to a persistent SSD disk on the second instance.  
3. In case of a zone outage, use the second instance.  

---

## ✅ Correct Answer

**C. Attach a regional SSD persistent disk and force-attach it to the other instance during a zone outage**

---

## Explanation

### Why **C** is correct ✅

- **Regional persistent disks** synchronously replicate data across **two zones in the same region**
- Provide **high performance**, especially when using **SSD**
- In the event of a **zone outage**, the disk can be **force-attached immediately** to the standby instance
- Results in:
  - **Minimal downtime**
  - **Zero or near-zero data loss**
  - No need for restores, snapshots, or manual data sync

This is the **Google-recommended solution** for zonal failure protection with stateful Compute Engine workloads.

---

## ❌ Why the other options are incorrect

### **A. Snapshots**
- Snapshots are **point-in-time**
- Hourly snapshots can cause **up to 1 hour of data loss**
- Recovery is slower and involves disk recreation

### **B. Cloud Storage with gcs-fuse**
- Cloud Storage is **not a POSIX filesystem**
- Higher latency and lower performance
- Not suitable for high-performance enterprise applications

### **D. Local SSD + rsync**
- Local SSD data is **lost on instance failure**
- Hourly sync introduces **data loss risk**
- Manual synchronization increases operational complexity

---

## ✅ Final Answer

**C**

---

# 4. BigQuery with Externally Generated Encryption Keys

## Question

You are designing a **Data Warehouse on Google Cloud** and want to store **sensitive data in BigQuery**.  
Your company has the following requirement:

- **Encryption keys must be generated outside of Google Cloud**

You need to implement a solution that satisfies this requirement.

What should you do?

A. Generate a new key in Cloud Key Management Service (Cloud KMS). Store all data in Cloud Storage using the customer-managed key option and select the created key. Set up a Dataflow pipeline to decrypt the data and store it in a new BigQuery dataset.  

B. Generate a new key in Cloud KMS. Create a dataset in BigQuery using the customer-managed key option and select the created key.  

C. Import a key in Cloud KMS. Store all data in Cloud Storage using the customer-managed key option and select the created key. Set up a Dataflow pipeline to decrypt the data and store it in a new BigQuery dataset.  

D. Import a key in Cloud KMS. Create a dataset in BigQuery using the customer-supplied key option and select the created key.  

---

## ✅ Correct Answer

**D**

---

## Explanation

### Why **D** is correct ✅

- The requirement states that **encryption keys must be generated outside Google Cloud**
- Cloud KMS supports **importing externally generated keys**
- BigQuery supports **customer-managed encryption keys (CMEK)** via Cloud KMS
- By importing the key into Cloud KMS and associating it with a BigQuery dataset, you:
  - Retain **full control over key generation**
  - Meet **compliance and security requirements**
  - Avoid unnecessary data movement or processing

This approach follows **Google-recommended practices** for handling sensitive data with external key ownership.

---

## ❌ Why the other options are incorrect

### **A. Cloud Storage + Dataflow**
- Adds unnecessary complexity
- Dataflow decryption is not required
- Does not directly address BigQuery encryption needs

### **B. Generate key in Cloud KMS**
- Violates the requirement because the key is **generated inside Google Cloud**

### **C. Import key but use Cloud Storage + Dataflow**
- Over-engineered solution
- BigQuery can natively use CMEK without intermediate pipelines

---

## ✅ Final Answer

**D**

---

# 5. Rotating Encryption Keys for Cloud Storage Data

## Question

Your organization has stored **sensitive data in a Cloud Storage bucket**.  
For **regulatory reasons**, your company must be able to:

- **Rotate the encryption key** used to encrypt the data
- Process the data using **Dataproc**
- Follow **Google-recommended security practices**

What should you do?

A. Create a key with Cloud Key Management Service (KMS). Encrypt the data using the encrypt method of Cloud KMS.  
B. Create a key with Cloud Key Management Service (KMS). Set the encryption key on the bucket to the Cloud KMS key.  
C. Generate a GPG key pair. Encrypt the data using the GPG key. Upload the encrypted data to the bucket.  
D. Generate an AES-256 encryption key. Encrypt the data in the bucket using the customer-supplied encryption keys feature.  

---

## ✅ Correct Answer

**B**

---

## Explanation

### Why **B** is correct ✅

- **Cloud KMS (CMEK)** is the Google-recommended way to manage encryption keys
- You can **rotate keys transparently** without re-encrypting data manually
- Cloud Storage natively supports **CMEK**
- Dataproc integrates seamlessly with Cloud Storage buckets encrypted with CMEK
- Centralized key management improves **security, auditability, and compliance**

This solution provides:
- Easy **key rotation**
- **Minimal operational overhead**
- Full compliance with regulatory requirements

---

## ❌ Why the other options are incorrect

### **A. Manual KMS encryption**
- Requires custom encryption/decryption logic
- Adds unnecessary complexity
- Not the recommended approach for Cloud Storage

### **C. GPG encryption**
- Not integrated with Google Cloud IAM or Dataproc
- Manual key management
- Poor scalability and auditability

### **D. Customer-supplied encryption keys (CSEK)**
- Requires managing and supplying keys on every request
- Error-prone and operationally complex
- **Not recommended** compared to CMEK

---

## ✅ Final Answer

**B**

---

# 6. GKE Internet Access Without Public IPs

## Question

Your team needs to create a **Google Kubernetes Engine (GKE) cluster** to host a newly built application that requires:

- Access to **third-party services on the internet**
- **No Compute Engine instance may have a public IP address**
- Compliance with company security guidelines

What should you do?

A. Configure the GKE cluster as a private cluster, and configure Cloud NAT Gateway for the cluster subnet.  
B. Configure the GKE cluster as a private cluster. Configure Private Google Access on the Virtual Private Cloud (VPC).  
C. Configure the GKE cluster as a route-based cluster. Configure Private Google Access on the Virtual Private Cloud (VPC).  
D. Create a Compute Engine instance, and install a NAT Proxy on the instance. Configure all workloads on GKE to pass through this proxy to access third-party services on the Internet.  

---

## ✅ Correct Answer

**A**

---

## Explanation

### Why **A** is correct ✅

- **Private GKE clusters** ensure that nodes do **not have public IP addresses**
- **Cloud NAT** allows outbound internet access **without exposing public IPs**
- Supports access to **third-party external services**
- Fully managed, scalable, and follows **Google-recommended practices**
- No need to maintain custom NAT infrastructure

This is the **standard and recommended design** for:
- Secure outbound internet access
- Enterprise compliance
- Kubernetes workloads in private environments

---

## ❌ Why the other options are incorrect

### **B. Private cluster + Private Google Access**
- Private Google Access only allows access to **Google APIs**
- Does **not** allow access to **third-party internet services**

### **C. Route-based cluster + Private Google Access**
- Route-based clusters are deprecated
- Still does not enable third-party internet access

### **D. Custom NAT proxy**
- Requires manual setup and maintenance
- Single point of failure
- Not scalable
- Not Google-recommended when Cloud NAT is available

---

## ✅ Final Answer

**A**

---

# 7. App Engine Communication with On-Premises Database

## Question

Your company has a **support ticketing solution** that uses **App Engine Standard**. The project already has:

- A **VPC network** connected to the on-premises environment via **Cloud VPN**

You want the App Engine application to **communicate with a database** running on-premises.  

What should you do?

A. Configure private Google access for on-premises hosts only.  
B. Configure private Google access.  
C. Configure private services access.  
D. Configure serverless VPC access.  

---

## ✅ Correct Answer

**D. Configure serverless VPC access**

---

## Explanation

### Why **Serverless VPC Access** is correct ✅

- App Engine Standard **cannot directly access resources in a VPC**
- **Serverless VPC Access** provides a **connector** that allows serverless environments (App Engine, Cloud Functions, Cloud Run) to reach **VPC resources**
- With the VPC already connected to **on-premises via VPN**, this allows App Engine to communicate with **on-prem databases**
- Fully managed and scalable

---

## ❌ Why the other options are incorrect

### **A & B. Private Google Access**
- Allows access **only to Google APIs and services**
- **Does not enable** access to on-premises resources

### **C. Private Services Access**
- Enables private IP connectivity to **Google-managed services** (e.g., Cloud SQL)
- **Does not enable** App Engine Standard to reach on-premises databases

---

## ✅ Final Answer

**D**

---

# 8. Verifying Uploaded Files in Cloud Storage

## Question

Your company is planning to upload several important files to **Cloud Storage**.  
After the upload, they want to **verify that the uploaded content matches the on-premises files**.  
You want to **minimize cost and effort**.

What should you do?

A. Use `shasum` locally, upload with `gsutil -m`, download files, compute `shasum` again, and compare.  
B. Upload with `gsutil -m`, develop a custom Java application to compute CRC32C hashes, then compare with `gsutil ls -L`.  
C. Upload with `gsutil -m`, download the files, and use `diff` to compare.  
D. Upload with `gsutil -m`, use `gsutil hash -c` locally to get CRC32C hashes, use `gsutil ls -L` to get hashes from Cloud Storage, and compare.  

---

## ✅ Correct Answer

**D. Use `gsutil hash -c` locally to generate CRC32C hashes and compare with `gsutil ls -L` output from Cloud Storage**

---

## Explanation

### Why **Option D** is correct ✅

- **Cloud Storage automatically computes CRC32C and MD5 hashes** for objects.  
- `gsutil hash -c` computes **CRC32C locally** for files on-premises.  
- `gsutil ls -L` lists **CRC32C hashes for uploaded objects** in the bucket.  
- By comparing **local and remote hashes**, you can verify **data integrity without downloading files**, saving time and bandwidth.  

This approach is **efficient, cost-effective, and fully supported** by Google Cloud.

---

## ❌ Why the other options are incorrect

### **A. Downloading files and computing `shasum`**
- Requires **downloading all files**, increasing **cost and time**.  
- Hashing with SHA differs from Cloud Storage's built-in CRC32C.

### **B. Custom Java application**
- Unnecessary; Cloud Storage already provides CRC32C hashes.  
- Adds **development overhead**.

### **C. Using `diff`**
- Requires **full download of all files**.  
- Impractical for **large datasets**.

---

## ✅ Final Answer

**D**
---

# 9. Alerting on Request Latency in Anthos

## Question

You have deployed an application on **Anthos clusters (Anthos GKE)**.  
According to your company's **SRE practices**, you need to be **alerted if request latency exceeds a threshold** for a certain duration.

What should you do?

A. Install Anthos Service Mesh on your cluster. Use the Cloud Console to define a **Service Level Objective (SLO)** and create an **alerting policy** based on this SLO.  
B. Enable the **Cloud Trace API** and use Cloud Monitoring Alerts to send alerts based on trace metrics.  
C. Use **Cloud Profiler** to track request latency, create a **custom metric** in Cloud Monitoring, and alert if the metric exceeds the threshold.  
D. Configure **Anthos Config Management**, and create a YAML file that defines the SLO and alerting policy.  

---

## ✅ Correct Answer

**A. Install Anthos Service Mesh on your cluster. Use the Cloud Console to define a Service Level Objective (SLO) and create an alerting policy based on this SLO.**

---

## Explanation

### Why **Option A** is correct ✅

- **Anthos Service Mesh (ASM)** provides **observability, telemetry, and metrics** for microservices.  
- You can define **Service Level Objectives (SLOs)** for request latency, availability, or error rates.  
- Cloud Monitoring can then create **alerting policies based on SLO violations**, aligning with **SRE best practices**.  
- No additional instrumentation or custom metrics are needed.

---

## ❌ Why the other options are incorrect

### **B. Cloud Trace API**
- Cloud Trace is for **distributed tracing** and performance analysis.  
- It does **not provide native alerting** for request latency thresholds over time.  

### **C. Cloud Profiler**
- Cloud Profiler tracks **CPU and memory profiles**, not request latency.  
- Creating custom metrics from Profiler for latency is **complex and unnecessary**.  

### **D. Anthos Config Management**
- Config Management is for **policy and configuration enforcement**, not **real-time monitoring or alerting**.  

---

## ✅ Final Answer

**A**

---

# 10. Reducing Latency for Global GKE API Users

## Question

Your company has a **stateless web API** running on a single **GKE cluster** in **us-central1**.  
You now have customers in **Asia**, and you want to **reduce latency** for them.

What should you do?

A. Create a second GKE cluster in **asia-southeast1**, and expose both APIs using a Service of type LoadBalancer. Add the public IPs to the Cloud DNS zone.  
B. Use a **global HTTP(S) load balancer** with **Cloud CDN** enabled.  
C. Create a second GKE cluster in **asia-southeast1**, and use **kubemci** to create a global HTTP(S) load balancer.  
D. Increase the **memory and CPU** allocated to the application in the cluster.  

---

## ✅ Correct Answer

**C. Create a second GKE cluster in asia-southeast1, and use kubemci to create a global HTTP(S) load balancer.**

---

## Explanation

### Why **Option C** is correct ✅

- **kubemci** (Kubernetes Multi-Cluster Ingress) allows you to **expose multiple regional GKE clusters behind a single global HTTP(S) load balancer**.  
- Traffic is **routed to the closest cluster** based on the user’s location, reducing latency for Asian users.  
- Works for **stateless workloads**, which is ideal in this scenario.  
- Enables **high availability** and **geographical failover**.

---

## ❌ Why the other options are incorrect

### **A. Separate clusters with individual LoadBalancers**
- Would require **manual DNS management** and does **not automatically route users to the nearest cluster**.  
- Increases operational overhead and is not optimized for latency.

### **B. Global HTTP(S) load balancer with Cloud CDN**
- **Cloud CDN** is for caching **static content**, not for dynamically generated API responses.  
- Does **not reduce latency** for dynamic API computations.

### **D. Increase memory and CPU**
- Improves **processing performance**, but does **not address network latency for geographically distant users**.  

---

## ✅ Final Answer

**C**
