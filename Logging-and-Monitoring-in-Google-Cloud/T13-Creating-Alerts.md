# 🚨 Creating Alerts in Google Cloud Monitoring

We've now explored alerting concepts and strategies — let’s dive into **how to actually create alerts** in Google Cloud! 🌩️

---

## 📘 What Is an Alerting Policy?

Google Cloud uses **Alerting Policies** to define when and how alerts are generated.

An alerting policy includes:

- 🏷️ **Name**  
- ⚠️ **One or more alert conditions**  
- 📢 **Notifications**  
- 📝 **Documentation**  

Use **descriptive names** and follow organizational naming conventions for clarity.

Alerting policies can be created using:
- Google Cloud Console  
- **gcloud CLI**  
- **Monitoring API**  
- **Terraform**  

💡 **Pro Tip:**  
Create an alert in the Console first → then run  
`gcloud monitoring policies list` or  
`gcloud monitoring policies describe`  
to learn the correct YAML/JSON structure.

---

## 📊 Types of Alerting Policies

### 🔹 Metric-Based Alerting
Triggered based on **metric data** from Cloud Monitoring.

Example:
> Alert when a VM has high latency over a specific time period.

Create via:
- Console  
- API  
- CLI  
- Terraform  

### 🔸 Log-Based Alerting
Triggered when **specific log messages** appear.

Example:  
> Notify when a human accesses the security key of a service account.

Create via:
- Logs Explorer  
- Monitoring API  

---

## ⚙️ The Alert Condition (Most Important Part!)

This is where you define:
- **What is monitored**
- **When the alert should fire**

You select:
- 🎯 Target resource  
- 📈 Metric  
- 🔍 Filters  
- 📊 Grouping  
- ➕ Aggregations  
- 🔎 Trigger logic + duration  

### ✳️ Types of Metric Conditions

1. **📉 Metric Threshold**  
   Trigger when a metric exceeds a threshold for a set duration.

2. **🕳️ Metric Absence**  
   Trigger when *no data* is reported for a set duration.

3. **🔮 Forecast Condition**  
   Uses historical data to **predict** likely threshold violations.

---

## 📡 Notification Channels

Alerts can notify humans or systems using:

### 👤 **To People**
- Email ✉️  
- SMS 📱  
- Slack 💬  
- Mobile Push 🔔  

### 🤖 **To Systems**
- Webhooks 🔗  
- Pub/Sub 📬  
- PagerDuty 🚨  

### 🏷️ Add Severity Labels
User-defined labels help prioritize alerts.

Example:
```json
"labels": { "severity": "critical" }
```
Downstream services can route alerts differently based on severity.

# 📝 Documentation in Alerting Policies

The **Documentation** section helps responders understand what to do when an alert fires.

Include:
- 🧰 **Troubleshooting steps**  
- 📘 **Playbooks**  
- 🔧 **Runbooks**  
- 🔗 **Helpful links**  
- 🏷️ **Dynamic labels** for context  

💡 If the fix is always the same → **automate it!** ⚙️

---

# 📬 Alert Notification Example

Email alerts automatically include:
- ❗ **Failing condition**  
- 📊 **Metric values**  
- 💻 **Resource details**  
- 📝 **Optional documentation**  

---

# 🔥 Incidents & Alerting Dashboard

When an alert condition is met, Cloud Monitoring creates an **incident**.

### Incident States:
- 🚨 **Firing** — alert is currently active  
- 👀 **Acknowledged** — someone has taken ownership  
- ✔️ **Closed** — condition resolved  

You can view:
- 📉 **Incident summary**  
- 🕒 **Recent incidents**  
- 😴 **Snoozed alerts**  

---

# 😴 Snoozing Alerts

Use **snooze** to:
- ⏸️ Pause notifications  
- 🌩️ Avoid alert storms during outages  

---

# 👥 Alerting on Groups

Groups let you monitor **sets of resources** instead of individual ones.

You can organize:
- 🏭 **Production vs Dev environments**  
- 🗺️ **Resources by zone**  
- 🏷️ **Resources by label or network tag**  

Groups can have **up to 6 levels** of subgroups.

### A resource belongs to a group if it matches criteria such as:
- Resource type  
- Resource name  
- Project  
- Region  
- Labels  
- Network tags  
- And more  

---

# 🧾 Logs-Based Metrics

Logs-based metrics extract data from log entries, such as:
- 🔢 Count of specific log messages  
- ⏱️ Latency recorded in logs  

They can be used in:
- 📈 Charts  
- 📊 Dashboards  
- 🚨 Alerting policies  

---

# 🛠️ Creating Alert Policies with Terraform

A basic Terraform alert policy includes:

- 🏷️ `display_name`  
- 🔗 `combiner` (how multiple conditions are evaluated)  
- 📌 `conditions` (up to **6** allowed)  

### Example:
```hcl
resource "google_monitoring_alert_policy" "alert_policy" {
  display_name = "High CPU Alert"
  combiner     = "OR"
  
  conditions {
    display_name = "CPU Threshold"
    ...
  }
}
```
For more detailed examples, refer to Google Cloud documentation or course resources.

---

### 🎯 Summary

Alerting in Google Cloud involves:
- 🏗️ Defining alerting policies
- 🔎 Setting metric/log conditions
- 📡 Choosing notification channels
- 📝 Enhancing alerts with documentation
- 🔥 Managing incidents & snoozes
- 👥 Using groups for scalable monitoring
- 🛠️ Automating with Terraform
Alerts help improve reliability, prioritize issues, and speed up response times. 🚀
