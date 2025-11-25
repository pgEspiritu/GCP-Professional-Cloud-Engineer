# ☁️ Storing, Routing, and Exporting Logs

Once logs are collected, they can be **routed** and **exported** for long-term storage and analysis.

---

## 🛠️ Cloud Logging Architecture

- **Centralized Logging API:** Cloud Logging exposes log data through a centralized API.
- **Log Router:** 
  - Receives entries from the API.
  - Optimized for streaming data.
  - Buffers and sends logs to storage or sink destinations.

---

## 📦 Default Log Storage

Each Google Cloud project has **two default buckets**:

1. **_Required bucket**
   - Stores **Admin Activity audit logs, System Event audit logs, and Access Transparency logs**.
   - Retention: 400 days.
   - Cannot modify or delete.
   - Free of charge.

2. **_Default bucket**
   - Stores all other ingested logs.
   - Retention: 30 days (can apply custom retention rules).
   - Standard Cloud Logging pricing applies.
   - Cannot delete, but the `_Default` log sink can be disabled.

**Logs Storage Page Stats:**
- Current total volume
- Previous month volume
- Projected volume by end of month
- Total usage by resource type (link to **Metrics Explorer**)

---

## 🔄 Log Router Sinks

Log Router **sinks** forward copies of log entries to non-default locations.

**Use Cases:**
- Extended storage
- SQL querying of logs
- Access control

**Sink Destinations:**
- **Cloud Logging Bucket:** Separate log entries into distinct storage buckets.
- **BigQuery Dataset:** Run SQL queries on complex logs.
- **Cloud Storage Bucket:** Long-term storage or processing with other systems.
- **Pub/Sub Topic:** Export logs to message handling systems (e.g., Dataflow, Cloud Run).
- **Splunk:** Integrate logs with existing Splunk systems.
- **Other Project:** Control access to subsets of log entries.

---

## ⚙️ Creating Log Sinks & Exclusions

**Creating Sinks:**
1. Use **Logs Explorer** to query the desired log entries.
2. Select a **destination**: Cloud Storage, BigQuery, or Pub/Sub.
3. Save the query in an object called a **sink**.
4. Sinks can exist at the **project, folder, or organization level**.

**Exclusion Filters:**
- Built using **Logs Explorer queries**.
- Name the exclusion and define filters for unwanted entries.
- Excluded logs are permanently deleted.
- Can leave some representative events depending on the use case.

---

## 🔄 Export & Processing Options

**Examples of Export Pipelines:**
- **Pub/Sub → Dataflow → BigQuery**
  - Real-time processing with long-term analysis
- **Cloud Storage**
  - Benefits: long-term retention, reduced storage costs, configurable object lifecycles
  - Features: automated storage class changes, auto-delete, guaranteed retention
- **Splunk**
  - Stream logs using Pub/Sub to Splunk Dataflow
  - Or use **Splunk Add-on for Google Cloud**

---

## 🌐 Aggregated Sinks

- **Project-level:** Exports logs for a single project.
- **Folder-level:** Aggregates logs from the folder and children projects.
- **Organization-level:** Aggregates logs across the organization and children resources.

**Use Cases:**
- Centralized log aggregation for auditing, retention, or security.
- Security analytics for threat detection (malware, ransomware, phishing, misconfigurations).
- Aggregate logs sent to tools like **Log Analytics, BigQuery, Chronicle**, or SIEMs.

---

## 📝 Log Entry Naming Conventions

- **LogEntry fields → BigQuery fields:** Names match exactly.
- **User-supplied fields:** Case normalized to lowercase.
- **Structured payloads without `@type`:** Case normalized, naming preserved.
- **Structured payloads with `@type`:** Refer to documentation for field mapping.

---

## 🔍 Query Examples

**Compute Engine Logs:**
- Query logs over multiple days.
- Example: `syslog` and `apache-access` logs for a specific instance ID over the last three days.

**BigQuery Example:**
- Retrieve **unsuccessful App Engine requests** from the last month.
- `from` clause specifies the table data range.
- Filter on `status != 200` to capture all failed requests.
