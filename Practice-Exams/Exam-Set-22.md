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



