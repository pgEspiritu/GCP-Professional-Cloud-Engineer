# 💰 Google Cloud Billing Overview

Because all resource consumption under a project accumulates into a single **billing account**, it's important to understand how to manage and optimize your costs.

---

# 📊 Budgets in Google Cloud

To support project planning and cost control, you can **set a budget**. Budgets help you track how your spending grows toward a defined amount.

### 📝 Budget Setup Steps
1. **Set a budget name**  
2. **Choose the project** the budget will apply to  
3. **Define the budget amount**  
   - A fixed amount **or**  
   - Match last month’s spend  

---

# 🔔 Budget Alerts

After selecting your budget amount, you can configure **budget alerts**.

These alerts notify Billing Admins when spending exceeds:
- ⚠️ **50%**
- 🚨 **90%**
- 🔥 **100%** of the budget

You can also set alerts for **forecasted overspend**, letting you know if projected usage will exceed the budget before the billing period ends.

### 📬 Email Alerts
An example email includes:
- Project name  
- Percent of budget exceeded  
- Total budget amount  

---

# 📡 Pub/Sub Spend Notifications

In addition to email alerts, you can:
- Use **Pub/Sub** to receive programmatic spend updates  
- Create a **Cloud Run** function to automatically react to cost spikes  
  - Example: Automatically stop non-critical VMs  

---

# 🏷️ Using Labels to Analyze and Reduce Costs

Labels can help you:
- Track spend across regions  
- Identify resources sending traffic across continents (which may increase cost)  
- Optimize placement of instances  
- Use **Cloud CDN** to cache content closer to users, lowering networking costs  

---

# 🧪 Exporting Billing Data to BigQuery

I recommend:
- Labeling **all your resources**  
- Exporting billing data to **BigQuery** for deep analysis  

BigQuery is Google's fully managed, scalable enterprise data warehouse offering:
- ⚡ Fast SQL queries  
- 📁 Easy data manipulation  
- 🧮 Strong analytical capability  

Example queries can break down cost by:
- Resource  
- Label  
- Service  
- Region  

---

# 📈 Visualizing Spend with Looker Studio

You can visualize your spending trends using **Looker Studio**, which provides:
- Interactive dashboards  
- Customizable reports  
- Easy sharing within your team  

For example, you can graph spending per label over time to understand cost drivers.

---

