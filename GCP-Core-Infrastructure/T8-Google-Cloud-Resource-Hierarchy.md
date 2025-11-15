# 🏗️ Google Cloud Resource Hierarchy

In this section, we explore how Google Cloud organizes and manages resources. Understanding the **resource hierarchy** is essential because it directly influences how **policies, permissions, and access controls** are applied.

---

## 🌳 Four Levels of the Resource Hierarchy

From bottom to top, Google Cloud’s hierarchy contains **four levels**:

1. **🧱 Resources**  
2. **📦 Projects**  
3. **🗂️ Folders**  
4. **🏛️ Organization Node**

---

## 🧱 1. Resources (Level 1)

Resources are the **individual components** you create and use in Google Cloud:

- Virtual Machines (VMs)  
- Cloud Storage buckets  
- BigQuery tables  
- And many more…

Every resource must belong to **exactly one project**.

---

## 📦 2. Projects (Level 2)

Projects are the foundation of all Google Cloud usage.

### 🌟 What Projects Do:
- Enable and manage **APIs**  
- Control **billing**  
- Add/remove **collaborators**  
- Group related **resources**  
- Act as separate **security + billing boundaries**  

### 🆔 Identifiers for Each Project:
- **Project ID** (immutable, globally unique — cannot be changed)  
- **Project Name** (user-defined, can be changed anytime, not unique)  
- **Project Number** (Google-generated, mostly for internal tracking)

### 🧰 Resource Manager API
Google Cloud offers the **Resource Manager API**, which allows you to:
- List all projects  
- Create/update/delete projects  
- Recover recently deleted projects  

Accessible via **RPC** and **REST APIs**.

---

## 🗂️ 3. Folders (Level 3)

Folders help **organize projects** and delegate administration.

### 📁 What Folders Can Contain:
- Projects  
- Other folders  
- Or both

### 🔑 Why Use Folders:
- Apply policies at a **granular level**  
- Group resources for different teams or departments  
- Inherit permissions downward ➝ projects and resources automatically follow parent folder policies  
- Reduce duplication of IAM policies across multiple projects  

📌 Example:  
Two projects managed by the same team can be placed inside one folder so they share the same permissions.

---

## 🏛️ 4. Organization Node (Level 4 — Top Level)

The **Organization Node** sits at the top of the hierarchy and represents your entire company in Google Cloud.

Everything under it—folders, projects, resources—are considered its **children**.

### ⚙️ Special Roles at the Organization Level:
- **Organization Policy Administrator** – controls who can modify policies  
- **Project Creator** – controls who can create new projects (helps manage spending)  

### 🏢 Creating an Organization Node
Depends on your account type:
- If your company uses **Google Workspace**, organization nodes are created automatically.  
- If not, you can use **Cloud Identity** to generate one.

Once created, members of the domain can:
- Create folders  
- Create projects  
- Manage resources under the organization

---

## 🔁 Policy Inheritance Across the Hierarchy

Policies can be applied at:
- Organization level  
- Folder level  
- Project level  
- Sometimes individual resources  

📥 **Policies are inherited downward**.  
A policy applied at the folder level applies to ALL:
- Projects inside that folder  
- Resources inside those projects  

This ensures consistent and efficient policy management.

---

## 🧩 Summary Diagram (Conceptual)

