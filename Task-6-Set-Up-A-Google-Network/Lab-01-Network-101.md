# 🌐 Networking 101

🧪 **Experiment** | 🧑‍🏫 **Lab**  
⏱️ **15 minutes** | 💳 **1 Credit**  
📊 **Introductory**  
ℹ️ This lab may incorporate AI tools to support your learning.  
🆔 **GSP016**

---

## 📘 Overview

In this lab, you will learn the **basics of networking on Google Cloud**, including how it differs from traditional on-premises environments.

You will build a network with **three subnetworks**, resulting in the following end-state environment:

- 🟦 `subnet-us-central`
- 🟩 `subnet-europe-west`
- 🟥 `asia-test-01`

You will also learn how to:
- 🔐 Create **firewall rules**
- 🏷️ Use **instance tags** to apply firewall rules

---

## 🎯 What You’ll Learn

- 🌐 Basic concepts of Google Cloud networking  
- 🏗️ Differences between **default** and **custom networks**  
- 🔥 How to create and apply **firewall rules** using instance tags  

---

## ⚙️ Setup and Requirements

### ⏳ Before You Start
- Labs are **timed** and **cannot be paused**
- The timer starts when you click **Start Lab**

### 🖥️ What You Need
- 🌍 Internet browser (Chrome recommended)
- 🕵️ Incognito / Private window (recommended)
- ⏱️ Enough time to finish the lab in one session

⚠️ **Important Notes**
- Use **only the student account** provided
- Do **not** use your personal Google Cloud account
- Using a personal account may incur charges

---

## 🔐 How to Start the Lab & Sign In

1. ▶️ Click **Start Lab**
2. Click **Open Google Cloud Console**
3. If prompted, select **Use Another Account**
4. Enter the provided:
   - 👤 **Username**
   - 🔑 **Password**
5. Click **Next**

### ✅ During Sign-in
- Accept terms and conditions
- ❌ Do not enable 2FA
- ❌ Do not sign up for free trials

Once complete, the **Google Cloud Console** opens.

---

## ☁️ Activate Cloud Shell

Cloud Shell provides a ready-to-use VM with development tools.

1. Click **Activate Cloud Shell** ☁️
2. Continue through setup dialogs
3. Authorize Cloud Shell access

When connected, you’ll see:
```bash
Your Cloud Platform project in this session is set to "PROJECT_ID"
```


---

## 🛠️ gcloud CLI Basics

(Optional) Check active account:
```bash
gcloud auth list
```


(Optional) Check project ID:
```bash
gcloud config list project
```


---

## 🌍 Understanding Regions and Zones

- **Region**: A geographic location
- **Zone**: An isolated location within a region

📍 Example:
- Region: `us-central1`
- Zones: `us-central1-a`, `us-central1-b`, `us-central1-c`, `us-central1-f`

### 📊 Common Regions & Zones

| Region | Zones |
|------|------|
| Western US | us-west1-a, us-west1-b |
| Central US | us-central1-a, b, d, f |
| Eastern US | us-east1-b, c, d |
| Western Europe | europe-west1-b, c, d |
| Eastern Asia | asia-east1-a, b, c |

📌 **Key Notes**
- VM instances and disks are **zonal resources**
- Resources must be in the **same zone or region** to attach or assign IPs

---

## 🧠 Google Cloud Network Concepts

### 🗂️ Projects
- Organize resources, IAM, billing, and quotas
- Commonly mapped to teams
- Isolated unless connected via:
  - Shared VPC
  - VPC Peering

### 🌐 Networks (VPC)
- Connect resources internally and externally
- Use **firewall rules** for access control
- Can be:
  - 🌍 **Global** (multi-region)
  - 📍 **Regional** (low-latency)

### 🧩 Subnetworks
- Regional resources
- Group related instances
- Use private RFC1918 IP ranges

#### 🔄 Network Modes
- **Auto Mode**
  - One subnet per region
  - IP ranges created automatically
- **Custom Mode**
  - No subnets by default
  - You create subnets manually
  - Full control over IP ranges

---

## 📍 Set Your Default Region and Zone

Run the following commands in **Cloud Shell**:
```bash
gcloud config set compute/zone "Zone"
export ZONE=$(gcloud config get compute/zone)

gcloud config set compute/region "Region"
export REGION=$(gcloud config get compute/region)
```

---

## 🧭 Task 1. Review the Default Network

When a new Google Cloud project is created, a **default VPC network** is automatically provided. This default network uses **auto subnet mode**, which means:

- 🌍 Each region automatically gets a subnetwork
- ➕ You can create up to **four additional networks** per project
- 🧩 Additional networks can be:
  - Auto subnet networks
  - Custom subnet networks
  - Legacy networks

📌 **IP Addressing**
- Every VM instance created in a subnetwork is assigned an **IPv4 address** from that subnetwork’s IP range.

---

## 🌐 Review the VPC Network

1. In the Cloud Console, click:  
   **Navigation menu (☰) > VPC network**
2. Review the list of VPC networks, including:
   - 📡 IP address ranges
   - 🚪 Gateways
   - 🌍 Network type (auto/custom)

---

## 🔥 Firewalls Overview

Each VPC network includes **firewall rules** to control traffic.

### 🔒 Default Firewall Behavior
- 🚫 **All inbound traffic is blocked by default**
- ✅ To allow inbound traffic, you must create **allow rules**
- 📤 Outbound traffic is **allowed by default**, unless restricted using **egress rules**

You can:
- Allow specific **ingress** traffic
- Deny or restrict **egress** traffic
- Create a **default-deny egress policy** to block all outbound connections

💡 **Best Practice**
- Always configure the **least permissive firewall rules**
- Target only the instances that need access
- Avoid overly broad rules that allow traffic to all instances

⚖️ **Firewall Rule Priority**
- Rules have priority values
- 🔢 Lower numbers = higher priority
- The first matching rule (lowest number) is applied
- Complex override rules can lead to unintended access—use carefully

---

## 🔐 Default Network Firewall Rules

Only the **default network** has pre-created firewall rules.  
Custom or auto networks you create later **do not** include any firewall rules by default.

### 🚪 Ingress Rules Automatically Created

- **default-allow-internal**  
  - Allows all protocols and ports between instances in the same network

- **default-allow-ssh**  
  - Allows SSH (TCP port 22) from any source

- **default-allow-rdp**  
  - Allows RDP (TCP port 3389) from any source

- **default-allow-icmp**  
  - Allows ICMP (ping) traffic from any source

---

## 🔍 Review Firewall Rules

1. In the Cloud Console, click:  
   **Navigation menu (☰) > VPC network > Firewall**
2. Review the firewall rules list, including:
   - 🔁 Direction (Ingress / Egress)
   - 🎯 Targets
   - 🔎 Filters
   - 🔌 Protocols / Ports
   - 🏷️ Priority
   - 🌐 Network

---

## 🛣️ Network Routes

All VPC networks automatically include:
- 🌍 A **default route to the Internet**
- 🧩 Routes to internal IP ranges within the network

📌 Route names are auto-generated and may differ per project.

---

## 🧭 Review Default Routes

1. In the Cloud Console, click:  
   **Navigation menu (☰) > VPC network > Routes**
2. Select a **Network** and **Region**
3. Review the routes, including:
   - 📄 Description
   - 🎯 Destination IP range
   - 🔢 Priority
   - 🌐 Associated network

✅ You have now reviewed the default network, firewall rules, and routing behavior in Google Cloud

---

## 🛠️ Task 2. Creating a Custom Network

When manually assigning subnetwork IP ranges, you must first create a **custom VPC network**, then add subnetworks per region.

### 📌 Key Concepts
- 🌍 You do **not** need to create subnetworks for all regions immediately
- 🚫 You **cannot create VM instances** in a region without a subnetwork
- 🏷️ Subnetwork names must be **unique per region within a project**
- 🔁 The same subnet name can be reused in another region
- 📡 Subnetworks do **not** have a network-level IPv4 range or gateway IP

⚠️ You may create the network using **either**:
- Cloud Console **or**
- Cloud Shell (`gcloud`)

👉 Choose **one method only** and follow it consistently.

---

## 🌐 Create a Custom VPC Network (Console)

1. Click:  
   **Navigation menu (☰) > VPC network**

2. Click **Create VPC Network**

3. Enter:
   - **Name**: `taw-custom-network`

---

## 🧩 Create Subnetworks (Custom Tab)

### First Subnet
- **Subnet name**: `subnet-<REGION>`
- **Region**: `<REGION>`
- **IP address range**: `10.0.0.0/16`
- Click **Done**

🪟 The **Create a VPC network** dialog box is now populated.

---

### Add Two More Subnets

Click **Add Subnet**, then add:

- `subnet-<REGION>`, `<REGION>`, `10.1.0.0/16`
- `subnet-<REGION>`, `<REGION>`, `10.2.0.0/16`

---

## ✅ Finish Network Creation

- Click **Create**

At this point:
- 🌍 Routes to the Internet are automatically created
- 🔁 Routes to internal instances are ready
- 🚫 **No firewall rules exist yet**, even between instances

🔐 To allow access to instances, you must **explicitly create firewall rules**.

➡️ Continue to the **Adding firewall rules** section.

---

## 🔥 Task 3. Adding Firewall Rules

To allow access to VM instances, you must configure **firewall rules**.  
For this lab, firewall rules are applied using **instance tags**, meaning the rule applies to **any VM with the same tag**.

### 🏷️ About Instance Tags
- Used by **VPC networks and firewalls** to target VM instances
- Helpful when multiple VMs serve the same role (e.g., web servers)
- Tags are also exposed in the **metadata server**, usable by applications

You will:
1. 🌐 Allow HTTP traffic from the Internet
2. ➕ Add additional rules for ICMP, internal traffic, SSH, and RDP

---

## 🌐 Add Firewall Rules Using the Console

1. In the Cloud Console, go to:  
   **Navigation menu (☰) > VPC network**

2. Click **taw-custom-network**

3. Open the **Firewalls** tab, then click **Add Firewall Rule**

---

## 🌍 Firewall Rule: Allow HTTP

| Field | Value | Notes |
|---|---|---|
| Name | `nw101-allow-http` | New rule name |
| Targets | Specified target tags | Apply rule by tag |
| Target tags | `http` | Tag used by VM |
| Source filter | IPv4 ranges | Internet access |
| Source IPv4 ranges | `0.0.0.0/0` | Allow all |
| Protocols and ports | tcp:80 | HTTP only |

📸 Your screen should resemble the populated **Create a firewall rule** dialog.

- Click **Create** and wait for success

---

## ➕ Create Additional Firewall Rules

### 📡 ICMP Rule

| Field | Value |
|---|---|
| Name | `nw101-allow-icmp` |
| Targets | Specified target tags |
| Target tags | `rules` |
| Source filter | IPv4 ranges |
| Source IPv4 ranges | `0.0.0.0/0` |
| Protocols and ports | Other protocols → `icmp` |

---

### 🔁 Internal Communication Rule

| Field | Value |
|---|---|
| Name | `nw101-allow-internal` |
| Targets | All instances in the network |
| Source filter | IPv4 ranges |
| Source IPv4 ranges | `10.0.0.0/16`, `10.1.0.0/16`, `10.2.0.0/16` |
| Protocols and ports | tcp:0-65535, udp:0-65535, icmp |

---

### 🔐 SSH Rule

| Field | Value |
|---|---|
| Name | `nw101-allow-ssh` |
| Targets | Specified target tags |
| Target tags | `ssh` |
| Source filter | IPv4 ranges |
| Source IPv4 ranges | `0.0.0.0/0` |
| Protocols and ports | tcp:22 |

---

### 🖥️ RDP Rule

| Field | Value |
|---|---|
| Name | `nw101-allow-rdp` |
| Targets | All instances in the network |
| Source filter | IPv4 ranges |
| Source IPv4 ranges | `0.0.0.0/0` |
| Protocols and ports | tcp:3389 |

---

## ✅ Review Firewall Rules

- Navigate to **VPC network > Firewall**
- Confirm all rules are listed under `taw-custom-network`

📸 The Firewall rules tab should display all newly created rules.

---

## 🛣️ Note on Routes
- Routes direct packets **between subnetworks** and **to the Internet**
- Default routes are **automatically created** when subnetworks exist
- These default routes **cannot be modified**
- Custom routes can be added (e.g., to VPNs or gateways)
- 🔗 **Routes + Firewalls** work together to control traffic flow

---

## 🎉 Congratulations!

You have successfully learned:

- 🌐 How **default and user-created VPC networks** are configured  
- 🧩 How to **add subnetworks** across regions  
- 🔥 How to **apply firewall rules** to control and secure network access  

Great job building a solid foundation in **Google Cloud Networking**! 🚀


