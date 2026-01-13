# Helicopter Racing League

Helicopter Racing League (HRL) is a global sports league for competitive helicopter racing. Each year, HRL holds the world championship and several regional league competitions where teams compete to earn a spot in the world championship. HRL offers a paid service to stream the races all over the world with live telemetry and predictions throughout each race.

HRL wants to migrate their existing service to a new platform to expand their use of managed AI and ML services to facilitate race predictions. Additionally, as new fans engage with the sport, particularly in emerging regions, they want to move the serving of their content, both real-time and recorded, closer to their users.

HRL is a public cloud-first company; the core of their mission-critical applications runs on their current public cloud provider. Video recording and editing is performed at the race tracks, and the content is encoded and transcoded, where needed, in the cloud.

---

## Company Overview

Existing content is stored in an object storage service on their existing public cloud provider.  
Video encoding and transcoding is performed on VMs created for each job.  
Race predictions are performed using TensorFlow running on VMs in the current public cloud provider.

---

## Solution Concept

- Migrate the existing service to a new platform with managed AI/ML services for race predictions.
- Serve content closer to users, reducing latency for emerging markets.
- Enhance global availability and quality of broadcasts.
- Increase the number of concurrent viewers.

---

## Existing Technical Environment

- Enterprise-grade connectivity and local compute is provided by truck-mounted mobile data centers.
- Race prediction services are hosted exclusively on the existing public cloud provider.
- Video encoding and transcoding is performed on VMs created per job.
- Predictive models run using TensorFlow on VMs.

---

## Business Requirements

- Support the ability to expose predictive models to partners.
- Increase predictive capabilities during and before races:
  - Race results
  - Mechanical failures
  - Crowd sentiment
- Increase telemetry and create additional insights.
- Measure fan engagement with new predictions.
- Enhance global availability and quality of broadcasts.
- Increase the number of concurrent viewers.
- Expand predictive capabilities and reduce latency in emerging markets.

---

## Technical Requirements

- Maintain or increase prediction throughput and accuracy.
- Reduce viewer latency.
- Increase transcoding performance.
- Create real-time analytics of viewer consumption patterns and engagement.
- Create a data mart to enable processing of large volumes of race data.
- Minimize operational complexity.
- Ensure compliance with regulations.
- Create a merchandising revenue stream.

---

## Executive Statement

> Our CEO, S. Hawke, wants to bring high-adrenaline racing to fans all around the world. We listen to our fans, and they want enhanced video streams that include predictions of events within the race (e.g., overtaking). Our current platform allows us to predict race outcomes but lacks the facility to support real-time predictions during races and the capacity to process season-long results.

---

# 1. Payment Card Tokenization Storage (HRL Case Study)

## Question

Refer to the **Helicopter Racing League (HRL)** case study.  
Your team is responsible for building a **payment card data vault** for credit card numbers used to bill:

- Tens of thousands of viewers  
- Merchandise buyers  
- Season ticket holders  

You must implement a **custom card tokenization service** that meets these requirements:

- **Low latency** at **minimal cost**  
- Must **identify duplicate credit cards**  
- Must **not store plaintext card numbers**  
- Must support **annual encryption key rotation**

Which **storage approach** should you adopt?

A. Store the card data in Secret Manager after running a query to identify duplicates.  
B. Encrypt the card data with a deterministic algorithm stored in Firestore using Datastore mode.  
C. Encrypt the card data with a deterministic algorithm and shard it across multiple Memorystore instances.  
D. Use column-level encryption to store the data in Cloud SQL.  

---

## ✅ Correct Answer

**B. Encrypt the card data with a deterministic algorithm stored in Firestore using Datastore mode**

---

## Explanation

This solution must balance **security, cost, performance, and deduplication**.

### Why **Deterministic Encryption** 🔐

- Deterministic encryption ensures:
  - The **same card number always produces the same ciphertext**
  - This allows **duplicate detection** without ever storing plaintext card numbers

---

### Why **Firestore (Datastore mode)** 🗄️

Firestore in Datastore mode provides:

- **Low-latency key-value access**
- **High availability and horizontal scaling**
- **Low operational overhead**
- Ability to index and query encrypted values for **duplicate detection**
- **Cost-efficient** for large numbers of small records

This makes it ideal for a **token vault**.

---

### Why **Key Rotation is supported**

Using deterministic encryption with **Cloud KMS**:
- Encryption keys can be rotated annually
- You can re-encrypt stored values in the background without downtime

---

## ❌ Why the other options are wrong

### **A. Secret Manager**
- Not designed for **large datasets**
- Cannot efficiently **query for duplicates**
- **Very expensive** at scale

---

### **C. Memorystore**
- **In-memory only**, not durable
- **High cost** for large datasets
- Risk of **data loss**

---

### **D. Cloud SQL**
- Higher **cost and operational overhead**
- Column encryption does **not allow duplicate detection**
- Requires decrypting data to compare

---

## ✅ Final Answer

**B**

---

# 2. Allowing Fastly CDN IPs (HRL Case Study)

## Question

For this question, refer to the **Helicopter Racing League (HRL)** case study. Recently HRL started a new regional racing league in **Cape Town, South Africa**. In an effort to give customers in Cape Town a better user experience, HRL has partnered with the **Content Delivery Network provider, Fastly**. HRL needs to allow traffic coming from **all of the Fastly IP address ranges** into their **Virtual Private Cloud network (VPC network)**. You are a member of the HRL security team and you need to configure the update that will allow **only the Fastly IP address ranges** through the **External HTTP(S) load balancer**. Which command should you use?

a.  
```bash
    gcloud compute security-policies rules update 1000
--security-policy from-fastly
--src-ip-ranges *
--action "allow"
```

b. 
```bash
gcloud compute firewall rules update sourceiplist-fastly
--priority 1000
--allow tcp:443
```

c. 
```bash
gcloud compute firewall rules update hlr-policy
--priority 1000
--target-tags=sourceiplist-fastly
--allow tcp:443
```

d.
```bash
gcloud compute security-policies rules update 1000
--security-policy hlr-policy
--expression "evaluatePreconfiguredExpr('sourceiplist-fastly')"
--action "allow"
```


---

## ✅ Correct Answer

**d**

---

## Explanation

Fastly publishes its IP ranges as a **Google-managed source IP list** called:
```text
sourceiplist-fastly
```


Because HRL is using an **External HTTP(S) Load Balancer**, traffic filtering must be enforced using **Cloud Armor security policies**, not VPC firewall rules.

The expression:

```text
evaluatePreconfiguredExpr('sourceiplist-fastly')
```


matches requests coming **only from Fastly’s IP address ranges**, ensuring that only Fastly traffic is allowed through the load balancer while all other traffic is blocked.

---

## ❌ Why the other options are wrong

**a.** Allows traffic from everyone (`*`), not just Fastly.  
**b.** Firewall rules do not control traffic at the External HTTP(S) load balancer.  
**c.** Target tags apply to VM instances, not to load balancer traffic.

---

## ✅ Final Answer

**d**

---

# 3. Automating Airwolf Penetration Test (HRL Case Study)

## Question

For this question, refer to the **Helicopter Racing League (HRL)** case study. The HRL development team releases a new version of their **predictive capability application** every Tuesday evening at 3 a.m. UTC to a repository. The security team at HRL has developed an in-house penetration test **Cloud Function** called **Airwolf**.  

The security team wants to run **Airwolf** against the predictive capability application **as soon as it is released every Tuesday**. You need to set up Airwolf to run at the recurring weekly cadence. What should you do?

a. Set up **Cloud Tasks** and a **Cloud Storage bucket** that triggers a Cloud Function.  
b. Set up a **Cloud Logging sink** and a **Cloud Storage bucket** that triggers a Cloud Function.  
c. Configure the **deployment job** to notify a **Pub/Sub queue** that triggers a Cloud Function.  
d. Set up **Identity and Access Management (IAM)** and **Confidential Computing** to trigger a Cloud Function.  

---

## ✅ Correct Answer

**c. Configure the deployment job to notify a Pub/Sub queue that triggers a Cloud Function.**

---

## Explanation

To automatically run **Airwolf** every time the new application version is released:

1. The deployment job is the **source of truth** for new releases.  
2. By notifying a **Pub/Sub topic** whenever a new release occurs:
   - Pub/Sub acts as an **event-driven trigger**.  
   - The Cloud Function (**Airwolf**) subscribes to this topic.  
   - Airwolf runs **immediately after each release**, fulfilling the weekly cadence automatically.  

This is a **scalable, event-driven, and serverless approach** that does not require polling or scheduling jobs separately.

---

## ❌ Why the other options are wrong

**a. Cloud Tasks + Cloud Storage:**  
- Cloud Tasks is for **queued task execution**, not event-based release triggers.  
- Cloud Storage would only trigger on **object changes**, not deployment events.

**b. Cloud Logging sink + Cloud Storage:**  
- Cloud Logging sinks export logs, not release events.  
- Does not guarantee the function runs immediately after deployment.

**d. IAM + Confidential Computing:**  
- IAM controls permissions; Confidential Computing secures compute.  
- Neither can trigger a function on deployment events.

---

## ✅ Final Answer

**c**

---

# 4. Improving ML Prediction Accuracy and Interpretability (HRL Case Study)

## Question

For this question, refer to the **Helicopter Racing League (HRL)** case study. HRL wants better **prediction accuracy** from their ML prediction models. They want you to use **Google's AI Platform** so HRL can **understand and interpret the predictions**. What should you do?

a. Use **Explainable AI**.  
b. Use **Vision AI**.  
c. Use **Google Cloud's operations suite**.  
d. Use **Jupyter Notebooks**.  

---

## ✅ Correct Answer

**a. Use Explainable AI**

---

## Explanation

To **understand and interpret ML predictions**, HRL needs a solution that provides:

- **Feature attribution** – identifying which inputs most influence the prediction.  
- **Model interpretability** – understanding why the model makes certain decisions.  
- **Actionable insights** – enabling the team to improve accuracy based on model behavior.  

**Explainable AI** (part of Google AI Platform) provides these capabilities:

- Works with **any ML model deployed on AI Platform**.  
- Offers **feature importance explanations**.  
- Supports both **structured and unstructured data**.  

This helps HRL improve **prediction accuracy** by understanding **how predictions are generated**.

---

## ❌ Why the other options are wrong

**b. Vision AI:**  
- Specialized for **image recognition tasks**, not general model interpretability.  

**c. Google Cloud's operations suite:**  
- Monitors infrastructure and application performance, not ML predictions.  

**d. Jupyter Notebooks:**  
- Useful for development and experimentation but does **not provide production-ready interpretability or feature explanations**.

---

## ✅ Final Answer

**a**

---

# 5. Cost-Effective Storage for Race Data (HRL Case Study)

## Question

For this question, refer to the **Helicopter Racing League (HRL)** case study. HRL is looking for a **cost-effective approach** for storing their **race data** such as telemetry. They want to:

- Keep **all historical records**  
- Train models using **only the previous season's data**  
- Plan for **data growth** in terms of **volume** and **information collected**  

You need to propose a **data solution**. Considering HRL business requirements and the goals expressed by CEO S. Hawke, what should you do?

a. Use **Firestore** for its scalable and flexible document-based database. Use collections to aggregate race data by season and event.  
b. Use **Cloud Spanner** for its scalability and ability to version schemas with zero downtime. Split race data using season as a primary key.  
c. Use **BigQuery** for its scalability and ability to add columns to a schema. Partition race data based on season.  
d. Use **Cloud SQL** for its ability to automatically manage storage increases and compatibility with MySQL. Use separate database instances for each season.  

---

## ✅ Correct Answer

**c. Use BigQuery for its scalability and ability to add columns to a schema. Partition race data based on season.**

---

## Explanation

**Why BigQuery?**  

HRL’s requirements focus on **historical data analysis** and **model training**, which aligns with **data warehousing**:

- **BigQuery** is **serverless and highly scalable**, ideal for **large datasets** like telemetry.  
- Supports **schema evolution**, allowing **new columns** to be added as more telemetry is collected.  
- **Partitioning by season** enables:
  - Efficient queries on **only the previous season's data**  
  - Reduced storage and query costs  
  - Easy management of growing historical datasets  

This approach **balances cost, scalability, and analytical performance**.

---

## ❌ Why the other options are wrong

**a. Firestore:**  
- Optimized for **OLTP workloads**, not **analytical queries** or historical data aggregation.  
- Not cost-efficient for **large-scale historical telemetry**.

**b. Cloud Spanner:**  
- Excellent for **highly transactional workloads** and schema evolution.  
- Overkill for **analytical queries** and **historical telemetry analysis**.  
- More expensive than BigQuery for this use case.

**d. Cloud SQL:**  
- Traditional relational database, limited **scalability** for large telemetry datasets.  
- Managing **separate databases per season** increases operational complexity and cost.

---

## ✅ Final Answer

**c**

---

# 6. Identifying Idle Compute Engine Instances (HRL Case Study)

## Question

For this question, refer to the **Helicopter Racing League (HRL)** case study. A recent **finance audit** of HRL’s cloud infrastructure noted an exceptionally high number of **Compute Engine instances** allocated to perform **video encoding and transcoding**. You suspect that these **Virtual Machines (VMs) are zombie machines** that were not deleted after their workloads completed. You need to **quickly get a list of which VM instances are idle**. What should you do?

a. Log into each Compute Engine instance and collect disk, CPU, memory, and network usage statistics for analysis.  
b. Use the `gcloud compute instances list` to list the virtual machine instances that have the `idle: true` label set.  
c. Use the `gcloud recommender` command to list the idle virtual machine instances.  
d. From the Google Console, identify which Compute Engine instances in the managed instance groups are no longer responding to health check probes.  

---

## ✅ Correct Answer

**c. Use the gcloud recommender command to list the idle virtual machine instances.**

---

## Explanation

**Why gcloud recommender?**  

- Google Cloud **Recommender** provides **intelligent recommendations** for idle or underutilized resources.  
- The `compute-idle-vm-recommender` identifies **idle Compute Engine instances** that have **low CPU, memory, and network usage** over a period of time.  
- This allows you to **quickly detect and clean up zombie VMs** without manual inspection.  

**Advantages:**
- Automated and fast for **large fleets of VMs**  
- Reduces **operational overhead**  
- Helps **optimize cost** by identifying unused resources  

---

## ❌ Why the other options are wrong

**a. Manual login and usage collection:**  
- Impractical for **tens or hundreds of VMs**  
- Time-consuming and error-prone  

**b. gcloud compute instances list with idle label:**  
- Only works if **idle VMs were previously labeled**, which may not exist  
- Not a reliable method for discovering zombie machines  

**d. Health check probes in managed instance groups:**  
- Only identifies **unhealthy instances**, not necessarily idle ones  
- Does not cover VMs outside managed instance groups  

---

## ✅ Final Answer

**c**


