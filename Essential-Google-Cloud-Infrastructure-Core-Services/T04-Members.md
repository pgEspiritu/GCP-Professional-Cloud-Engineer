# 👥 IAM Members

## 🧩 What Are Members?
Members define the **“who”** in the IAM model of **“who can do what on which resource.”**

There are **five types of IAM members**:

1. 👤 **Google Accounts**  
2. 🤖 **Service Accounts**  
3. 👥 **Google Groups**  
4. 🏢 **Google Workspace Domains**  
5. ☁️ **Cloud Identity Domains**

---

# 👤 Google Accounts
- Represents individuals such as developers or administrators.  
- Any email linked to a Google Account can act as an identity (`@gmail.com` or other domains).  
- Users can create Google Accounts without using Gmail.

---

# 🤖 Service Accounts
- These accounts belong to **applications**, not users.  
- When running code on Google Cloud, you specify **which service account it runs as**.  
- You can create many service accounts for different components of your application.

---

# 👥 Google Groups
- A named collection of Google Accounts **and** service accounts.  
- Each group has a **unique email address**.  
- Great for applying access policies to many users at once.

---

# 🏢 Google Workspace Domains
- Represents all Google Accounts in your organization’s Workspace domain (e.g., `example.com`).  
- Adding a user to Workspace creates a corresponding Google Account for them.

---

# ☁️ Cloud Identity Domains
- For organizations not using Google Workspace.  
- Lets you manage users and groups without Workspace apps like Gmail or Drive.  
- Managed through the Google Admin console.

---

# 🚫 IAM Does *Not* Create Users or Groups
- IAM is **not** a user-management system.  
- Use **Cloud Identity** or **Google Workspace** to create and manage users/groups.

---

# 🪢 IAM Policies & Bindings
A policy = a list of **bindings**  
A binding = **members** + **role**

### Example
- Members: user accounts, groups, domains, service accounts  
- Role: A named list of permissions  

Policies are attached to resources and inherit down the resource hierarchy.

---

# 🌳 Policy Inheritance
Resources inherit IAM policies from their parents.

- Parent policies + Resource policies = **Effective policy**
- **Less restrictive parent policies always override** restrictive resource-level ones.
- Moving a resource (like a project) changes its inherited policy.

### Example
- You get **Editor** at Folder "Department X"  
- You get **Viewer** at Project "Bookshelf"  
→ You still have **Editor** on Bookshelf because parent permissions override child restrictions.

---

# 🛡️ Principle of Least Privilege
Always grant the **minimum permissions** needed:
- Smallest identity scope  
- Smallest role scope  
- Smallest resource scope  

Google Cloud provides **Recommender** to help remove excess permissions.

### 🧠 How Recommender Works
- Uses **Policy Insights** (ML-based analysis)  
- Suggests removing or replacing roles with unnecessary permissions  
- Helps maintain least-privilege at scale  

---

# ✔️ Allow Policies (IAM Policies)
Allow policies control access to resources **and their descendants**.

An allow policy binds:
- 🧑 **Principals (members)**  
- 🎭 **Roles**  
- ⚙️ Optional **conditions**  

### Example (from the slide)
- Jie → **Organization Admin**  
- Jie & Raha → **Project Creator**

This results in:
- Jie: more access  
- Raha: project creation only  

---

# ⛔ IAM Deny Policies
Deny policies allow you to set **guardrails** by *blocking* certain permissions.

### Key Points:
- Deny policies contain **deny rules**.  
- Rules specify:
  - The principals
  - The permissions to deny
  - Optional conditions  
- **Deny rules always take priority** over allow rules.  
- If a user is denied a permission, they cannot use it—even if an allow policy grants it.

---

# ⚙️ IAM Conditions
IAM Conditions allow **attribute-based access control** (ABAC).

You can grant access **only if certain conditions are met**, such as:
- Temporary emergency access  
- Access only from a corporate network  
- Time-based access  

### Condition Behavior:
- Exists in the policy's role binding  
- Access granted only if condition expression is **true**  
- Expressions use logical statements referencing attributes

---

# 🏛️ Organization Policies
Organization policies set **restriction configurations**.

- Applied at the org, folder, or project level  
- Inherited by all descendants  
- Exceptions allowed *only* by someone with **Organization Policy Admin** role  
- Enforces high-level organization-wide security or compliance rules  

---

# 🔄 Integrating with External Directories

## 🪪 Google Cloud Directory Sync (GCDS)
If you already use Active Directory or LDAP:

- GCDS syncs users & groups into Cloud Identity  
- Sync is **one-way** (no changes to AD/LDAP)  
- Sync can run automatically on a schedule  
- Users log into Google Cloud with their existing credentials  

---

# 🔐 Single Sign-On (SSO)
You can use your existing identity provider to authenticate users.

### How It Works:
1. User tries to access Google Cloud  
2. Google redirects to your IdP  
3. IdP authenticates user  
4. If successful → user gains access  

### Requirements:
- If your IdP supports **SAML2**, setup requires:  
  - Three links  
  - One certificate  
- Third-party IdPs like **ADFS, Ping, Okta** are also supported.

---

# ✉️ Using Google Accounts Without Gmail
- You can create a Google Account **without** enabling Gmail.  
- Useful for organizations that handle email outside Google.

