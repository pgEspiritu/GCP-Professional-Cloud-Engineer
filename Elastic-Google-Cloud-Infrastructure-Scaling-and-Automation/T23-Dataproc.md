# ☁️ Cloud Dataproc Overview

**Cloud Dataproc** is a fast, easy-to-use, fully managed service for running **Apache Spark** and **Apache Hadoop** clusters on Google Cloud.

---

## 1️⃣ Key Features

- **Fully managed:** Simplifies cluster creation, scaling, and shutdown  
- **Fast provisioning:** Clusters start, scale, or shut down in **~90 seconds**  
- **Cost-effective:**  
  - Pay **per second** for used resources  
  - Use **preemptible instances** to reduce cost further  
- **Integration with GCP services:**  
  - `BigQuery`, `Cloud Storage`, `Cloud Bigtable`  
  - `Stackdriver Logging` and `Monitoring`  
- **Familiar tools:** Supports Spark, Hadoop, Pig, Hive with **no need to learn new APIs**  

---

## 2️⃣ Advantages

- Quickly create and manage clusters  
- Save time and money on cluster administration  
- Focus on jobs and data rather than infrastructure  
- Easily migrate existing on-prem or cloud Hadoop/Spark projects  

---

## 3️⃣ Cloud Dataproc vs Cloud Dataflow

| Question                                         | Use Cloud Dataproc                | Use Cloud Dataflow                |
|-------------------------------------------------|---------------------------------|---------------------------------|
| Do you have dependencies on Hadoop/Spark tools?| ✅ Yes                          | ❌ No                           |
| Operations style                                 | DevOps / hands-on               | Serverless / hands-off           |
| Batch & streaming                                | Supported                        | Supported                        |

> Choose Dataproc if you require **Hadoop/Spark ecosystem** compatibility and hands-on control; otherwise, Dataflow offers a fully managed, serverless alternative.
