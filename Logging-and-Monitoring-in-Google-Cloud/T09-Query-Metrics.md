# 🔍 Query Metrics in Cloud Monitoring

Before wrapping up dashboards, charts, and Metrics Explorer, let’s look at a more flexible way to interact with metrics using **query languages** like **Monitoring Query Language (MQL)** and **PromQL**. 🚀

---

# 🧠 Monitoring Query Language (MQL)

## 📌 What is MQL?
MQL is an **advanced, expressive, text-based query language** used to:
- Retrieve 📥
- Filter 🔍
- Manipulate 🔄  
time-series data in Cloud Monitoring.

It provides capabilities far beyond the menu-driven Metrics Explorer.

---

# 📊 PromQL in Cloud Monitoring

PromQL is an alternative interface for querying and charting Cloud Monitoring data.  
You can use PromQL for metrics coming from:

### 🔹 Google Cloud services:
- Google Kubernetes Engine (GKE) ☸️  
- Compute Engine 🖥️  
- Other Cloud Monitoring system metrics

### 🔹 User-defined metrics:
- Log-based metrics  
- Custom Cloud Monitoring metrics  

### 🔹 Google Managed Service for Prometheus
A **fully managed multi-cloud Prometheus solution** with built-in PromQL support.

### 🔹 Third-party tools:
You may also use **Grafana** to visualize metric data:
- Prometheus metrics via Managed Service  
- Cloud Monitoring system metrics  

---

# 🧰 When to Use MQL

MQL is extremely powerful. Here are common use cases:

### ✳️ Create ratio-based charts and alerts  
(e.g., error rate ratio)

### ⏳ Perform time-shift analysis  
Compare:
- Week over week  
- Month over month  
- Year over year  

### ➗ Apply math, logic, table operations  
Build advanced calculations

### 🔗 Fetch, join, and aggregate multiple metrics  

### 📊 Select any percentile  
(even those not predefined)

### 🏷️ Create new labels  
Using string manipulation and regex

MQL gives you limitless flexibility! ♾️

---

# ⚙️ How MQL Works

MQL uses **operations and functions**, connected via the familiar **pipe (`|`)** syntax:
```text
fetch → filter → transform → aggregate
```


Just like in Linux pipes:
- The **output** of one step  
- Becomes the **input** of the next  

This allows incremental, readable, flexible query building.

---

# 📉 Example Scenario: Request Failure Ratio

You have a distributed web service running on Compute Engine + Cloud Load Balancing.  
You want to monitor one of the SRE **“Golden Signals”** → **Error Rate** ❗

### 🎯 Goal:
Create a chart showing the ratio of:
```texy
HTTP 500 responses
Total number of HTTP requests
```


### 🔹 Relevant metric:
```bash
loadbalancing.googleapis.com/https/request_count
```

This metric includes a label:
- `response_code_class` → groups response codes

### 🔍 MQL logic:
1. Use `if()` to select only HTTP 500 responses  
2. Use `sum()` to count all 500-class responses  
3. Use another `sum()` to count **all** requests (via `val()`)  
4. Divide the two sums → failure ratio  

This gives the **request failure ratio** graph.

---

# 💻 Using MQL or PromQL in Metrics Explorer

You can write queries directly in the Cloud Console:

1. Open **Metrics Explorer**  
2. Click **CODE**  
3. Select:
   - **MQL**  
   - **PromQL**  
(using the radio selector)

PromQL will be covered in a later module.

---

