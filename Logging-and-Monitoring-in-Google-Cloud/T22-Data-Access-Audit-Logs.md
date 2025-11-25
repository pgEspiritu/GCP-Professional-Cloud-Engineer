# 🔐 Data Access Audit Logs in Google Cloud

Data Access audit logs provide detailed visibility into **read and write operations** on your Google Cloud resources. They help answer: *“Who accessed what data, where, and when?”*

---

## 🏗️ Configuration Levels

Data Access audit logs can be **enabled at various levels** in the Google Cloud resource hierarchy:

- **Organization**
- **Folder**
- **Project**
- **Resources**
- **Billing accounts**

> You can also **exempt specific principals** from recording data access logs.  
> The final configuration is the **union of all enabled levels**:
> - Example: If a service is enabled at the project level, it **cannot be disabled** if already enabled at a parent organization or folder.

---

## 💰 Cost

- Data Access audit logs are **disabled by default** (except for BigQuery).  
- Enabling them **adds cost**: currently **$0.50 per GB for ingestion**.  
- High-volume logging can increase costs proportionally.  
- Use exemptions to reduce **noise and cost**.

---

## 📝 Types of Data Access Audit Logs

1. **Admin-read** 🗂️
   - Records operations that read **metadata or configuration information**.
   - Example: Viewing bucket configuration settings.

2. **Data-read** 📥
   - Records operations that **read user-provided data**.
   - Example: Listing files in Cloud Storage and downloading a file.

3. **Data-write** ✏️
   - Records operations that **write user-provided data**.
   - Example: Creating a new file in Cloud Storage.

> Exempting users or groups can help **reduce costs** for logs that are not of interest.

---

## ⚙️ Enabling via CLI or API

You can configure Data Access audit logs using the **Google Cloud CLI** or the **API**:

1. **Get current IAM policies**:
   ```bash
   gcloud projects get-iam-policy PROJECT_ID > /tmp/policy.yaml
   ```
2. Edit policy.yaml to add or update auditLogConfigs:
  - You can enable logging per service, e.g., Cloud Run.
  - Optionally, enable logging on all services.
3. Apply updated IAM policy:
   ```bash
   gcloud projects set-iam-policy PROJECT_ID /tmp/policy.yaml
   ```
> This ensures that the desired Data Access audit logs are enabled at the appropriate level.

