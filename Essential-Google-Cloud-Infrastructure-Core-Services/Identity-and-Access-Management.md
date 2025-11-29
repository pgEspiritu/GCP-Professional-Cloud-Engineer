# 🌐 Identity and Access Management (IAM)

## 🤔 What is Identity and Access Management?

Identity and Access Management (IAM) is a system that defines **who** can do **what** on **which resource** in Google Cloud.

- **👤 Who** — This can be a person, group, or application.  
- **🛠️ What** — Refers to specific privileges or actions.  
- **📦 Resource** — Any Google Cloud service.

### 📝 Example  
You could be given the **Compute Viewer** role.  
This means:  
- ✅ You can **get** and **list** Compute Engine resources.  
- 🚫 You **cannot** read the data stored on those resources.

---

## 🧩 IAM Components  
IAM is made up of different objects, which will be covered in this module.

To understand how they fit together, we look at:  
- 🔐 IAM policies  
- 🌳 The IAM resource hierarchy  

---

## 🌲 Google Cloud Resource Hierarchy

Google Cloud organizes resources in a **hierarchical structure**:

### 🏢 1. Organization (Root Node)
- Represents your company.  
- Roles granted here are **inherited** by *all resources* under it.  

### 🗂️ 2. Folders
- Represent departments or teams.  
- Roles granted here are inherited by everything inside the folder.  

### 📁 3. Projects
- Represent a **trust boundary** in your company.  
- Services within the same project have the same default trust level.  

### 🔧 4. Resources
- Individual services (VMs, buckets, databases, etc.).  
- Each resource has **exactly one parent** in the hierarchy.

