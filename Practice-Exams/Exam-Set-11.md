# 1. App Engine Application Update with Production Traffic Testing

## Question

You have an **App Engine application** that needs to be updated. You want to **test the update with production traffic** before replacing the current application version.

What should you do?

A. Deploy the update using the Instance Group Updater to create a partial rollout, which allows for canary testing.  
B. Deploy the update as a new version in the App Engine application, and split traffic between the new and current versions.  
C. Deploy the update in a new VPC, and use Google's global HTTP load balancing to split traffic between the update and current applications.  
D. Deploy the update as a new App Engine application, and use Google's global HTTP load balancing to split traffic between the new and current applications.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

- **App Engine supports multiple versions** of the same service running simultaneously.
- You can **deploy a new version** without impacting the existing one.
- App Engine provides **traffic splitting** (by percentage, cookie, or IP address) to:
  - Send a portion of **real production traffic** to the new version
  - Perform **canary testing** safely
- Once validated, you can **gradually shift 100% of traffic** to the new version.

👉 This is the **recommended and built-in approach** for testing updates in App Engine.

---

## ❌ Why the other options are incorrect

### **A. Instance Group Updater**
❌ Incorrect  
- Instance Group Updater applies to **Compute Engine managed instance groups**
- App Engine **does not use instance groups**

---

### **C. Deploy in a new VPC and use load balancing**
❌ Incorrect  
- Overly complex
- App Engine already provides **native traffic splitting**
- No need for separate VPCs or external load balancers

---

### **D. Deploy a new App Engine application**
❌ Incorrect  
- You cannot load-balance between **separate App Engine applications**
- App Engine traffic splitting works **within the same application**

---

## ✅ Final Answer

**B. Deploy the update as a new version in the App Engine application, and split traffic between the new and current versions.**

---

# 2. VPC Firewall Rules for Restricted Egress Traffic

## Question

All **Compute Engine instances** in your VPC should be able to connect to an **Active Directory server on specific ports**.  
Any **other outbound traffic must be blocked**.

You want to enforce this using **VPC firewall rules**.

How should you configure the firewall rules?

A. Create an egress rule with priority **1000** to deny all traffic for all instances. Create another egress rule with priority **100** to allow the Active Directory traffic for all instances.  
B. Create an egress rule with priority **100** to deny all traffic for all instances. Create another egress rule with priority **1000** to allow the Active Directory traffic for all instances.  
C. Create an egress rule with priority **1000** to allow the Active Directory traffic. Rely on the implied deny egress rule with priority **100** to block all traffic for all instances.  
D. Create an egress rule with priority **100** to allow the Active Directory traffic. Rely on the implied deny egress rule with priority **1000** to block all traffic for all instances.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

- **Firewall rule priority**:  
  - Lower number = **higher priority**
- You must:
  1. **Explicitly allow** Active Directory traffic first
  2. **Explicitly deny** all other outbound traffic afterward

**Rule order:**
1. **Allow AD traffic**  
   - Priority: **100** (evaluated first)
   - Egress rule
   - Destination: AD server IP(s)
   - Ports: AD-required ports (e.g., 389, 636, 445, etc.)
2. **Deny all other egress traffic**  
   - Priority: **1000**
   - Applies to all instances

👉 This ensures **only AD traffic is allowed**, and everything else is blocked.

---

## ❌ Why the other options are incorrect

### **B. Deny first, allow later**
❌ Incorrect  
- The deny rule (priority 100) is evaluated first
- AD traffic would **never be allowed**

---

### **C. Rely on implied deny**
❌ Incorrect  
- The **implied deny rule has the lowest priority**
- Without an explicit deny-all egress rule, outbound traffic may still be allowed

---

### **D. Wrong priority order**
❌ Incorrect  
- Implied deny does not have higher priority than explicit allow
- Does not guarantee full egress restriction

---

## ✅ Final Answer

**A. Create an egress rule with priority 1000 to deny all traffic for all instances, and another egress rule with priority 100 to allow the Active Directory traffic.**

---

# 3. Improving Machine Learning Model Results Over Time

## Question

Your customer runs a web service used by e-commerce sites to offer **product recommendations** to users.  
The company has begun experimenting with a **machine learning model on Google Cloud Platform** to improve the quality of results.

What should the customer do to **improve their model's results over time**?

A. Export Cloud Machine Learning Engine performance metrics from Stackdriver to BigQuery, to be used to analyze the efficiency of the model.  
B. Build a roadmap to move the machine learning model training from Cloud GPUs to Cloud TPUs, which offer better results.  
C. Monitor Compute Engine announcements for availability of newer CPU architectures, and deploy the model to them as soon as they are available for additional performance.  
D. Save a history of recommendations and results of the recommendations in BigQuery, to be used as training data.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### Why **D** is correct
- Machine learning models improve primarily through **better and more representative training data**
- By storing:
  - The **recommendations made**
  - The **user interactions and outcomes** (clicks, purchases, conversions)
- You create a **feedback loop** that allows:
  - Continuous retraining
  - Improved accuracy and relevance
  - Adaptation to changing user behavior

👉 **BigQuery** is ideal for:
- Storing large-scale historical data
- Running analytics
- Feeding data back into ML training pipelines

---

## ❌ Why the other options are incorrect

### **A. Export ML Engine metrics to BigQuery**
❌ Incorrect  
- Performance metrics help with **monitoring**, not improving prediction quality
- They do not provide new training signals for the model

---

### **B. Move from GPUs to TPUs**
❌ Incorrect  
- TPUs may improve **training speed**, not model quality
- Faster training ≠ better recommendations

---

### **C. Deploy on newer CPU architectures**
❌ Incorrect  
- Hardware improvements affect **performance**, not **model accuracy**
- Recommendation quality depends on data, not CPU generation

---

## ✅ Final Answer

**D. Save a history of recommendations and results of the recommendations in BigQuery, to be used as training data.**

---

# 4. Deploying an Auto-Scaling HTTPS Application on Google Kubernetes Engine (GKE)

## Question

A development team at your company has created a **dockerized HTTPS web application**.  
You need to deploy the application on **Google Kubernetes Engine (GKE)** and make sure that the application **scales automatically**.

How should you deploy to GKE?

A. Use the Horizontal Pod Autoscaler and enable cluster autoscaling. Use an Ingress resource to load-balance the HTTPS traffic.  
B. Use the Horizontal Pod Autoscaler and enable cluster autoscaling on the Kubernetes cluster. Use a Service resource of type LoadBalancer to load-balance the HTTPS traffic.  
C. Enable autoscaling on the Compute Engine instance group. Use an Ingress resource to load-balance the HTTPS traffic.  
D. Enable autoscaling on the Compute Engine instance group. Use a Service resource of type LoadBalancer to load-balance the HTTPS traffic.  

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

This option follows **Google-recommended best practices** for scalable, production-grade GKE deployments:

- **Horizontal Pod Autoscaler (HPA)**
  - Automatically scales the number of pods based on CPU or custom metrics
- **Cluster Autoscaler**
  - Automatically adds or removes nodes when pods cannot be scheduled
- **Ingress resource**
  - Provides:
    - Layer 7 (HTTP/HTTPS) load balancing
    - TLS termination
    - Integration with Google Cloud HTTP(S) Load Balancer
    - Better scalability and flexibility than per-Service load balancers

👉 Together, these components ensure **automatic scaling at both the pod and node levels**, with efficient HTTPS traffic management.

---

## ❌ Why the other options are incorrect

### **B. Use Service type LoadBalancer for HTTPS**
❌ Suboptimal  
- Works, but:
  - Creates a separate external load balancer per Service
  - Less scalable and more expensive than Ingress
- Google recommends **Ingress** for HTTP(S) workloads

---

### **C. Autoscale only the Compute Engine instance group**
❌ Incorrect  
- Does not scale pods
- Kubernetes workloads should scale via **HPA**, not direct VM autoscaling
- Breaks Kubernetes abstraction and best practices

---

### **D. Autoscale instance group + Service LoadBalancer**
❌ Incorrect  
- Still missing **pod-level autoscaling**
- Uses a less efficient load-balancing model for HTTPS

---

## ✅ Final Answer

**A. Use the Horizontal Pod Autoscaler and enable cluster autoscaling. Use an Ingress resource to load-balance the HTTPS traffic.**

---

# 5. Global Load Balancing with URL-Based Routing and In-Transit Encryption

## Question

You need to design a solution for **global load balancing** based on the **URL path being requested**.  
You must ensure:

- **Operational reliability**
- **End-to-end in-transit encryption**
- **Google-recommended best practices**

What should you do?

A. Create a cross-region load balancer with URL Maps.  
B. Create an HTTPS load balancer with URL Maps.  
C. Create appropriate instance groups and instances. Configure SSL proxy load balancing.  
D. Create a global forwarding rule. Configure SSL proxy load balancing.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

An **HTTPS Load Balancer with URL Maps** is the **Google-recommended solution** for this scenario:

- **Global load balancing**
  - Uses Google’s global Anycast network
  - Routes users to the closest healthy backend
- **URL-based routing**
  - URL Maps allow routing traffic based on path (e.g. `/api`, `/images`)
- **End-to-end in-transit encryption**
  - Supports HTTPS with TLS encryption
  - Can encrypt traffic from client → load balancer → backend
- **High reliability**
  - Built-in health checks
  - Automatic failover across regions

👉 This is the standard **Layer 7 (HTTP/HTTPS)** load balancing architecture on GCP.

---

## ❌ Why the other options are incorrect

### **A. Cross-region load balancer with URL Maps**
❌ Incorrect  
- This is not a valid or specific GCP load balancer type
- GCP uses **global HTTP(S) load balancers** for cross-region traffic

---

### **C. SSL proxy load balancing**
❌ Incorrect  
- SSL Proxy Load Balancing is **Layer 4**
- Does **not support URL-based routing**
- Used for non-HTTP(S) encrypted TCP traffic

---

### **D. Global forwarding rule with SSL proxy load balancing**
❌ Incorrect  
- Still **Layer 4**
- URL path inspection is impossible at this layer
- Does not meet the URL-based routing requirement

---

## ✅ Final Answer

**B. Create an HTTPS load balancer with URL Maps.**

---

# 6. Handling Transient HTTP Errors When Accessing Cloud Storage

## Question

You have an application that makes **HTTP requests to Cloud Storage**. Occasionally, the requests fail with **HTTP status codes 5xx and 429**.

How should you handle these types of errors?

A. Use gRPC instead of HTTP for better performance.  
B. Implement retry logic using a truncated exponential backoff strategy.  
C. Make sure the Cloud Storage bucket is multi-regional for geo-redundancy.  
D. Monitor https://status.cloud.google.com/feed.atom and only make requests if Cloud Storage is not reporting an incident.

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct
- **HTTP 5xx** errors indicate **temporary server-side issues**
- **HTTP 429** indicates **rate limiting (Too Many Requests)**
- Google **explicitly recommends** using:
  - **Retries with truncated exponential backoff**  
- This approach:
  - Reduces load during transient failures
  - Improves reliability
  - Prevents overwhelming the service with immediate retries

👉 This is a **standard best practice** across Google Cloud APIs, including **Cloud Storage**.

---

## ❌ Why the other options are incorrect

### **A. Use gRPC instead of HTTP**
❌ Incorrect  
- Cloud Storage primarily exposes **HTTP/REST APIs**
- Switching protocols does **not address transient errors**

---

### **C. Use a multi-regional bucket**
❌ Incorrect  
- Multi-regional buckets improve **availability and durability**
- They do **not prevent 429 or transient 5xx errors**

---

### **D. Check service status before making requests**
❌ Incorrect  
- Not practical for real-time applications
- Transient errors can occur even when no incident is reported

---

## ✅ Final Answer

**B. Implement retry logic using a truncated exponential backoff strategy**

---

# 7. Testing a Disaster Recovery Plan Using Google-Recommended Practices

## Question

You need to develop procedures to **test a disaster recovery plan** for a **mission-critical application**.  
You want to use **Google-recommended practices** and **native capabilities within GCP**.

What should you do?

A. Use Deployment Manager to automate service provisioning. Use Activity Logs to monitor and debug your tests.  
B. Use Deployment Manager to automate service provisioning. Use Stackdriver to monitor and debug your tests.  
C. Use gcloud scripts to automate service provisioning. Use Activity Logs to monitor and debug your tests.  
D. Use gcloud scripts to automate service provisioning. Use Stackdriver to monitor and debug your tests.

---

## ✅ **Correct Answer: B**

---

## Explanation

### Why **B** is correct

Google best practices recommend:

#### ✅ **Infrastructure automation**
- **Deployment Manager** provides:
  - Declarative, repeatable infrastructure provisioning
  - Version-controlled infrastructure as code
  - Consistent recreation of environments for DR testing

#### ✅ **Monitoring and observability**
- **Stackdriver (Cloud Monitoring & Logging)** provides:
  - Metrics, logs, dashboards, and alerts
  - Visibility into application health and system behavior during DR tests
  - Centralized troubleshooting and debugging

👉 Together, **Deployment Manager + Stackdriver** form a **native, recommended GCP approach** for disaster recovery testing.

---

## ❌ Why the other options are incorrect

### **A. Deployment Manager + Activity Logs**
❌ Activity Logs:
- Only track **administrative actions**
- Do **not provide application metrics or runtime diagnostics**
- Insufficient for DR testing and validation

---

### **C. gcloud scripts + Activity Logs**
❌ Not recommended:
- Imperative scripting is less repeatable and error-prone
- Activity Logs lack operational insight

---

### **D. gcloud scripts + Stackdriver**
❌ Partially correct but suboptimal:
- Stackdriver is correct
- **Deployment Manager** is preferred over ad-hoc scripts for:
  - Repeatability
  - Maintainability
  - Disaster recovery testing consistency

---

## ✅ Final Answer

**B. Use Deployment Manager to automate service provisioning. Use Stackdriver to monitor and debug your tests.**

---

# 8. Minimizing Global Download Latency for Software Distribution

## Question

Your company creates **rendering software** which users can download from the company website.  
Your company has customers **all over the world**.

You want to:
- **Minimize latency** for all customers  
- Follow **Google-recommended practices**

How should you store the files?

A. Save the files in a **Multi-Regional Cloud Storage bucket**.  
B. Save the files in a Regional Cloud Storage bucket, one bucket per zone of the region.  
C. Save the files in multiple Regional Cloud Storage buckets, one bucket per zone per region.  
D. Save the files in multiple Multi-Regional Cloud Storage buckets, one bucket per multi-region.

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

- **Multi-Regional Cloud Storage**:
  - Automatically stores data across **multiple geographically distributed regions**
  - Optimized for **serving frequently accessed content**
  - Provides **low latency access** for users worldwide
  - Integrates seamlessly with **Cloud CDN** for even better global performance

👉 This is the **simplest, most cost-effective, and Google-recommended** solution for globally distributed downloads.

---

## ❌ Why the other options are incorrect

### **B. Regional bucket per zone**
❌ Incorrect  
- Cloud Storage buckets are **regional or multi-regional**, not zonal
- Would not improve global latency

---

### **C. Multiple regional buckets per zone per region**
❌ Incorrect  
- Overly complex
- Requires custom replication and routing logic
- Not Google-recommended

---

### **D. Multiple multi-regional buckets**
❌ Incorrect  
- Unnecessary duplication
- Increases operational complexity and cost
- A **single multi-regional bucket** already provides global availability

---

## ✅ Final Answer

**A. Save the files in a Multi-Regional Cloud Storage bucket**

---

# 9. Secure Retention and Deletion of Healthcare Data

## Question

Your company acquired a **healthcare startup** and must retain its customers' **medical information** for up to **4 more years**, depending on when it was created.

Your corporate policy requires you to:
- **Securely retain** the data
- **Automatically delete** the data **as soon as regulations allow**

Which approach should you take?

A. Store the data in Google Drive and manually delete records as they expire.  
B. Anonymize the data using the Cloud Data Loss Prevention API and store it indefinitely.  
C. Store the data in Cloud Storage and use lifecycle management to delete files when they expire.  
D. Store the data in Cloud Storage and run a nightly batch script that deletes all expired data.

---

## ✅ **Correct Answer: C**

---

## Explanation

### Why **C** is correct

- **Cloud Storage lifecycle management** allows you to:
  - Automatically delete objects based on **age or creation date**
  - Enforce **regulatory retention periods** with no manual intervention
  - Reduce operational overhead and human error
- Data remains:
  - **Securely stored**
  - **Automatically removed** exactly when allowed by regulation
- This is the **Google-recommended**, scalable, and auditable approach

👉 Lifecycle rules are ideal for **compliance-driven data retention and deletion**.

---

## ❌ Why the other options are incorrect

### **A. Store data in Google Drive and manually delete**
❌ Incorrect  
- Manual processes are **error-prone**
- Not suitable for regulated healthcare data retention
- Does not scale or meet compliance best practices

---

### **B. Anonymize data and store indefinitely**
❌ Incorrect  
- Violates the requirement to **delete data when regulations allow**
- Retaining data indefinitely increases compliance risk

---

### **D. Nightly batch script to delete expired data**
❌ Incorrect  
- Custom scripts add operational complexity
- Higher risk of failure or misconfiguration
- Not as reliable or auditable as native lifecycle rules

---

## ✅ Final Answer

**C. Store the data in Cloud Storage and use lifecycle management to delete files when they expire**

---

# Minimizing Cloud SQL Queries with App Engine PHP

## Question

You are deploying a **PHP App Engine Standard service** with **Cloud SQL** as the backend.  
You want to **minimize the number of queries** to the database.

Which approach should you take?

A. Set the **memcache service level to dedicated**. Create a key from the **hash of the query**, and return database values from memcache before issuing a query to Cloud SQL.  
B. Set the memcache service level to dedicated. Create a **cron task** that runs every minute to populate the cache with keys containing query results.  
C. Set the memcache service level to shared. Create a cron task that runs every minute to save all expected queries to a key called `cached_queries`.  
D. Set the memcache service level to shared. Create a key called `cached_queries`, and return database values from the key before using a query to Cloud SQL.

---

## ✅ **Correct Answer: A**

---

## Explanation

### Why **A** is correct

- **Dedicated memcache** provides:
  - Reliable, higher-capacity caching for your App Engine service
  - Minimal eviction risk compared to shared memcache
- Caching **query results** in memcache:
  - Reduces load on Cloud SQL
  - Improves application performance
- Using a **hash of the query** as the key ensures:
  - Unique cache entries per query
  - Accurate retrieval without collisions
- Workflow:
  1. Check memcache for the query hash.
  2. If found, return cached results.
  3. If not, query Cloud SQL, then store the results in memcache.

This is a **Google-recommended pattern** for App Engine + Cloud SQL applications.

---

## ❌ Why the other options are incorrect

### **B. Cron task populates cache every minute**
❌ Incorrect  
- Not all queries are known in advance
- May cache unnecessary queries
- Inefficient and potentially stale data

---

### **C. Shared memcache with cron task**
❌ Incorrect  
- Shared memcache is **less reliable** (higher eviction)
- Cron-based caching does not scale with dynamic queries
- Risk of overwriting keys

---

### **D. Shared memcache with single key**
❌ Incorrect  
- Storing all queries under a **single key** is inefficient
- Increases cache management complexity
- High risk of collisions or stale data

---

## ✅ Final Answer

**A. Set the memcache service level to dedicated. Create a key from the hash of the query, and return database values from memcache before issuing a query to Cloud SQL.**
