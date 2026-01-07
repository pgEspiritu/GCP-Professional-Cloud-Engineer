# 1. GKE Application Errors with No Existing Logging

## Question

You have an application that runs in **Google Kubernetes Engine (GKE)**.  
Over the last **2 weeks**, customers have reported that a **specific part of the application frequently returns errors**.

You currently have **no logging or monitoring solution enabled** on your GKE cluster.  
You want to **diagnose the problem**, but you **cannot replicate the issue**, and you want to cause **minimal disruption** to the application.

What should you do?

A.  
1. Update your GKE cluster to use **Cloud Operations for GKE**.  
2. Use the **GKE Monitoring dashboard** to investigate logs from affected Pods.

B.  
1. Create a new GKE cluster with Cloud Operations for GKE enabled.  
2. Migrate the affected Pods to the new cluster and redirect traffic.  
3. Use the GKE Monitoring dashboard to investigate logs.

C.  
1. Update your GKE cluster to use Cloud Operations for GKE and deploy **Prometheus**.  
2. Set an alert to trigger whenever the application returns an error.

D.  
1. Create a new GKE cluster with Cloud Operations for GKE enabled and deploy Prometheus.  
2. Migrate the affected Pods and redirect traffic.  
3. Set an alert to trigger whenever the application returns an error.

---

## ✅ Correct Answer

**A. Update your GKE cluster to use Cloud Operations for GKE and use the GKE Monitoring dashboard to investigate logs from affected Pods.**

---

## Explanation

### Why **A** is correct ✅

- **Cloud Operations for GKE** (formerly Stackdriver) provides:
  - Centralized **logs, metrics, and traces**
  - Pod-level and container-level visibility
  - Native integration with GKE
- You can **enable it on an existing cluster** with minimal disruption
- It allows you to:
  - Inspect historical and live logs
  - Identify error patterns in specific Pods
  - Diagnose issues you cannot reproduce locally

This approach:
- 🚫 Avoids migrating workloads
- ⚙️ Requires minimal configuration
- 📊 Uses Google-native, recommended tooling

---

## ❌ Why the other options are not correct

### **B & D. Creating a new cluster**
- Introduces unnecessary complexity and risk
- Causes avoidable operational overhead
- Violates the requirement for **minimal disruption**

---

### **C & D. Deploying Prometheus**
- Prometheus is useful for metrics but **not required** here
- Adds complexity without helping with **historical log analysis**
- Alerts help detect issues but **do not diagnose existing ones**

---

## ✅ Final Answer

**A**

---

# 2. Stateful Workload with Shared POSIX Filesystem

## Question

You need to deploy a **stateful workload** on Google Cloud.

- The workload can **scale horizontally**
- Each instance must **read and write to the same POSIX-compliant filesystem**
- At **high load**, the workload must support up to **100 MB/s of write throughput**

What should you do?

A. Use a persistent disk for each instance.  
B. Use a regional persistent disk for each instance.  
C. Create a Cloud Filestore instance and mount it in each instance.  
D. Create a Cloud Storage bucket and mount it in each instance using gcsfuse.

---

## ✅ Correct Answer

**C. Create a Cloud Filestore instance and mount it in each instance.**

---

## Explanation

### Why **C** is correct ✅

- **Cloud Filestore** provides:
  - A **fully managed, POSIX-compliant shared filesystem**
  - **Simultaneous read/write access** from multiple Compute Engine or GKE instances
  - High throughput options that easily support **100 MB/s of writes**
- It is specifically designed for:
  - **Stateful, horizontally scalable workloads**
  - Applications that require **shared file locking and consistency**
- Mounting Filestore across instances allows all workloads to safely access the same data

This is the **Google-recommended solution** for shared POSIX filesystems at scale.

---

## ❌ Why the other options are not correct

### **A. Persistent disk for each instance**
- Persistent disks are **single-writer**
- Instances cannot safely share the same filesystem
- Does not meet the shared POSIX requirement

---

### **B. Regional persistent disk for each instance**
- Regional PD improves **availability**, not **shared access**
- Still **single-writer**
- Does not allow concurrent writes from multiple instances

---

### **D. Cloud Storage with gcsfuse**
- Cloud Storage is **object storage**, not a true filesystem
- gcsfuse:
  - Does not fully support POSIX semantics
  - Has lower and inconsistent write performance
  - Is not recommended for high-throughput, write-heavy workloads

---

## ✅ Final Answer

**C**

---

# 3. Identifying Latency in Anthos Microservices

## Question

Your company has an application deployed on **Anthos clusters** (formerly Anthos GKE) that is running multiple **microservices**.

- The cluster has **Anthos Service Mesh** enabled
- **Anthos Config Management** is also configured
- End users report that the application is **responding very slowly**

You want to **identify which microservice is causing the delay**.

What should you do?

A. Use the Service Mesh visualization in the Cloud Console to inspect the telemetry between the microservices.  
B. Use Anthos Config Management to create a ClusterSelector selecting the relevant cluster. On the Google Cloud Console page for Google Kubernetes Engine, view the Workloads and filter on the cluster. Inspect the configurations of the filtered workloads.  
C. Use Anthos Config Management to create a namespaceSelector selecting the relevant cluster namespace. On the Google Cloud Console page for Google Kubernetes Engine, visit the workloads and filter on the namespace. Inspect the configurations of the filtered workloads.  
D. Reinstall Istio using the default Istio profile in order to collect request latency. Evaluate the telemetry between the microservices in the Cloud Console.

---

## ✅ Correct Answer

**A. Use the Service Mesh visualization in the Cloud Console to inspect the telemetry between the microservices.**

---

## Explanation

### Why **A** is correct ✅

- **Anthos Service Mesh** (based on Istio) automatically collects:
  - Request latency
  - Error rates
  - Traffic flow between microservices
- The **Service Mesh visualization** in the Cloud Console provides:
  - End-to-end service dependency graphs
  - Per-service latency and request metrics
  - Clear identification of bottlenecks and slow services

This is the **fastest, least disruptive, and Google-recommended** way to identify which microservice is introducing latency.

---

## ❌ Why the other options are not correct

### **B. ClusterSelector with Anthos Config Management**
- Anthos Config Management is used for **policy and configuration management**
- It does **not provide runtime telemetry or latency analysis**
- Inspecting configurations does not help identify performance bottlenecks

---

### **C. NamespaceSelector with Anthos Config Management**
- Similar to option B, this focuses on **configuration state**, not performance
- Does not provide request-level latency or traffic insights

---

### **D. Reinstall Istio**
- **Unnecessary and disruptive**
- Anthos Service Mesh already includes telemetry collection by default
- Reinstalling Istio risks downtime and configuration drift

---

## ✅ Final Answer

**A**

---

# 4. Preventing Deletion or Overwrite of Cloud Storage Objects

## Question

You are working at a **financial institution** that stores **mortgage loan approval documents** on **Cloud Storage**.

- Any change to these approval documents must be uploaded as a **separate approval file**
- You want to ensure that these documents **cannot be deleted or overwritten for the next 5 years**

What should you do?

A. Create a retention policy on the bucket for the duration of 5 years. Create a lock on the retention policy.  
B. Create the bucket with uniform bucket-level access, and grant a service account the role of Object Writer. Use the service account to upload new files.  
C. Use a customer-managed key for the encryption of the bucket. Rotate the key after 5 years.  
D. Create the bucket with fine-grained access control, and grant a service account the role of Object Writer. Use the service account to upload new files.

---

## ✅ Correct Answer

**A. Create a retention policy on the bucket for the duration of 5 years. Create a lock on the retention policy.**

---

## Explanation

### Why **A** is correct ✅

- **Cloud Storage retention policies** enforce **Write Once, Read Many (WORM)** behavior
- A **5-year retention policy** ensures that:
  - Objects **cannot be deleted**
  - Objects **cannot be overwritten**
- **Locking the retention policy** makes it:
  - **Irreversible**, even by project owners
  - Compliant with financial and regulatory requirements

This is the **Google-recommended and compliance-safe approach** for immutability and legal retention.

---

## ❌ Why the other options are not correct

### **B. Uniform bucket-level access**
- Controls **who can access objects**, not **whether objects can be deleted**
- Object Writers can still delete or overwrite objects

---

### **C. Customer-managed encryption keys**
- Encryption controls **data confidentiality**, not immutability
- Rotating keys does **not prevent deletion or overwrite**

---

### **D. Fine-grained access control**
- Deprecated in favor of uniform bucket-level access
- Still does **not enforce retention or immutability**

---

## ✅ Final Answer

**A**

---

# 5. Automated CI/CD for Microservices on Google Kubernetes Engine

## Question

Your team will start developing a new application using **microservices architecture on Google Kubernetes Engine (GKE)**.

As part of the development lifecycle:
- Any code change pushed to the **remote `develop` branch** on GitHub should be **built and tested automatically**
- When the build and tests succeed, the **relevant microservice should be deployed automatically** to the **development environment**
- You want to **ensure that all code deployed to the development environment follows this process**, with no manual bypass

What should you do?

A. Have each developer install a pre-commit hook on their workstation that tests the code and builds the container when committing on the development branch. After a successful commit, have the developer deploy the newly built container image on the development cluster.  
B. Install a post-commit hook on the remote git repository that tests the code and builds the container when code is pushed to the development branch. After a successful commit, have the developer deploy the newly built container image on the development cluster.  
C. Create a Cloud Build trigger based on the development branch that tests the code, builds the container, and stores it in Container Registry. Create a deployment pipeline that watches for new images and deploys the new image on the development cluster. Ensure only the deployment tool has access to deploy new versions.  
D. Create a Cloud Build trigger based on the development branch to build a new container image and store it in Container Registry. Rely on Vulnerability Scanning to ensure the code tests succeed. As the final step of the Cloud Build process, deploy the new container image on the development cluster. Ensure only Cloud Build has access to deploy new versions.

---

## ✅ Correct Answer

**C. Create a Cloud Build trigger based on the development branch that tests the code, builds the container, and stores it in Container Registry. Create a deployment pipeline that watches for new images and deploys the new image on the development cluster. Ensure only the deployment tool has access to deploy new versions.**

---

## Explanation

### Why **C** is correct ✅

This option implements a **Google-recommended CI/CD pipeline** with proper separation of concerns:

- **Cloud Build trigger**:
  - Automatically runs on every push to the `develop` branch
  - Executes **tests and builds container images**
- **Artifact storage**:
  - Stores validated images in **Container Registry / Artifact Registry**
- **Deployment pipeline**:
  - Automatically deploys only **successfully built and tested images**
- **Access control**:
  - Developers cannot deploy directly
  - Only the deployment system has permission to update workloads

This guarantees that **every deployment to the development environment**:
- Is automated
- Is tested
- Is auditable
- Cannot bypass the CI/CD process

---

## ❌ Why the other options are not correct

### **A. Local pre-commit hooks**
- Can be bypassed or disabled
- Not centrally enforced
- Violates best practices for CI/CD and security

---

### **B. Post-commit hooks with manual deployment**
- Still relies on **developers manually deploying**
- No enforcement that only tested code reaches the cluster
- Poor separation of duties

---

### **D. Vulnerability scanning instead of tests**
- Vulnerability scanning **does not replace unit or integration testing**
- Deploying directly from Cloud Build couples build and deploy tightly
- Less flexible and not recommended for scalable microservices delivery

---

## ✅ Final Answer

**C**

---

# 6. Restoring Service During CPU Saturation on Compute Engine

## Question

Your operations team has asked you to help diagnose a **performance issue in a production application** running on **Compute Engine**.

Observed behavior:
- Requests are being **dropped under heavy load**
- A **single application process** is consuming **all available CPU**
- **Autoscaling has already reached the upper limit** of instances
- No abnormal load on other systems (including the database)

You want to **restore production traffic as quickly as possible**.

What should you recommend?

A. Change the autoscaling metric to `agent.googleapis.com/memory/percent_used`.  
B. Restart the affected instances on a staggered schedule.  
C. SSH to each instance and restart the application process.  
D. Increase the maximum number of instances in the autoscaling group.

---

## ✅ Correct Answer

**D. Increase the maximum number of instances in the autoscaling group.**

---

## Explanation

### Why **D** is correct ✅

- The application is **CPU-bound**, and:
  - Each instance is fully saturated
  - Autoscaling is already working as designed
  - The only current limiter is the **maximum instance count**
- Increasing the autoscaling limit:
  - Immediately allows **new instances to be created**
  - Distributes load across more CPUs
  - Restores service **without restarting or disrupting existing instances**
- This is the **fastest and safest way** to recover capacity during a production incident

This aligns with **Google SRE best practices**:
- Prefer **horizontal scaling** over manual intervention
- Avoid restarts or SSH-based fixes in production unless absolutely necessary

---

## ❌ Why the other options are not correct

### **A. Change autoscaling metric to memory**
- The bottleneck is clearly **CPU**, not memory
- Changing metrics won’t fix the immediate overload
- Risky during an active production incident

---

### **B. Restart instances on a staggered schedule**
- Causes **temporary loss of capacity**
- May worsen request drops during restarts
- Does not address the root cause (insufficient capacity)

---

### **C. SSH and restart application processes**
- Manual and error-prone
- Not scalable
- Violates production best practices
- The issue will recur under load since capacity hasn’t changed

---

## ✅ Final Answer

**D**

---

# 6. High-Throughput Web Service with Cost Optimization

## Question

You are implementing the infrastructure for a **web service on Google Cloud**.  
The requirements are:

- Handle **~500,000 requests per second**
- **Store incoming data**
- Support **real-time queries** using **exact matches** on a known set of attributes
- There are **idle periods with zero traffic**
- The business wants to **keep costs as low as possible**

Which **web service platform and database** should you use?

A. Cloud Run and BigQuery  
B. Cloud Run and Cloud Bigtable  
C. A Compute Engine autoscaling managed instance group and BigQuery  
D. A Compute Engine autoscaling managed instance group and Cloud Bigtable  

---

## ✅ Correct Answer

**B. Cloud Run and Cloud Bigtable**

---

## Explanation

### Why **Cloud Run** 🏃‍♂️

- **Scales automatically** from zero to very high request rates
- **No cost when idle**, which is critical since there are periods of no traffic
- Handles massive concurrency with minimal operational overhead
- Fully managed → no VM management required

This makes Cloud Run ideal for:
- **Spiky or unpredictable traffic**
- **Cost optimization**

---

### Why **Cloud Bigtable** 🗄️

- Designed for **very high write throughput** (hundreds of thousands to millions of writes per second)
- Optimized for **low-latency, real-time queries**
- Excellent for **exact key-based lookups**
- Horizontally scalable and proven for large-scale workloads

Bigtable is commonly used for:
- Time-series data
- Event ingestion
- Real-time analytics with known access patterns

---

## ❌ Why the other options are not correct

### **A. Cloud Run and BigQuery**
- BigQuery is optimized for **analytical (OLAP) workloads**
- Not suitable for **high-frequency real-time writes**
- Queries are batch-oriented and higher latency

---

### **C. Compute Engine MIG and BigQuery**
- BigQuery still not appropriate for real-time ingestion at this scale
- Compute Engine incurs **costs even when idle**
- Higher operational overhead

---

### **D. Compute Engine MIG and Cloud Bigtable**
- Cloud Bigtable is a good choice, but:
  - Compute Engine instances **do not scale to zero**
  - Higher baseline costs
  - More operational complexity compared to Cloud Run

---

## ✅ Final Answer

**B**

---

# 7. Internal Microservices Communication on GKE

## Question

You are developing an application using **different microservices** that should remain **internal to the cluster**.  
Your requirements are:

- Configure **each microservice with a specific number of replicas**
- Address a microservice **uniformly**, regardless of how many replicas it scales to
- Allow **any microservice to communicate with another** inside the cluster
- Implement the solution on **Google Kubernetes Engine (GKE)**

What should you do?

A. Deploy each microservice as a Deployment. Expose the Deployment in the cluster using a Service, and use the Service DNS name to address it from other microservices within the cluster.  
B. Deploy each microservice as a Deployment. Expose the Deployment in the cluster using an Ingress, and use the Ingress IP address to address the Deployment from other microservices within the cluster.  
C. Deploy each microservice as a Pod. Expose the Pod in the cluster using a Service, and use the Service DNS name to address the microservice from other microservices within the cluster.  
D. Deploy each microservice as a Pod. Expose the Pod in the cluster using an Ingress, and use the Ingress IP address name to address the Pod from other microservices within the cluster.  

---

## ✅ Correct Answer

**A. Deploy each microservice as a Deployment. Expose the Deployment in the cluster using a Service, and use the Service DNS name to address it from other microservices within the cluster.**

---

## Explanation

### Why **Deployments + Services** are the right choice ✅

- **Deployments**
  - Manage replica count declaratively
  - Handle scaling, rolling updates, and self-healing
- **Services (ClusterIP by default)**
  - Provide a **stable virtual IP and DNS name**
  - Automatically load-balance traffic across all replicas
  - Abstract away individual Pod IPs, which change frequently

Using a **Service DNS name** (for example, `orders-service.default.svc.cluster.local`) allows:
- Consistent addressing
- Seamless scaling up/down
- Internal-only communication (no external exposure)

This is the **standard and recommended Kubernetes pattern** for microservices.

---

## ❌ Why the other options are not correct

### **B. Deployment + Ingress**
- Ingress is designed for **external HTTP(S) traffic**
- Not intended for **internal service-to-service communication**
- Adds unnecessary complexity

---

### **C. Pod + Service**
- Pods are **ephemeral** and not meant to be managed individually
- You cannot easily control scaling or rolling updates
- Not suitable for production microservices

---

### **D. Pod + Ingress**
- Combines two incorrect patterns:
  - Pods instead of Deployments
  - Ingress for internal traffic
- Highly fragile and not scalable

---

## ✅ Final Answer

**A**

---

# 8. Separate Network & Compute Admins for Sensitive Compute Instances

## Question

Your company has a **networking team** and a **development team**.  

- The development team runs applications on **Compute Engine instances** that contain **sensitive data**.  
- The development team requires **administrative permissions for Compute Engine**.  
- The company requires all **network resources** to be managed by the networking team.  
- The development team does **not want the networking team** to have access to the sensitive data on the instances.  

What should you do?

A. Create a project with a **standalone VPC** and assign the **Network Admin** role to the networking team. Create a second project with a **standalone VPC** and assign the **Compute Admin** role to the development team. Use **Cloud VPN** to join the two VPCs.  

B. Create a project with a **standalone VPC**, assign the **Network Admin** role to the networking team, and assign the **Compute Admin** role to the development team.  

C. Create a project with a **Shared VPC** and assign the **Network Admin** role to the networking team. Create a second project **without a VPC**, configure it as a **Shared VPC service project**, and assign the **Compute Admin** role to the development team.  

D. Create a project with a **standalone VPC** and assign the **Network Admin** role to the networking team. Create a second project with a **standalone VPC** and assign the **Compute Admin** role to the development team. Use **VPC Peering** to join the two VPCs.  

---

## ✅ Correct Answer

**C. Create a project with a Shared VPC for networking and a Shared VPC service project for Compute Admins.**

---

## Explanation

- **Shared VPC** separates **network management** from **compute workloads**.  
- The **networking team** has **Network Admin** permissions on the **host project**, controlling subnets, firewalls, and routes.  
- The **development team** has **Compute Admin** permissions on the **service project**, managing Compute Engine instances **without accessing network resources**.  
- This ensures **least privilege**, **separation of duties**, and **simplifies management**.

---

## ❌ Why the other options are not correct

- **A. Standalone VPC + Cloud VPN**  
  - Adds unnecessary complexity  
  - Does not cleanly separate compute and networking roles  

- **B. Standalone VPC with both roles in one project**  
  - Compute Admins gain access to network resources  
  - Violates separation of duties  

- **D. Standalone VPC + VPC Peering**  
  - Operational overhead  
  - Still does not fully separate compute and network responsibilities  

---

## ✅ Final Answer

**C**

---

# 10. Cost-Effective, Highly Reliable Web Application

## Question

Your company wants you to build a **highly reliable web application** with a few public APIs as the backend.  

- You **don’t expect a lot of user traffic**, but traffic could **spike occasionally**.  
- You want to leverage **Cloud Load Balancing**.  
- The solution must be **cost-effective** for users.  

Which architecture should you use?

A. Store static content such as HTML and images in **Cloud CDN**. Host the APIs on **App Engine** and store the user data in **Cloud SQL**.  

B. Store static content such as HTML and images in a **Cloud Storage bucket**. Host the APIs on a **zonal Google Kubernetes Engine cluster** with worker nodes in multiple zones, and save the user data in **Cloud Spanner**.  

C. Store static content such as HTML and images in **Cloud CDN**. Use **Cloud Run** to host the APIs and save the user data in **Cloud SQL**.  

D. Store static content such as HTML and images in a **Cloud Storage bucket**. Use **Cloud Functions** to host the APIs and save the user data in **Firestore**.  

---

## ✅ Correct Answer

**C. Store static content in Cloud CDN, host APIs on Cloud Run, and use Cloud SQL for user data.**

---

## Explanation

### Why **Cloud CDN + Cloud Run + Cloud SQL** 🌐

- **Cloud CDN**  
  - Efficiently serves **static content** globally  
  - Reduces load on backend services  
  - Cost-effective for low but spiky traffic  

- **Cloud Run**  
  - Fully managed container platform  
  - **Scales from zero to handle spikes** automatically  
  - Pay only for the requests processed → **cost-efficient**  

- **Cloud SQL**  
  - Managed relational database  
  - Ideal for small to medium workloads  
  - Easy to connect from Cloud Run  

This combination provides:  
- **High reliability** (managed services, automatic scaling)  
- **Cost efficiency** (scale to zero when idle)  
- **Simple architecture** (low operational overhead)  

---

## ❌ Why the other options are not correct

### **A. Cloud CDN + App Engine + Cloud SQL**  
- App Engine is fine, but Cloud Run provides **more flexible container deployment**  
- Slightly higher cost if the app is idle because App Engine standard instances may still consume resources  

### **B. Cloud Storage + GKE + Cloud Spanner**  
- Overkill for **low traffic** scenario  
- GKE requires **more operational management**  
- Cloud Spanner is **expensive** for small workloads  

### **D. Cloud Storage + Cloud Functions + Firestore**  
- Firestore is **NoSQL**, may require schema changes for relational data  
- Cloud Functions could have **cold start latency** for APIs with spikes  
- Not ideal if relational queries are needed  

---

## ✅ Final Answer

**C**

---

# 9. Real-Time Security Log Monitoring

## Question

Your company sends all **Google Cloud logs** to Cloud Logging.  

- The **security team** wants to monitor the logs.  
- You want to ensure that the security team can **react quickly** if an anomaly such as an **unwanted firewall change** or **server breach** is detected.  
- You want to follow **Google-recommended practices**.  

Which approach should you implement?

A. Schedule a **cron job** with Cloud Scheduler. The scheduled job queries the logs every minute for the relevant events.  

B. Export logs to **BigQuery**, and trigger a query in BigQuery to process the log data for the relevant events.  

C. Export logs to a **Pub/Sub topic**, and trigger a **Cloud Function** with the relevant log events.  

D. Export logs to a **Cloud Storage bucket**, and trigger **Cloud Run** with the relevant log events.  

---

## ✅ Correct Answer

**C. Export logs to a Pub/Sub topic, and trigger a Cloud Function with the relevant log events.**

---

## Explanation

### Why **Cloud Logging → Pub/Sub → Cloud Function** 🔔

- **Cloud Logging** supports **log exports** in real time.  
- **Pub/Sub** acts as a **reliable, scalable event bus** for log events.  
- **Cloud Functions** can process events **immediately** and take automated action.  
- This approach follows **Google best practices** for **real-time anomaly detection**.  

Benefits:  
- **Low latency** for detection and response  
- **No polling required** (push-based system)  
- Scales automatically with log volume  

---

## ❌ Why the other options are not correct

### **A. Cloud Scheduler + Cron Job**
- Polling logs every minute introduces **latency**  
- Inefficient and not event-driven  

### **B. Export to BigQuery**
- BigQuery is **batch-oriented**, not real-time  
- Delays detection and response  

### **D. Export to Cloud Storage + Cloud Run**
- Cloud Storage is **not real-time**, designed for archival  
- Cloud Run would require polling or complex orchestration  

---

## ✅ Final Answer

**C**
