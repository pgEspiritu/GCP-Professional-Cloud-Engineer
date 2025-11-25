# 🎯 SLOs, SLIs, and SLAs Explained

When you express your **Service Level Indicators (SLIs)** properly, your **Service Level Objectives (SLOs)** usually target values just below **100%**, such as **99.9% (three nines)**. You can’t measure everything, so your SLOs should follow the **S.M.A.R.T.** framework.

---

## 🧠 What Makes a Good SLO? (S.M.A.R.T.)

### 🔹 **S — Specific**
- ❌ *“Is the site fast enough?”* → Too subjective  
- ✅ *“The 95th percentile of requests return in under 100 ms.”* → Clear and specific

### 🔹 **M — Measurable**
- SLIs must be based on **quantifiable data**.  
- Monitoring involves numbers over time, math, and patterns.  
- An SLI must be a **number or delta** that can be placed into a mathematical equation.

### 🔹 **A — Achievable**
- ❌ *100% availability* is not realistic or sustainable  
- SLO must be a goal you can consistently meet over time

### 🔹 **R — Relevant**
- Does the metric matter to the user?  
- Does it support the application’s goals?  
- If not, it’s not a good SLO.

### 🔹 **T — Time-Bound**
When you say **99% availability**, specify:
- Over what time period?  
  - per year?  
  - per month?  
  - per day?  
- Is it a **fixed window** (e.g., Sunday–Sunday)?  
- Or a **rolling window** (e.g., last 7 days)?  

If the window is unclear, the metric cannot be measured properly.

---

# 📃 Service Level Agreements (SLAs)

A **Service Level Agreement (SLA)** is a **commitment to customers** about the minimum level of service you will provide, usually tied to **paying customers**.

### 📌 SLAs include:
- Minimum acceptable service levels  
- Definitions of “downtime” or failures  
- **Compensation policies** (refunds or credits) for breaches  

SLAs represent *minimum commitments* to avoid losing customers.

---

# 🌐 Why SLAs Alone Are Not Enough

SLAs often push companies to:
- Promise only the minimum needed  
- Avoid frequent compensation  
- Maintain service just above the threshold that prevents customer churn

Customers can still feel the impact of poor reliability **long before** the SLA is breached.

---

# 🛠️ Internal Reliability Targets

Organizations need **internal SLOs** that are **stricter** than customer-facing SLAs to catch issues earlier.

👉 **Alerting thresholds are usually higher than SLA requirements**  
This gives teams time to:
- Detect problems early  
- Take corrective action  
- Prevent damage to user experience and reputation  

---

# 🚨 Being Out of SLO Must Have Consequences

For SLIs/SLOs/SLAs to truly improve reliability:
- All teams must agree they accurately reflect **user experience**  
- The business must use them to guide **decision-making**  
- Falling out of SLO must trigger:
  - Reduced rate of change ⚠️  
  - Increased engineering effort on reliability 🔧  
  - Prioritization of risk mitigation 🛡️  

Operations teams need **executive support** to enforce these consequences effectively.

---

# 📌 Example of SLA, SLO, and SLI

- **SLI (Indicator):** Error rate  
- **SLO (Objective):** Error rate must be **< 0.3%**  
- **SLA (Agreement):** If billing system exceeds 0.3% error rate for longer than allowed → customers receive compensation

---

# 🧩 Summary

- **SLIs** → *What you measure*  
- **SLOs** → *Your internal performance targets*  
- **SLAs** → *Your formal promises to customers*

And to protect your service and reputation, your **alerting thresholds** should be stricter than your SLAs.

