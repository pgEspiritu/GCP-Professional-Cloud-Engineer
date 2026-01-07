# 1. Secure SSH Access to Private Compute Engine Instances

## Question

You have deployed several instances on **Compute Engine**.  

- **Security requirement:** Instances **cannot have a public IP address**.  
- There is **no VPN connection** between Google Cloud and your office.  
- You need to **connect via SSH** to a specific machine **without violating security requirements**.  

Which approach should you use?

A. Configure **Cloud NAT** on the subnet where the instance is hosted. Create an SSH connection to the Cloud NAT IP address to reach the instance.  

B. Add all instances to an **unmanaged instance group**. Configure **TCP Proxy Load Balancing** with the instance group as a backend. Connect to the instance using the TCP Proxy IP.  

C. Configure **Identity-Aware Proxy (IAP)** for the instance and ensure that you have the role of **IAP-secured Tunnel User**. Use the **gcloud** command line tool to SSH into the instance.  

D. Create a **bastion host** in the network to SSH into the bastion host from your office location. From the bastion host, SSH into the desired instance.  

---

## ✅ Correct Answer

**C. Configure Identity-Aware Proxy (IAP) for the instance and ensure that you have the role of IAP-secured Tunnel User. Use the gcloud command line tool to SSH into the instance.**

---

## Explanation

### Why **IAP SSH** 🔐

- **IAP** allows secure access to **instances without public IPs**.  
- Access is **identity-based** and controlled via **IAM roles**.  
- Uses a **tunneled connection over HTTPS**, so no VPN or public IP is needed.  
- Simple to use with the `gcloud compute ssh --tunnel-through-iap` command.  

Benefits:  
- Fully compliant with **security requirement of no public IP**  
- **No additional infrastructure** like bastion hosts or VPN required  
- Access can be **audited via Cloud Audit Logs**  

---

## ❌ Why the other options are not correct

### **A. Cloud NAT**
- Cloud NAT only allows **outbound connections** from private instances  
- Does **not provide inbound SSH access**  

### **B. TCP Proxy Load Balancer**
- TCP Proxy LB is for **load balancing traffic**, not for SSH access  
- Overcomplicated and not recommended for secure admin access  

### **D. Bastion Host**
- Works but **requires provisioning and maintaining a bastion host**  
- Adds operational overhead  
- IAP is the **managed, recommended solution**  

---

## ✅ Final Answer

**C**

---

# 2. Restrict Project Creation in Specific Folder

## Question

Your company is using **Google Cloud**.  

- You have two folders under the Organization: **Finance** and **Shopping**.  
- The members of the **development team** are in a **Google Group**.  
- The development team group has been assigned the **Project Owner** role on the **Organization**.  

You want to **prevent the development team from creating resources in projects in the Finance folder**.  

What should you do?

A. Assign the development team group the **Project Viewer** role on the Finance folder, and assign the development team group the **Project Owner** role on the Shopping folder.  

B. Assign the development team group **only the Project Viewer** role on the Finance folder.  

C. Assign the development team group the **Project Owner** role on the Shopping folder, and **remove** the development team group Project Owner role from the Organization.  

D. Assign the development team group **only the Project Owner** role on the Shopping folder.  

---

## ✅ Correct Answer

**C. Assign the development team group the Project Owner role on the Shopping folder, and remove the development team group Project Owner role from the Organization.**

---

## Explanation

### Why **C** ✅

- **Project Owner role at the Organization level** allows creation of projects in **any folder**.  
- To prevent creation in the **Finance folder**, you must **remove the Organization-level Project Owner role**.  
- Assigning **Project Owner** only on the **Shopping folder** restricts project creation to that folder.  

This ensures:  
- Development team can **only create resources in the Shopping folder**  
- They **cannot create or modify projects in Finance**  

---

## ❌ Why the other options are not correct

### **A. Viewer on Finance + Owner on Shopping**
- If the dev team still has **Organization-level Owner**, they can override folder-level restrictions.  

### **B. Only Project Viewer on Finance**
- Does not address **Organization-level Project Owner permissions**  
- The dev team can still create projects anywhere in the Organization  

### **D. Only Project Owner on Shopping**
- Same issue as B — if they still have Org-level Project Owner, they can create projects in Finance  

---

## ✅ Final Answer

**C**

---

# 3. Simulating Microservice Failure on GKE

## Question

You are developing your **microservices application on Google Kubernetes Engine (GKE)**.  

During testing, you want to **validate the behavior of your application** in case a specific microservice **suddenly crashes**.  

What should you do?

A. Add a **taint** to one of the nodes of the Kubernetes cluster. For the specific microservice, configure a **pod anti-affinity label** that has the name of the tainted node as a value.  

B. Use **Istio's fault injection** on the particular microservice whose faulty behavior you want to simulate.  

C. Destroy one of the nodes of the Kubernetes cluster to observe the behavior.  

D. Configure **Istio's traffic management features** to steer the traffic away from a crashing microservice.  

---

## ✅ Correct Answer

**B. Use Istio's fault injection on the particular microservice whose faulty behavior you want to simulate.**

---

## Explanation

### Why **Istio Fault Injection** ✅

- Istio provides **built-in mechanisms** for testing microservices resilience.  
- **Fault injection** allows you to:
  - Simulate **crashes**, **delays**, or **errors** in a microservice.
  - Test **service behavior** under failure scenarios without affecting the entire cluster.
- This is ideal for **chaos testing** and **observing error handling**.

---

## ❌ Why the other options are not correct

### **A. Node taint and pod anti-affinity**
- Taints and anti-affinity are used for **scheduling control**, not for simulating crashes.
- Does not directly test **failure behavior** of a microservice.

### **C. Destroy a node**
- Drastic measure, affects **all pods on the node**, not just the target microservice.
- Not safe for controlled testing and **highly disruptive**.

### **D. Istio traffic management**
- Traffic routing **avoids failures**, but does **not simulate crashes**.
- Useful for resilience routing, but not for **testing failure behavior**.

---

## ✅ Final Answer

**B**

---

# 4. Globally Scalable Application with Minimal Infrastructure Management

## Question

Your company is developing a new application that will allow **globally distributed users to upload pictures and share them** with other selected users.  

The application will support **millions of concurrent users**.  

You want to allow developers to **focus on just building code** without having to create and maintain the underlying infrastructure.  

Which service should you use to deploy the application?

A. App Engine  
B. Cloud Endpoints  
C. Compute Engine  
D. Google Kubernetes Engine  

---

## ✅ Correct Answer

**A. App Engine**

---

## Explanation

### Why **App Engine** ✅

- **Fully managed platform** → developers do not manage servers or infrastructure.  
- **Automatic scaling** → handles millions of concurrent users seamlessly.  
- Supports **global access** with minimal configuration.  
- Ideal for applications where **developers want to focus only on code** and not on infrastructure provisioning or maintenance.  

---

## ❌ Why the other options are not correct

### **B. Cloud Endpoints**
- API management platform, not a **compute/deployment platform**.
- Cannot host the application by itself.

### **C. Compute Engine**
- IaaS solution → requires provisioning, managing, and scaling VMs manually.
- Adds **significant operational overhead**.

### **D. Google Kubernetes Engine**
- Offers container orchestration → requires managing clusters, nodes, and scaling policies.
- More flexible but **not fully serverless**, so developers still manage some infrastructure.

---

## ✅ Final Answer

**A**

---

# 5. API Versioning for Stability

## Question

Your company provides a **recommendation engine for retail customers**.  
You provide retail customers with an API where they can **submit a user ID** and the API **returns a list of recommendations** for that user.  

You are responsible for the **API lifecycle** and want to ensure **stability for your customers** in case the API makes **backward-incompatible changes**.  

You want to follow **Google-recommended practices**.  

What should you do?

A. Create a distribution list of all customers to inform them of an upcoming backward-incompatible change at least one month before replacing the old API with the new API.  
B. Create an automated process to generate API documentation, and update the public API documentation as part of the CI/CD process when deploying an update to the API.  
C. Use a versioning strategy for the APIs that increases the version number on every backward-incompatible change.  
D. Use a versioning strategy for the APIs that adds the suffix "DEPRECATED" to the current API version number on every backward-incompatible change. Use the current version number for the new API.  

---

## ✅ Correct Answer

**C. Use a versioning strategy for the APIs that increases the version number on every backward-incompatible change.**

---

## Explanation

### Why **API versioning** is important 📌

- Backward-incompatible changes can **break existing clients** if they continue using the old API.  
- Using **semantic versioning or a version increment strategy** ensures:
  - Existing clients continue to work with the old version.
  - New clients can migrate to the new API version.  
- This is **Google-recommended practice** for API lifecycle management.  

---

## ❌ Why the other options are not correct

### **A. Distribution list notification**
- Informing customers is good practice, but it **does not prevent broken integrations**.  
- Requires manual coordination and is not scalable.

### **B. Automated documentation updates**
- Helps with clarity and onboarding, but **does not ensure backward compatibility**.  
- Documentation alone cannot prevent client failures.

### **D. DEPRECATED suffix strategy**
- Confusing and non-standard.
- Clients may not correctly distinguish which version to use.  
- Version numbers should **always increase on backward-incompatible changes**, rather than reusing old numbers with "DEPRECATED".

---

## ✅ Final Answer

**C**

---

# 6. Re-architecting a Monolithic Application to Microservices

## Question

Your company has developed a **monolithic, 3-tier application** that allows external users to **upload and share files**.  

- The solution **cannot be easily enhanced** and **lacks reliability**.  
- The development team wants to **re-architect the application** to adopt **microservices** and a **fully managed service approach**.  
- They need to **convince leadership** that the effort is worthwhile.  

Which advantage(s) should they highlight to leadership?

A. The new approach will be significantly less costly, make it easier to manage the underlying infrastructure, and automatically manage the CI/CD pipelines.  
B. The monolithic solution can be converted to a container with Docker. The generated container can then be deployed into a Kubernetes cluster.  
C. The new approach will make it easier to decouple infrastructure from application, develop and release new features, manage the underlying infrastructure, manage CI/CD pipelines and perform A/B testing, and scale the solution if necessary.  
D. The process can be automated with Migrate for Compute Engine.  

---

## ✅ Correct Answer

**C. The new approach will make it easier to decouple infrastructure from application, develop and release new features, manage the underlying infrastructure, manage CI/CD pipelines and perform A/B testing, and scale the solution if necessary.**

---

## Explanation

### Why **microservices + fully managed services** are beneficial 🌐

- **Decouple infrastructure from application**
  - Services can evolve independently without affecting the whole system.
- **Faster development and feature releases**
  - Teams can deploy individual microservices quickly.
- **CI/CD integration**
  - Managed services simplify deployment pipelines and testing.
- **Scalability**
  - Each microservice can scale independently to handle spikes.
- **Reliability and resilience**
  - Failures in one microservice do not crash the entire application.
- **Support for experimentation**
  - Features like **A/B testing** are easier to implement with independent services.

This combination **reduces operational complexity** and **improves agility**, which are key arguments for leadership.

---

## ❌ Why the other options are not correct

### **A. Cost reduction and automatic CI/CD management**
- While partially true, it **over-simplifies the benefits**.  
- The main value is agility, scalability, and maintainability, not just cost reduction.

### **B. Docker container conversion**
- Converting a monolith to a container does **not provide microservices benefits**.  
- It may simplify deployment but **does not address reliability or scalability issues**.

### **D. Migrate for Compute Engine**
- This tool is for **lift-and-shift migrations**.  
- It **does not modernize architecture** or provide microservices and managed service benefits.

---

## ✅ Final Answer

**C**

---

# 7. Load Testing a GKE Web Application

## Question

Your team is developing a **web application** that will be deployed on **Google Kubernetes Engine (GKE)**.  

- The CTO expects a **successful launch**, and you need to ensure the application can handle **tens of thousands of users**.  
- You want to test the current deployment to ensure **latency stays below a certain threshold**.  

What should you do?

A. Use a load testing tool to simulate the expected number of concurrent users and total requests to your application, and inspect the results.  
B. Enable autoscaling on the GKE cluster and enable horizontal pod autoscaling on your application deployments. Send curl requests to your application, and validate if the auto scaling works.  
C. Replicate the application over multiple GKE clusters in every Google Cloud region. Configure a global HTTP(S) load balancer to expose the different clusters over a single global IP address.  
D. Use Cloud Debugger in the development environment to understand the latency between the different microservices.  

---

## ✅ Correct Answer

**A. Use a load testing tool to simulate the expected number of concurrent users and total requests to your application, and inspect the results.**

---

## Explanation

### Why **load testing** is the right approach 🚀

- Load testing **simulates real user traffic** to measure application behavior under expected and peak loads.
- Provides **quantitative latency measurements**, throughput, and error rates.
- Helps identify **bottlenecks** in the application, network, or cluster configuration.
- Can be integrated with **autoscaling validation**, but focuses directly on **latency under load**.

### Tools commonly used for load testing:

- **Locust**  
- **JMeter**  
- **Gatling**  
- **k6**  

These tools allow you to **simulate tens of thousands of concurrent users** and generate traffic patterns similar to production.

---

## ❌ Why the other options are not correct

### **B. Autoscaling + curl requests**
- Sending curl requests **does not simulate high load**.
- Autoscaling behavior is **not fully validated** with single-request testing.
- Does not measure **latency or performance under expected load**.

### **C. Multi-cluster deployment**
- Replicating across regions **does not help with load testing**.
- Premature optimization; unnecessary complexity before performance validation.

### **D. Cloud Debugger**
- Designed for **real-time debugging in development**, not load testing.
- Cannot simulate **tens of thousands of concurrent users** or measure latency under high load.

---

## ✅ Final Answer

**A**

---

# 8. Scaling a Kubernetes Application Processing Pub/Sub Messages

## Question

Your company has a **Kubernetes application** that pulls messages from **Pub/Sub** and stores them in **Filestore**.  

- The application is currently deployed as a **single pod**.  
- Analysis of Pub/Sub metrics shows that the application **cannot process messages in real time**; most messages wait for minutes before being processed.  
- The workload is **I/O-intensive**.  

You need to **scale the processing** to handle the backlog efficiently.  

What should you do?

A. Use `kubectl autoscale deployment APP_NAME --max 6 --min 2 --cpu-percent 50` to configure Kubernetes autoscaling deployment.  
B. Configure a Kubernetes autoscaling deployment based on the `subscription/push_request_latencies` metric.  
C. Use the `--enable-autoscaling` flag when you create the Kubernetes cluster.  
D. Configure a Kubernetes autoscaling deployment based on the `subscription/num_undelivered_messages` metric.  

---

## ✅ Correct Answer

**D. Configure a Kubernetes autoscaling deployment based on the `subscription/num_undelivered_messages` metric.**

---

## Explanation

### Why **num_undelivered_messages** is the right approach 📈

- **Pub/Sub metric `num_undelivered_messages`** directly represents the backlog of messages waiting to be processed.  
- Autoscaling based on this metric ensures **more pods are added when the backlog grows**, allowing the system to catch up efficiently.  
- For I/O-intensive workloads like writing to Filestore, scaling based on CPU alone may not be sufficient.  

### Kubernetes supports **custom metrics for autoscaling**:

- You can use the **Horizontal Pod Autoscaler (HPA)** with custom metrics from **Cloud Monitoring** (formerly Stackdriver).  
- Example:

```bash
kubectl autoscale deployment APP_NAME \
  --min 2 \
  --max 10 \
  --cpu-percent 50 \
  --metric-name subscription/num_undelivered_messages
```
This ensures real-time processing scales dynamically based on message backlog.

---

## ❌ Why the other options are not correct

### A. CPU-based autoscaling
- CPU usage may not correlate with message backlog in I/O-intensive workloads.
- Could under-provision or over-provision pods.

### B. Latency-based metric
- push_request_latencies shows response times, not the size of the backlog.
- Scaling based on latency may not reduce the message queue quickly.

### C. Cluster-level autoscaling flag
- --enable-autoscaling affects node scaling, not pod scaling.
- Does not address backlog in Pub/Sub directly.

---

## ✅ Final Answer

**D**

---

# 9. Auditable Production Deployments

## Question

Your company is developing a **web-based application**.  

You need to ensure that **production deployments** are:

- Linked to **source code commits**
- **Fully auditable**

Which approach should you take?

A. Make sure a developer is tagging the code commit with the date and time of commit.  
B. Make sure a developer is adding a comment to the commit that links to the deployment.  
C. Make the **container tag match the source code commit hash**.  
D. Make sure the developer is tagging the commits with `latest`.  

---

## ✅ Correct Answer

**C. Make the container tag match the source code commit hash.**

---

## Explanation

### Why **container tag = commit hash** 🔗

- Ensures **traceability**: Every deployment can be traced back to a specific **source code commit**.  
- Provides **audibility**: Auditors can verify exactly which code was deployed.  
- Fits naturally in **CI/CD pipelines**, e.g., Cloud Build or other deployment tools.  
- Avoids ambiguity associated with mutable tags like `latest`.  

---

## ❌ Why the other options are not correct

### **A. Tagging commits with date/time**
- Does not inherently link the deployment to the commit.
- Manual process prone to **errors or omissions**.

### **B. Adding comments to commits**
- Comments are **informational only** and not enforced in deployments.
- Hard to **automate and audit reliably**.

### **D. Tagging commits with `latest`**
- `latest` is **mutable** and **overwrites previous deployments**, breaking audibility.  
- Does **not uniquely identify** the commit deployed.

---

## ✅ Final Answer

**C**

---

# 10. Deploying a Highly Reliable HTTP(S) API

## Question

An **application development team** is planning to write and deploy an **HTTP(S) API** using **Go 1.12**.  

The requirements are:

- Workload is **very unpredictable**  
- Must remain **reliable during traffic peaks**  
- Minimize **operational overhead**  

Which approach should you recommend?

A. Develop the application with containers, and deploy to Google Kubernetes Engine.  
B. Develop the application for **App Engine standard environment**.  
C. Use a Managed Instance Group when deploying to Compute Engine.  
D. Develop the application for **App Engine flexible environment**, using a custom runtime.  

---

## ✅ Correct Answer

**B. Develop the application for App Engine standard environment.**

---

## Explanation

### Why **App Engine Standard** 🌐

- Fully **managed platform**, reducing operational overhead  
- **Automatic scaling** from zero to handle unpredictable workloads  
- Supports **Go 1.12 runtime** natively  
- Handles **load balancing, health checks, and HTTPS** automatically  
- No need to manage instances or containers manually  

---

## ❌ Why the other options are not correct

### **A. GKE with containers**
- Offers flexibility, but requires **manual cluster and autoscaling management**  
- Higher operational overhead compared to App Engine Standard  

### **C. Managed Instance Group**
- Provides scaling, but you still need to manage **VMs, OS patches, and autoscaling policies**  
- More operational effort and slower scaling than App Engine Standard  

### **D. App Engine Flexible with custom runtime**
- Flexible environment supports custom runtimes but **slower scaling**  
- Not as cost-effective for highly spiky workloads  
- More management complexity than Standard environment  

---

## ✅ Final Answer

**B**
