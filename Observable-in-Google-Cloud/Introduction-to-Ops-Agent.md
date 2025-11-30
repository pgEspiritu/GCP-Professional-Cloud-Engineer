# 🖥️ Introduction to Ops Agent

The first part of this module covers how to collect metrics from applications deployed on **Google Cloud Compute Engine**.

---

## 📊 Why Use the Ops Agent?

Monitoring data can originate from many different sources.  
For Compute Engine VMs, Cloud Monitoring can access some metrics *without* an agent:

- 🧮 CPU utilization  
- 💾 Disk traffic metrics  
- 🌐 Network traffic  
- ⏱️ Uptime  

But these can be **augmented** by installing an agent inside the VM’s operating system.

Many mission-critical services run directly on Compute Engine. To improve observability for these workloads, Google provides the **Ops Agent**.

---

## 🚀 What Is the Ops Agent?

The Ops Agent is the **primary telemetry collector** for Compute Engine instances.

### Key characteristics:
- Combines **logging + metrics** in a single agent  
- Uses **Fluent Bit** for high-throughput logs  
- Uses the **OpenTelemetry Collector** for metrics  
- Collects internal VM metrics that the hypervisor **cannot** access (e.g., memory usage)  
- Supports monitoring third-party applications like:
  - Apache  
  - MySQL  
  - Oracle DB  
  - SAP HANA  
  - NGINX  

Supports major OSes: **CentOS, Ubuntu, Windows**

---

## 🧠 Why Do We Need the Ops Agent?

Example: Infrastructure dashboard showing multiple Compute Engine VMs.  
You might notice **missing memory usage data**.

➡️ The hypervisor only knows how much memory is *allocated*, not how much is *used*.  
➡️ Only an in-guest agent (like Ops Agent) can collect this.

### Benefits of running Ops Agent:
- 🔧 No extra configuration needed after installation  
- 📡 Monitors 3rd-party applications  
- 🪟 Supports both Windows & Linux  
- 📈 Exposes more metrics (CPU, disk, network, processes)  
- ➕ Provides metrics beyond the 80+ default Compute Engine metrics  
- 🔗 Unifies logs & metrics  
- 📥 Ingests custom Prometheus-format metrics  

---

## 🛠️ How to Install the Ops Agent

You can install the Ops Agent using three methods:

1. **Google Cloud CLI or Console** – for individual VMs  
2. **Agent Policies** – for managing installation across a fleet  
3. **Automation Tools** – Ansible, Chef, Puppet, Terraform  

This module covers methods **1** and **2**.

---

## 📋 Method 1: Installing Using Agent Policies

Steps:

1. Install the beta component  
2. Enable required APIs  
3. Create a policy (example: `ops-agents-test-policy`) targeting:
   - VM: `test-instance`
   - OS: CentOS 7  
   - Installs both Logging + Monitoring agents  

---

## 🖱️ Method 2: Installing on Individual VMs

1. Go to **VM instances** page  
2. Click the VM name  
3. Open the **Observability** tab  
4. Click **Install**  
5. A window opens → click **Run in Cloud Shell**  
6. Cloud Shell launches with the install command pre-filled  
7. Press **Enter**  
8. Click **Authorize**  

You can also install specific agent versions depending on OS.

---

## 🐛 Troubleshooting

If logs are not reaching Cloud Logging:

- Check the **metrics module** in syslog  
- If no logs → Agent service may not be running  
- If you see **403 permission errors** →  
  - Enable the **Logging API**  
  - Assign the **Logs Writer** role  

Always refer to Google documentation for detailed troubleshooting.

---

## 📊 Viewing Data in Dashboards

Ops Agent data can be previewed in dashboards for third-party applications like:

- Apache  
- MySQL  
- Redis  

To view logs, metrics, and dashboards:

1. Go to **Monitoring**  
2. Open **Integrations**  

Integrations unify:

- Metrics  
- Logs  
- Dashboards  
- Alerts  

Built on open-source foundations, they are fully customizable.

---
