# 🎭 IAM Roles

## 🎯 What Are Roles?
Roles define **“who can do what on which resource”** in IAM.  
There are **three types of roles** in Google Cloud IAM:

- 🧱 **Basic Roles**  
- 🧩 **Predefined Roles**  
- 🛠️ **Custom Roles**

---

# 🧱 Basic Roles
Basic roles are the *original*, broad Google Cloud roles.  
They apply at the **project level** and affect **all resources** within that project.  
They provide **coarse-grained** levels of access.

### 🔑 Basic Roles Include:
- 👑 **Owner** — Full administrative access  
  - Can add/remove members  
  - Can delete projects  
- ✏️ **Editor** — Modify and delete access  
  - Developers can deploy apps and configure resources  
- 👀 **Viewer** — Read-only access  
- 💰 **Billing Administrator** — Manage billing  
  - Can add/remove billing admins  
  - **Cannot** modify project resources  

### 🎯 Concentric Permissions  
- Owner ➝ includes *Editor* permissions  
- Editor ➝ includes *Viewer* permissions  
- Viewer ➝ read-only  

Each project can have **multiple** Owners, Editors, Viewers, and Billing Admins.

---

# 🧩 Predefined Roles
Google Cloud services provide their own **predefined roles** with **granular** access control.

### 🌟 Characteristics:
- More **fine-grained** than basic roles  
- Designed for specific services (e.g., Compute Engine, Storage, Networking)  
- Contain **collections of permissions** needed to perform meaningful tasks  
- Prevent users from gaining permissions they *don't* need  

### 🧪 Example
A group is granted the **InstanceAdmin** role on `project_a`  
→ They get all the necessary Compute Engine permissions for managing instances.

### 💡 Permissions Format  
Permissions are structured as:  
`service.resource.verb`  
Example:  
- `compute.instances.start`  
  - Service: Compute Engine  
  - Resource: instances  
  - Verb: start  
  - Meaning: Start a stopped VM instance  

These map closely to Google Cloud REST APIs.

---

# 🖥️ Common Predefined Roles in Compute Engine

### 🧑‍💻 Compute Admin  
- Full control over **all** Compute Engine resources  
- Includes **all permissions** starting with `compute.*`  

### 🌐 Network Admin  
- Create, modify, and delete networking resources  
- **Except:** firewall rules and SSL certificates  
- Read-only access to:  
  - Firewall rules  
  - SSL certificates  
  - Instances (to view ephemeral IPs)

### 💾 Storage Admin  
- Create, modify, and delete:  
  - Disks  
  - Images  
  - Snapshots  
- Useful for roles like an image manager  
- Safer alternative to granting full Editor role  

For the full list, see the student PDF.

---

# 🛠️ Custom Roles
Custom roles are used when predefined roles are **too broad** or **don’t fit** your exact needs.

### 🧭 Least-Privilege Model  
Many organizations follow the principle of **least privilege**:  
Each user gets only the **minimum permissions** needed for their job.

### 📝 Example  
You want to create an **“Instance Operator”** role:  
- Can **start** and **stop** VM instances  
- ❌ Cannot reconfigure them  
→ Custom roles allow you to build this precise set of permissions.
