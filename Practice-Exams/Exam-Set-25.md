# 🚜 TerramEarth

TerramEarth manufactures **heavy equipment** for the mining and agricultural industries. The company operates through **500+ dealers and service centers in over 100 countries**, with a mission to build products that make customers more productive.

There are currently **2 million TerramEarth vehicles** in operation, with an annual growth rate of **20%**. These vehicles collect telemetry data from numerous sensors during operation.

- A **small subset of critical data** is transmitted in real time to support fleet management.
- The remaining sensor data is **collected, compressed, and uploaded daily** when vehicles return to home base.
- Each vehicle generates approximately **200–500 MB of data per day**.

---

## 🏢 Company Overview

- Vehicle data aggregation and analytics infrastructure runs on **Google Cloud**
- Services customers globally
- Manufacturing plant sensor data is sent to **private data centers**
- Private data centers host **legacy inventory and logistics systems**
- Multiple **network interconnects** connect private data centers to Google Cloud
- Web frontend for dealers and customers runs on **Google Cloud**
- Frontend provides access to **stock management and analytics**

---

## 💡 Solution Concept

- Leverage Google Cloud for large-scale telemetry ingestion and analytics
- Enable predictive maintenance and just-in-time repair workflows
- Gradually modernize legacy systems without operational disruption
- Build a scalable platform for internal and partner innovation

---

## 🧱 Existing Technical Environment

- Google Cloud hosts:
  - Vehicle telemetry aggregation
  - Data analytics workloads
  - Dealer and customer web applications
- Private data centers host:
  - Legacy inventory systems
  - Logistics management platforms
- Hybrid connectivity via **multiple network interconnects**

---

## 📋 Business Requirements

- Predict and detect vehicle malfunctions
- Enable rapid shipment of parts for **just-in-time repairs**
- Decrease cloud operational costs and adapt to **seasonality**
- Improve speed and reliability of the **development workflow**
- Enable remote developers while maintaining strong security
- Create a flexible, scalable platform for **custom APIs** for dealers and partners

---

## ⚙️ Technical Requirements

- Create an **HTTP API abstraction layer** for legacy systems
- Enable gradual cloud migration without disrupting operations
- Modernize all **CI/CD pipelines**
- Support container-based workloads in scalable environments
- Allow secure developer experimentation
- Create a **self-service portal** for:
  - Project creation
  - Resource requests for analytics jobs
  - Centralized API access management
- Use cloud-native **keys and secrets management**
- Optimize for **identity-based access control**
- Improve and standardize tools for:
  - Application monitoring
  - Network monitoring
  - Troubleshooting

---

## 🧑‍💼 Executive Statement

> Our competitive advantage has always been our focus on the customer, with our ability to provide excellent customer service and minimize vehicle downtime.  
>  
> After migrating multiple systems to Google Cloud, we are seeking new ways to deliver best-in-class online fleet management services and improve dealership operations.  
>  
> Our 5-year strategic plan includes building a **partner ecosystem**, enabling **data access**, increasing **autonomous vehicle capabilities**, and creating a clear path to migrate remaining legacy systems to the cloud.


---

# 27. API Design Strategy for TerramEarth

## Question

The **TerramEarth development team** wants to create an **API** to meet the company's **business requirements**.  
You want the development team to focus their effort on **business value** instead of creating and maintaining a **custom framework**.

Which method should they use?

a. Use **Google App Engine with Google Cloud Endpoints**. Focus on an API for **dealers and partners**  
b. Use **Google App Engine with a JAX-RS Jersey Java-based framework**. Focus on an API for the **public**  
c. Use **Google App Engine with the Swagger (Open API Specification) framework**. Focus on an API for the **public**  
d. Use **Google Container Engine** with a **Django Python container**. Focus on an API for the **public**  
e. Use **Google Container Engine** with a **Tomcat container** with the **Swagger (Open API Specification)** framework. Focus on an API for **dealers and partners**

---

## ✅ Correct Answer

**a. Use Google App Engine with Google Cloud Endpoints. Focus on an API for dealers and partners**

---

## Explanation

TerramEarth wants to **maximize developer productivity** and **minimize undifferentiated heavy lifting**.

### Why Google App Engine + Cloud Endpoints is the best choice

- **Fully managed API framework**
- Built-in support for:
  - **Authentication and authorization**
  - **Monitoring and logging**
  - **Quotas and rate limiting**
  - **Client library generation**
- Tight integration with **Google Cloud IAM**
- Allows developers to focus on **business logic**, not infrastructure

Cloud Endpoints is especially well-suited for **partner and dealer APIs**, where:
- Access control is critical
- Stability and governance matter
- You want standardized API management without custom tooling

---

## ❌ Why the other options are wrong

**b. JAX-RS Jersey on App Engine**  
- Requires more **custom framework management**
- Less built-in API management

**c. Swagger on App Engine**  
- Swagger defines APIs, but does not provide **managed API services** like auth, quotas, and monitoring

**d. Django on GKE**  
- Requires managing containers and clusters
- Higher operational overhead

**e. Tomcat on GKE with Swagger**  
- Overly complex
- Requires managing infrastructure and API tooling manually

---

## ✅ Final Answer

**a**

---

# 2. Delegated Authorization for Vehicle Data API

## Question

Your development team has created a **structured API** to retrieve **vehicle data**.  
They want to allow **third parties** to develop tools for **dealerships** that use this **vehicle event data**.  
You want to support **delegated authorization** against this data.

What should you do?

a. Build or leverage an **OAuth-compatible access control system**  
b. Build **SAML 2.0 SSO** compatibility into your authentication system  
c. Restrict data access based on the **source IP address** of the partner systems  
d. Create **secondary credentials** for each dealer that can be given to the trusted third party  

---

## ✅ Correct Answer

**a. Build or leverage an OAuth-compatible access control system**

---

## Explanation

The requirement is to support **delegated authorization**, where a **dealer (resource owner)** can grant a **third-party application** limited access to vehicle event data without sharing credentials.

### Why OAuth is the correct approach 🔐

OAuth is the **industry-standard protocol** for delegated authorization and provides:

- **Token-based access** instead of credential sharing  
- **Scoped permissions** to limit what data third parties can access  
- **Revocable access**, improving security and control  
- Broad support across **APIs, mobile apps, and partner integrations**

This enables dealerships to safely authorize third-party tools while maintaining control over their data.

---

## ❌ Why the other options are wrong

**b. SAML 2.0 SSO**  
- Focused on **authentication**, not API authorization or delegation  

**c. IP-based restrictions**  
- Inflexible and insecure  
- Does not support **user consent or delegation**

**d. Secondary credentials**  
- Encourages **credential sharing**  
- Hard to rotate, audit, and revoke  

---

## ✅ Final Answer

**a**

---

# 3. High-Volume Data Ingestion for Connected Vehicles

## Question

TerramEarth plans to connect **all 20 million vehicles** in the field to the cloud.  
This increases the volume to **20 million 600-byte records per second**, or **40 TB per hour**.

How should you design the **data ingestion**?

a. Vehicles write data directly to **Google Cloud Storage (GCS)**  
b. Vehicles write data directly to **Google Cloud Pub/Sub**  
c. Vehicles stream data directly to **Google BigQuery**  
d. Vehicles continue to write data using the existing system (**FTP**)  

---

## ✅ Correct Answer

**b. Vehicles write data directly to Google Cloud Pub/Sub**

---

## Explanation

The ingestion system must handle:

- **Extremely high throughput** (millions of events per second)  
- **Global scale** and bursty traffic  
- **Reliable, durable ingestion**  
- **Decoupling** producers (vehicles) from downstream consumers  

### Why **Cloud Pub/Sub** is the right choice 🚀

Cloud Pub/Sub is designed for **massive, real-time event ingestion** and provides:

- Automatic **horizontal scaling**
- Support for **millions of messages per second**
- **Durable message storage** with at-least-once delivery
- Ability to **fan out** data to multiple consumers (e.g., Dataflow, BigQuery, storage)
- Loose coupling between vehicles and backend processing systems

This makes Pub/Sub the **standard Google-recommended ingestion layer** for IoT-scale workloads.

---

## ❌ Why the other options are wrong

**a. Writing directly to GCS**  
- Not designed for **high-frequency, small-record ingestion**  
- Poor performance for millions of writes per second  

**c. Streaming directly to BigQuery**  
- BigQuery is an **analytics platform**, not an ingestion buffer  
- Streaming limits and costs make this impractical at this scale  

**d. FTP**  
- Not scalable, reliable, or secure for modern cloud workloads  

---

## ✅ Final Answer

**b**

---

# 4. Reducing Aggregate Reporting Time for TerramEarth

## Question

You analyzed **TerramEarth's business requirement to reduce downtime** and found that they can achieve a majority of time savings by **reducing customers' wait time for parts**.  
You decided to focus on reducing the **3-week aggregate reporting time**.

Which modifications to the company’s processes should you recommend?

a. Migrate from **CSV to binary format**, migrate from **FTP to SFTP transport**, and develop **machine learning analysis of metrics**  
b. Migrate from **FTP to streaming transport**, migrate from **CSV to binary format**, and develop **machine learning analysis of metrics**  
c. Increase fleet **cellular connectivity to 80%**, migrate from **FTP to streaming transport**, and develop **machine learning analysis of metrics**  
d. Migrate from **FTP to SFTP transport**, develop **machine learning analysis of metrics**, and increase **dealer local inventory by a fixed factor**  

---

## ✅ Correct Answer

**b. Migrate from FTP to streaming transport, migrate from CSV to binary format, and develop machine learning analysis of metrics**

---

## Explanation

The primary goal is to **reduce the time it takes to collect, process, and analyze data** so that parts demand can be predicted sooner.

### Why this combination is correct 🚀

- **Streaming transport instead of FTP**
  - Eliminates batch delays
  - Enables **near real-time ingestion and processing**
- **Binary format instead of CSV**
  - Reduces payload size
  - Improves **network efficiency and parsing speed**
- **Machine learning analysis**
  - Enables **predictive insights**
  - Helps forecast parts demand earlier, reducing customer wait time

Together, these changes **directly address the reporting latency bottleneck**.

---

## ❌ Why the other options are wrong

**a. FTP → SFTP**
- Improves security, but **does not reduce latency significantly**

**c. Increasing cellular connectivity**
- Costly and slow to implement
- Does not address the **reporting pipeline inefficiency**

**d. Increasing dealer inventory**
- Treats the symptom, not the root cause
- Increases cost without improving data timeliness

---

## ✅ Final Answer

**b**

---

# 5. Impact of Google Cloud Adoption on TerramEarth Processes

## Question

Which of **TerramEarth's legacy enterprise processes** will experience **significant change** as a result of increased **Google Cloud Platform adoption**?

a. **Opex/capex allocation**, **LAN changes**, **capacity planning**  
b. **Capacity planning**, **TCO calculations**, **opex/capex allocation**  
c. **Capacity planning**, **utilization measurement**, **data center expansion**  
d. **Data center expansion**, **TCO calculations**, **utilization measurement**  

---

## ✅ Correct Answer

**b. Capacity planning, TCO calculations, opex/capex allocation**

---

## Explanation

Moving to Google Cloud fundamentally changes how infrastructure is **planned, financed, and evaluated**.

### Why these processes change significantly ☁️

- **Capacity planning**
  - Shifts from long-term hardware forecasting to **on-demand, elastic scaling**
- **TCO calculations**
  - Move from capital-heavy depreciation models to **usage-based cost modeling**
- **Opex/Capex allocation**
  - Cloud reduces upfront **capex** and increases **operational expenditure (opex)**

These changes require **new financial and operational models** compared to traditional on-premises environments.

---

## ❌ Why the other options are wrong

**a. LAN changes**
- Cloud adoption does not significantly change internal LAN architecture  

**c. Utilization measurement**
- Still relevant, but **less impactful** due to elasticity  

**d. Data center expansion**
- Becomes less important rather than significantly changing  

---

## ✅ Final Answer

**b**

---

# 5. Impact of Google Cloud Adoption on TerramEarth Processes

## Question

Which of **TerramEarth's legacy enterprise processes** will experience **significant change** as a result of increased **Google Cloud Platform adoption**?

a. **Opex/capex allocation**, **LAN changes**, **capacity planning**  
b. **Capacity planning**, **TCO calculations**, **opex/capex allocation**  
c. **Capacity planning**, **utilization measurement**, **data center expansion**  
d. **Data center expansion**, **TCO calculations**, **utilization measurement**  

---

## ✅ Correct Answer

**b. Capacity planning, TCO calculations, opex/capex allocation**

---

## Explanation

Moving to Google Cloud fundamentally changes how infrastructure is **planned, financed, and evaluated**.

### Why these processes change significantly ☁️

- **Capacity planning**
  - Shifts from long-term hardware forecasting to **on-demand, elastic scaling**
- **TCO calculations**
  - Move from capital-heavy depreciation models to **usage-based cost modeling**
- **Opex/Capex allocation**
  - Cloud reduces upfront **capex** and increases **operational expenditure (opex)**

These changes require **new financial and operational models** compared to traditional on-premises environments.

---

## ❌ Why the other options are wrong

**a. LAN changes**
- Cloud adoption does not significantly change internal LAN architecture  

**c. Utilization measurement**
- Still relevant, but **less impactful** due to elasticity  

**d. Data center expansion**
- Becomes less important rather than significantly changing  

---

## ✅ Final Answer

**b**

---

# 6. Cost-Effective Processing of Global Telemetry Data

## Question

TerramEarth's **20 million vehicles** are scattered around the world. Based on the vehicle's location, its **telemetry data** is stored in a **Google Cloud Storage (GCS) regional bucket** (US, Europe, or Asia).  

The CTO has asked you to run a report on the **raw telemetry data** to determine why vehicles are breaking down after 100,000 miles.  
You want to run this job on **all the data** in the **most cost-effective way**.

a. Move all the data into **1 zone**, then launch a **Cloud Dataproc cluster** to run the job  
b. Move all the data into **1 region**, then launch a **Google Cloud Dataproc cluster** to run the job  
c. Launch a **cluster in each region** to preprocess and compress the raw data, then move the data into a **multi-region bucket** and use a **Dataproc cluster** to finish the job  
d. Launch a **cluster in each region** to preprocess and compress the raw data, then move the data into a **regional bucket** and use a **Cloud Dataproc cluster** to finish the job  

---

## ✅ Correct Answer

**d. Launch a cluster in each region to preprocess and compress the raw data, then move the data into a region bucket and use a Cloud Dataproc cluster to finish the job**

---

## Explanation

The goal is to **minimize costs** while processing **large volumes of globally distributed data**.

### Why this approach is cost-effective 💰

1. **Process data locally in each region**
   - Reduces **egress costs** from GCS
   - Avoids unnecessary data transfer between regions

2. **Preprocess and compress data**
   - Minimizes the amount of data moved
   - Speeds up subsequent processing

3. **Move into a single regional bucket for final processing**
   - Centralizes data for **final aggregation or analysis**
   - Reduces the complexity of multi-region queries

4. **Use Cloud Dataproc for the final job**
   - Provides **scalable, managed Spark/Hadoop clusters**
   - Efficiently handles large datasets

This strategy **balances cost, performance, and scalability** for global datasets.

---

## ❌ Why the other options are wrong

**a. Move all data into 1 zone**
- Very high **egress costs**  
- Unnecessary data movement, inefficient

**b. Move all data into 1 region directly**
- Still incurs **large egress costs**  
- Skipping preprocessing increases processing volume

**c. Multi-region bucket for final processing**
- Multi-region storage is **more expensive** than regional storage  
- Not required if a single region can handle the processed data  

---

## ✅ Final Answer

**d**

---

# 7. Cost-Effective Storage of Telemetry Data for Machine Learning

## Question

TerramEarth has equipped all **connected trucks** with servers and sensors to collect **telemetry data**.  
Next year they want to use the data to **train machine learning models**.  
They want to **store this data in the cloud** while **reducing costs**.

a. Have the vehicle's computer compress the data in **hourly snapshots**, and store it in a **Google Cloud Storage (GCS) Nearline bucket**  
b. Push the telemetry data in **real-time** to a **streaming Dataflow job** that compresses the data, and store it in **Google BigQuery**  
c. Push the telemetry data in **real-time** to a **streaming Dataflow job** that compresses the data, and store it in **Cloud Bigtable**  
d. Have the vehicle's computer compress the data in **hourly snapshots**, and store it in a **GCS Coldline bucket**  

---

## ✅ Correct Answer

**d. Have the vehicle's computer compress the data in hourly snapshots, and store it in a GCS Coldline bucket**

---

## Explanation

The goal is **cost-effective long-term storage** for **telemetry data** that will be used **once next year** for **ML training**.

### Why this approach works 💰

1. **Batch compression in hourly snapshots**
   - Reduces storage size and **network transfer costs**  
   - Simplifies ingestion for future ML training  

2. **GCS Coldline**
   - Designed for **infrequently accessed data** (accessed <1 per year)  
   - Extremely **low storage cost**  
   - Retrieval cost is incurred only once during ML model training, making it **cheaper overall than Nearline** for this scenario  

3. **Avoid real-time streaming pipelines**
   - Real-time ingestion (Dataflow) is **more expensive**  
   - Not necessary because the ML workload is **not time-sensitive**  

---

## ❌ Why the other options are wrong

**a. Nearline storage**  
- Slightly more expensive for storage compared to Coldline  
- Retrieval costs are lower, but since access is **rare**, Coldline is cheaper overall  

**b. Real-time Dataflow to BigQuery**  
- BigQuery is **optimized for analytics queries**, not for raw telemetry storage  
- Streaming ingestion is **costly for large volumes**  

**c. Real-time Dataflow to Cloud Bigtable**  
- Bigtable is ideal for **low-latency time-series workloads**, but expensive for long-term bulk storage  
- Not necessary if the data is only needed **next year**  

---

## ✅ Final Answer

**d**

---

# 8. Secure Architecture for Autonomous Vehicles

## Question

Your **agricultural division** is experimenting with **fully autonomous vehicles**.  
You want your architecture to **promote strong security** during vehicle operation.

Which **two architectures** should you consider? (Choose two.)

a. Treat every **microservice call between modules on the vehicle as untrusted**  
b. Require **IPv6** for connectivity to ensure a secure address space  
c. Use a **trusted platform module (TPM)** and verify **firmware and binaries on boot**  
d. Use a **functional programming language** to isolate code execution cycles  
e. Use **multiple connectivity subsystems** for redundancy  
f. Enclose the vehicle's drive electronics in a **Faraday cage** to isolate chips  

---

## ✅ Correct Answers

**A. Treat every microservice call between modules on the vehicle as untrusted**  
**C. Use a trusted platform module (TPM) and verify firmware and binaries on boot**

---

## Explanation

Autonomous vehicles require **strong security at both software and hardware levels**.

### Why **A** is correct 🔐

- Treating **all internal microservice calls as untrusted** ensures that:
  - Any compromised module cannot propagate malicious activity  
  - Authentication, authorization, and integrity checks are applied between modules  
- This is a **zero-trust principle** applied within the vehicle  

### Why **C** is correct 🛡️

- **Trusted Platform Module (TPM)**:
  - Provides hardware-based **root of trust**  
  - Ensures that **firmware and binaries are verified on boot**  
  - Protects against tampering or malicious code injections  

---

### ❌ Why the other options are wrong

**B. IPv6 connectivity**  
- IPv6 alone **does not guarantee security**; the protocol version is not a security control  

**D. Functional programming language**  
- Language choice may improve reliability, but **it does not inherently secure the system**  

**E. Multiple connectivity subsystems**  
- Improves **redundancy**, not security  

**F. Faraday cage**  
- Isolates electromagnetic interference, **not cyber security**  

---

## ✅ Final Answer

**A, C**

---

# 9. Increasing Vehicle Operating Efficiency for TerramEarth

## Question

Operational parameters such as **oil pressure** are adjustable on each of TerramEarth's vehicles to **increase efficiency**, depending on **environmental conditions**.  

Your primary goal is to **increase the operating efficiency of all 20 million cellular and unconnected vehicles** in the field.  

Which approach should you take?

a. Have your engineers **inspect the data for patterns**, and then create an algorithm with rules that make operational adjustments automatically  
b. Capture all operating data, **train machine learning models** that identify ideal operations, and run **locally** to make operational adjustments automatically  
c. Implement a **Google Cloud Dataflow streaming job** with a sliding window, and use Google Cloud Messaging (GCM) to make operational adjustments automatically  
d. Capture all operating data, **train machine learning models** that identify ideal operations, and host in **Google Cloud Machine Learning (ML) Platform** to make operational adjustments automatically  

---

## ✅ Correct Answer

**B. Capture all operating data, train machine learning models that identify ideal operations, and run locally to make operational adjustments automatically**

---

## Explanation

The primary goals are **scalability, automation, and real-time operational adjustments** across **millions of vehicles**.  

### Why **B** is correct 🔧

1. **Train ML models**  
   - Models analyze telemetry from all vehicles  
   - Identify **ideal operational parameters** based on environmental conditions  

2. **Run locally on each vehicle**  
   - Ensures **low-latency automatic adjustments**  
   - Works for both **cellular-connected and unconnected vehicles**  
   - Eliminates dependency on continuous cloud connectivity  

3. **Automated efficiency improvements**  
   - Models continuously optimize operations without human intervention  
   - Scales to **millions of vehicles**  

---

### ❌ Why the other options are wrong

**A. Engineers create rules manually**  
- Not scalable to **20 million vehicles**  
- Cannot handle **dynamic environmental conditions** effectively  

**C. Cloud Dataflow with GCM**  
- Works only for **connected vehicles**  
- Cannot control **unconnected vehicles**, which is a large part of the fleet  

**D. ML models hosted in Cloud ML Platform**  
- Requires **continuous connectivity**  
- Cannot apply operational adjustments to **offline/unconnected vehicles**  

---

## ✅ Final Answer

**B**

---

# 10. GDPR Data Retention Compliance for TerramEarth

## Question

For this question, refer to the TerramEarth case study. To be compliant with European GDPR regulation, TerramEarth is required to delete data generated from its
European customers after a period of 36 months when it contains personal data. In the new architecture, this data will be stored in both Cloud Storage and
BigQuery. What should you do?

A. Create a BigQuery table for the European data, and set the table retention period to 36 months. For Cloud Storage, use gsutil to enable lifecycle management using a DELETE action with an Age condition of 36 months.  
B. Create a BigQuery table for the European data, and set the table retention period to 36 months. For Cloud Storage, use gsutil to create a SetStorageClass to NONE action when with an Age condition of 36 months.  
C. Create a BigQuery time-partitioned table for the European data, and set the partition expiration period to 36 months. For Cloud Storage, use gsutil to enable lifecycle management using a DELETE action with an Age condition of 36 months.  
D. Create a BigQuery time-partitioned table for the European data, and set the partition expiration period to 36 months. For Cloud Storage, use gsutil to create a SetStorageClass to NONE action with an Age condition of 36 months.  

---

## ✅ Correct Answer

**C. Create a BigQuery time-partitioned table for the European data, and set the partition expiration period to 36 months. For Cloud Storage, use gsutil to enable lifecycle management using a DELETE action with an Age condition of 36 months.**

---

## Explanation

- **BigQuery**:  
  Using a **time-partitioned table** with a **partition expiration** of 36 months ensures that data is **automatically deleted** at the partition level, which is the recommended and scalable approach for managing time-based data retention.

- **Cloud Storage**:  
  **Lifecycle management with a DELETE action** and an **Age condition of 36 months** automatically removes objects that exceed the GDPR retention requirement.

This solution is **fully automated**, **scalable**, and **GDPR-compliant** across both storage services.

---

## ❌ Why the other options are wrong

**A. Non-partitioned BigQuery table**  
- Table-level expiration is less flexible and not ideal for large, time-series datasets.

**B. SetStorageClass to NONE**  
- Does **not delete data**, which violates GDPR requirements.

**D. SetStorageClass instead of DELETE**  
- Changing storage class does not remove personal data and is **not GDPR compliant**.

---

## ✅ Final Answer

**C**

---

# 11. Cloud Storage Lifecycle Rules for TerramEarth

## Question

For this question, refer to the TerramEarth case study. TerramEarth has decided to store data files in Cloud Storage. You need to configure Cloud Storage lifecycle rule to store 1 year of data and minimize file storage cost.  
Which two actions should you take?

A. Create a Cloud Storage lifecycle rule with Age: “30”, Storage Class: “Standard”, and Action: “Set to Coldline”, and create a second GCS life-cycle rule with Age: “365”, Storage Class: “Coldline”, and Action: “Delete”.  
B. Create a Cloud Storage lifecycle rule with Age: “30”, Storage Class: “Coldline”, and Action: “Set to Nearline”, and create a second GCS life-cycle rule with Age: “91”, Storage Class: “Coldline”, and Action: “Set to Nearline”.  
C. Create a Cloud Storage lifecycle rule with Age: “90”, Storage Class: “Standard”, and Action: “Set to Nearline”, and create a second GCS life-cycle rule with Age: “91”, Storage Class: “Nearline”, and Action: “Set to Coldline”.  
D. Create a Cloud Storage lifecycle rule with Age: “30”, Storage Class: “Standard”, and Action: “Set to Coldline”, and create a second GCS life-cycle rule with Age: “365”, Storage Class: “Nearline”, and Action: “Delete”.

---

## ✅ Correct Answer

**A**

---

## Explanation

TerramEarth needs to:

- **Retain data for exactly 1 year**
- **Minimize storage costs**
- **Automatically manage storage transitions and deletion**

Option **A** meets these requirements:

- Data starts in **Standard** storage for active access
- After **30 days**, it moves to **Coldline**, which is much cheaper for infrequently accessed data
- After **365 days**, the data is **deleted**, ensuring compliance with the 1-year retention requirement

This approach balances **cost optimization** and **data retention** using Google Cloud Storage lifecycle rules.

---

## ❌ Why the other options are wrong

**B.**  
- Attempts to move data from **Coldline to Nearline**, which increases cost  
- Does **not delete data after 1 year**

**C.**  
- Proper tiering from Standard → Nearline → Coldline  
- **No deletion rule**, so data is stored longer than 1 year

**D.**  
- Deletes data based on **Nearline** storage class, but data was already moved to **Coldline**, so deletion may never occur as intended

---

## ✅ Final Answer

**A**

---

# 12. Data Warehouse Solution for TerramEarth

## Question

For this question, refer to the TerramEarth case study. You need to implement a reliable, scalable GCP solution for the data warehouse for your company,  
TerramEarth.  
Considering the TerramEarth business and technical requirements, what should you do?

A. Replace the existing data warehouse with BigQuery. Use table partitioning.  
B. Replace the existing data warehouse with a Compute Engine instance with 96 CPUs.  
C. Replace the existing data warehouse with BigQuery. Use federated data sources.  
D. Replace the existing data warehouse with a Compute Engine instance with 96 CPUs. Add an additional Compute Engine preemptible instance with 32 CPUs.  

---

## ✅ Correct Answer

**A. Replace the existing data warehouse with BigQuery. Use table partitioning.**

---

## Explanation

TerramEarth requires a **reliable**, **scalable**, and **low-maintenance** data warehouse solution to handle massive and growing datasets.

**BigQuery** is Google Cloud’s fully managed, serverless data warehouse and is designed for:

- Massive data volumes  
- High concurrency analytics  
- Minimal operational overhead  

Using **table partitioning**:

- Improves **query performance**
- Reduces **query cost** by scanning only relevant partitions
- Supports **time-based analytics**, which is common for telemetry and historical data

This aligns perfectly with TerramEarth’s business and technical requirements.

---

## ❌ Why the other options are wrong

**B. Compute Engine with 96 CPUs**  
- Requires manual scaling and maintenance  
- Not cost-effective or elastic for analytics workloads  

**C. BigQuery with federated data sources**  
- Federated queries are useful for ad-hoc access  
- Not optimal for a **primary enterprise data warehouse**

**D. Compute Engine with additional preemptible instance**  
- Preemptible VMs are unreliable for core data warehouse workloads  
- Still requires heavy operational management

---

## ✅ Final Answer

**A**

---

# 13. Automated Data Quality Management for BigQuery

## Question

For this question, refer to the TerramEarth case study. A new architecture that writes all incoming data to BigQuery has been introduced. You notice that the data is dirty, and want to ensure data quality on an automated daily basis while managing cost.  
What should you do?

A. Set up a streaming Cloud Dataflow job, receiving data by the ingestion process. Clean the data in a Cloud Dataflow pipeline.  
B. Create a Cloud Function that reads data from BigQuery and cleans it. Trigger the Cloud Function from a Compute Engine instance.  
C. Create a SQL statement on the data in BigQuery, and save it as a view. Run the view daily, and save the result to a new table.  
D. Use Cloud Dataprep and configure the BigQuery tables as the source. Schedule a daily job to clean the data.  

---

## ✅ Correct Answer

**D. Use Cloud Dataprep and configure the BigQuery tables as the source. Schedule a daily job to clean the data.**

---

## Explanation

TerramEarth needs an **automated**, **cost-effective**, and **low-maintenance** solution to ensure **data quality** in BigQuery.

**Cloud Dataprep** is designed specifically for:

- Visual and rule-based **data cleaning**
- Native **BigQuery integration**
- **Scheduled jobs** for recurring data preparation
- Minimal operational overhead

Scheduling a daily Dataprep job ensures consistent data quality without the cost and complexity of running always-on streaming pipelines.

---

## ❌ Why the other options are wrong

**A. Streaming Cloud Dataflow job**  
- More complex and costly than needed for **daily batch cleaning**
- Best suited for real-time transformation, not scheduled data quality checks  

**B. Cloud Function triggered by Compute Engine**  
- Adds unnecessary orchestration and operational overhead  
- Not designed for large-scale data cleaning  

**C. BigQuery view with daily execution**  
- Views do not modify or clean source data  
- Requires additional orchestration and still leaves dirty data intact  

---

## ✅ Final Answer

**D**

---

# 14. Reducing Unplanned Vehicle Downtime in GCP

## Question

For this question, refer to the TerramEarth case study. Considering the technical requirements, how should you reduce the unplanned vehicle downtime in GCP?

A. Use BigQuery as the data warehouse. Connect all vehicles to the network and stream data into BigQuery using Cloud Pub/Sub and Cloud Dataflow. Use Google Data Studio for analysis and reporting.  
B. Use BigQuery as the data warehouse. Connect all vehicles to the network and upload gzip files to a Multi-Regional Cloud Storage bucket using gcloud. Use Google Data Studio for analysis and reporting.  
C. Use Cloud Dataproc Hive as the data warehouse. Upload gzip files to a Multi-Regional Cloud Storage bucket. Upload this data into BigQuery using gcloud. Use Google Data Studio for analysis and reporting.  
D. Use Cloud Dataproc Hive as the data warehouse. Directly stream data into partitioned Hive tables. Use Pig scripts to analyze data.  

---

## ✅ Correct Answer

**A. Use BigQuery as the data warehouse. Connect all vehicles to the network and stream data into BigQuery using Cloud Pub/Sub and Cloud Dataflow. Use Google Data Studio for analysis and reporting.**

---

## Explanation

Reducing **unplanned vehicle downtime** requires **near real-time visibility** into vehicle telemetry data so issues can be detected and addressed early.

This option provides:

- **Real-time data ingestion** using **Cloud Pub/Sub**
- **Scalable stream processing** with **Cloud Dataflow**
- A fully managed, highly scalable **analytics data warehouse (BigQuery)**
- Fast visualization and reporting with **Google Data Studio**

Streaming data enables **predictive maintenance**, anomaly detection, and faster operational insights—directly supporting downtime reduction.

---

## ❌ Why the other options are wrong

**B. Batch uploads to Cloud Storage**  
- Introduces **latency**
- Delays insights and corrective actions  

**C. Dataproc Hive with batch ingestion**  
- Adds unnecessary operational complexity  
- Slower analytics compared to BigQuery  

**D. Dataproc Hive with Pig scripts**  
- Uses legacy technologies  
- High maintenance overhead and poor real-time capabilities  

---

## ✅ Final Answer

**A**

---

# 15. Vehicle Data Ingestion Architecture

## Question

For this question, refer to the TerramEarth case study. You are asked to design a new architecture for the ingestion of the data of the 200,000 vehicles that are connected to a cellular network. You want to follow Google-recommended practices.  
Considering the technical requirements, which components should you use for the ingestion of the data?

A. Google Kubernetes Engine with an SSL Ingress  
B. Cloud IoT Core with public/private key pairs  
C. Compute Engine with project-wide SSH keys  
D. Compute Engine with specific SSH keys  

---

## ✅ Correct Answer

**B. Cloud IoT Core with public/private key pairs**

---

## Explanation

For large-scale ingestion of data from **connected vehicles**, Google-recommended practices focus on **secure device identity**, **scalability**, and **managed services**.

**Cloud IoT Core** provides:

- **Device-level authentication** using **public/private key pairs**
- Secure ingestion over **MQTT or HTTP**
- Native integration with **Cloud Pub/Sub** for scalable downstream processing
- Designed specifically for **IoT and telemetry workloads** at massive scale

This makes it the best fit for ingesting data from **200,000 cellular-connected vehicles** securely and reliably.

---

## ❌ Why the other options are wrong

**A. Google Kubernetes Engine with an SSL Ingress**  
- Not designed for device-level authentication  
- Adds unnecessary operational complexity for ingestion  

**C. Compute Engine with project-wide SSH keys**  
- Insecure for large fleets  
- Not scalable or appropriate for IoT devices  

**D. Compute Engine with specific SSH keys**  
- Still not designed for IoT-scale ingestion  
- High management overhead and poor security posture  

---

## ✅ Final Answer

**B**
