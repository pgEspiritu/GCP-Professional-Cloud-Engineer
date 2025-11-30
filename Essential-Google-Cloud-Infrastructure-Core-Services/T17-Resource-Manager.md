# 📘 Resource Manager Overview

Let’s get started with an overview of **Resource Manager**.  

The Resource Manager lets you **hierarchically manage resources** by project, folder, and organization.  
This should sound familiar because we covered it in the IAM module. Let’s refresh your memory. 🔁

---

## 🛡️ IAM Policies and Inheritance

- Policies contain a set of **roles** and **members**, and policies are applied to **resources**.
- Resources **inherit policies** from their parent.  
- Resource policies = **union of parent policy + resource policy** (if the policy is an IAM *allow* policy).
- If an **IAM deny policy** is associated with a resource, it **blocks specific principals** from using certain permissions, even if they have roles granting them.

---

## 💰 Billing Hierarchy (Bottom-Up)

Billing works differently: it is accumulated **from the bottom up**.

- Resource consumption is measured in:
  - Rate of use
  - Time
  - Number of items
  - Feature usage
- A resource belongs to **one project**, and the project accumulates all its resource consumption.
- Each project is linked to **one billing account**.
- An **organization** contains all billing accounts.

---

## 🏢 Organizations, Projects, and Access

The **organization node** is the **root** for all Google Cloud resources.

Example:

- **Bob** controls the organization's domain via the **Organization Admin** role.
- Bob grants **Project Creator** privileges to **Alice**, allowing her to manage individual projects.

Projects allow you to:

- Enable billing  
- Manage permissions and credentials  
- Enable services and APIs  
- Track resource usage and quotas  

---

## 🆔 Identifying a Project

Every request in Google Cloud requires project identifiers. A project has:

- **Project Name** — human-readable (not used by APIs)
- **Project Number** — system-generated
- **Project ID** — unique, derived from the project name

You can find these on the Google Cloud Console dashboard or via the Resource Manager API.

---

## 🗂️ Resource Hierarchy (Physical Organization)

Resources are classified as:

### 🌐 Global Resources
- Images  
- Snapshots  
- Networks  

### 📍 Regional Resources
- External IP addresses  

### 📦 Zonal Resources
- Instances  
- Disks  

Regardless of category, **every resource belongs to a project**, enabling per-project billing and reporting. 📊

---
