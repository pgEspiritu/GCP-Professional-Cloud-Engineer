# 🚨 Developing an Effective Alerting Strategy

Now that we understand **SLA**, **SLI**, and **SLO**, let’s explore how to build a strong **alerting strategy** in Google Cloud Monitoring. Alerts help you detect issues early, preserve your error budget, and protect user experience.

---

# 🔔 What Is an Alert?

An **alert** is an automated notification sent by Google Cloud through a chosen **notification channel** (email, SMS, webhook, ticketing tool, etc.).

### 👉 Alerts are triggered when:
- A service is down  
- An SLO is not being met  
- Something requires action or investigation  

Alerts are based on **time-series data**: event data points captured in equal, consecutive time windows.

These windows determine:
- how events are summarized  
- how error rates are calculated  
- when alert conditions are met  

---

# 📉 Error Budget Concept

Your **error budget** = *100% − SLO*  
Example:  
- **SLO:** “90% of requests must respond within 200 ms”  
- **Error Budget:** 100% − 90% = **10%**  

Alerts should trigger **before** the error budget is consumed.  
A system trending toward exhausting the error budget should notify you early.

---

# 🎯 Measuring Alert Strategy Effectiveness

Good alerting relies on four core attributes:

### 🔹 **Precision**
- Proportion of *correctly triggered* alerts  
- Decreases with **false positives**

### 🔹 **Recall**
- Proportion of *missed alerts*  
- Decreases with **false negatives**

**Precision = exactness**  
**Recall = completeness**

### 🔹 **Detection Time**
- How fast the system identifies a problem  
- Too fast → false positives  
- Too slow → burns error budget

### 🔹 **Reset Time**
- How quickly alerts stop after the issue is resolved  
- Long reset times → confusion, noise

---

# 🪟 Understanding Alert Windows

Alerting involves two types of “windows”:

### 1️⃣ **SLO Measurement Window**
Example:  
- SLO: 99.9% availability over **30 days**

### 2️⃣ **Alert Evaluation Window**
Example:  
- Trigger an alert if errors exceed **0.1%** in any **60-minute window**

---

# ⏱️ Choosing Window Length: Short vs Long

### 🟩 **Short Windows**
- Faster detection ⏱️  
- Shorter reset times  
- More false positives ❗  
- Example:  
  - With a **10-minute window** on a 99.9% SLO (30 days):  
    - Full outage triggers in **0.6 sec**  
    - Consumes **0.02%** of error budget  

### 🟦 **Long Windows**
- Slower detection  
- Fewer false alarms  
- Higher error budget consumption before alert  
- Example:  
  - Using a **36-hour window**:  
    - Full outage detected in **2 min 10 sec**  
    - Consumes **5%** of the error budget  

---

# 🔁 Using Successive Failure Counts

A balanced approach:
- Don’t alert on 1 failed window  
- Alert only after **multiple consecutive failures** (e.g., 3 windows in a row)

This:
- Reduces flukes  
- Captures real, sustained issues  
- Mirrors how humans behave (“Is that sound real?”)

But:  
- Higher precision → lower recall  
- Short outages might be missed entirely

---

# 🧠 Achieving Good Precision *and* Recall

Use **multiple alerting conditions**:

- Different windows  
- Different thresholds  
- Different evaluation criteria  

You don’t need one perfect alert—you combine several.

Example:
- A **short-window alert** triggers a Pub/Sub message  
- A Cloud Run service performs deeper analysis  
- A **human alert** only fires if multiple conditions confirm the issue

This approach improves:
- precision ✔️  
- recall ✔️  
- detection time ✔️  
- reset time ✔️  

---

# 📡 Notification Channels & Severity Levels

Alerts must be prioritized by **customer impact and SLA**.  
Not all alerts should wake a human.

### 🔥 High-Priority Notifications
- Slack  
- SMS  
- PagerDuty  
- Multiple redundant channels  

### 📬 Low-Priority Notifications
- Email  
- Logging only  
- Ticketing systems  

Google Cloud supports **custom severity levels** embedded in notifications for:
- Email  
- Webhooks  
- Pub/Sub  
- PagerDuty  

This allows downstream systems to route and process alerts intelligently.

---

# 📝 Summary

A strong alerting strategy requires:
- Clear SLOs and error budgets  
- Understanding precision vs recall  
- Smart window selection  
- Combining multiple alert conditions  
- Correct notification severity  
- Correct routing to channels  

Alerts should reflect **customer impact**, not just system metrics.  
And remember: **Don’t wake humans unless absolutely necessary.** 😄

