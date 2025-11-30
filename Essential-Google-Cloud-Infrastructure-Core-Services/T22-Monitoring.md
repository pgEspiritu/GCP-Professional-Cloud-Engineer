# 📊 Google Cloud Monitoring Overview

Cloud Monitoring is a core part of **Google Cloud Observability**, enabling monitoring of resources, applications, and services for reliability, scalability, and performance.

---

## 🏗 Importance of Monitoring

- Monitoring is at the base of **Site Reliability Engineering (SRE)**.  
- SRE applies software engineering principles to operations to build **ultra-scalable and highly reliable systems**.  
- Google uses SRE to build, deploy, monitor, and maintain some of the **largest software systems in the world**.

---

## ⚡ Key Features

- **Dynamic Configuration**  
  - Automatically configures monitoring for deployed resources.  
  - Provides intelligent defaults to easily create charts and dashboards.

- **Metrics Ingestion & Insights**  
  - Collects data such as **metrics, events, and metadata**.  
  - Generates insights via **dashboards, charts, and alerts**.

- **Custom Dashboards**  
  - Visualize CPU utilization, network traffic, and other metrics.  
  - Customize with **filters**, **groups**, and **aggregates**.

---

## 📝 Metrics Scope

- Root entity holding **monitoring and configuration information**.  
- Supports **1–375 monitored projects**.  
- Contains:
  - Custom dashboards  
  - Alerting policies  
  - Uptime checks  
  - Notification channels  
  - Group definitions

- Metrics data and logs remain in individual projects.  
- First project in a metrics scope is the **scoping project**, which defines the metrics scope name.  
- Supports **AWS account integration** via the AWS Connector.  
- Provides a **single pane of glass** for viewing multiple projects and accounts.  

> 💡 Tip: Use separate metrics scopes if you need different roles or data visibility for projects.

---

## 🚨 Alerting Policies

- Alerts notify when specific conditions are met (e.g., high network egress).  
- Notifications via **email, SMS, or other channels**.  
- Best Practices:
  - Alert on **symptoms**, not causes.  
  - Use **multiple notification channels**.  
  - Customize alerts for the **audience and required actions**.  
  - Avoid noise—only create **actionable alerts**.

---

## 🌍 Uptime Checks

- Test availability of **public services** globally.  
- Types: **HTTP, HTTPS, TCP**.  
- Resources: App Engine, Compute Engine, URLs, AWS instances/load balancers.  
- Alerts can be tied to uptime checks.  
- Example: HTTP check every minute with a **10-second timeout**.

---

## 🖥 Telemetry & Data Collection

- **Ops Agent** collects metrics inside VMs (not hypervisor-level).  
- Supports **Compute Engine** and many third-party applications.  
- Compatible with **CentOS, Ubuntu, Windows**, etc.  
- Enables dashboards, alerts, uptime checks, and notifications.

---

## 🛠 Custom Metrics & Autoscaling

- Create **custom metrics** if standard metrics don’t fit your needs.  
  - Example: Pass current number of users on a game server.  
- **Autoscaler** maintains metrics at target values:
  - Creates VMs if metric exceeds target.  
  - Deletes VMs if metric is below target.  
  - Supports metrics per VM or per managed instance group.  
  - Filters can be applied for **individual metric values**.

---

## 🔗 References

- For a full list of metrics, uptime checks, alerting policies, and Ops Agent support, refer to the **Google Cloud Monitoring documentation**.
