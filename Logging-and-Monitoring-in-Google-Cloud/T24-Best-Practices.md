# 🛡️ Best Practices for Audit Logs and Data Access

Before finishing this module, let's review some **best practices** for managing audit logs in Google Cloud.

---

## 📝 Planning and Testing

- Start with **planning** your Data Access audit logs.
- Consider the hierarchy: **Organization → Folder → Project**.
- Some projects are specialized, but most fit into **common organizational types**.
- **Create a test project** to experiment and validate logging behavior.
- Roll out the plan gradually and **use automation (Infrastructure as Code, IaC)**.

---

## ⚙️ Infrastructure as Code (IaC)

- Automate creation and modification of infrastructure via configuration files.
- Use **CI/CD pipelines** to deploy infrastructure changes consistently.
- **Terraform** is a popular IaC tool:
  - Open-source from HashiCorp.
  - Installed by default in Cloud Shell.
- **State management** can be:
  - **Remote:** Cloud Storage, Terraform Cloud
  - **Local:** On-premises or HashiCorp Enterprise service.

- Benefits:
  - Track resources provisioned via IaC.
  - Set default include/exclude log filters for every project.

---

## 📂 Log Storage Management

- Aggregate logs from across the organization into **centralized Cloud Logging buckets**.
- Create **user-defined buckets** to centralize or subdivide logs.
- Customize storage based on **compliance and usage requirements**:
  - Choose log storage locations.
  - Define **data retention periods**.
- Consider **region-specific latency, compliance, and availability requirements**.

---

## 🔐 Security and Encryption

- Cloud Logging **encrypts customer content at rest by default**.
- For advanced requirements, use **Customer-Managed Encryption Keys (CMEK)**.
- Control **who gets access** to logs and exported data.
- Limit access to sensitive logs, especially **Data Access audit logs** containing **PII**.

---

## 🚀 Exporting and Filtering Logs

- Decide what logs to **export from Aggregated Exports** at the organization level.
- Use **filters carefully**—for what to include or exclude.
- Excluded entries are **deleted permanently**.
- Consider **side-channel leakage risks**—monitor access to logs.

---

## 👥 Access Control

- Apply **appropriate IAM roles** on logs:
  - **Org-level:** Admins, Security team.
  - **Folder/project-level:** Developers.
- Use **custom log views** to restrict access to specific logs or users.
- For exported logs in **Cloud Storage or BigQuery**, control access via IAM.
- Consider **Sensitive Data Protection** to redact PII from logs.

---

## 🏗️ Team Access Scenarios

### Operational Monitoring

- **CTO:** Organization admin role.
- **Security team:** 
  - `logging.viewer` → Admin Activity audit logs.
  - `logging.privateLogViewer` → Data Access audit logs.
- Control access to exported data via IAM.

### Development Teams

- `logging.viewer` at folder level → Admin Activity logs.
- `logging.privateLogViewer` → Data Access logs (limit access to test data).
- Use **prebuilt dashboards** for external auditors.

---

## 📊 Reporting and Visualization

- Pre-create **dashboards** for operational monitoring.
- External auditors can have limited access:
  - Logs Viewer at organization level.
  - BigQuery Data Viewer on exported datasets.
  - Temporary Cloud Storage URLs for secure access.

---
