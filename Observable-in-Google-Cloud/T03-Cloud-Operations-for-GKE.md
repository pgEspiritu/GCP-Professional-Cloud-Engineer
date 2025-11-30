## ☸️ Cloud Operations for GKE

We’ve already explored monitoring for Compute Engine and other compute options.  
Now let’s focus on **monitoring specifically for Google Kubernetes Engine (GKE)**.

---

### 🔍 Built-In Observability for GKE
Google Kubernetes Engine includes native integration with:

- **Cloud Logging**
- **Cloud Monitoring**
- **Google Cloud Managed Service for Prometheus**

These tools provide observability **tailored for Kubernetes environments**.

---

### ⚙️ Default Monitoring & Logging
When you create a GKE cluster on Google Cloud:

- **Cloud Logging and Cloud Monitoring are enabled by default**
- Kubernetes-specific metrics and logs are automatically collected
- You can also enable **Managed Service for Prometheus** to collect Prometheus metrics from:
  - Workloads running on GKE
  - Workloads running outside of GKE (multi-environment monitoring)

> More details about Managed Service for Prometheus will be covered in the next section.

---

### 🛠️ Configuration Options
You can configure GKE observability **during cluster creation** or **after cluster creation**.

#### During Cluster Creation:
1. Go to the **Operations** section.
2. Select **Standard mode**.
3. Ensure **Cloud Logging** and **Cloud Monitoring** are enabled (they are ON by default).
4. Under **Components**, choose which logs and metrics to collect.
5. To enable Prometheus monitoring:
   - Select **Managed Service for Prometheus** under Managed Collection.

---

### 📦 Summary
GKE provides integrated, customizable monitoring with Cloud Operations tools:
- Automatic Logging + Monitoring for Kubernetes clusters
- Optional Prometheus metric collection through a managed service
- Flexible configuration at creation time or afterward
