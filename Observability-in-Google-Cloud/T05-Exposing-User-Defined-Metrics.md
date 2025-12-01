## 📐 Exposing User-Defined Metrics in Google Cloud Monitoring

Beyond the 1,000+ automatically collected Google Cloud metrics, you can also create **your own custom (user-defined) metrics** to monitor application-specific behavior that built-in metrics cannot capture.

---

## 🎯 What Are User-Defined (Custom) Metrics?
Custom metrics are **application-specific metrics** that you instrument and send to Cloud Monitoring.  
They allow you to:

- Track app-specific data points  
- Build dashboards using your custom metrics  
- Create alerting policies  
- Integrate them with existing monitoring workflows  

---

## 🛠️ Two Ways to Create Custom Metrics

### **1️⃣ Using the Classic Cloud Monitoring API**
You manually:
- Define a metric descriptor  
- Push TimeSeries data using API calls  
- Associate data points with monitored resources (like VM instances)

### **2️⃣ Using OpenTelemetry + Ops Agent**
The recommended modern approach.

- The **Ops Agent** includes an **OTLP (OpenTelemetry Protocol) receiver**  
- Your app sends OTLP metric data → Agent forwards it to Cloud Monitoring  
- No need for ingestion or authorization configuration in your code  
- Works for dashboards, alerts, uptime checks, etc.

---

## ⚙️ Configuring OTLP with Ops Agent

### **Step 1: Install Ops Agent**
Required on the VM hosting your application.

### **Step 2: Modify Ops Agent Configuration**
Add an OTLP receiver in the Ops Agent user configuration file.

- **Default mode:** `googlemanagedprometheus`
- To create Cloud Monitoring custom metrics, set:

```yaml
metrics_mode: googlecloudmonitoring
```
This ensures the metric is ingested as a Cloud Monitoring custom metric, not a Prometheus metric.

### 🧰 Creating a Custom Metric (Using the API)
1️⃣ Define a Metric Descriptor
Example custom metric:
- Name: my_metric
- Type: Gauge (double)
- Description: "Custom metric example"

2️⃣ Create the Metric Descriptor
Call the API’s create method with a MetricDescriptor object.

3️⃣ Write Time Series (Data Points)
Use create_time_series to push your data.

Each `TimeSeries` requires:
- A metric field → your custom metric name
- A resource field → e.g., a Compute Engine VM
- A single Point object → value + timestamp

Example:
1. Create the TimeSeries
2. Add the Point
3. Push the metric to Cloud Monitoring

---
  
### 📊 Querying Your Custom Metrics

After configuring OTLP or the Monitoring API:

You can query your custom metrics via:
- Metrics Explorer
- Dashboards
- PromQL (if using managed Prometheus)
- Monitoring Query Language (MQL)
- Custom alerting policies

Example (using Metrics Explorer UI):
View the custom metric ingested by the Monitoring API.

---

### 🔍 Example: Viewing Custom Metric in Metrics Explorer
1. Go to Monitoring → Metrics Explorer
2. Search your metric (e.g., custom.googleapis.com/my_metric)
3. Visualize it on a chart
4. Use it for alerts / dashboards
