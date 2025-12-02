# 🐞 Error Reporting

## 📌 What It Is
**Error Reporting** is part of Google Cloud’s APM suite.  
It automatically **scans logs**, **detects exceptions**, **groups errors**, and **notifies you** in real time.

---

# ⭐ Key Capabilities

## 🔍 Error Detection & Aggregation
- Scans **all logs** stored in **global Cloud Logging buckets**.
- Aggregates similar errors into groups using stack trace analysis.
- Helps you quickly find root causes without digging through logs.

## 🚨 Real-Time Notifications
- Sends alerts for **new, ungrouped errors**.
- Supports notifications via:
  - 📧 Email
  - 📱 Mobile app (iOS/Android)
  - 🔔 Slack
  - 🔗 Webhooks

## 📊 Clear Dashboard
- Shows **top**, **new**, and **frequent** errors.
- Auto-refresh every 10 seconds (optional).
- Errors sorted by occurrence, first seen/last seen.

---

# 🧰 Supported Languages & Platforms
Supports many languages:
- Go, Java, Node.js, PHP, Python, Ruby, .NET

Supports many environments:
- App Engine (standard & flexible)
- Cloud Functions
- Cloud Run
- Apps Script
- Compute Engine
- GKE
- Even AWS EC2 (via API/client libraries)

---

# 🔧 Setup Requirements

## ✔️ Logging Requirements
Error Reporting **only analyzes logs** that meet ALL criteria:
- Stored in **global** Cloud Logging buckets  
- Same **source + destination project**
- **CMEK disabled**
- Logs *not* routed to another project or regional bucket

## ✔️ Environment Setup
- **App Engine, Cloud Run, Cloud Functions, Apps Script** → auto-enabled  
- **GKE** → add `cloud-platform` scope at cluster creation  
- **Compute Engine** → service account must have:
  - `roles/errorreporting.writer`
- **Outside GCP** → provide project ID + service account credentials to the library

---

# 🧑‍💻 How Developers Use It (Node.js Example)
1. Enable Error Reporting API  
2. Install the Node.js library  
3. Import and initialize client  
4. Call `.report()` to manually send errors  
5. Optional: integrate with Express.js middleware  
6. Customize metadata using the Error Message Builder

---

# 🔍 Viewing Errors
In **Error Reporting dashboard**, you can:
- View grouped error lists  
- Expand error details (stack trace, frequency, history)  
- See sample instances  
- Click **View logs** to open Logs Explorer  
- Attach issue tracker links (e.g., Jira, GitHub)

---

# ✅ Final Summary
Error Reporting gives you:
- Real-time alerts 🚨  
- Automatic grouping & de-duplication 🧩  
- Dashboard insights 📊  
- Support for major languages & environments 🧑‍💻  
- Faster troubleshooting 🔧  

It is essential for maintaining **reliable and observable production systems**.
