# 🔐 Service Accounts & IAM – Explained

## 👤 What Is a Service Account?
A **service account** is an account that belongs to your **application**, not to a human user.  
It allows secure service-to-service interactions **without using user credentials**.

Example:  
If an app needs to interact with **Cloud Storage**, it must authenticate with the Cloud Storage XML or JSON API.  
You can grant a service account read/write access on the VM where the app runs. The application then retrieves credentials from the service account—no embedded secret keys needed. 🔑

Service accounts use an email identifier, such as:
```text
service-account-name@project-id.iam.gserviceaccount.com
```


---

## 🧰 Types of Service Accounts
There are **three types**:

1. **👤 Custom (User-Created) Service Accounts**  
2. **🧱 Built-in Service Accounts**  
3. **🤖 Google APIs Service Accounts**

Every project includes:

- **Compute Engine Default Service Account**  
- **Google Cloud APIs Service Account**:  
  Format:  
```text
project-number@cloudservices.gserviceaccount.com
```

➡️ Automatically given the **Editor** role.

You can also launch VMs using **custom service accounts** for more flexibility.

---

## 🖥 Default Compute Engine Service Account
- Automatically created per project  
- Format:  
```text
project-number-compute@developer.gserviceaccount.com
```

- Automatically has **Editor** role
- Enabled by default when creating an instance via `gcloud`, unless overridden

You may:
- Replace it with another service account  
- Disable service accounts entirely for an instance

---

## 🔏 Authorization & Scopes
**Authorization** determines what an authenticated identity can do.  
**Scopes** decide whether the identity is authorized.

Example:  
Both App A and App B request access to a Cloud Storage bucket.

- **App A** → receives **read-only** scope 📖  
- **App B** → receives **read-write** scope ✍️

Scopes can be set during VM creation and changed later (requires VM stop).

⚠️ *Scopes are legacy.*  
Use **IAM Roles** instead for user-created service accounts.

---

## 👥 Assigning Roles to Service Accounts
Example scenario:

1. You create a service account with the **InstanceAdmin** role  
 → can create, modify, delete VMs & disks 💽
2. You treat the service account as a resource  
 → Assign **Service Account User** role to specific users  
3. Those users can now *act as* the service account  
 → They can perform all actions the service account can

⚠️ Be careful when granting **Service Account User** —  
it gives users full access to everything the service account can access.

---

## 🧩 Microservices with Service Accounts
Example:

- Component_1 VMs use **Service Account 1** → has **Editor** access to `project_b`  
- Component_2 VMs use **Service Account 2** → has **objectViewer** access to `bucket_1`

This setup lets you isolate permissions for each microservice **without recreating VMs**.

---

## 🔐 Service Account Authentication
There are **two types of service account keys**:

### 1️⃣ Google-Managed Keys (Recommended) ⭐
- Google stores both public and private keys
- Keys rotate automatically
- Private key never exposed  
- Public key valid for ~2 weeks

### 2️⃣ User-Managed Keys (External Keys) ⚠️
- Used for access *outside* Google Cloud
- You manage & secure the private key  
- Google only stores the public portion  
- Up to **10 keys per service account** allowed  
- Created via:
- IAM API  
- `gcloud`  
- Cloud Console  

⚠️ Lost private keys **cannot be recovered**.  
⚠️ Should be used **only as a last resort**.

Alternatives:
- Short-lived credentials  
- Service Account Impersonation 🔁

---

## 📜 Useful Command
List all keys for a service account:
```bash
gcloud iam service-accounts keys list --iam-account=<SERVICE_ACCOUNT_EMAIL>
```
