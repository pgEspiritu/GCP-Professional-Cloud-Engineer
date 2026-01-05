# 1. Google Cloud Case Study: Public Health Information Website Migration

## Question

Anonymous users from all over the world access a public health information website hosted in an on-premises EHR data center. The servers that host this website are older, and users are complaining about sluggish response times. There has also been a recent increase of distributed denial-of-service attacks toward the website. The attacks always come from the same IP address ranges. EHR management has identified the public health information website as an easy, low risk application to migrate to Google Cloud. You need to improve access latency and provide a security solution that will prevent the denial-of-service traffic from entering your Virtual Private Cloud (VPC) network. What should you do?

A. Deploy an external HTTP(S) load balancer, configure VPC firewall rules, and move the applications onto Compute Engine virtual machines.  
B. Deploy an external HTTP(S) load balancer, configure Google Cloud Armor, and move the application onto Compute Engine virtual machines.  
C. Containerize the application and move it into Google Kubernetes Engine (GKE). Create a GKE service to expose the pods within the cluster, and set up a GKE network policy.  
D. Containerize the application and move it into Google Kubernetes Engine (GKE). Create an internal load balancer to expose the pods outside the cluster, and configure Identity-Aware Proxy (IAP) for access.  

---

## ✅ **Correct Answer:** B  
**Deploy an external HTTP(S) load balancer, configure Google Cloud Armor, and move the application onto Compute Engine virtual machines.**

---

## 🧠 Explanation

### 1. **External HTTP(S) Load Balancer**
- The **External HTTP(S) Load Balancer** is a **global**, fully-distributed load balancing service that provides **low latency** and **high availability**.
- It operates at the **edge of Google’s network**, meaning user traffic is terminated **before** it reaches the VPC.
- This helps reduce latency for users across the world by directing traffic to the nearest Google edge location.

### 2. **Google Cloud Armor**
- **Cloud Armor** integrates with the external HTTP(S) load balancer to provide **DDoS protection** and **WAF (Web Application Firewall)** capabilities.
- It can filter requests **before** they reach your backend services.
- You can configure **IP-based rules** to **block known malicious IP ranges** identified from the previous DDoS attacks.
- This ensures the **malicious traffic never enters the VPC**, improving both **security** and **performance**.

### 3. **Compute Engine Virtual Machines**
- Migrating the web application to **Compute Engine VMs** is a straightforward way to lift and shift from on-premises servers.
- You can still achieve scalability by using **managed instance groups** behind the load balancer.
- This option fits the “easy, low-risk” migration criteria stated in the scenario.

---

## ❌ Why Not the Other Options?

### **A. VPC Firewall Rules**
- VPC firewall rules act **inside the VPC**, meaning DDoS traffic would still reach your network before being filtered.
- This doesn’t protect your infrastructure from **high-volume external attacks**.

### **C. GKE with Network Policies**
- While **GKE** offers scalability and flexibility, it adds unnecessary **complexity** for a “low-risk” public site.
- Network policies in GKE control **pod-to-pod communication**, not **external DDoS traffic**.

### **D. Internal Load Balancer with IAP**
- **Internal Load Balancers** are for **private** applications, not public-facing websites.
- **IAP (Identity-Aware Proxy)** is used for authenticated access, which doesn’t fit a **public health information** site meant for **anonymous users**.

---

## 🏁 Summary
The best solution provides:
- 🌐 **Global load balancing** for low latency  
- 🛡️ **DDoS protection at the edge** via Cloud Armor  
- ☁️ **Simple migration** path using Compute Engine  

✅ **Final Answer:** **B**

---

# 2. Google Cloud Networking: Connectivity Recommendation

## Question

EHR wants to connect one of their data centers to Google Cloud. The data center is in a remote location over 100 kilometers from a Google-owned point of presence. They can’t afford new hardware, but their existing data center can accomodate future throughput growth. Tey also shared these data points:

- Servers in their on-premises data center need to talk to Google Kubernetes Engine (GKE) resources in the cloud.  
- Both on-premises servers and cloud resources are configured with private RFC 1918 IP addresses.  
- The service provider has informed the customer that basic Internet connectivity is a best-effort service with no SLA.

You need to recommend a connectivity option. What should you recommend?

A. Provision Carrier Peering.  
B. Provision a new Internet connection.  
C. Provision a Partner Interconnect connection.  
D. Provision a Dedicated Interconnect connection.  

---

## ✅ **Correct Answer: C**  
**Provision a Partner Interconnect connection.**

---

## Explanation

**Why Partner Interconnect (C) is the right choice**

- **Distance constraint:** The data center is **>100 km from a Google PoP**, so a Dedicated Interconnect (which requires colocating at a Google edge/colo and direct physical cross-connect) is impractical or impossible without moving or adding hardware near a Google facility.  
- **No new hardware budget:** Partner Interconnect lets you connect via a **supported service provider/partner** that already has connectivity into Google’s network — you don’t need to install new Google-facing colo hardware yourself.  
- **Private RFC1918 connectivity:** Partner Interconnect supports **private connectivity** to your VPC (using VLAN attachments and Cloud Router with BGP), so on-prem RFC1918 addresses can route to GKE/private cloud resources securely and privately.  
- **Better than best-effort Internet:** The service provider partner can offer **SLA-backed connectivity** options (and typically better performance and predictability than “basic Internet”), addressing the customer’s concern that basic Internet is best-effort with no SLA.  
- **Scalability:** Partner Interconnect is designed to scale (you can order multiple VLAN attachments/partner circuits) to accommodate future throughput growth without the upfront capital expense of Dedicated Interconnect.

**Why the other options are not appropriate**

- **A — Carrier Peering:** Carrier (or Public) Peering is for exchanging **public Internet** traffic with Google (reachability to Google’s public IPs and services). It does **not** provide private RFC1918 connectivity into a VPC and is therefore not suitable for on-prem → GKE private IP communication.  
- **B — New Internet connection:** A plain Internet connection is **best-effort** and has no guaranteed SLA; it also exposes traffic to the public Internet (unless you build VPN on top). The scenario specifically calls out that the provider labels Internet as best-effort, so this fails the reliability/security requirement.  
- **D — Dedicated Interconnect:** Dedicated Interconnect provides a private, high-capacity connection but **requires colocation at a Google edge/colo** and physical cross-connects; because the site is remote (>100 km from a PoP) and they can’t afford new hardware/colocation, Dedicated Interconnect is not feasible.

---

## Implementation notes (practical next steps)
- Engage a **Partner Interconnect** service provider that has presence reachable from the EHR data center.  
- Configure **VLAN attachments** from the provider into your Google Cloud project and attach them to the appropriate VPC.  
- Use **Cloud Router + BGP** to exchange routes dynamically between on-prem RFC1918 subnets and the VPC (GKE private clusters or node pools).  
- Consider **redundant partner circuits** (different providers or diverse paths) for high availability.  
- If encryption is required and the provider does not encrypt by default, consider IPsec tunnels on top of the partner link or ensure provider offers an encrypted service.

---

**Final Answer:** **C — Provision a Partner Interconnect connection.**

---

# 3. Google Cloud Case Study — Data Residency for Cloud Storage

## Question

One of EHR’s healthcare customers is an internationally renowned research and hospital facility. Many of their patients are well-known public personalities. Sources both inside and outside have tried many times to obtain health information on these patients for malicious purposes. The hospital requires that patient information stored in Cloud Storage buckets not leave the geographic areas in which the buckets are hosted. You need to ensure that information stored in Cloud Storage buckets in the "europe-west2" region does not leave that area. What should you do?

A. Encrypt the data in the application on-premises before the data is stored in the "europe-west2" region.  
B. Enable Virtual Private Cloud Service Controls, and create a service perimeter around the Cloud Storage resources.  
C. Assign the Identity and Access Management (IAM) "storage.objectViewer" role only to users and service accounts that need to use the data.  
D. Create an access control list (ACL) that limits access to the bucket to authorized users only, and apply it to the buckets in the "europe-west2" region.  

---

## ✅ **Correct Answer: B**  
**Enable Virtual Private Cloud Service Controls, and create a service perimeter around the Cloud Storage resources.**

---

## Explanation

**Why B is correct**
- **VPC Service Controls (VPC-SC)** are designed to prevent data exfiltration from Google Cloud services by defining a *service perimeter* around the resources (for example, Cloud Storage buckets).  
- With a properly configured perimeter and *access levels*, API calls that would move or access data from outside the allowed boundary (for example, from other regions or from the public Internet) are blocked.  
- This directly addresses the requirement to **ensure Cloud Storage data does not leave the geographic area** by preventing access or API-based transfers from resources or principals outside the perimeter.

**Implementation / defense-in-depth suggestions**
- Put the buckets in the `europe-west2` region (set bucket location) to ensure data is stored in the desired region.  
- Configure a **VPC-SC service perimeter** that includes the Cloud Storage resources and any projects that should legitimately access them.  
- Use **Access Context Manager** access levels (IP, device, or identity-based conditions) to restrict which sources can call the Storage APIs from inside the perimeter.  
- Combine VPC-SC with **least-privilege IAM** (grant only the roles needed) and, if desired, **Customer-Managed Encryption Keys (CMEK)** whose keys are also restricted to the same region for stronger guarantees.

**Why the other options are not sufficient**
- **A (Encrypt data on-premises before upload):** Encryption before upload protects confidentiality but does **not** prevent the stored objects from being copied or accessed and moved out of the region (an attacker could exfiltrate the ciphertext). It does not enforce geographic residency.  
- **C (Only assign `storage.objectViewer` to needed principals):** Least-privilege IAM helps reduce who can access objects but **does not prevent** authorized principals from copying data out of the region or from applications in other regions that are allowed to access the data. IAM alone does not enforce geographic data residency.  
- **D (Use ACLs to limit access):** ACLs control who can read/write objects but **do not stop** API or network-level egress from occurring when an allowed principal or compromised account accesses and then transfers the data outside the region. ACLs are not a mechanism to enforce where data may be used or transferred geographically.

---

## Short checklist to meet the requirement
- [x] Set Cloud Storage bucket location to `europe-west2`.  
- [x] Enable and configure **VPC Service Controls** with a perimeter that includes the buckets and allowed projects.  
- [x] Define **Access Context Manager** access levels (IP ranges, device posture, or identity conditions) for stricter access control.  
- [x] Apply least-privilege **IAM** and consider **CMEK** in the same region for additional protection.

**Final answer:** **B**

---

# 4. Google Cloud Case Study — BeyondCorp Migration for Sales Employees

## Question

The EHR sales employees are a remote-based workforce that travels to different locations to do their jobs. Regardless of their location, the sales employees need to access web-based sales tools located in the EHR data center. EHR is retiring their current Virtual Private Network (VPN) infrastructure, and you need to move the web-based sales tools to a BeyondCorp access model. Each sales employee has a Google Workspace account and uses that account for single sign-on (SSO). What should you do?

A. Create an Identity-Aware Proxy (IAP) connector that points to the sales tool application.  
B. Create a Google group for the sales tool application, and upgrade that group to a security group.  
C. Deploy an external HTTP(S) load balancer and create a custom Cloud Armor policy for the sales tool application.  
D. For every sales employee who needs access to the sales tool application, give their Google Workspace user account the predefined AppEngine Viewer role.  

---

## ✅ **Correct Answer: A**  
**Create an Identity-Aware Proxy (IAP) connector that points to the sales tool application.**

---

## Explanation

### Why A is correct
- **BeyondCorp** is Google’s zero-trust model that replaces network-based VPN access with identity- and context-based access controls.  
- **Identity-Aware Proxy (IAP)** enforces access to web apps based on the user’s Google identity (Google Workspace SSO) and context (device, IP, access level) without requiring a VPN.  
- An **IAP connector** (or IAP tunnel for on-prem/back-end apps) allows you to protect on-premises web apps or apps not directly exposed to the internet by terminating authentication/authorization at Google’s edge and proxying authorized requests to the internal application. This aligns exactly with the requirement to remove VPN and use BeyondCorp while keeping the application reachable only via secure, identity-checked access.
- IAP integrates directly with Google Workspace SSO and allows you to grant access using IAM (for example by assigning the `roles/iap.httpsResourceAccessor` role to a Google Group containing the sales employees), making administration straightforward and auditable.

### Why the other options are not suitable
- **B — Create/upgrade a Google group:** A Google group is useful for managing identities, but **by itself** it doesn’t enforce zero-trust access to the application. Groups are part of the solution (used to assign IAP/IAM roles), not the enforcement mechanism.
- **C — External HTTP(S) load balancer + Cloud Armor:** This provides secure, scalable edge termination and DDoS/WAF protections but **does not provide identity-aware access controls** (BeyondCorp). You’d still need IAP or another identity gate to replace VPN.
- **D — Grant AppEngine Viewer role to users:** `AppEngine Viewer` is an IAM role for GCP resource visibility and is unrelated to application-level access or web SSO. It is not an appropriate or secure way to grant end-user access to an on-prem web application and violates least-privilege principles.

---

## Implementation notes (practical steps)
1. Register the sales application with IAP (choose the appropriate IAP type for your backend: App Engine / Compute Engine / Cloud Run / IAP for TCP forwarding or use the IAP connector/tunnel for on-prem apps).  
2. Configure OAuth client credentials for IAP so users authenticate via Google Workspace SSO.  
3. Create a Google Group containing all sales employees (convenient for lifecycle management) and grant that group the `roles/iap.httpsResourceAccessor` (or equivalent IAP role) on the IAP-protected resource.  
4. Optionally add context-aware access rules (Access Context Manager) to enforce device posture, IP range, or other conditions as part of the BeyondCorp model.  
5. Monitor and audit access with Cloud Audit Logs and IAP logs.

**Final answer:** **A**

---

# 5. Google Cloud Case Study — Protecting and Managing PII for Mountkirk Games

## Question

You are the data compliance officer for Mountkirk Games and must protect customers' personally identifiable information (PII). Mountkirk Games wants to make sure they can generate anonymized usage reports about their new game and delete PII data after a specific period of time. The solution should have minimal cost. You need to ensure compliance while meeting business and technical requirements. What should you do?

A. Archive audit logs in Cloud Storage, and manually generate reports.  
B. Write a Cloud Logging filter to export specific date ranges to Pub/Sub.  
C. Archive audit logs in BigQuery, and generate reports using Google Data Studio.  
D. Archive user logs on a locally attached persistent disk, and cat them to a text file for auditing.  

---

## ✅ **Correct Answer: C**  
**Archive audit logs in BigQuery, and generate reports using Google Data Studio.**

---

## Explanation

### Why **C** is correct
- **BigQuery** provides a **low-cost, serverless, and scalable** data warehouse for storing and analyzing structured or semi-structured logs, including those containing user data or anonymized fields.  
- You can configure **Cloud Logging sinks** to **export logs to BigQuery** in near real-time, allowing you to query and analyze usage data efficiently.  
- With **BigQuery**, you can:
  - Use SQL queries to **anonymize or pseudonymize** data (e.g., remove or hash PII fields).
  - Apply **data retention policies** to automatically delete records after a specified time period (fulfilling the compliance requirement to delete PII).  
  - Integrate directly with **Google Data Studio (now Looker Studio)** for **visual, real-time reporting** without extra cost for visualization.  

This provides a **compliant, cost-effective, and automated** solution for generating anonymized reports and managing data lifecycle.

---

### Why the other options are not suitable

**A. Archive audit logs in Cloud Storage, and manually generate reports**  
- Storing logs in Cloud Storage is cheap, but generating reports manually is inefficient and prone to error.  
- No built-in capability for automated anonymization, analytics, or data lifecycle management.  

**B. Write a Cloud Logging filter to export specific date ranges to Pub/Sub**  
- Pub/Sub is for **streaming/event-driven pipelines**, not for long-term storage or analytical reporting.  
- You’d still need another system (e.g., Dataflow, BigQuery) to process and store logs, which increases complexity and cost.  

**D. Archive user logs on a locally attached persistent disk, and cat them to a text file for auditing**  
- This is not scalable, secure, or compliant.  
- No automatic anonymization, no lifecycle management, and no centralized analysis capabilities.  
- Persistent disks also incur costs and are not suitable for large or long-term data storage.  

---

## Implementation Steps (Summary)

1. **Create a Cloud Logging sink** to export relevant game usage logs to **BigQuery**.  
2. In **BigQuery**, use SQL queries or scheduled jobs to:
   - Remove or hash PII fields for anonymization.  
   - Configure **table expiration times** to automatically delete logs after the compliance period.  
3. Connect **Google Data Studio** (Looker Studio) to the BigQuery dataset to build dashboards and generate anonymized usage reports.  
4. Use **IAM and audit logs** to ensure only authorized users can access sensitive datasets.  

---

## ✅ Final Answer
**C. Archive audit logs in BigQuery, and generate reports using Google Data Studio.**

---

# 6. Google Cloud Case Study — Operating Mountkirk Games Platform Securely and Efficiently

## Question

Mountkirk Games wants you to make sure their new gaming platform is being operated according to Google best practices. You want to verify that Google-recommended security best practices are being met while also providing the operations teams with the metrics they need. What should you do? (Choose two)

A. Ensure that you aren’t running privileged containers.  
B. Ensure that you are using obfuscated Tags on workloads.  
C. Ensure that you are using the native logging mechanisms.  
D. Ensure that workloads are not using securityContext to run as a group.  
E. Ensure that each cluster is running GKE metering so each team can be charged for their usage.  

---

## ✅ **Correct Answers: A and C**

---

## Explanation

### **A. Ensure that you aren’t running privileged containers.**
- Running containers in **privileged mode** grants them unrestricted access to the host system, which violates **Google Cloud security best practices**.
- Google recommends **principle of least privilege** — containers should have only the permissions they need.  
- Disabling privileged mode helps prevent **container escape attacks** and reduces the potential blast radius in case of compromise.  
- Security scanning tools like **GKE Security Posture dashboard**, **Binary Authorization**, and **Policy Controller** (OPA/Gatekeeper) can enforce this policy.

### **C. Ensure that you are using the native logging mechanisms.**
- Using **Google Cloud’s native logging mechanisms** (such as **Cloud Logging** and **Cloud Monitoring**, previously Stackdriver) ensures:
  - Comprehensive **visibility into application and cluster activity**.
  - **Integration** with Google’s monitoring, alerting, and auditing systems.
  - **Centralized metrics** collection for operations teams to track performance, health, and cost trends.
- This is a Google-recommended best practice for **observability and compliance**.

---

## ❌ Why the other options are not correct

### **B. Ensure that you are using obfuscated Tags on workloads.**
- Google Cloud does **not recommend obfuscating workload tags**.  
- Labels and tags should be meaningful and descriptive (e.g., team, environment, app) for **billing, troubleshooting, and monitoring** purposes.

### **D. Ensure that workloads are not using securityContext to run as a group.**
- The `securityContext` field is not inherently insecure; it allows configuring security options like user/group IDs, privilege levels, or capabilities.  
- Running as a group (`runAsGroup`) is **not a risk by itself**; in fact, properly setting user/group IDs is part of a secure configuration.

### **E. Ensure that each cluster is running GKE metering so each team can be charged for their usage.**
- **GKE usage metering** helps with cost allocation, not with security or operational best practices.
- While it’s good for cost tracking, it does not address **security validation** or **operations metrics** requirements mentioned in the question.

---

## ✅ Final Answers
**A. Ensure that you aren’t running privileged containers.**  
**C. Ensure that you are using the native logging mechanisms.**

---

# 7. Google Cloud Case Study — Implementing VPC Service Controls for Mountkirk Games

## Question

You need to implement **Virtual Private Cloud (VPC) Service Controls** for Mountkirk Games. Mountkirk Games wants to allow **Cloud Shell usage** by its developers. Developers should **not have full access to managed services**. You need to balance these conflicting goals with Mountkirk Games’ business requirements. What should you do?

A. Use VPC Service Controls for the entire platform.  
B. Prioritize VPC Service Controls implementation over Cloud Shell usage for the entire platform.  
C. Include all developers in an access level associated with the service perimeter, and allow them to use Cloud Shell.  
D. Create a service perimeter around only the projects that handle sensitive data, and do not grant your developers access to it.  

---

## ✅ **Correct Answer: D**

---

## Explanation

### **D. Create a service perimeter around only the projects that handle sensitive data, and do not grant your developers access to it.**
- **VPC Service Controls** are designed to reduce the risk of **data exfiltration** from sensitive managed services such as **Cloud Storage**, **BigQuery**, and **Cloud SQL**.
- Applying service perimeters only to **projects that contain sensitive data** follows Google’s best practice of **scoping security controls narrowly**.
- This approach allows developers to continue using **Cloud Shell**, which runs in Google-managed projects and often conflicts with strict service perimeters.
- By not granting developers access to the sensitive service perimeter, Mountkirk Games:
  - Protects critical data.
  - Avoids breaking developer workflows.
  - Prevents developers from having unrestricted access to protected managed services.
- This solution achieves the required balance between **security** and **developer productivity**.

---

## ❌ Why the other options are not correct

### **A. Use VPC Service Controls for the entire platform.**
- Applying VPC Service Controls to the entire platform is **overly restrictive**.
- This would likely interfere with **Cloud Shell access** and slow down development.
- Google recommends limiting service perimeters to **sensitive workloads**, not all projects.

### **B. Prioritize VPC Service Controls implementation over Cloud Shell usage for the entire platform.**
- This ignores a key business requirement: developers **must be able to use Cloud Shell**.
- Google Cloud best practices emphasize balancing **security with usability**, not sacrificing developer efficiency.

### **C. Include all developers in an access level associated with the service perimeter, and allow them to use Cloud Shell.**
- Cloud Shell does not operate inside customer-defined VPC Service Control perimeters.
- Granting developers access levels tied to the perimeter may unintentionally provide **broader access to sensitive managed services**, violating the requirement.

---

## ✅ Final Answer
**D. Create a service perimeter around only the projects that handle sensitive data, and do not grant your developers access to it.**

---

# 8. Google Cloud Case Study — Designing SLOs for a Public Beta Game

## Question

Your new game running on Google Cloud is in **public beta**, and you want to design **meaningful service level objectives (SLOs)** before the game becomes **generally available**. What should you do?

A. Define one SLO as 99.9% game server availability. Define the other SLO as less than 100-ms latency.  
B. Define one SLO as service availability that is the same as Google Cloud's availability. Define the other SLO as 100-ms latency.  
C. Define one SLO as 99% HTTP requests return the 2xx status code. Define the other SLO as 99% requests return within 100 ms.  
D. Define one SLO as total uptime of the game server within a week. Define the other SLO as the mean response time of all HTTP requests that are less than 100 ms.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Define one SLO as 99% HTTP requests return the 2xx status code. Define the other SLO as 99% requests return within 100 ms.**
- Google SRE best practices recommend defining SLOs using **user-focused, measurable indicators**, known as **Service Level Indicators (SLIs)**.
- HTTP success rate (2xx responses) directly reflects **user-perceived availability**, not just whether servers are running.
- Latency-based SLOs should use **percentiles** (e.g., 99% of requests) rather than averages to account for tail latency, which greatly affects user experience.
- These SLOs are:
  - **Quantifiable and observable**
  - Closely aligned with **real user experience**
  - Appropriate for a **public beta**, where learning from real traffic is critical before GA

---

## ❌ Why the other options are not correct

### **A. Define one SLO as 99.9% game server availability. Define the other SLO as less than 100-ms latency.**
- Server availability does not necessarily reflect **end-user experience**.
- The latency SLO is vague and does not specify a **measurement method** (e.g., percentiles).
- Google recommends avoiding infrastructure-only metrics for SLOs.

### **B. Define one SLO as service availability that is the same as Google Cloud's availability. Define the other SLO as 100-ms latency.**
- You should not base your SLOs on **Google Cloud’s platform SLAs**, as they do not represent your application’s behavior.
- Latency is again undefined in terms of **percentiles or request success criteria**.

### **D. Define one SLO as total uptime of the game server within a week. Define the other SLO as the mean response time of all HTTP requests that are less than 100 ms.**
- **Total uptime** is an infrastructure metric, not a user-centric SLI.
- **Mean (average) latency** hides outliers and does not capture poor tail performance.
- Google SRE explicitly discourages using averages for latency SLOs.

---

## ✅ Final Answer
**C. Define one SLO as 99% HTTP requests return the 2xx status code. Define the other SLO as 99% requests return within 100 ms.**

---

# 9. Google Cloud Case Study — Expanding HRL Video Content to Emerging Regions

## Question

HRL wants you to help them bring **existing recorded video content** to **new fans in emerging regions**. Considering the **HRL business and technical requirements**, what should you do?

A. Serve the video content directly from a multi-region Cloud Storage bucket.  
B. Use Cloud CDN to cache the video content from HRL’s existing public cloud provider.  
C. Use Apigee Edge to cache the video content from HRL’s existing public cloud provider.  
D. Replicate the video content in Google Kubernetes Engine clusters in regions close to the fans.  

---

## ✅ **Correct Answer: B**

---

## Explanation

### **B. Use Cloud CDN to cache the video content from HRL’s existing public cloud provider.**
- **Cloud CDN** is designed to deliver **static and cacheable content** such as recorded video with **low latency** and **high throughput**.
- It leverages Google’s **global edge network**, bringing content closer to users in emerging regions without requiring full infrastructure deployment.
- Cloud CDN can cache content from an **external (non-Google Cloud) origin**, allowing HRL to:
  - Reuse its **existing public cloud provider**.
  - Avoid costly data migration or duplication.
  - Scale globally with minimal operational overhead.
- This approach aligns with HRL’s likely goals of **fast global reach**, **cost efficiency**, and **minimal architectural changes**.

---

## ❌ Why the other options are not correct

### **A. Serve the video content directly from a multi-region Cloud Storage bucket.**
- This would require **migrating or duplicating** all existing video content into Google Cloud.
- It introduces unnecessary data transfer costs and operational effort.
- It does not leverage HRL’s **existing content hosting environment**.

### **C. Use Apigee Edge to cache the video content from HRL’s existing public cloud provider.**
- **Apigee Edge** is an **API management platform**, not a content delivery network.
- While it can cache API responses, it is **not optimized for large video streaming workloads**.
- Using Apigee for video delivery would be inefficient and costly.

### **D. Replicate the video content in Google Kubernetes Engine clusters in regions close to the fans.**
- Running video content in GKE clusters adds significant **operational complexity**.
- This approach is expensive and unnecessary compared to a CDN.
- Kubernetes is not intended to function as a **global video distribution platform**.

---

## ✅ Final Answer
**B. Use Cloud CDN to cache the video content from HRL’s existing public cloud provider.**

---

# 10. Google Cloud Case Study — Protecting PII While Personalizing Recommendations for TerramEarth

## Question

You are the **data compliance officer** for TerramEarth and must protect customers' **personally identifiable information (PII)**, such as **credit card information**. TerramEarth wants to **personalize product recommendations** for its large industrial customers. You need to **respect data privacy** and still **deliver a usable solution**. What should you do?

A. Use AutoML to provide data to the recommendation service.  
B. Process PII data on-premises to keep the private information more secure.  
C. Use the Cloud Data Loss Prevention (DLP) API to provide data to the recommendation service.  
D. Manually build, train, and test machine learning models to provide product recommendations anonymously.  

---

## ✅ **Correct Answer: C**

---

## Explanation

### **C. Use the Cloud Data Loss Prevention (DLP) API to provide data to the recommendation service.**
- **Cloud DLP API** is specifically designed to **detect, classify, and protect sensitive data** such as PII (e.g., credit card numbers, names, IDs).
- It can **mask, tokenize, or redact** sensitive fields before data is used by downstream services like **machine learning recommendation engines**.
- This enables TerramEarth to:
  - Maintain **data privacy and regulatory compliance**.
  - Still use customer data in an **anonymized or de-identified form**.
  - Safely leverage Google Cloud analytics and ML services for personalization.
- This approach aligns with Google Cloud best practices for **privacy-preserving analytics**.

---

## ❌ Why the other options are not correct

### **A. Use AutoML to provide data to the recommendation service.**
- **AutoML** builds machine learning models but does **not protect or anonymize PII** by itself.
- Sensitive data would still be exposed unless additional privacy controls are applied.

### **B. Process PII data on-premises to keep the private information more secure.**
- Processing data on-premises does not automatically ensure better security.
- This approach limits scalability and does not address the need for **privacy-aware data sharing** with cloud-based recommendation services.

### **D. Manually build, train, and test machine learning models to provide product recommendations anonymously.**
- Manually anonymizing data is error-prone and difficult to maintain at scale.
- Google Cloud provides **managed services (Cloud DLP)** specifically designed to handle this requirement more securely and efficiently.

---

## ✅ Final Answer
**C. Use the Cloud Data Loss Prevention (DLP) API to provide data to the recommendation service.**

