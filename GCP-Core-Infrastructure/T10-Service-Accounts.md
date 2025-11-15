# 🔐 Service Accounts

Imagine you have a Compute Engine virtual machine running a program that needs to access other cloud services regularly.  
Instead of requiring a person to manually grant access each time the program runs, you can give the virtual machine itself the necessary permissions.  

This is where **service accounts** come in.  

Service accounts allow you to assign specific permissions to a virtual machine, so it can interact with other cloud services **without human intervention**. 🚀

---

## 🖥️ Example Scenario

Let’s say you have an application running in a VM that needs to store data in **Cloud Storage**, but you don’t want anyone on the internet to have access to that data—only that particular VM.

👉 You can create a **service account** to authenticate that VM to Cloud Storage.

Service accounts are named with an email address, but instead of passwords they use **cryptographic keys** to access resources. 🔑

---

## 🛠️ Service Account Permissions

If a service account has been granted **Compute Engine’s Instance Admin role**, this allows an application running in a VM with that service account to:

- Create VMs  
- Modify VMs  
- Delete VMs  

---

## 👥 Managing Service Accounts

Service accounts need to be managed just like other identities.

For example:  
- **Alice** needs to manage which Google accounts can *act as* service accounts.  
- **Bob** only needs to view a list of service accounts.

Because a service account is both **an identity and a resource**, it can have its **own IAM policy** attached to it.

✔️ Alice can have the **editor** role on a service account.  
✔️ Bob can have the **viewer** role.  

This works the same way as granting roles for any other Google Cloud resource. 🎯
