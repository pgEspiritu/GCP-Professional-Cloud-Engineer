# 📜 Google Cloud Logging

Cloud Logging is a core part of **Google Cloud Observability**, complementing monitoring with log management, analysis, and alerting.

---

## 📝 Overview

- Stores, searches, analyzes, monitors, and alerts on log data from **Google Cloud** and **AWS**.  
- Fully managed service that scales to ingest logs from **thousands of VMs**.  
- Includes:
  - **Storage** for logs  
  - **Logs Explorer** (user interface)  
  - **API** for programmatic log management

- Capabilities:
  - Read and write log entries  
  - Search and filter logs  
  - Create **log-based metrics**

- Logs are **retained for 30 days** by default.  
- Longer retention or streaming is possible via **export** to:
  - Cloud Storage (for long-term storage)  
  - BigQuery (for analysis and visualization)  
  - Pub/Sub (for streaming to applications or endpoints)

---

## 📊 Analyzing Logs with BigQuery

- Exporting logs to BigQuery allows:
  - Fast SQL queries on **gigabytes to petabytes** of data  
  - Analysis of network traffic, traffic growth, or forensics  
  - Forecasting capacity and optimizing network costs

- Example Use Case:
  - Query logs to identify top IP addresses interacting with a web server  
  - Take action: relocate infrastructure, deny certain IPs, or optimize traffic routing

---

## 📈 Visualization with Looker Studio

- Connect BigQuery tables to **Looker Studio**  
- Transform raw log data into **metrics and dimensions**  
- Create **easy-to-understand dashboards and reports** for operational insights

---

## 🔄 Streaming Logs with Pub/Sub

- Enables real-time **log streaming** to applications or endpoints  
- Useful for event-driven processing, alerting, or integration with other systems
