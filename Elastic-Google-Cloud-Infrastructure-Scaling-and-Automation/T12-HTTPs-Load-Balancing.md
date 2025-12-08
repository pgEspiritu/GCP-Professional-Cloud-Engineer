# 🔒 HTTPS Load Balancing

HTTPS Load Balancing in Google Cloud builds on **HTTP load balancing** with added **encryption, SSL termination, and advanced backend options**.

---

## 1️⃣ Key Differences from HTTP Load Balancer

- Uses a **Target HTTPS Proxy** instead of a Target HTTP Proxy 🔑  
- Requires **at least one signed SSL certificate** installed on the target HTTPS proxy  
- **SSL sessions terminate at the load balancer**  
- Supports **QUIC transport protocol** for:  
  - Faster client connection initiation 🚀  
  - Eliminates head-of-line blocking in multiplexed streams  
  - Connection migration when a client's IP changes 🌐  

> ⚡ More on QUIC: see course resources.

---

## 2️⃣ SSL Certificates

- Up to **15 SSL certificates** can be configured per target HTTPS proxy  
- Each certificate is represented as an **SSL certificate resource**  
- SSL certificate resources are used only with **load balancing proxies** (target HTTPS proxy or target SSL proxy)

---

## 3️⃣ Backend Buckets

- Allows using **Google Cloud Storage buckets** as backends for HTTP(S) Load Balancing  
- Uses **URL maps** to route traffic to:  
  - **Backend services** → Dynamic content (data, APIs)  
  - **Backend buckets** → Static content (images, files) 📂  

### Example:

| Path                   | Destination                   | Region          |
|------------------------|-------------------------------|----------------|
| `/love-to-fetch/`       | Cloud Storage bucket           | europe-north   |
| All other requests      | Cloud Storage bucket           | us-east        |

---

## 4️⃣ Network Endpoint Groups (NEGs)

- NEGs define **groups of backend endpoints or services**  
- Common for **containerized services** and **granular traffic distribution**  
- Can be used with **load balancers** or **Traffic Director**

### NEG Types:

| Type             | Endpoints                                | Notes |
|-----------------|-----------------------------------------|-------|
| **Zonal**       | One or more Compute Engine VMs or services | Endpoints specified by IP or IP:port |
| **Internet**    | Single endpoint outside Google Cloud      | Specified by hostname FQDN:port or IP:port |
| **Hybrid**      | Traffic Director services outside GCP     | Supports hybrid connectivity |
| **Serverless**  | Cloud Run, App Engine, Cloud Functions   | Must be in the same region as NEG |

> ⚡ More on NEGs: see course resources.
