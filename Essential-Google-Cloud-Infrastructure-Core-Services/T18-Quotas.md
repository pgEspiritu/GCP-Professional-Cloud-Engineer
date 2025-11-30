# 📊 Quotas in Google Cloud

Now that we know a project accumulates the consumption of all its resources, let's talk about **quotas**.

All resources in Google Cloud are subject to **project quotas** or **limits**. These quotas typically fall into three categories:

---

## 🔢 1. Resource Quantity Limits
How many resources you can create per project.

- Example: You can only have **15 VPC networks** per project.

---

## ⚡ 2. API Rate Limits
How quickly you can make API requests (rate limits).

- Example: By default, you can only make **5 administrative actions per second** per project when using the **Spanner API**.

---

## 📍 3. Regional Quotas
Quotas that apply per region.

- Example: By default, you can only have **24 CPUs per region**.

---

## 🤔 What About Large VM Types?
You may wonder: *How can I spin up a 96-core VM if my region quota is 24 CPUs?*

- As your usage grows, Google Cloud may automatically **increase your quotas**.
- You can also **request quota increases** manually through the **Quotas page** in the Google Cloud Console.
- This page also displays your **current quota usage**.

---

## 🛑 Why Quotas Exist

### 🔐 1. Prevent Runaway Consumption  
Quotas stop excessive resource creation due to errors or malicious attacks.  
Example: You accidentally create **100** Compute Engine instances instead of **10** using gcloud.

### 💵 2. Avoid Billing Surprises  
Quotas help prevent unexpected cost spikes.  
(Budgets and alerts—covered later—also help with billing.)

### 🧠 3. Encourage Resource Planning  
Quotas force you to think about appropriate sizing.  
Example: Do you really need a **96-core VM**, or can you use something smaller and cheaper?

---

## ⚠️ Important Note: Quotas ≠ Availability

Quotas represent the **maximum allowed resources**, **not guaranteed availability**.

- Example: If a region runs **out of local SSDs**, you **cannot** create new local SSDs in that region **even if your quota allows it**.

---
