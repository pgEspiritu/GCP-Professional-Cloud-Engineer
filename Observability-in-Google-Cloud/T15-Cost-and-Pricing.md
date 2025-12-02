# 💰 Google Cloud Observability — Cost & Pricing

## 📌 General Pricing Model
- Google Cloud Observability services are **usage-based**, not infrastructure-based.
- **Cloud Profiler is free**.
- **Cloud Logging, Monitoring, and Cloud Trace have usage-based costs**.
- Error Reporting costs depend on **Cloud Logging ingestion**.

---

# 📝 Cloud Logging Costs
💸 **Charges apply based on:**
- Volume of **chargeable logs ingested**.
- **Cloud Load Balancing logs**.
- **Custom logs**.
- **Error Reporting logs** (since they are ingested via Logging).
- **Write operations** using Cloud Logging API.
- **Retention beyond 30 days** for non-required buckets.

📌 Networking logs (VPC Flow Logs, Firewall Logs, Cloud NAT logs):
- **No extra cost to generate** if stored in Cloud Logging.
- **Cost incurred** if exported to **external destinations**.

---

# 📊 Cloud Monitoring Costs
💸 **Charges apply for:**
- Volume of **chargeable metrics** ingested.
- Number of **Monitoring API calls**.
- **Uptime checks**.
- Metrics from **Managed Service for Prometheus**.

📌 Cost drivers:
- Custom metrics  
- AWS metrics  
- Monitoring API read operations (outside Cloud Console)

---

# 🔍 Cloud Trace Costs
💸 **Trace costs depend on:**
- Number of **spans ingested**.
- Number of **spans scanned**.

🎁 **Free allotment:** _2.5 million spans per month_.

📌 Cost-generating use cases:
- Additional spans from App Engine apps (beyond defaults).
- Cloud Load Balancing spans.
- Custom app instrumentation.

---

# 🆓 Free Features Across Observability
These functions **do not incur charges**:
- ✅ Cloud Profiler  
- ✅ Cloud Audit Logs, Access Transparency logs, BigQuery Data Access logs  
- ✅ Creating & using dashboards  
- ✅ Viewing Google Cloud & Anthos metrics/log streams  
- ✅ App Engine Standard trace spans  
- ✅ Uptime checks  
- ✅ Logs analytics (running queries in Cloud Logging)  

⚠️ After the **free tier**, usage fees apply.

---

# 🌐 Network Intelligence Center Costs
Costs apply for:
- Metrics shown in the **topology view**
- **Network Analyzer**
- **Performance Dashboard**
- **Connectivity Tests**
- **Firewall Insights** (pricing varies by usage)

---

# 📎 Reminder
Always check the official **Google Cloud Pricing Documentation** for the most up-to-date pricing information.
