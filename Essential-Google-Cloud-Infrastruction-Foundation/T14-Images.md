# 🖼️ Images in Google Cloud Compute Engine

## 🔍 Overview  
When creating a virtual machine (VM) in Google Cloud, you need to choose a **boot disk image**. This image contains everything the VM needs to start and run.

---

## 📦 What’s Inside a Boot Disk Image?
A boot disk image typically includes:
- 🧹 **Boot loader**
- 🖥️ **Operating system**
- 📁 **File system structure**
- ⚙️ **Pre-configured software**
- 🛠️ Any other customizations

---

## 🆚 Public Images vs. Custom Images

### 🌐 Public Images  
You can choose from:
- 🐧 **Linux images**
- 🪟 **Windows images**

Some are **premium images** (marked with **(p)**):
- 💲 Billed per second after a 1-minute minimum  
- 🗃️ SQL Server images: billed per minute after a 10-minute minimum  
- 🌍 Premium image pricing is **global**—it does not vary by region or zone.

---

### 🧩 Custom Images  
You can create a custom image by:
- Installing software pre-approved by your organization 🏢  
- Importing images from your on-premises systems, workstation, or another cloud ☁️  
  - ✅ This import service is **free** and only requires installing an agent  
- Sharing custom images across projects 🔄

---

## 🖥️ Machine Images

A **machine image** is a Compute Engine resource that stores:
- 🧾 Configuration  
- 🔐 Metadata  
- 👥 Permissions  
- 💾 Data from one or more disks  

### 🛠️ Why Use Machine Images?
They are ideal for:
- 🔁 VM instance creation  
- 💽 Backup and recovery  
- 🧬 Instance cloning and replication  

Machine images provide a convenient, all-in-one snapshot for system maintenance scenarios.  

---
