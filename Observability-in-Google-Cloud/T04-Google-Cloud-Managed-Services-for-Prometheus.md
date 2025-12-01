## 📊 Google Cloud Managed Service for Prometheus

Let’s explore **Google Cloud Managed Service for Prometheus**, its purpose, and how it simplifies collecting and analyzing Prometheus metrics.

---

### 🔧 What Is Managed Service for Prometheus?
Google Cloud Managed Service for Prometheus is a **fully managed, scalable, production-ready Prometheus service** that allows you to:

- Collect Prometheus metrics from **Kubernetes** and **VM-based environments**
- Store and analyze metrics at **massive scale**
- Eliminate operational overhead such as scaling, sharding, and maintaining Prometheus servers

It is powered by **Monarch**, Google’s globally distributed time-series database.

---

### 🌐 What Makes Monarch Special?
Managed Service for Prometheus is built on **Monarch**, the same backend used by Cloud Monitoring. Monarch provides:

- A globally scalable monitoring system  
- High-level data modeling  
- Efficient data collection and querying  
- Multi-region data unification  
- **Up to 2 years of metric retention** at no additional cost  

Monarch handles:
- **Query evaluation**
- **Data storage**
- **Cross-region query union**

---

### 🔍 How It Works – Component Breakdown
Managed Service for Prometheus splits responsibilities into several components:

| Component | Responsibility |
|----------|----------------|
| **Collectors** | Scrape metrics and forward to Monarch |
| **Monarch** | Evaluate queries & store metrics |
| **Rule Evaluators** | Handle local alerting and recording rules |

Collectors can be:
- Managed collectors (recommended)
- Self-deployed collectors
- Ops Agent (on VMs)
- OpenTelemetry

---

### 📈 Querying Your Metrics
You can query Prometheus data through:

- **Grafana**
- **Cloud Monitoring**
- **Any UI compatible with the Prometheus query API**

PromQL can query:
- 1,500+ free Cloud Monitoring Kubernetes metrics  
- Custom metrics  
- Log-based metrics  

Your existing PromQL dashboards will continue to work with no changes.

---

### 🗂️ Data Collection Options

#### **1️⃣ Managed Collection (Recommended for Kubernetes)**
- Uses operator-managed collectors
- Best for a hands-off, fully managed experience
- Supported on **GKE** and **non-GKE Kubernetes**

#### **2️⃣ Self-Deployed Collection**
- You deploy your own Prometheus server
- Use the drop-in replacement binary from Managed Service for Prometheus

#### **3️⃣ Ops Agent (for Compute Engine VMs)**
- Easiest way to collect Prometheus metrics from VMs  
- No need to install or configure Prometheus manually

#### **4️⃣ OpenTelemetry Collector**
- Supports multi-signal workflows (e.g., exemplars)
- Deploy manually or via Terraform  
- Works in any environment  

---

### 💡 Choosing the Right Collection Method
| Environment | Best Option |
|------------|-------------|
| GKE & Kubernetes | **Managed Collection** |
| Complex/custom environment | **Self-Deployed** |
| Compute Engine VMs | **Ops Agent** |
| Multi-signal pipelines | **OpenTelemetry** |

---

### 🔔 Rule Evaluation
- Managed Service for Prometheus includes a **standalone rule evaluator**  
- Supports standard Prometheus rule file formats  
- No need to colocate data in one Prometheus server  
- Works across all projects within a metrics scope  

---

### 🚀 Enabling Managed Collection (GKE Example)
1. Go to your **GKE Cluster**  
2. Select the cluster  
3. Click **ENABLE SELECTED** under Managed Collection  

After enabling:
- In-cluster components start running
- But **no metrics will appear yet** until you define a **PodMonitoring** resource

---

### 📄 Example: PodMonitoring Resource
This manifest defines a `PodMonitoring` resource that scrapes pods with the label `app=prom-example`:

```yaml
apiVersion: monitoring.googleapis.com/v1
kind: PodMonitoring
metadata:
  name: prom-example
  namespace: default
spec:
  selector:
    matchLabels:
      app: prom-example
  endpoints:
  - port: metrics
    interval: 30s
    path: /metrics
```

To apply:
```sh
kubectl apply -f podmonitoring.yaml
```
For cluster-wide monitoring, use ClusterPodMonitoring.

### 🔎 Verifying Your Prometheus Metrics

Using Cloud Monitoring → Metrics Explorer:
1. Open Monitoring in the Google Cloud console
2. Go to Metrics Explorer
3. Select the PromQL tab
4. Run a simple query like:
```promql
up
```
Click Run Query to verify data ingestion.

### 📉 Example PromQL Query

Average CPU utilization for Compute Engine instances:
```promql
avg(rate(instance_cpu_usage_time_seconds[5m]))
```
This query visualizes compute instance CPU usage over time—in this example, over a 1-hour window.
