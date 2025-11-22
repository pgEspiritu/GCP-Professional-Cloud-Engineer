# 📊 Data Models and Dashboards in Cloud Monitoring

## 🧩 Understanding the Cloud Monitoring Data Model
Monitoring data in Google Cloud is recorded as **time series** ⏱️.  
Each time series contains several important components:

### 🔹 1. Metric Field
- Describes the metric itself  
- Includes:
  - **Metric labels** → one combination of label values  
  - **Metric type** → specifies labels + what the data represents  

### 🔹 2. Resource Field
- Describes the monitored resource  
- Includes:
  - **Resource labels**  
  - The **specific resource** monitored (e.g., a VM instance)

### 🔹 3. `metricKind` and `valueType`
- **valueType** → data type of the measurement  
- **metricKind** → how to interpret values across time  
  - 📌 **Gauge**  
  - 📌 **Delta**  
  - 📌 **Cumulative**

### 🔹 4. Points Field
- An array of **timestamped values**  

---

## 📡 Viewing Time Series Data
1. Go to **Time Series List API Explorer**  
2. Filter:
   - `metric.type = "logging.googleapis.com/log_entry_count"`
   - `resource.type = "gce_instance"`
3. Specify start and end times  
4. Run the request → receive time series data 📈

### Example Interpretation
- **Metric:** `log_entry_count` with severity `INFO`  
- **Resource:** A specific Compute Engine VM  
- **Metric Kind:** `DELTA`  
- **Value Type:** Integer  
- **Points:** Timestamp + numeric value  

---

# 📊 Dashboards: Visualizing Your Metrics

## 🚀 Getting Started
1. Identify the resources you want to monitor  
2. Check for **auto-created dashboards** (Compute Engine, GKE, etc.)  
3. If missing → use **Metrics Explorer**  
4. You can:
   - Use 1500+ built-in metrics  
   - Use custom metrics  
   - Build your own dashboards  

---

## 🖥️ Auto-Created Dashboards
Google Cloud automatically creates dashboards for:
- 🖥️ Compute Engine VMs  
- ☸️ GKE clusters  

These provide a strong foundation for monitoring.

---

# 🛠️ Building Dashboards with Metrics Explorer

Metrics Explorer allows you to:
- Build ad-hoc charts  
- Save charts to dashboards  
- Share charts via URL  
- View chart configs as JSON  
- Explore temporary data without saving  

---

## 🧱 Metrics Explorer Interface
The interface has 3 main regions:

### 1️⃣ **Configuration Panel**
Choose:
- Resource type  
- Metric  
- Filters  
- Grouping  
- Alignment  

### 2️⃣ **Chart Area**
Visual display of the metric 📈

### 3️⃣ **Display Panel**
Configure:
- Axes  
- Threshold lines  
- Legend alias  

---

# 🎨 Defining a Chart

## 🔹 Metric Selection
A chart must specify:
- **Resource type**
- **Metric type**

---

## 🔹 Filtering
Filters reduce noise by removing irrelevant data.

You can filter by:
- Resource group  
- Resource name  
- Resource labels  
- Metric labels  

Example filter → machine type = `"e2-medium"`

---

## 🔹 Grouping
Grouping combines multiple time series using:
- Grouping **labels**
- Aggregation **functions** (sum, mean, etc.)

Example: Group all VM instances by zone.

---

## 🔹 Alignment
Alignment regularizes timestamps to equal intervals.

- Default: **1 minute**  
- You can change to 1 minute, 5 minutes, 1 hour, etc.

### Alignment Functions:
- Sum  
- Mean  
- Max  
- Min  
- Other valid functions based on metric type

---

# 📈 Chart Display Modes

### 🎚️ Standard Mode
- Each time series shows as a unique colored line  

### 📊 Stats Mode
- Shows statistical summaries (mean, moving average, etc.)

### 🔦 X-Ray Mode
- Each line is translucent gray  
- Overlapping areas appear bright  
- Useful for charts with **many** time series  

### 🔁 Compare to Past
- Adds a second value column:
  - **Today**
  - **Last Week**, **Yesterday**, etc.  

---

# 🎯 Threshold Lines
Add horizontal reference lines to visually indicate:
- Alert thresholds  
- SLO boundaries  
- Capacity limits  

Thresholds can be added on:
- Left Y-axis  
- Right Y-axis

---

# 🏷️ Legend Alias (Custom Labels)
Customize chart legend names using variables and templates.

### Steps:
1. Open **Display** panel  
2. Expand **Legend Alias**  
3. Add template variables (e.g., project ID, instance name)

This makes chart legends easier to understand.

---

