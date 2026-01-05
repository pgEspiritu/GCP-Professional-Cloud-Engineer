# 1. BigQuery Centralized Billing with Read-Only Access

## Question

Your company uses **BigQuery** as its enterprise data warehouse. Data is distributed across multiple **Google Cloud projects**.  

Requirements:  
- All queries must be **billed to a single project**.  
- Users can **query datasets**, but **cannot edit** them.  
- Projects that contain the data should **not incur query costs**.

Which configuration of IAM roles should you use?

A. Add all users to a group. Grant the group the role of **BigQuery user** on the billing project and **BigQuery dataViewer** on the projects that contain the data.  
B. Add all users to a group. Grant the group the roles of **BigQuery dataViewer** on the billing project and **BigQuery user** on the projects that contain the data.  
C. Add all users to a group. Grant the group the roles of **BigQuery jobUser** on the billing project and **BigQuery dataViewer** on the projects that contain the data.  
D. Add all users to a group. Grant the group the roles of **BigQuery dataViewer** on the billing project and **BigQuery jobUser** on the projects that contain the data.

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **BigQuery jobUser** on the **billing project** allows users to **run queries** and have all costs billed to that project.  
- **BigQuery dataViewer** on the **projects containing the data** allows users to **read/query datasets** without modifying them.  
- This setup ensures:
  - Queries are executed **against the data projects** but **billed only to the billing project**.
  - Users **cannot edit the datasets**, satisfying read-only access requirements.
  - No unexpected costs are incurred on the data projects.

---

## ❌ Why the other options are incorrect

### **A. BigQuery user on billing project**
❌ Incorrect  
- **BigQuery user** grants both the ability to run queries **and create datasets/tables**, which is unnecessary and may violate the read-only requirement.

### **B. DataViewer on billing project, BigQuery user on data projects**
❌ Incorrect  
- Users would **not be able to run queries** billed to the billing project.  
- BigQuery user on the data projects allows **editing of datasets**, which violates the requirement.

### **D. DataViewer on billing project, jobUser on data projects**
❌ Incorrect  
- JobUser on data projects would **bill queries to the data projects**, which we want to avoid.

---

## ✅ Final Answer

**C. Add all users to a group. Grant the group the roles of BigQuery jobUser on the billing project and BigQuery dataViewer on the projects that contain the data.**

---

# 2. Temporary Image Upload for Cloud ML Engine Application

## Question

You have developed an application using **Cloud ML Engine** that recognizes famous paintings from uploaded images. You want to **test the application** and allow specific people to **upload images for the next 24 hours**. Not all users have a Google Account.  

Which approach should you use?

A. Have users upload the images to **Cloud Storage**. Protect the bucket with a password that expires after 24 hours.  
B. Have users upload the images to **Cloud Storage** using a **signed URL** that expires after 24 hours.  
C. Create an **App Engine web application** where users can upload images. Configure App Engine to disable the application after 24 hours. Authenticate users via **Cloud Identity**.  
D. Create an **App Engine web application** where users can upload images for the next 24 hours. Authenticate users via **Cloud Identity**.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Signed URLs** allow users to upload files to a **Cloud Storage bucket** without needing a Google account.  
- The **URL can be configured to expire** after 24 hours, fulfilling the temporary access requirement.  
- This approach is **simple**, **secure**, and **does not require managing user accounts**.  
- Works well for a small, temporary test of your ML application.

---

## ❌ Why the other options are incorrect

### **A. Protect the bucket with a password**
❌ Incorrect  
- Cloud Storage **does not support password protection** at the bucket level.  
- Only signed URLs or IAM permissions are supported.

### **C. App Engine with Cloud Identity**
❌ Incorrect  
- Requires users to **have Google accounts** to authenticate via Cloud Identity.  
- Not all users have Google accounts, which violates the requirement.

### **D. App Engine with Cloud Identity**
❌ Incorrect  
- Same as **C**, authentication requires Google accounts.  
- There is **no built-in 24-hour automatic expiration** for access without additional coding.

---

## ✅ Final Answer

**B. Have users upload the images to Cloud Storage using a signed URL that expires after 24 hours.**

---

# 3. GDPR Compliance for a Web Application on Google Cloud

## Question

Your web application must comply with the requirements of the **European Union's General Data Protection Regulation (GDPR)**. You are responsible for the **technical architecture** of your web application.  

Which approach should you take?

A. Ensure that your web application only uses native features and services of Google Cloud Platform, because Google already has various certifications and provides “pass-on” compliance when you use native features.  
B. Enable the relevant GDPR compliance setting within the GCP Console for each of the services in use within your application.  
C. Ensure that **Cloud Security Scanner** is part of your test planning strategy in order to pick up any compliance gaps.  
D. Define a **design for the security of data** in your web application that meets GDPR requirements.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct
- **GDPR compliance is a shared responsibility**: Google provides a compliant infrastructure, but you are responsible for **application design and data handling**.  
- You must define **how personal data is collected, stored, processed, and deleted**.  
- Measures include:
  - Encryption of data at rest and in transit  
  - Access control policies  
  - Data minimization and anonymization  
  - Mechanisms for data subject requests (right to access, delete, or correct data)  
- Designing your application **with GDPR in mind ensures compliance** regardless of the GCP services used.

---

## ❌ Why the other options are incorrect

### **A. Using only native GCP features**
❌ Incorrect  
- Google’s certifications do **not automatically make your application GDPR compliant**.  
- Compliance depends on **how you design and process data** in your application.

### **B. Enabling GDPR compliance settings**
❌ Incorrect  
- There is **no single “GDPR compliance setting”** in GCP.  
- Compliance requires **application-level and process-level design decisions**.

### **C. Using Cloud Security Scanner**
❌ Incorrect  
- Cloud Security Scanner can **identify vulnerabilities**, but it does **not enforce GDPR compliance**.  
- GDPR compliance is **broader than security scanning**.

---

## ✅ Final Answer

**D. Define a design for the security of data in your web application that meets GDPR requirements.**

---

# 3. High Availability for Microsoft SQL Server on GCP

## Question

You need to set up **Microsoft SQL Server** on Google Cloud Platform. Management requires that there is **no downtime** in case of a **data center outage in any of the zones** within a GCP region.  

Which approach should you take?

A. Configure a Cloud SQL instance with high availability enabled.  
B. Configure a Cloud Spanner instance with a regional instance configuration.  
C. Set up SQL Server on Compute Engine, using Always On Availability Groups with Windows Failover Clustering. Place nodes in different subnets.  
D. Set up SQL Server Always On Availability Groups using Windows Failover Clustering. Place nodes in different zones.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct
- **SQL Server Always On Availability Groups** provides **high availability and automatic failover**.  
- Placing nodes in **different zones** ensures the system can withstand **zonal outages**.  
- This setup supports **minimal downtime** and continues service even if a data center fails.

---

## ❌ Why the other options are incorrect

### **A. Cloud SQL with high availability**
❌ Incorrect  
- Cloud SQL HA provides failover **within a region**, but for SQL Server, GCP supports **Compute Engine deployment with Always On** for multi-zone HA.  
- Cloud SQL for SQL Server **may not provide zero downtime for all enterprise scenarios**.

### **B. Cloud Spanner**
❌ Incorrect  
- Cloud Spanner is **not Microsoft SQL Server**, so it is **incompatible with SQL Server workloads**.

### **C. Always On AG in different subnets**
❌ Incorrect  
- Placing nodes in **different subnets in the same zone** does **not protect against zonal outages**.  
- Multi-zone placement is required for high availability against data center failure.

---

## ✅ Final Answer

**D. Set up SQL Server Always On Availability Groups using Windows Failover Clustering. Place nodes in different zones.**

---

# 4. Deploying a Kubernetes Application on GCP

## Question

The development team has provided you with a **Kubernetes Deployment YAML file**. You have **no infrastructure yet** and need to deploy the application.  

What should you do?

A. Use gcloud to create a Kubernetes cluster. Use Deployment Manager to create the deployment.  
B. Use gcloud to create a Kubernetes cluster. Use kubectl to create the deployment.  
C. Use kubectl to create a Kubernetes cluster. Use Deployment Manager to create the deployment.  
D. Use kubectl to create a Kubernetes cluster. Use kubectl to create the deployment.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **gcloud** is the recommended CLI tool to **create a Google Kubernetes Engine (GKE) cluster**.  
- Once the cluster exists, you use **kubectl** to **apply the Deployment YAML**.  
- This follows **GCP best practices** and is the standard workflow for deploying Kubernetes applications on GKE.

---

## ❌ Why the other options are incorrect

### **A. Use Deployment Manager for the deployment**
❌ Incorrect  
- Deployment Manager manages **GCP resources**, not Kubernetes objects.  
- You **cannot deploy a Kubernetes Deployment YAML using Deployment Manager**.

### **C. Use kubectl to create a cluster**
❌ Incorrect  
- kubectl **cannot create clusters**; it only manages objects **inside an existing cluster**.

### **D. Use kubectl to create a cluster**
❌ Incorrect  
- Same reason as C; kubectl cannot create the cluster, only deploy resources once the cluster exists.

---

## ✅ Final Answer

**B. Use gcloud to create a Kubernetes cluster. Use kubectl to create the deployment.**

---

# 5. Evaluating Team Readiness for GCP Projects

## Question

You need to **evaluate your team readiness** for a new GCP project. You must perform the evaluation and create a **skills gap plan** which incorporates the business goal of **cost optimization**. Your team has deployed **two GCP projects successfully** to date.  

What should you do?

A. Allocate budget for team training. Set a deadline for the new GCP project.  
B. Allocate budget for team training. Create a roadmap for your team to achieve Google Cloud certification based on job role.  
C. Allocate budget to hire skilled external consultants. Set a deadline for the new GCP project.  
D. Allocate budget to hire skilled external consultants. Create a roadmap for your team to achieve Google Cloud certification based on job role.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Cost optimization** is a business goal, so leveraging **existing team members** and **training them** is more cost-effective than hiring consultants.  
- Creating a **skills gap plan** involves:
  - Assessing current team skills
  - Identifying gaps relative to project requirements
  - Building a **training roadmap aligned with Google Cloud certifications**  
- This ensures the team is prepared for future projects while maintaining cost efficiency.

---

## ❌ Why the other options are incorrect

### **A. Allocate budget for training and set a deadline**
❌ Incorrect  
- Setting a deadline alone does not address the **skills gap plan**.  
- It misses the structured approach of aligning training with **roles and certifications**.

### **C. Hire external consultants and set a deadline**
❌ Incorrect  
- Hiring consultants increases costs, which **does not align with the goal of cost optimization**.  
- It bypasses **team development**, which is critical for long-term sustainability.

### **D. Hire external consultants and create a certification roadmap**
❌ Incorrect  
- Even with a roadmap, hiring consultants **adds unnecessary cost** and is not optimized for budget efficiency.  

---

## ✅ Final Answer

**B. Allocate budget for team training. Create a roadmap for your team to achieve Google Cloud certification based on job role.**

---

# 6. Choosing a Compute Resource that Scales to Zero

## Question

You are designing an application for use **only during business hours**. For the **minimum viable product (MVP)** release, you'd like to use a **managed product that automatically `scales to zero`** so you **don't incur costs** when there is no activity.  

Which primary compute resource should you choose?

A. Cloud Functions  
B. Compute Engine  
C. Google Kubernetes Engine  
D. App Engine flexible environment  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- **Cloud Functions** is a **serverless, fully managed compute platform**.
- Key features that match the requirement:
  - Automatically **scales to zero** when there is no incoming request.
  - Scales up **dynamically on demand**, so you only pay for execution time.
  - Ideal for **short-lived workloads** like business-hour activity triggers.

- Compared to other options:
  - **Compute Engine**: Requires VMs; cannot scale to zero automatically. Costs are incurred as long as VM is running.
  - **Google Kubernetes Engine (GKE)**: Can scale workloads, but clusters **cannot automatically scale to zero** without additional configuration (e.g., Knative on top of GKE).
  - **App Engine flexible environment**: Always has at least one instance running; does **not scale to zero**.

---

## ✅ Final Answer

**A. Cloud Functions**

---

# 6. Efficient Retrieval of Root Entities in Cloud Datastore

## Question

You are creating an **App Engine application** that uses **Cloud Datastore** as its persistence layer. You need to retrieve **several root entities** for which you have the identifiers.  

You want to **minimize the overhead** in operations performed by Cloud Datastore.  

Which approach should you take?

A. Create the Key object for each Entity and run a batch get operation  
B. Create the Key object for each Entity and run multiple get operations, one operation for each entity  
C. Use the identifiers to create a query filter and run a batch query operation  
D. Use the identifiers to create a query filter and run multiple query operations, one operation for each entity  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct
- Cloud Datastore supports **batch get operations**, which allow you to retrieve **multiple entities in a single request**.
- Steps:
  1. Create a **Key object** for each entity you want to retrieve.
  2. Use the **Datastore batch get API** to fetch all entities in one call.
- Benefits:
  - **Reduces the number of round trips** to Datastore.
  - **Minimizes operational overhead** compared to multiple individual get requests.
  - **More efficient** than performing queries when you already have the entity keys.

### Why the other options are incorrect

#### **B. Run multiple get operations individually**
❌ Incorrect  
- Causes **one request per entity**, increasing latency and overhead.
- Inefficient compared to **batch get**.

#### **C. Use a query filter and batch query**
❌ Incorrect  
- Queries are **less efficient than key-based lookups** for entities with known keys.
- Introduces **index lookups** unnecessarily.

#### **D. Use a query filter and multiple queries**
❌ Incorrect  
- Same inefficiency as option C, **plus multiple queries** add more round trips and overhead.

---

## ✅ Final Answer

**A. Create the Key object for each Entity and run a batch get operation**

---

# 7. Uploading Files to Cloud Storage with Customer-Supplied Encryption Keys (CSEK)

## Question

You need to upload files from your **on-premises environment** to **Cloud Storage**.  

You want the files to be **encrypted on Cloud Storage** using **customer-supplied encryption keys (CSEK)**.  

Which approach should you take?

A. Supply the encryption key in a `.boto` configuration file. Use `gsutil` to upload the files.  
B. Supply the encryption key using `gcloud config`. Use `gsutil` to upload the files to that bucket.  
C. Use `gsutil` to upload the files, and use the flag `--encryption-key` to supply the encryption key.  
D. Use `gsutil` to create a bucket, and use the flag `--encryption-key` to supply the encryption key. Use `gsutil` to upload the files to that bucket.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **Customer-supplied encryption keys (CSEK)** allow you to manage your own encryption keys for Cloud Storage objects.  
- To use CSEK with `gsutil`:
  - Use the flag `--encryption-key <base64-encoded-key>` during **upload**:
    ```bash
    gsutil cp --encryption-key <base64-key> local-file gs://my-bucket/
    ```
- Benefits:
  - Files are **encrypted at rest** using your supplied key.
  - **No need to modify bucket-level encryption**; encryption is applied **per object**.

### Why the other options are incorrect

#### **A. Supply the encryption key in a .boto file**
❌ Incorrect  
- The `.boto` file supports configuration for gsutil, but **CSEK must be supplied per object or per command**.
- This approach does **not automatically apply your key** during upload.

#### **B. Supply the encryption key using gcloud config**
❌ Incorrect  
- `gcloud config` **does not store CSEK** for gsutil uploads.
- Using this method will **not encrypt the object** with your key.

#### **D. Use --encryption-key when creating the bucket**
❌ Incorrect  
- `--encryption-key` cannot be applied at **bucket creation**.
- CSEK must be specified **per object** during upload.

---

## ✅ Final Answer

**C. Use `gsutil` to upload the files, and use the flag `--encryption-key` to supply the encryption key**

---

# 8. Capturing Real-Time KPIs from Game Servers on GCP

## Question

Your customer wants to capture **multiple GBs of aggregate real-time key performance indicators (KPIs)** from their game servers running on **Google Cloud Platform** and monitor the KPIs with **low latency**.  

How should they capture the KPIs?

A. Store time-series data from the game servers in **Google Bigtable**, and view it using **Google Data Studio**.  
B. Output custom metrics to **Stackdriver** from the game servers, and create a **Dashboard** in Stackdriver Monitoring Console to view them.  
C. Schedule **BigQuery load jobs** to ingest analytics files uploaded to Cloud Storage every ten minutes, and visualize the results in **Google Data Studio**.  
D. Insert the KPIs into **Cloud Datastore** entities, and run ad hoc analysis and visualizations of them in **Cloud Datalab**.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Stackdriver Monitoring (Cloud Monitoring)** is designed for:
  - **Custom metrics**
  - **Low-latency monitoring**
  - **Dashboard visualization**
- Game servers can **push custom metrics** (e.g., KPIs) to Stackdriver in **real time**.  
- Dashboards allow **live monitoring** without storing massive amounts of historical data in a database.

### Why the other options are incorrect

#### **A. Store in Bigtable and view in Data Studio**
❌ Incorrect  
- Bigtable is great for **large-scale time-series storage**, but it does **not provide real-time dashboards** natively.  
- Additional setup is needed to visualize data, so **low-latency monitoring is not met**.

#### **C. BigQuery batch loads every ten minutes**
❌ Incorrect  
- BigQuery is designed for **analytics and batch queries**, not real-time monitoring.  
- Loading every ten minutes introduces **high latency**, failing the low-latency requirement.

#### **D. Cloud Datastore with ad hoc analysis**
❌ Incorrect  
- Cloud Datastore is a NoSQL database optimized for transactional data, not **real-time KPI monitoring**.  
- Ad hoc analysis in Datalab introduces **latency and complexity**.

---

## ✅ Final Answer

**B. Output custom metrics to Stackdriver from the game servers, and create a Dashboard in Stackdriver Monitoring Console to view them**

---

# 9. Deploying a Python Web Application with Many Dependencies on GCP

## Question

You have a **Python web application** with many dependencies that requires **0.1 CPU cores** and **128 MB of memory** to operate in production. You want to **monitor and maximize machine utilization**. You also want to **reliably deploy new versions** of the application.  

Which set of steps should you take?

A. Create a **managed instance group** with `f1-micro` machines. Use a **startup script** to clone the repository, install dependencies, and start the app. Restart instances to deploy new releases.  

B. Create a **managed instance group** with `n1-standard-1` machines. Build a **Compute Engine image** containing all dependencies and start the app. Rebuild the image and update the instance template to deploy new releases.  

C. Create a **Google Kubernetes Engine (GKE) cluster** with `n1-standard-1` machines. Build a **Docker image** with all dependencies and tag it with a version number. Create a **Kubernetes Deployment** with `imagePullPolicy: IfNotPresent` in staging, then promote it to production after testing.  

D. Create a **GKE cluster** with `n1-standard-4` machines. Build a **Docker image** tagged `latest`. Create a **Kubernetes Deployment** in the default namespace with `imagePullPolicy: Always`. Restart pods to deploy new releases.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct
- **GKE (Kubernetes)** allows:
  - **Fine-grained CPU/memory scheduling**, ideal for small workloads (0.1 CPU, 128 MB memory)
  - **High machine utilization** by packing multiple pods per node
  - **Versioned Docker images** for predictable deployments
- Using **staging and production namespaces** allows:
  - Testing new releases safely
  - Controlled promotion after verification
- `imagePullPolicy: IfNotPresent` ensures efficient use of local caches while still allowing versioned updates.

---

### Why the other options are incorrect

#### **A. Managed instance group with f1-micro**
❌ Incorrect  
- `f1-micro` instances have **very limited resources** and low reliability  
- Using a startup script to install dependencies **on every boot** is slow and error-prone  
- Cannot efficiently utilize machine resources for multiple small workloads

#### **B. Managed instance group with n1-standard-1 and prebuilt images**
❌ Incorrect  
- While prebuilt images **solve dependency management**, **Compute Engine instances are not as efficient** for very small workloads  
- Cannot **pack multiple workloads per machine** efficiently  
- Kubernetes gives better control for deployment and resource optimization

#### **D. GKE with n1-standard-4 and latest tag**
❌ Incorrect  
- Using `latest` tag **risks unpredictable deployments**  
- Restarting pods manually is **less reliable** than versioned image deployments  
- Over-provisioning (n1-standard-4) **wastes resources** for a lightweight app

---

## ✅ Final Answer

**C. Create a GKE cluster with n1-standard-1 machines. Build a Docker image with all dependencies, tag it with a version, and deploy using a Kubernetes Deployment with `imagePullPolicy: IfNotPresent` in staging, then promote to production**

---

# 10. Integrating On-Premises Active Directory with Google Cloud

## Question

Your company wants to start using **Google Cloud resources** but wants to **retain their on-premises Active Directory (AD) domain controller** for identity management.  

What should you do?

A. Use the **Admin Directory API** to authenticate against the Active Directory domain controller.  

B. Use **Google Cloud Directory Sync (GCDS)** to synchronize Active Directory usernames with **Cloud Identities** and configure **SAML SSO**.  

C. Use **Cloud Identity-Aware Proxy** configured to use the on-premises AD domain controller as an identity provider.  

D. Use **Compute Engine** to create an AD domain controller that is a **replica of the on-premises AD** using Google Cloud Directory Sync.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **Google Cloud Directory Sync (GCDS)** allows you to **synchronize users, groups, and contacts** from your on-premises Active Directory to **Google Cloud identities**.  
- By configuring **SAML Single Sign-On (SSO)**:
  - Users can authenticate with **their existing AD credentials**.  
  - No need to move or replicate the AD domain controller to the cloud.  
- This approach follows **Google recommended best practices** for hybrid identity management.

---

### Why the other options are incorrect

#### **A. Admin Directory API**
❌ Incorrect  
- The **Admin Directory API** is for **managing Google Workspace users** programmatically  
- It **does not authenticate directly against an on-premises AD**.

#### **C. Cloud Identity-Aware Proxy (IAP)**
❌ Incorrect  
- **IAP** protects applications running in GCP  
- It **cannot directly use on-premises AD as an identity provider** without SAML integration  
- The correct approach is to combine GCDS + SAML.

#### **D. Creating an AD replica on Compute Engine**
❌ Incorrect  
- While possible, **replicating an AD domain controller to GCP** is unnecessary for hybrid identity  
- Adds **complexity, maintenance, and cost** compared to GCDS + SAML.

---

## ✅ Final Answer

**B. Use Google Cloud Directory Sync to synchronize Active Directory usernames with cloud identities and configure SAML SSO**
