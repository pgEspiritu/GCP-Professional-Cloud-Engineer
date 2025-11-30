# 🔗 Google Cloud Observability: Partner Integration

Google Cloud Observability supports a rich ecosystem of technology partners, which enhances **IT operations, security, and compliance capabilities**.

---

## 🔹 How Partner Integrations Work

### Example: BindPlane & Blue Medora
- BindPlane collects **logs and metrics** from partner systems  
- Metrics and logs are pushed into Google Cloud's core observability services:
  - **Cloud Logging** for logs  
  - **Cloud Monitoring** for metrics  
- Once ingested, you can:
  - View logs in real time  
  - Create **log-based metrics**  
  - Alert on logs  
  - Analyze logs and metrics side-by-side  

---

### Example: Splunk Integration
1. **Cloud Logging** collects logs into a centralized location  
2. Logs are published to **Google Cloud Pub/Sub**  
3. **Dataflow pipelines** process logs:
   - Primary pipeline extracts logs from Pub/Sub and delivers them to Splunk  
   - Secondary pipeline ensures logs are resent if delivery fails  
4. **Splunk** (on-premises, SaaS, or hybrid) receives logs for analysis  

- Streaming pipelines natively push Google Cloud data to **Splunk Cloud** or **Splunk Enterprise**  
- Pub/Sub to Splunk Dataflow template allows forwarding **any message** from Pub/Sub to Splunk  

---

For more information about partner integr
