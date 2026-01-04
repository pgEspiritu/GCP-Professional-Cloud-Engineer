# 1. Compute Engine Managed Instance Group — Troubleshooting Access

## Question

You have an outage in your **Compute Engine managed instance group (MIG)**:  
all instances keep **restarting after 5 seconds**. You have a **health check configured**, but **autoscaling is disabled**.

Your colleague (a Linux expert) offered to investigate the issue. You need to ensure he can **access the VMs**.

What should you do?

A. Grant your colleague the IAM role of **Project Viewer**  
B. Perform a **rolling restart** on the instance group  
C. **Disable the health check** for the instance group. Add his SSH key to the **project-wide SSH keys**  
D. Disable autoscaling for the instance group. Add his SSH key to the project-wide SSH keys  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why instances keep restarting
- In a **Managed Instance Group**, instances are automatically **recreated** if they fail the **health check**
- Even with **autoscaling disabled**, health checks still enforce instance replacement
- Since instances restart every **5 seconds**, the health check is likely failing before your colleague can log in

---

### Why **C** is correct
- **Disabling the health check**:
  - Stops the MIG from continuously restarting instances
  - Allows instances to stay up long enough for investigation
- **Adding his SSH key to project-wide SSH keys**:
  - Ensures he can log in immediately to any instance
  - Works even if instances are recreated later

This is the **fastest and safest way** to stabilize access during an outage.

---

## ❌ Why the other options are incorrect

### **A. Project Viewer**
- Viewer role provides **read-only access**
- Does **not** allow SSH access to VMs

### **B. Rolling restart**
- Will **restart instances again**
- Does not address the underlying health check failure

### **D. Disable autoscaling**
- Autoscaling is **already disabled**
- Does **nothing** to stop health-check-based restarts

---

## ✅ Final Answer

**C. Disable the health check for the instance group. Add his SSH key to the project-wide SSH Keys**

---

# 2. PCI DSS Compliance with Google Kubernetes Engine (GKE)

## Question

Your company is migrating its on-premises data center into the cloud. As part of the migration, you want to integrate **Google Kubernetes Engine (GKE)** for workload orchestration. Parts of your architecture must also be **PCI DSS–compliant**.

Which of the following is **most accurate**?

A. App Engine is the only compute platform on GCP that is certified for PCI DSS hosting.  
B. GKE cannot be used under PCI DSS because it is considered shared hosting.  
C. GKE and GCP provide the tools you need to build a PCI DSS-compliant environment.  
D. All Google Cloud services are usable because Google Cloud Platform is certified PCI-compliant.

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **Google Cloud Platform (GCP)** is **PCI DSS certified**, but **compliance is a shared responsibility**
- **GKE is allowed** for PCI DSS workloads
- Google provides:
  - PCI DSS–certified infrastructure
  - Security controls (IAM, VPC isolation, encryption, logging, monitoring)
- **You are responsible** for:
  - Secure cluster configuration
  - Network segmentation
  - Pod security, access control, and application-level compliance

👉 In short: **GKE + GCP give you the tools**, but **you must design and operate the environment correctly** to meet PCI DSS requirements.

---

## ❌ Why the other options are incorrect

### **A. App Engine is the only PCI DSS–certified compute platform**
❌ Incorrect  
- GCE, GKE, and other services can also be used for PCI DSS workloads

---

### **B. GKE cannot be used because it is shared hosting**
❌ Incorrect  
- PCI DSS **does not prohibit shared infrastructure**
- Proper isolation and controls make GKE acceptable

---

### **D. All GCP services are automatically usable**
❌ Incorrect  
- GCP’s certification **does not automatically make your application compliant**
- Not all services may be appropriate depending on your architecture and controls

---

## ✅ Final Answer

**C. GKE and GCP provide the tools you need to build a PCI DSS-compliant environment**

---

# 3. Detecting Anomalies in Degraded On-Premises Data

## Question

Your company has multiple on-premises systems that serve as sources for reporting. The data has not been maintained well and has become degraded over time.

You want to use **Google-recommended practices** to detect anomalies in your company data. What should you do?

A. Upload your files into Cloud Storage. Use Cloud Datalab to explore and clean your data.  
B. Upload your files into Cloud Storage. Use Cloud Dataprep to explore and clean your data.  
C. Connect Cloud Datalab to your on-premises systems. Use Cloud Datalab to explore and clean your data.  
D. Connect Cloud Dataprep to your on-premises systems. Use Cloud Dataprep to explore and clean your data.

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Cloud Dataprep** is Google’s recommended service for:
  - Data profiling
  - Detecting anomalies
  - Cleaning and transforming degraded datasets
- **Cloud Storage** is the standard ingestion and staging layer for data coming from on-premises environments
- Dataprep provides a **visual, rule-based interface** that automatically identifies data quality issues without requiring custom code

👉 This approach follows **Google best practices** for ingesting, profiling, and cleaning poorly maintained data.

---

## ❌ Why the other options are incorrect

### **A. Cloud Datalab with Cloud Storage**
❌ Incorrect  
- Cloud Datalab is notebook-based and requires manual coding
- Not optimized for automated anomaly detection or data quality assessment

---

### **C. Cloud Datalab connected directly to on-premises systems**
❌ Incorrect  
- Direct connections increase complexity
- Not the recommended ingestion pattern for large or degraded datasets

---

### **D. Cloud Dataprep connected directly to on-premises systems**
❌ Incorrect  
- Dataprep is designed to work with **Cloud Storage or BigQuery**
- Staging data first is the recommended architecture

---

## ✅ Final Answer

**B. Upload your files into Cloud Storage. Use Cloud Dataprep to explore and clean your data.**

---

# 4. IAM Policy Evaluation in the GCP Resource Hierarchy

## Question

Google Cloud Platform resources are managed hierarchically using **organization**, **folders**, and **projects**.  
When **Cloud Identity and Access Management (IAM)** policies exist at these different levels, what is the **effective policy** at a particular node of the hierarchy?

A. The effective policy is determined only by the policy set at the node  
B. The effective policy is the policy set at the node and restricted by the policies of its ancestors  
C. The effective policy is the union of the policy set at the node and policies inherited from its ancestors  
D. The effective policy is the intersection of the policy set at the node and policies inherited from its ancestors  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- IAM policies in GCP are **inherited downward** through the resource hierarchy:
  - **Organization → Folder → Project → Resource**
- The **effective IAM policy** at any level is the **union** of:
  - Policies defined at that level
  - Policies inherited from all ancestor levels
- Permissions **accumulate**, meaning:
  - If a user has a role at the organization level, they retain that role at all descendant levels unless explicitly restricted by policy constraints (not IAM)

👉 In short: **permissions add up as you go down the hierarchy**.

---

## ❌ Why the other options are incorrect

### **A. Only the policy set at the node**
❌ Incorrect  
- This ignores inherited permissions from parent resources

---

### **B. Policy restricted by ancestors**
❌ Incorrect  
- IAM does not work by restriction or override
- Ancestors **grant**, they do not limit permissions

---

### **D. Intersection of policies**
❌ Incorrect  
- GCP IAM does not reduce permissions by intersection
- Permissions are **additive**, not subtractive

---

## ✅ Final Answer

**C. The effective policy is the union of the policy set at the node and policies inherited from its ancestors**

---

# 5. Networking Design for Hybrid Migration with Cloud VPN

## Question

You are migrating your on-premises solution to **Google Cloud** in several phases.  
You will use **Cloud VPN** to maintain a connection between your on-premises systems and Google Cloud until the migration is completed.

You want to make sure **all your on-premises systems remain reachable** during this period.

How should you organize your networking in Google Cloud?

A. Use the same IP range on Google Cloud as you use on-premises  
B. Use the same IP range on Google Cloud as you use on-premises for your primary IP range and use a secondary range that does not overlap with the range you use on-premises  
C. Use an IP range on Google Cloud that does not overlap with the range you use on-premises  
D. Use an IP range on Google Cloud that does not overlap with the range you use on-premises for your primary IP range and use a secondary range with the same IP range as you use on-premises  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **Cloud VPN requires non-overlapping IP address ranges**
- Overlapping CIDR blocks between on-premises and Google Cloud:
  - Break routing
  - Cause ambiguous network paths
  - Make systems unreachable
- Using a **completely distinct IP range in GCP** ensures:
  - Reliable routing
  - Full reachability of on-premises systems
  - Clean hybrid connectivity during phased migration

👉 This is a **Google-recommended best practice** for hybrid networking.

---

## ❌ Why the other options are incorrect

### **A. Same IP range on both sides**
❌ Incorrect  
- Overlapping IP ranges are **not supported** with Cloud VPN
- Leads to routing conflicts and unreachable systems

---

### **B. Same primary range, different secondary range**
❌ Incorrect  
- Even partial overlap causes routing failures
- Primary ranges must never overlap

---

### **D. Different primary range, overlapping secondary range**
❌ Incorrect  
- Any overlapping CIDR (primary or secondary) breaks VPN routing
- Secondary ranges are also part of the routed network

---

## ✅ Final Answer

**C. Use an IP range on Google Cloud that does not overlap with the range you use on-premises**

---

# Deploying Cloud Datastore Indexes in App Engine

## Question

You have found an error in your **App Engine** application caused by **missing Cloud Datastore indexes**.  
You have created a **YAML file** with the required indexes and want to deploy these new indexes to **Cloud Datastore**.

What should you do?

A. Point `gcloud datastore create-indexes` to your configuration file  
B. Upload the configuration file to App Engine's default Cloud Storage bucket, and have App Engine detect the new indexes  
C. In the GCP Console, use Datastore Admin to delete the current indexes and upload the new configuration file  
D. Create an HTTP request to the built-in python module to send the index configuration file to your application  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- Cloud Datastore indexes are deployed using the **gcloud CLI**
- The correct approach is to explicitly deploy the index configuration file
- This ensures:
  - Controlled index creation
  - Proper validation
  - Safe rollout without deleting existing indexes

Typical command:
```bash
gcloud datastore indexes create index.yaml
```
> 👉 This is the official and recommended method for deploying Datastore indexes.

---

## ❌ Why the other options are incorrect

### B. Upload to Cloud Storage and auto-detect
❌ Incorrect
- App Engine does not automatically detect index files
- Index deployment must be explicitly triggered

### C. Delete indexes and upload via Console
❌ Incorrect
- Deleting indexes can cause:
  - Application downtime
  - Query failures
- Indexes should be updated, not deleted

### D. Send index file via HTTP request
❌ Incorrect
- Datastore index management is not handled by application code
- There is no supported API endpoint for this method

---

## ✅ Final Answer

**A. Point gcloud datastore create-indexes to your configuration file**

---

# 6. Regional Disaster Recovery for Compute Engine Applications

## Question

You have an application that will run on **Google Compute Engine**. You need to design an architecture that takes into account a **disaster recovery (DR) plan** that requires your application to **fail over to another region** in case of a **regional outage**.

What should you do?

A. Deploy the application on two Compute Engine instances in the same project but in a different region. Use the first instance to serve traffic, and use the HTTP load balancing service to fail over to the standby instance in case of a disaster.  

B. Deploy the application on a Compute Engine instance. Use the instance to serve traffic, and use the HTTP load balancing service to fail over to an instance on your premises in case of a disaster.  

C. Deploy the application on two Compute Engine **instance groups**, each in the same project but in a different region. Use the first instance group to serve traffic, and use the HTTP load balancing service to fail over to the standby instance group in case of a disaster.  

D. Deploy the application on two Compute Engine instance groups, each in a separate project and a different region. Use the first instance group to serve traffic, and use the HTTP load balancing service to fail over to the standby instance group in case of a disaster.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **Regional disaster recovery** requires:
  - Resources deployed in **multiple regions**
  - **Managed instance groups (MIGs)** for scalability and reliability
- **Global HTTP(S) Load Balancing**:
  - Supports **cross-region failover**
  - Automatically routes traffic to a healthy backend in another region
- Using **instance groups** (not single instances) ensures:
  - Autoscaling
  - Self-healing
  - Production-grade availability

👉 This is the **Google-recommended architecture** for regional failover on Compute Engine.

---

## ❌ Why the other options are incorrect

### **A. Single instances in different regions**
❌ Incorrect  
- Single VM instances:
  - Do not provide autoscaling
  - Do not provide self-healing
- Not suitable for production-grade disaster recovery

---

### **B. Fail over to on-premises**
❌ Incorrect  
- HTTP(S) Load Balancing **cannot directly fail over to on-premises backends**
- This adds unnecessary complexity and does not follow GCP best practices

---

### **D. Separate projects for DR**
❌ Incorrect  
- Using separate projects is **not required** for regional DR
- Adds operational complexity without clear benefit
- A single project with multi-region resources is sufficient

---

## ✅ Final Answer

**C. Deploy the application on two Compute Engine instance groups in different regions and use HTTP load balancing for failover**

---

# 7. Secure Integration of App Engine with On-Premises Database

## Question

You are deploying an application on **Google App Engine** that needs to integrate with an **on-premises database**. For security purposes, your on-premises database **must not be accessible through the public internet**.

What should you do?

A. Deploy your application on App Engine **standard environment** and use App Engine firewall rules to limit access to the open on-premises database.  

B. Deploy your application on App Engine **standard environment** and use **Cloud VPN** to limit access to the on-premises database.  

C. Deploy your application on App Engine **flexible environment** and use App Engine firewall rules to limit access to the on-premises database.  

D. Deploy your application on App Engine **flexible environment** and use **Cloud VPN** to limit access to the on-premises database.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct
- **App Engine flexible environment** supports **VPC connectivity**, allowing:
  - Connection to **private networks**
  - Use of **Cloud VPN** or **Cloud Interconnect** to reach on-premises systems securely
- **Cloud VPN** ensures that:
  - Traffic between App Engine and the on-premises database stays **private**
  - The database is **not exposed to the public internet**
- App Engine **flexible** allows **direct network integration**, unlike the standard environment which has limited VPC support.

---

## ❌ Why the other options are incorrect

### **A & C. Use firewall rules instead of VPN**
❌ Incorrect  
- App Engine firewall rules **cannot create a private connection** to on-premises networks
- Firewalls **only control inbound traffic to App Engine apps**, not secure access to private databases

### **B. Standard environment with Cloud VPN**
❌ Incorrect  
- App Engine **standard environment** has **restricted VPC capabilities**
- Cannot connect directly to an on-premises database via Cloud VPN

---

## ✅ Final Answer

**D. Deploy your application on App Engine flexible environment and use Cloud VPN to limit access to the on-premises database**

---

# 8. Installing Software on Compute Engine in a Private Environment

## Question

You are working in a **highly secured environment** where **public Internet access from the Compute Engine VMs is not allowed**. You do not yet have a VPN connection to access an on-premises file server. You need to install specific software on a Compute Engine instance.  

How should you install the software?

A. Upload the required installation files to **Cloud Storage**. Configure the VM on a subnet with a **Private Google Access** subnet. Assign **only an internal IP address** to the VM. Download the installation files to the VM using `gsutil`.  

B. Upload the required installation files to **Cloud Storage** and use **firewall rules** to block all traffic except the IP address range for Cloud Storage. Download the files to the VM using `gsutil`.  

C. Upload the required installation files to **Cloud Source Repositories**. Configure the VM on a subnet with a **Private Google Access** subnet. Assign **only an internal IP address** to the VM. Download the installation files to the VM using `gcloud`.  

D. Upload the required installation files to **Cloud Source Repositories** and use **firewall rules** to block all traffic except the IP address range for Cloud Source Repositories. Download the files to the VM using `gsutil`.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- **Private Google Access** allows VMs with **only internal IPs** to reach **Google APIs and services**, including Cloud Storage, without requiring public Internet access.
- Uploading installation files to **Cloud Storage** ensures they are **centrally accessible**.
- Using **`gsutil`** on the VM allows secure download of the files via **Google’s private network**.
- This approach aligns with Google best practices for **highly secured, private networks**.

---

## ❌ Why the other options are incorrect

### **B. Using firewall rules instead of Private Google Access**
❌ Incorrect  
- Firewall rules cannot provide **access to Google services** without a public IP.
- VMs with **internal IPs only** cannot reach Cloud Storage via public endpoints.

### **C & D. Using Cloud Source Repositories**
❌ Incorrect  
- Cloud Source Repositories are meant for **source code**, not binary software packages.
- `gcloud` cannot directly download arbitrary installation files from Cloud Source Repositories like `gsutil` can from Cloud Storage.
- Best practice is to use **Cloud Storage** for distributing installation files.

---

## ✅ Final Answer

**A. Upload the required installation files to Cloud Storage. Configure the VM on a subnet with a Private Google Access subnet. Assign only an internal IP address to the VM. Download the installation files to the VM using `gsutil`.**

---

# 9. Migrating Large Data Sets to Google Cloud Storage

## Question

Your company is moving **75 TB of data** into Google Cloud. You want to use **Cloud Storage** and follow **Google-recommended practices**.  

Which approach should you take?

A. Move your data onto a **Transfer Appliance**. Use a **Transfer Appliance Rehydrator** to decrypt the data into Cloud Storage.  

B. Move your data onto a **Transfer Appliance**. Use **Cloud Dataprep** to decrypt the data into Cloud Storage.  

C. Install **gsutil** on each server that contains data. Use **resumable transfers** to upload the data into Cloud Storage.  

D. Install **gsutil** on each server containing data. Use **streaming transfers** to upload the data into Cloud Storage.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- **Transfer Appliance** is designed for **large-scale data migration** to Google Cloud (tens to hundreds of TBs).  
- Data is physically shipped to Google, which **reduces the impact of limited network bandwidth**.  
- The **Transfer Appliance Rehydrator** securely decrypts and uploads the data to **Cloud Storage**, following Google best practices for **large-scale, offline transfers**.  
- Recommended for datasets **>10 TB** where network upload is impractical.

---

## ❌ Why the other options are incorrect

### **B. Using Cloud Dataprep to decrypt the data**
❌ Incorrect  
- **Cloud Dataprep** is for **data cleaning and transformation**, not secure bulk ingestion.  
- Does **not handle encrypted Transfer Appliance uploads**.

### **C. Using gsutil with resumable transfers**
❌ Incorrect  
- While **resumable transfers** are useful for moderate amounts of data, **75 TB is too large** to upload over the network efficiently.  
- This method may take weeks depending on bandwidth and is **not recommended by Google** for this volume.

### **D. Using gsutil with streaming transfers**
❌ Incorrect  
- **Streaming transfers** are designed for **smaller files or continuous ingestion**, not large-scale migration.  
- Attempting to upload 75 TB would be **impractical and error-prone**.

---

## ✅ Final Answer

**A. Move your data onto a Transfer Appliance. Use a Transfer Appliance Rehydrator to decrypt the data into Cloud Storage.**

---

# 10. Updating a GKE Deployment with Minimal Downtime

## Question

You have an application deployed on **Google Kubernetes Engine (GKE)** using a **Deployment** named `echo-deployment`. The deployment is exposed using a **Service** called `echo-service`. You need to perform an update to the application with **minimal downtime**.  

Which approach should you take?

A. Use `kubectl set image deployment/echo-deployment <new-image>`  
B. Use the rolling update functionality of the Instance Group behind the Kubernetes cluster  
C. Update the deployment YAML file with the new container image. Use `kubectl delete deployment/echo-deployment` and `kubectl create -f <yaml-file>`  
D. Update the service YAML file with the new container image. Use `kubectl delete service/echo-service` and `kubectl create -f <yaml-file>`

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- `kubectl set image` updates the container image for a deployment **in-place**.
- GKE **Deployments** automatically perform a **rolling update**:
  - Creates new pods with the updated image
  - Gradually terminates old pods as new pods become ready
  - Ensures continuous availability of the service during the update
- Minimal downtime is achieved because at least some pods remain available to serve traffic during the update.

---

## ❌ Why the other options are incorrect

### **B. Use rolling update on the Instance Group**
❌ Incorrect  
- Instance Groups manage VMs, not containers inside GKE.
- The Kubernetes Deployment controller handles rolling updates automatically, so updating the underlying VM group is unnecessary.

### **C. Delete and recreate the deployment**
❌ Incorrect  
- Deleting the deployment will **terminate all existing pods immediately**, causing downtime.
- Minimal downtime cannot be guaranteed with this approach.

### **D. Update the service YAML and recreate the service**
❌ Incorrect  
- Services in Kubernetes define **network access**, not the container image.
- Updating the service does **not update the pods**; deleting it would **interrupt traffic** temporarily.

---

## ✅ Final Answer

**A. Use `kubectl set image deployment/echo-deployment <new-image>`**
