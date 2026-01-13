# 🎮 Mountkirk Games

Mountkirk Games creates **online, session-based multiplayer games** for mobile platforms. They recently started expanding to other platforms after successfully migrating their on-premises environments to **Google Cloud**.

Their latest project is a **retro-style first-person shooter (FPS)** game that allows hundreds of simultaneous players to join **geo-specific digital arenas** from multiple platforms and locations. A **real-time digital banner** will display a **global leaderboard** of the top players across all active arenas.

Mountkirk Games plans to deploy the game backend on **Google Kubernetes Engine (GKE)** to:  

- Scale rapidly  
- Use **Google’s global load balancer** to route players to the closest regional arenas  
- Keep the **global leaderboard in sync** using a **multi-region Spanner cluster**

Their previous environment was migrated to Google Cloud with five games moved using **lift-and-shift VM migrations**, with minor exceptions.

---

## 🏢 Company Overview

- Online multiplayer games for mobile and other platforms  
- Expansion to cross-platform gameplay  
- Global leaderboard for all active arenas  
- Previous games migrated to Google Cloud via lift-and-shift  

---

## 💡 Solution Concept

- Cloud-native backend using **GKE** for dynamic scaling  
- **Global load balancing** for minimal latency  
- Multi-region **Cloud Spanner** for synchronized leaderboards  
- Advanced analytics on player behavior and game telemetry  

---

## 🧱 Existing Technical Environment

- Each new game runs in an **isolated Google Cloud project** within a folder managing permissions and network policies  
- Legacy, low-traffic games are **consolidated into a single project**  
- Separate **development and testing environments** exist  
- Previous migration included **five games using VM lift-and-shift**

---

## 📋 Business Requirements

- Support **multiple gaming platforms**  
- Support **multiple regions**  
- Enable **rapid iteration** of game features  
- Minimize **latency**  
- Optimize for **dynamic scaling**  
- Use **managed services** and pooled resources  
- Minimize **costs**

---

## ⚙️ Technical Requirements

- Dynamically **scale based on game activity**  
- Publish scoring data on a **near real-time global leaderboard**  
- Store **game activity logs** in structured files for analysis  
- Use **GPU processing** for server-side graphics rendering across platforms  
- Support eventual **migration of legacy games** to the new platform  

---

## 🧑‍💼 Executive Statement

> Our last game was the first time we used Google Cloud, and it was a tremendous success. We were able to analyze **player behavior and game telemetry** in ways that were never possible before.  
>  
> This success enabled a **full migration to the cloud** and the development of new games using **cloud-native design principles**.  
>  
> Our new game is our most ambitious to date, supporting multiple gaming platforms. **Latency is our top priority**, followed by cost management.  
>  
> The cloud enables advanced analytics and allows us to **rapidly iterate deployments**, apply bug fixes, and release new functionality.

---

# 1. Designing Testing Strategy for Mountkirk Games

## Question

Mountkirk Games wants you to design their **new testing strategy**. How should the test coverage differ from their **existing backends on the other platforms**?

a. Tests should **scale well beyond the prior approaches**  
b. **Unit tests are no longer required**, only end-to-end tests  
c. Tests should be applied **after the release is in the production environment**  
d. Tests should include **directly testing the Google Cloud Platform (GCP) infrastructure**  

---

## ✅ Correct Answer

**a. Tests should scale well beyond the prior approaches**

---

## Explanation

For a modern cloud-based platform:

- Testing must handle **larger and more complex workloads**  
- Must be **automated, scalable, and comprehensive**  
- Ensures that the platform can maintain **high reliability and performance** as it grows

This differs from prior platforms where testing may have been **limited in scope or scale**.

**Key points:**
- Unit, integration, and end-to-end tests are all still important  
- Tests must run **pre-deployment** and continuously during CI/CD pipelines  
- The focus is on **scalable, automated test coverage**

---

## ❌ Why the other options are wrong

**b. Skip unit tests**  
- Unit tests are still important for **early detection of bugs**

**c. Test after production release**  
- Reactive testing is insufficient for modern DevOps practices

**d. Test GCP infrastructure directly**  
- Infrastructure should be **validated through deployment pipelines**, not tested like application code

---

## ✅ Final Answer

**a**

---

# 2. Designing Scalable Testing Environment for Mountkirk Games

## Question

Mountkirk Games has deployed their new **backend on Google Cloud Platform (GCP)**. You want to create a **thorough testing process** for new versions of the backend **before they are released to the public**. You want the **testing environment to scale in an economical way**. How should you design the process?

a. Create a **scalable environment in GCP** for simulating production load  
b. Use the **existing infrastructure** to test the GCP-based backend at scale  
c. Build **stress tests into each component** of your application using resources internal to GCP to simulate load  
d. Create a set of **static environments in GCP** to test different levels of load — for example, high, medium, and low  

---

## ✅ Correct Answer

**a. Create a scalable environment in GCP for simulating production load**

---

## Explanation

For **cloud-native backends**, the testing environment should:

- **Scale dynamically** to simulate different levels of production traffic  
- Be **cost-efficient**, using resources only when needed  
- Accurately reflect **real-world usage patterns**  

Creating a **scalable environment in GCP** allows:

- Automatic scaling up and down based on test scenarios  
- Economical use of cloud resources  
- Accurate performance and load testing without maintaining always-on infrastructure  

This is a **best practice for cloud testing** and ensures reliability before public release.

---

## ❌ Why the other options are wrong

**b. Use existing infrastructure**  
- Cannot replicate **cloud scalability** and might not reflect production conditions  

**c. Stress tests per component**  
- Useful for internal validation, but does **not simulate full production load**  

**d. Static environments**  
- Inefficient and costly  
- Cannot dynamically adjust to varying load patterns  

---

## ✅ Final Answer

**a**

---

# 3. Continuous Delivery Pipeline for Mountkirk Games

## Question

Mountkirk Games wants to set up a **continuous delivery pipeline**. Their architecture includes many **small services** that they want to be able to **update and roll back quickly**. Mountkirk Games has the following requirements:

- Services are deployed **redundantly across multiple regions** in the US and Europe  
- Only **frontend services** are exposed on the public internet  
- They can provide a **single frontend IP** for their fleet of services  
- **Deployment artifacts are immutable**  

Which set of products should they use?

a. Google Cloud Storage, Google Cloud Dataflow, Google Compute Engine  
b. Google Cloud Storage, Google App Engine, Google Network Load Balancer  
c. Google Kubernetes Registry, Google Container Engine, Google HTTP(S) Load Balancer  
d. Google Cloud Functions, Google Cloud Pub/Sub, Google Cloud Deployment Manager  

---

## ✅ Correct Answer

**c. Google Kubernetes Registry, Google Container Engine, Google HTTP(S) Load Balancer**

---

## Explanation

Mountkirk Games needs a **modern container-based architecture** with **continuous delivery** features. The requirements indicate:

- **Immutable deployment artifacts** → Use **containers stored in Container Registry**  
- **Multiple small services** → Use **Google Kubernetes Engine (GKE)** for orchestration  
- **Single frontend IP** → Use **HTTP(S) Load Balancer** to expose only frontend services  
- **Redundancy across regions** → GKE + global load balancer enables multi-region deployment  

This combination ensures:

- Fast rollbacks  
- Scalability  
- Multi-region availability  
- Proper separation of frontend and backend services  

---

## ❌ Why the other options are wrong

**a. Cloud Storage + Dataflow + Compute Engine**  
- Dataflow is for batch/stream processing, not service deployment  
- Compute Engine VMs are not ideal for managing many small services with fast rollbacks  

**b. Cloud Storage + App Engine + Network Load Balancer**  
- App Engine is PaaS but less flexible for multi-service orchestration  
- Network Load Balancer cannot provide a **single frontend IP with HTTP(S) routing** for multiple services  

**d. Cloud Functions + Pub/Sub + Deployment Manager**  
- Cloud Functions are serverless and event-driven, not suitable for multi-service frontends needing a single IP  
- Deployment Manager does not manage runtime deployments of multiple services efficiently  

---

## ✅ Final Answer

**c**

---

# 4. Investigating Auto-Scaling Issues for Mountkirk Games

## Question

Mountkirk Games' **gaming servers** are not automatically scaling properly. Last month, they rolled out a **new feature**, which suddenly became very popular. A record number of users are trying to use the service, but many of them are getting **503 errors** and very slow response times. What should they investigate first?

a. Verify that the **database is online**  
b. Verify that the **project quota hasn't been exceeded**  
c. Verify that the **new feature code did not introduce any performance bugs**  
d. Verify that the **load-testing team is not running their tool against production**  

---

## ✅ Correct Answer

**b. Verify that the project quota hasn't been exceeded**

---

## Explanation

**Symptoms:**
- Many users are experiencing **503 errors**  
- Application is **not scaling automatically**  

In GCP, **auto-scaling can fail** if **project quotas** for resources (like CPU, memory, or instance count) are **exceeded**. This would prevent new instances from being provisioned and result in:

- Service unavailability  
- Slow response times  

Checking quotas is the **first step**, as it directly affects **auto-scaling behavior**.

---

## ❌ Why the other options are wrong

**a. Database offline**  
- Would cause errors, but **503s from frontend services** usually indicate server capacity issues  

**c. Performance bugs in feature code**  
- Could slow performance, but **auto-scaling should mitigate load** unless quotas are reached  

**d. Load-testing against production**  
- Unlikely to be the primary cause; quota limits will block scaling regardless of traffic source  

---

## ✅ Final Answer

**b**

---

# 5. Isolating Environments for Mountkirk Games

## Question

Mountkirk Games needs to create a **repeatable and configurable mechanism** for deploying **isolated application environments**. Developers and testers can access each other's environments and resources, but they **cannot access staging or production resources**. The **staging environment** needs access to some services from **production**.  

What should you do to **isolate development environments from staging and production**?

a. Create a **project** for development and test and another for staging and production  
b. Create a **network** for development and test and another for staging and production  
c. Create **one subnetwork** for development and another for staging and production  
d. Create **one project for development, a second for staging, and a third for production**  

---

## ✅ Correct Answer

**d. Create one project for development, a second for staging, and a third for production**

---

## Explanation

Using **separate projects** provides **strong isolation** at the resource, network, and IAM level:

- **Development and testing project**: allows collaboration among developers/testers  
- **Staging project**: can selectively access production services without exposing full production resources  
- **Production project**: fully isolated, with strict access control  

Advantages of separate projects:

- Clear separation of **environments**  
- Enforces **least privilege access** via IAM  
- Allows **environment-specific policies, quotas, and billing**  
- Supports **repeatable and automated deployment pipelines**  

This is the **best practice recommended by Google Cloud** for multi-environment setups.

---

## ❌ Why the other options are wrong

**a. Two projects (dev/test + staging/prod)**  
- Staging and production are **not fully isolated**, violating access control requirements  

**b. Separate networks**  
- Networks alone do not enforce **resource or IAM isolation**  

**c. Separate subnetworks**  
- Subnets provide **network segmentation**, but not **full environment isolation**  

---

## ✅ Final Answer

**d**

---

# 6. Real-Time Analytics Platform for Mountkirk Games

## Question

Mountkirk Games wants to set up a **real-time analytics platform** for their new game. The new platform must meet their **technical requirements**.  

Which combination of Google technologies will meet all of their requirements?

a. Kubernetes Engine, Cloud Pub/Sub, and Cloud SQL  
b. Cloud Dataflow, Cloud Storage, Cloud Pub/Sub, and BigQuery  
c. Cloud SQL, Cloud Storage, Cloud Pub/Sub, and Cloud Dataflow  
d. Cloud Dataproc, Cloud Pub/Sub, Cloud SQL, and Cloud Dataflow  
e. Cloud Pub/Sub, Compute Engine, Cloud Storage, and Cloud Dataproc  

---

## ✅ Correct Answer

**b. Cloud Dataflow, Cloud Storage, Cloud Pub/Sub, and BigQuery**

---

## Explanation

Mountkirk Games requires a **real-time analytics platform**, which implies:

- **Streaming data ingestion**  
- **Processing and transformation in real-time**  
- **Storage for historical and analytical queries**  

**Solution components:**

- **Cloud Pub/Sub** → Ingests **real-time game events**  
- **Cloud Dataflow** → Processes **streaming and batch data**, applies transformations  
- **Cloud Storage** → Stores **raw and intermediate data**  
- **BigQuery** → Provides **fast analytics and querying** on processed data  

This combination ensures:

- Real-time analytics capability  
- Scalable and serverless processing  
- Support for **ad-hoc queries and reporting**  

---

## ❌ Why the other options are wrong

**a. GKE + Pub/Sub + Cloud SQL**  
- Cloud SQL is **not suitable for real-time analytics at scale**  
- GKE adds unnecessary operational complexity  

**c. Cloud SQL + Cloud Storage + Pub/Sub + Dataflow**  
- Cloud SQL cannot handle **large-scale real-time analytics**  

**d. Dataproc + Pub/Sub + Cloud SQL + Dataflow**  
- Mixing Dataproc and Dataflow unnecessarily complicates the architecture  
- Cloud SQL still unsuitable for analytics  

**e. Pub/Sub + Compute Engine + Cloud Storage + Dataproc**  
- Requires manual cluster management  
- Not fully serverless or optimized for **streaming analytics**  

---

## ✅ Final Answer

**b**
