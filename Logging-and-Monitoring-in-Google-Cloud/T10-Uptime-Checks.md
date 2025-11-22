# ⏱️ Uptime Checks in Cloud Monitoring

Another key monitoring component is **Uptime Checks**, which ensure the availability of your services globally. 🌎

---

## 🔹 Overview

- Uptime checks test **public services** from multiple locations around the world.
- Supported **protocols**: `HTTP`, `HTTPS`, `TCP`
- Supported **resources**:
  - App Engine application 🚀  
  - Compute Engine instance 🖥️  
  - URL of a host 🌐  
  - AWS instance or load balancer ☁️
- Each uptime check can:
  - Trigger an **alerting policy** 🚨  
  - Show **latency** for each global location ⏱️  

Uptime checks help ensure external services are running reliably and prevent burning your **error budgets** unnecessarily.

---

## 🔹 Example: HTTP Uptime Check

- Resource is checked **every minute** ⏳  
- Timeout: **10 seconds**  
- If no response within timeout → counted as a **failure** ❌  
- Status example: **100% uptime with no outages** ✅

---

## 🔹 Creating an Uptime Check

1. Go to **Monitoring → Uptime Checks**  
2. Click **Create Uptime Check**  
3. Provide a **descriptive name** for the uptime check  
4. Select:
   - **Check type / protocol**  
   - **Resource type**  
   - Additional information for the resource (e.g., hostname, optional page path for a URL)  

### Optional Advanced Options:
- Log failures 📋  
- Limit test locations globally 🌍  
- Add **custom headers** 📝  
- Set **check timeout** ⏲️  
- Enable **authentication** 🔑  

- Easily create **alerts** for failing uptime checks.

---

## 🔹 Monitoring Multiple Projects

Cloud Monitoring allows **monitoring multiple projects from a single metrics scope**:

1. Start with **three Google Cloud projects**:  
   - Two have **monitorable resources**  
   - One to **host the metrics scope**  
2. Attach the two resource projects to the metrics scope  
3. Build **uptime checks**  
4. Construct a **centralized dashboard** 🖥️
