## 🧩 Non-VM Resources in Google Cloud Monitoring

In addition to Google Cloud virtual machines, many Google Cloud services support built-in monitoring capabilities.

### 📌 Monitoring Without the Ops Agent
When working with the following **non-VM Google Cloud services**, the Ops Agent is **not required** and **should NOT be installed**:

---

### 🚀 App Engine Standard Environment
- Monitoring is **built-in**.
- Logging is supported by writing to `stdout` or `stderr`.
- Enhanced logging is possible with language-specific libraries (e.g., **Winston** for Node.js).
- Logs appear under **GAE Application** resources in Cloud Logging.

---

### 🧱 App Engine Flexible Environment
- Built on top of **Google Kubernetes Engine (GKE)**.
- Comes with the Monitoring agent **pre-installed and configured**.
- Supports both Cloud Monitoring and Cloud Logging.

---

### 🐳 Google Kubernetes Engine (GKE)
- Standard GKE nodes (VMs) have **Cloud Monitoring + Cloud Logging enabled by default**.
- No Ops Agent needed unless performing custom logging or monitoring.

---

### ⚡ Cloud Run Functions
Cloud Run functions provide lightweight, event-driven compute.

#### Example Use Case:
Upload a PDF → triggers an event → Cloud Run function executes → translates PDF from English to Spanish.

#### Monitoring:
- Automatic and built-in.
- Metrics available:
  - Invocations
  - Execution times
  - Memory usage
  - Active instances
- Viewable in:
  - Google Cloud Console
  - Cloud Monitoring (with support for custom dashboards + alerts)

#### Logging:
- Automatically supported.
- Logs written to `stdout` or `stderr` appear in Cloud Logging.
- Logging API can be used for advanced logging.

---

### 🏃 Cloud Run (Managed Containers)
Cloud Run runs your containers either:
- Fully managed (serverless), or
- On GKE (Cloud Run for Anthos)

#### Monitoring:
- **Integrated automatically** — no setup needed.
- Metrics captured automatically while services are running.
- Can be viewed via:
  - Cloud Run console page
  - Cloud Monitoring (more filtering + charting options)

#### Resource Types:
- Fully managed Cloud Run → `cloud_run_revision`
- Cloud Run for Anthos → `knative_revision`

#### Logging:
Cloud Run automatically sends two log types to Cloud Logging:
1. **Request logs** – HTTP request details
2. **Container logs** – application logs from `stdout`, `stderr`, or logging libraries

---
