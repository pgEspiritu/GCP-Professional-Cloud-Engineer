# 1. Google Cloud Case Study — Resiliency Testing for a LAMP Stack Web Portal

## Question

Your company's user-feedback portal comprises a standard LAMP stack replicated across two zones. It is deployed in the **us-central1** region and uses autoscaled managed instance groups on all layers, except the database. Currently, only a small group of select customers have access to the portal. The portal meets a 99.99% availability SLA under these conditions.  

Next quarter, the company will make the portal available to all users, including unauthenticated users. You need to develop a **resiliency testing strategy** to ensure the system maintains the SLA once they introduce additional user load.  

What should you do?  

A. Capture existing users input, and replay captured user load until autoscale is triggered on all layers. At the same time, terminate all resources in one of the zones  
B. Create synthetic random user input, replay synthetic load until autoscale logic is triggered on at least one layer, and introduce "chaos" to the system by terminating random resources on both zones  
C. Expose the new system to a larger group of users, and increase group size each day until autoscale logic is triggered on all layers. At the same time, terminate random resources on both zones  
D. Capture existing users input, and replay captured user load until resource utilization crosses 80%. Also, derive estimated number of users based on existing users' usage of the app, and deploy enough resources to handle 200% of expected load  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Create synthetic random user input, replay synthetic load until autoscale logic is triggered on at least one layer, and introduce "chaos" to the system by terminating random resources on both zones**
- **Synthetic load testing** allows you to simulate future traffic patterns, including **higher volumes and diversity of user actions**.  
- **Chaos testing** by terminating resources across zones tests **resiliency and autoscaling logic** under failure conditions.  
- This approach ensures the system can **maintain the SLA** under **unexpected high load and failures**, without waiting for real users to generate traffic.  
- This aligns with **Google Cloud best practices** for load and resiliency testing in production-like environments.

---

## ❌ Why the other options are not correct

### **A. Capture existing users input, and replay captured user load until autoscale is triggered on all layers. At the same time, terminate all resources in one of the zones**
- Captured user load only represents **current traffic patterns**, which may **not reflect expected growth** or spikes from full user rollout.  
- Terminating **all resources in a zone** is extreme and could **unnecessarily disrupt testing**.

### **C. Expose the new system to a larger group of users, and increase group size each day until autoscale logic is triggered on all layers**
- Testing with real users is risky and **could violate SLA**.  
- It’s not safe to rely on live user traffic for resiliency testing.

### **D. Capture existing users input, replay until utilization crosses 80%, and deploy resources for 200% expected load**
- This approach only simulates **resource provisioning**, but does not test **autoscaling, failure recovery, or real resiliency**.  
- It lacks **chaos testing**, which is crucial for SLA assurance.

---

## ✅ Final Answer

**B. Create synthetic random user input, replay synthetic load until autoscale logic is triggered on at least one layer, and introduce "chaos" to the system by terminating random resources on both zones**

---

# 2. Google Cloud Case Study — Reducing Performance Bugs in Production

## Question

Your solution is producing **performance bugs in production** that you did not see in staging and test environments. You want to adjust your test and deployment procedures to avoid this problem in the future.  

What should you do?  

A. Deploy fewer changes to production  
B. Deploy smaller changes to production  
C. Increase the load on your test and staging environments  
D. Deploy changes to a small subset of users before rolling out to production  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Deploy changes to a small subset of users before rolling out to production**
- This approach is known as a **canary deployment**.  
- Canary deployments allow you to **test new changes under real production conditions** for a limited audience.  
- Benefits include:
  - Detecting **performance regressions** before they affect all users.  
  - Limiting the **blast radius** of potential issues.  
  - Gathering **real user metrics** to validate performance improvements.

---

## ❌ Why the other options are not correct

### **A. Deploy fewer changes to production**
- Reducing the quantity of changes does not guarantee that performance issues will be detected.  
- The underlying **performance testing strategy** is not addressed.

### **B. Deploy smaller changes to production**
- While smaller changes reduce risk, they **do not expose performance bugs under real production load**.

### **C. Increase the load on your test and staging environments**
- This may help reveal some issues, but it cannot perfectly replicate **real-world traffic patterns, network conditions, or user behavior** in production.

---

## ✅ Final Answer

**D. Deploy changes to a small subset of users before rolling out to production**

---

# 3. Google Cloud Case Study — Identifying Latency in Microservices

## Question

A small number of API requests to your **microservices-based application** take a very long time. You know that each request to the API can traverse many services.  

You want to know **which service takes the longest** in those cases.  

What should you do?  

A. Set timeouts on your application so that you can fail requests faster  
B. Send custom metrics for each of your requests to Stackdriver Monitoring  
C. Use Stackdriver Monitoring to look for insights that show when your API latencies are high  
D. Instrument your application with Stackdriver Trace in order to break down the request latencies at each microservice  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Instrument your application with Stackdriver Trace in order to break down the request latencies at each microservice**
- **Stackdriver Trace** provides **distributed tracing** for requests that pass through multiple services.  
- It allows you to:
  - See **latency per service and per span** of a request.  
  - Identify **bottlenecks** or services causing high response times.  
  - Analyze **traces for rare, long-running requests**, which are often missed by average metrics.  
- This is the most precise way to measure **end-to-end request latency across multiple microservices**.

---

## ❌ Why the other options are not correct

### **A. Set timeouts on your application so that you can fail requests faster**
- Timeouts help **fail fast** but do not provide insight into which service caused the delay.

### **B. Send custom metrics for each of your requests to Stackdriver Monitoring**
- Custom metrics can track request counts or latency averages, but **cannot break down latencies per microservice** for individual requests.

### **C. Use Stackdriver Monitoring to look for insights that show when your API latencies are high**
- Monitoring shows **overall latency trends** but lacks the **per-service breakdown** necessary to pinpoint which microservice is slow.

---

## ✅ Final Answer

**D. Instrument your application with Stackdriver Trace in order to break down the request latencies at each microservice**

---

# 4. Google Cloud Case Study — Ensuring High Availability for Relational Databases

## Question

During a high traffic portion of the day, one of your **relational databases crashes**, but the replica is **never promoted to a master**. You want to avoid this in the future.  

What should you do?  

A. Use a different database  
B. Choose larger instances for your database  
C. Create snapshots of your database more regularly  
D. Implement routinely scheduled failovers of your databases  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Implement routinely scheduled failovers of your databases**
- Many relational database systems (like **Cloud SQL** or MySQL/PostgreSQL) support **high availability with automatic failover**.  
- By **testing or scheduling failovers**, you can:
  - Ensure that **replicas are promoted correctly** if the master crashes.  
  - Verify that **replication and promotion logic works under load**.  
  - Reduce **downtime during traffic spikes**.

---

## ❌ Why the other options are not correct

### **A. Use a different database**
- Changing the database type **does not guarantee automatic failover**.  
- The key issue is **HA configuration**, not the database vendor.

### **B. Choose larger instances for your database**
- Larger instances may **reduce crashes due to load**, but they do not address **replica promotion failures**.

### **C. Create snapshots of your database more regularly**
- Snapshots provide **point-in-time recovery**, but **cannot replace automatic failover** in case of a master crash.  
- They do **not prevent downtime** during traffic spikes.

---

## ✅ Final Answer

**D. Implement routinely scheduled failovers of your databases**

---

# 5. Google Cloud Case Study — Long-Term Retention of Application Metrics

## Question

Your organization requires that **metrics from all applications** be retained for **5 years** for future analysis in possible legal proceedings.  

Which approach should you use?  

A. Grant the security team access to the logs in each Project  
B. Configure Stackdriver Monitoring for all Projects, and export to BigQuery  
C. Configure Stackdriver Monitoring for all Projects with the default retention policies  
D. Configure Stackdriver Monitoring for all Projects, and export to Google Cloud Storage  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Configure Stackdriver Monitoring for all Projects, and export to BigQuery**
- **Stackdriver Monitoring (Cloud Monitoring)** allows you to **export metrics to BigQuery**, which supports **long-term storage** and **querying**.  
- Benefits:
  - **Retention of 5+ years** without manual intervention.  
  - **Structured querying** for compliance or legal analysis.  
  - Centralized **metrics across multiple projects**.  

- Exporting metrics to BigQuery ensures **legal and analytical requirements** are met without relying on default retention limits.

---

## ❌ Why the other options are not correct

### **A. Grant the security team access to the logs in each Project**
- Providing access does **not extend retention**; metrics may still be **deleted according to default policies**.  
- Not suitable for long-term legal retention.

### **C. Configure Stackdriver Monitoring for all Projects with the default retention policies**
- Default retention for metrics is **6 weeks (for most metric types)**, far shorter than the required **5 years**.

### **D. Configure Stackdriver Monitoring for all Projects, and export to Google Cloud Storage**
- Cloud Storage can retain data long-term, but metrics in raw JSON/Protobuf format may be **harder to query and analyze** compared to BigQuery.  
- BigQuery is better suited for **large-scale analytics** and compliance reporting.

---

## ✅ Final Answer

**B. Configure Stackdriver Monitoring for all Projects, and export to BigQuery**
