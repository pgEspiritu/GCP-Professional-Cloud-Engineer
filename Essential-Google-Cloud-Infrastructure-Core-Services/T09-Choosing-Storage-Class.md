# 🗂️ Choosing a Storage Class in Cloud Storage

Use this decision tree to help determine the best **Cloud Storage class** for your data. 🌥️

---

## 📦 Choose Based on Access Frequency

- 📁 **Archive Storage**  
  Use if you will read your data **less than once per year**.

- ❄️ **Coldline Storage**  
  Use if you will read your data **less than once per 90 days**.

- 📉 **Nearline Storage**  
  Use if you will read your data **less than once per 30 days**.

- ⚡ **Standard Storage**  
  Use if you will read/write your data **more frequently** than the above thresholds.

---

## 🌎 Choose Based on Location Type

### 🏙️ Region
- Optimized for **latency** and **network bandwidth**  
- Best for data consumers (e.g., analytics pipelines) located **in the same region**

### 🌐 Dual-Region
- Offers **regional performance**  
- Adds **geo-redundancy** for higher availability

### 🌍 Multi-Region
- Best for serving content to **distributed users** outside the Google network  
- Designed for **maximum availability** and geographic redundancy

---

## 🤖 Autoclass (Recommended for Unpredictable Access Patterns)

Choose **Autoclass** when access frequency is:
- Mixed  
- Unknown  
- Unpredictable  

### How Autoclass Works
- All new objects **start in Standard storage**, regardless of the requested class  
- Data **not accessed** → moves to colder storage classes automatically ❄️  
- Data **accessed** → moves back to Standard storage 🔁  
- Optimizes cost without manual configuration

### 💸 Cost Benefits
- No early deletion charges  
- No retrieval charges  
- No charges for storage class transitions  

Autoclass makes Cloud Storage **automated, cost-efficient, and smart**. 🤖💡
