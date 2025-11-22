# 🔐 Identity and Access Management (IAM)

When an organization has many folders, projects, and resources, it becomes essential to **control who has access to what**.  
Google Cloud’s **Identity and Access Management (IAM)** provides the framework to manage permissions across resources effectively.

---

## 🧭 What IAM Does

IAM allows administrators to define policies that specify:

- **Who** (the principal)  
- **Can do what** (the role or permission)  
- **On which resource**  

---

## 👤 IAM Principals (The “Who”)

A **principal** is an identity that IAM can grant permissions to. Principals can be:

- **Google Accounts** (individual users)  
- **Google Groups**  
- **Service Accounts**  
- **Cloud Identity Domains**  

Each principal is identified by an **email address**.

---

## 🛠️ Roles (The “Can Do What”)

A **role** is a set of permissions.  
When you grant a role to a principal, you grant **all the permissions** within that role.

### Example:
To manage VM instances, a principal needs permissions to:
- Create  
- Delete  
- Start  
- Stop  
- Modify  

These are grouped into a **single role** for easier assignment and management.

---

## 🏗️ IAM and the Resource Hierarchy

When you assign a role at any level in the resource hierarchy, it applies to that level **and all levels beneath it**:

- Organization  
- Folder  
- Project  
- Resource  

This makes IAM scalable and consistent across large organizations.

---

## 🚫 Deny Rules

IAM supports **deny policies**, which:

- Explicitly prevent certain principals from using specific permissions  
- Override any allow policies  
- Also inherit downward through the hierarchy  

IAM always checks **deny policies first**.

---

# 🎭 Types of IAM Roles

Google Cloud IAM offers **three types of roles**:

## 1️⃣ Basic Roles  
Broad, project-wide roles:
- 👁️ **Viewer** – Read-only access  
- ✏️ **Editor** – Read + write access  
- 👑 **Owner** – Editor + manage roles + set up billing  
- 💳 **Billing Administrator** – Manage billing only  

⚠️ **Caution:** Basic roles may be too broad for sensitive environments.

---

## 2️⃣ Predefined Roles  
More fine-grained roles created by Google for specific services.

Example: Compute Engine predefined roles  
- `roles/compute.instanceAdmin`  
- `roles/compute.viewer`  
- `roles/compute.networkAdmin`  

These can be assigned at:
- Resource level  
- Project level  
- Folder level  
- Organization level  

They allow very specific permissions tailored to different job functions.

---

## 3️⃣ Custom Roles  
Created by administrators for **precise, least-privilege access**.

Example:  
You want users to *start/stop* VMs but **not** reconfigure them.  
You could create a custom role called **instanceOperator** with only those permissions.

### ⚠️ Important Notes About Custom Roles:
1. You must **manage** the permissions manually.  
   Many organizations prefer predefined roles to reduce overhead.  
2. Custom roles can only be applied at:  
   - **Project level**  
   - **Organization level**  
   ❌ Not at the folder level.

---

IAM empowers organizations to follow the **principle of least privilege**, ensuring that each user and service has only the access they truly need — nothing more. 🔒✨
