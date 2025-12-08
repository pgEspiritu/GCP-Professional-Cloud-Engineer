# ⚡ Auto Scaling and Health Checks in Managed Instance Groups

Managed Instance Groups (MIGs) provide **autoscaling** and **health check** capabilities to ensure optimal performance, reliability, and cost efficiency.

---

## 1️⃣ Autoscaling

- **Definition:** Automatically adds or removes VM instances based on **load changes** 📈📉  
- **Benefits:**  
  - Handles traffic spikes gracefully  
  - Reduces costs during low-demand periods 💰  
- **Autoscaling Policies:**  
  - **CPU utilization**  
  - **Load balancing capacity**  
  - **Monitoring metrics** (Cloud Monitoring)  
  - **Queue-based workloads** (e.g., Pub/Sub)  
  - **Scheduled scaling** (start-time, duration, recurrence)  
- **Example:**  
  - 2 instances at **100%** and **85% CPU utilization**  
  - Target CPU: **75%**  
  - Autoscaler adds a new instance to distribute load  
  - Reduces instances if load falls below target  

- **Monitoring:**  
  - Metrics can be visualized via **Cloud Console** or **Cloud Monitoring**  
  - Track CPU, disk, and network usage  
  - Set up alerts through notification channels  
- **Resources:** Link to [Autoscaling Documentation](#) in course resources  

---

## 2️⃣ Health Checks

- **Definition:** Monitors instance health, similar to **Cloud Monitoring uptime checks** 🩺  
- **Configuration Parameters:**  
  - **Protocol** (HTTP, TCP, HTTPS)  
  - **Port**  
  - **Check Interval:** How often to check instance health  
  - **Timeout:** Maximum wait for a response  
  - **Healthy Threshold:** Successful checks required to mark instance healthy ✅  
  - **Unhealthy Threshold:** Failed checks required to mark instance unhealthy ❌  

- **Example:**  
  - Health check fails **2 times over 15 seconds** → instance considered **unhealthy**  

---

## 3️⃣ Stateful IP Addresses

- **Definition:** Preserve **internal and external IPs** across autohealing, updates, or instance recreation 🔄  
- **Benefits:**  
  - Maintain static IPs for applications that require them  
  - Ensure existing configuration and network accessibility remain intact  
  - Useful for migrating workloads without changing network configurations  
- **Configuration:**  
  - Assign IPs automatically or specify for each VM  
  - Configure **stateful policy** for existing and future instances  
  - Promotes ephemeral IPs to static IPs when needed  

> ⚡ **Key takeaway:** Autoscaling ensures resource efficiency, health checks maintain reliability, and stateful IPs provide continuity for network-dependent applications.
