# 🌐 Virtual Private Cloud (VPC) in Google Cloud

With **Google Cloud Platform (GCP)**, you can provision resources, connect them, and isolate them using a **Virtual Private Cloud (VPC)**. VPC allows you to define fine-grained networking policies within GCP and even between **GCP, on-premises**, or **other public clouds**.

A VPC is a **comprehensive set of Google-managed networking objects**, which we will explore throughout this module.

---

## 🧩 High-Level Overview of VPC Components

### 🗂️ Projects
- A **project** contains every GCP service you use — including your networks.

---

### 🕸️ Networks
Networks come in **three types**:
- **Default Network** 🌐  
- **Auto Mode VPC** 🔄  
- **Custom Mode VPC** 🛠️ (recommended for production)  

---

### 🧱 Subnetworks
- Used to **divide**, **segment**, or **organize** your environment.
- Each subnetwork is regional and provides internal IP ranges.

---

### 🌎 Regions and Zones
- Represent Google’s global datacenters.
- Offer **high availability** and **continuous data protection**.

---

### 🔢 IP Addresses
VPC provides:
- **Internal IP addresses** (private) 🔒  
- **External IP addresses** (public) 🌍  
- Full control of **IP range selection** for your subnets.

---

### 🖥️ Virtual Machines (VMs)
- In this module, the focus is on configuring **VM networking**, including:
  - IP assignments  
  - Network interfaces  
  - Subnet placement  

---

### 🛣️ Routes
- Control how traffic **flows** within your VPC.
- Determine paths from one resource to another.

---

### 🔥 Firewall Rules
- Provide **granular control** over network traffic.
- Define which connections are allowed or denied.

---
