# 🏷️ Cost Control Best Practices (Google Cloud Observability)

## 🧠 1. Understand and Monitor Your Spend
- Track costs regularly (logs, metrics, traces).
- Identify high-cost SKUs (log volume, spans ingested, metric volume).

---

# 🪓 2. Reduce Logging Costs

### 🔹 Exclude Unnecessary Logs  
- Excluding logs = no ingestion cost (but logs are permanently dropped).
- Common exclusions:
  - Cloud Load Balancer logs → often keep only ~10%  
  - VPC Flow Logs → often exclude 95%+

### 🔹 Be Careful with VPC Flow Logs  
- If logs are **ingested to Cloud Logging**, VPC Flow Log generation is **free**.  
- If logs are **generated but excluded**, VPC Flow Log **charges apply**.  
- To avoid cost → disable VPC Flow Logs fully.

### 🔹 Remove Noisy Logs  
- Exclude HTTP **200 OK** logs for web apps.
- Avoid ingesting extra Ops Agent logs from Dev/Test VMs.

### 🔹 Use Log Exports for Cheaper Storage  
Export logs **without ingesting** them into Cloud Logging:
- Cloud Storage → cheapest  
- BigQuery → storage + query cost  
- Pub/Sub → egress + message cost  

💡 Example:  
- 2 TiB logs in Logging → ~$1000  
- 2 TiB in Cloud Storage → ~$40 (regional)

---

# 📉 3. Reduce Metric Volume

### 🔹 Avoid High-Cardinality Labels  
- Metric labels multiply time series count.  
- Example:  
  - `cost_center (11 values) × env (5 values) = 55 time series`  
- Avoid labels like `user_id`.

### 🔹 Minimize Custom Metrics  
- Custom metrics increase Monitoring costs.
- Newer OpenTelemetry-based metrics support **sampling** to reduce volume.

### 🔹 Reduce Ops Agent Metrics  
- Ops Agent metrics are **chargeable**.  
- Reduce volume by:
  - Not installing the agent in nonessential (Dev/Test) environments.
  - Customizing what metrics the agent collects.

---

# 📉 4. Reduce Prometheus Sample Volume (Managed Service for Prometheus)

### 🔹 Increase Scrape Interval  
- Example:  
  - 10s → 30s interval = **66% fewer samples**.

### 🔹 Scrape Only What You Need  
- Reduce number of scrape targets.
- Filter exported metrics.

### 🔹 Aggregate High-Cardinality Metrics Locally  
- Use recording rules or aggregation (kube-prometheus, prometheus-operator).
- Reduce samples sent to Monarch backend.

---

# 🔍 5. Control Trace Costs

### 🔹 Use Sampling  
- Trace cost = spans ingested.  
- Sampling reduces spans dramatically.
- Example:  
  - 5000 QPS app → 20% sampling → reduce to 5%  
  - Cuts spans to 25% of original volume.

### 🔹 Set Span Quotas  
- Use API-specific quotas to cap trace ingestion.
- Prevent unexpected billing spikes.

### 🔹 Understand Microservice Interactions  
- Sampling rate of dependent services matters.
- If frontend samples at 20%, downstream services must handle that volume.

---

# 🎯 Summary of Key Savings
- Exclude low-value logs (esp. 200 OK, LB logs, VPC Flow Logs).  
- Use log exports instead of Cloud Logging storage.  
- Reduce metric cardinality + unnecessary custom metrics.  
- Tune Prometheus scrape intervals + aggregation.  
- Apply trace sampling + quotas to control span ingestion.

These practices significantly reduce Observability costs while keeping useful visibility across your systems.
