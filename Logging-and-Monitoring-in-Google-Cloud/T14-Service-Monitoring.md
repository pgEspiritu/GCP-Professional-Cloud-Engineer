# 🔧 Service Monitoring in Google Cloud

Modern applications contain many interconnected services—when one fails, others may appear to fail too. **Service Monitoring** helps reduce this complexity by offering **SLO-based monitoring**, dependency insights, and consolidated dashboards.

---

# 📌 What Service Monitoring Helps You Answer

- 🧩 **What are your services?**  
- 🌐 **What functionality do they provide to internal/external users?**  
- 🤝 **What commitments (SLOs) do you make about performance & availability?**  
- 📊 **Are your services meeting those commitments?**  
- 🔁 **How do your microservices depend on each other?**  
- 🚦 **Is new code causing degradation?**  
- 🛠️ **Can you triage issues faster with holistic monitoring signals? (Lower MTTR)**  

---

# 🧭 Supported Service Types

Cloud Monitoring automatically identifies services such as:

- 🚢 **GKE namespaces**  
- 🧱 **GKE services**  
- ⚙️ **GKE workloads**  
- 🏃 **Cloud Run services**  

---

# 📋 Services Overview Page

The **Service Monitoring dashboard** displays:

- 🔤 Service name  
- 🏗️ Service type  
- 🎯 SLO status  
- 🚨 Active SLO alerts  

You can click any service to see detailed metrics, SLOs, dependencies, and error budgets.

---

# 🎯 Request-Based vs Window-Based SLOs

## 📌 **Request-Based SLOs**
Measure:  
**Good requests / Total requests**

Example:  
📍 *At least 95% of requests must be under 100ms.*

## 🪟 **Window-Based SLOs**
Measure:  
**Good windows / Total windows**

Example:  
📍 *99% of 10-minute windows must have 95% of requests under 100ms.*

### 🔍 Comparison Example
- 1,000,000 monthly requests  
- **99.9% request-based SLO → only 1,000 bad requests allowed**  
- **99.9% window-based SLO → 43 bad windows allowed**  
  (Because 43,200 one-minute windows × 99.9%)

⚠️ **Window-based SLOs may hide burst failures** (e.g., errors only Friday 9:00–9:05).

---

# 🛠️ Creating SLOs in Service Monitoring

1. Go to **Services Overview**  
2. Select a service  
3. Click **Create SLO**  
4. Choose an **SLI metric**:
   - 🟢 **Availability** (success ratio)  
   - ⚡ **Latency** (responses under threshold)  
   - 🛠️ **Other** (custom metrics via Metrics Explorer)  
5. Choose:
   - 📊 **Request-based or windows-based** SLO  
   - 🗓️ **Compliance period** (calendar or rolling)  
   - 🎯 **Performance goal (% target)**  
6. Click **Create alerting policy** to automatically generate an SLO alert.

---

# 📉 Monitoring SLO Performance

For each SLO, you can view:

- 📈 **SLI status**  
- 🧮 **Error budget remaining**  
- ✔️ **Compliance percentage**  
- 🚨 **Alert status**

Expanding an SLO shows deeper metrics and trend details.

---

# 🔥 Error Budget Alerts (Burn Rate Alerts)

Burn rate alerts warn you when you're consuming the error budget too quickly.

Example:
- ⏱️ **60-minute lookback window**
- 🔥 **Burn rate threshold = 1**  
  → would use 100% of error budget in 7 days  
- ⚠️ Alert if trending to burn the budget **10× faster** (in less than 1/10 of the period)

This prevents SLO violations before they happen.

---

# 📊 After Creating an SLO

You can monitor:

- 📍 **Current SLI values**  
- 🧮 **Error budget consumption**  
- 🚨 **Alerts firing**  

Each tab helps diagnose service health and avoid cascading failures.

---

# 🚀 Summary

Service Monitoring enables you to:

- Define SLOs easily  
- Understand service dependencies  
- Track error budgets  
- Centralize service health metrics  
- Create meaningful SLO-based alerts  
- Reduce MTTR and improve reliability  

It’s a powerful tool for modern, microservices-driven architectures.  
