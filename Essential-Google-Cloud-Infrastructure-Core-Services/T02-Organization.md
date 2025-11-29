# 🏢 Organization Node in Google Cloud

## 🌳 Understanding the Organization Node  
The **organization resource** is the **root node** in the Google Cloud resource hierarchy. It sits at the very top and plays a central role in managing access and structure.

---

## 👥 Key Organization-Level Roles

### 🛡️ Organization Admin  
- A powerful role that gives a user (e.g., **Bob**) the ability to administer **all resources** in the organization.  
- Useful for auditing and high-level administration.

### 🏗️ Project Creator  
- Allows a user (e.g., **Alice**) to **create projects** within the organization.  
- When applied at the organization level, this role is **inherited** by all projects.

---

## 🔗 Relationship With Google Workspace / Cloud Identity  
- The **organization resource** is tightly connected to a **Google Workspace** or **Cloud Identity** account.  
- When a user with these accounts creates a Google Cloud project:  
  - 🏢 An **organization resource** is automatically created.  
  - 📣 Google Cloud notifies the Workspace/Cloud Identity **super admins**.  

### ⚠️ Super Admin Accounts  
Super admins have **broad control**, so they must be used carefully.

---

## 👤 Roles Involved in Organization Setup

### 🧑‍💼 Google Workspace / Cloud Identity Super Administrator  
Responsibilities:  
- 🔐 Assign the **Organization Admin** role  
- ☎️ Serve as point of contact for recovery issues  
- 🔄 Manage lifecycle of the Workspace/Cloud Identity account and the organization resource  

### 🧑‍💼 Organization Admin  
Responsibilities:  
- 📝 Define IAM policies  
- 🏗️ Structure the resource hierarchy  
- 🎯 Delegate responsibility for critical components (networking, billing, hierarchy)  
- ❗ Does *not* automatically have permission to create folders — additional roles are needed

---

# 🗂️ Folders in Google Cloud

Folders act like **sub-organizations** and create additional structure and isolation inside an organization.

## 🧱 Why Use Folders?
Folders can be used to organize by:  
- 🏢 Departments  
- ⚖️ Legal entities  
- 👥 Teams  
- 🧩 Applications  

### 🌲 Example Folder Structure  
- **Level 1:** Departments X & Y  
- **Level 2:** Subfolders for teams (e.g., Team A, Team B)  
- **Level 3:** Product or application folders (e.g., Product 1, Product 2)

### 🎯 Benefits  
- Delegation of admin rights (e.g., department heads manage their own resources)  
- Isolation (users in one department only access resources in their folder)

---

# 🛠️ Resource Manager Roles

Remember: **Policies are inherited top → bottom**.

## 🏢 Organization-Level Roles
- 👀 **Organization Viewer** — view all resources in the organization

## 🗂️ Folder-Level Roles
- 🛠️ **Folder Admin** — full control over folders  
- 🏗️ **Folder Creator** — browse hierarchy and create folders  
- 👀 **Folder Viewer** — view folders and projects under a resource  

## 📁 Project-Level Roles
- 🆕 **Project Creator** — create new projects (becomes owner automatically)  
- 🗑️ **Project Deleter** — delete projects  

