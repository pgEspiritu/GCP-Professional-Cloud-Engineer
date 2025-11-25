# 📊 Using Log-Based Metrics

Logs-based metrics allow you to **generate monitoring metrics from logging data** and transform them into **time series** for use in **Cloud Monitoring Charts** and **Alerting Policies**.

---

## 📝 Overview

- **Logs-based metrics** derive metric data from log entries.
- Example use cases:
  - Track the number of entries containing specific messages.
  - Extract latency or performance data reported in logs.

---

## ⚡ Types of Log-Based Metrics

1. **System-Defined Metrics**
   - Provided by Cloud Logging.
   - Calculated from logs ingested by Logging.
   - Excluded logs are **not included**.

2. **User-Defined Metrics**
   - Created by you to track custom events or metrics.
   - Example: Count `/score called` log entries.
   - Useful for:
     - Counting occurrences of specific messages.
     - Observing trends like latency values.
     - Creating charts for numeric data.

---

## 👥 IAM Roles Related to Log Metrics

- **Logging Side**
  - Logs Configuration Writers: list, create, get, update, delete log-based metrics
  - Logs Viewers: view existing metrics
- **Monitoring Side**
  - Monitoring Viewers: read time series from log-based metrics
- **Broad Roles**
  - Logging Admins, Editors, Owners: full creation capabilities

---

## 🏷️ Metric Types

1. **Counter Metrics**
   - Count log entries matching a filter.
   - Example: Count `/score called` requests.

2. **Distribution Metrics**
   - Record statistical distribution of extracted log values in histogram buckets.
   - Track count, mean, and sum of squared deviations.

3. **Boolean Metrics**
   - Record log entries matching a specific filter.

**Scope:**

- System-defined: Google Cloud project level.
- User-defined: Project-level or **bucket-scoped** metrics.
  - Bucket-scoped metrics evaluate logs from:
    - Different projects routed into the bucket.
    - Aggregated sinks.

---

## 🛠️ Creating User-Defined Log-Based Metrics

**Example workflow:**

1. Generate relevant log entries (e.g., from Cloud Run `/score` requests).
2. Filter log entries using **Query Builder**.
3. Click **Create Metric**.
4. Select **metric type**: Counter or Distribution.
5. Configure metric (if Distribution type).
6. Add **labels** as needed for grouping/filtering.

---

## 🏷️ Labels

- Labels allow metrics to contain **multiple time series** (one per label value).
- Can be applied to **counter** and **distribution** metrics.
- **Extractor expression** specifies how to extract label values from logs.
  - Use entire field contents or a regexp.
  - Fields: `httpRequest.status`, `textPayload`, `jsonPayload`, `protoPayload`.
- **Limits & Considerations:**
  - Up to 10 user-defined labels per metric.
  - Metrics cannot be deleted after creation.
  - Each metric limited to ~30,000 active time series.
  - Labels can multiply the time series count:
    - Example: 100 resources × 20 label values = 2,000 time series.

**Label Form Fields:**

- **Name:** Identifier for Monitoring.
- **Description:** Explain label purpose.
- **Type:** String, Boolean, or Integer.
- **Field name:** Log entry field containing label value.
- **Extraction regexp:** Optional regex to extract label value if not the entire field.

---

## ✅ Summary

- Log-based metrics enable **monitoring of custom log data**.
- System vs. user-defined metrics provide flexibility for Google Cloud projects.
- Labels enhance **grouping and filtering** in monitoring dashboards.
