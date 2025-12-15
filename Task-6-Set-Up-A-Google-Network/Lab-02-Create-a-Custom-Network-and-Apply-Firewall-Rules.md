# 🌐 Create a Custom Network and Apply Firewall Rules

**Experiment · Lab · ⏱️ 30 minutes · 💳 1 Credit**  
**Level:** Introductory  
**Lab ID:** GSP159  
**Platform:** Google Cloud Skills Boost  

> ℹ️ This lab may incorporate AI tools to support your learning.

---

## 📘 Overview

In this hands-on lab, you will **design and implement a secure network architecture** on Google Cloud. Using **Cloud Shell** and the **gcloud CLI**, you will:

- 🏗️ Create a **custom VPC network**
- 🧩 Create **three subnetworks**
- 🔥 Apply **firewall rules** to control traffic to VM instances

---

## 🎯 What You’ll Learn

By completing this lab, you will learn how to create:

- 🌐 A **custom VPC network**
- 🧱 **Three subnetworks**
- 🔐 **Firewall rules** using **network tags**

---

## ⚙️ Setup and Requirements

### ⏱️ Before You Start
- Labs are **timed** and **cannot be paused**
- The timer starts when you click **Start Lab**
- Resources are temporary and removed after the lab

### ✅ What You Need
- 🌍 A standard web browser (**Chrome recommended**)
- 🕶️ Incognito / Private window (recommended)
- ⏳ Sufficient uninterrupted time to complete the lab

> ⚠️ **Important:**  
> - Use **only the student account** provided  
> - Using a personal Google Cloud account may incur charges

---

## 🔐 Start the Lab & Sign In

1. Click **Start Lab**
2. Review the **Lab Details pane**, which includes:
   - Open Google Cloud Console button
   - Time remaining
   - Temporary credentials

3. Click **Open Google Cloud Console**  
   _(or open in an Incognito window)_

4. On the Sign in page:
   - Click **Use another account**
   - Enter the provided **Username**
   - Click **Next**
   - Enter the provided **Password**
   - Click **Next**

5. Complete the setup:
   - ✅ Accept terms and conditions
   - ❌ Do not add recovery options or 2FA
   - ❌ Do not sign up for free trials

➡️ The **Google Cloud Console** will open after a few moments.

---

## ☁️ Activate Cloud Shell

Cloud Shell provides a preconfigured VM with the **gcloud CLI** and tools.

1. Click **Activate Cloud Shell** ☁️
2. Continue through the prompts:
   - Review Cloud Shell info
   - Authorize API access

Once connected, your session is already authenticated and set to:

```text
Your Cloud Platform project in this session is set to "PROJECT_ID"
```

## 🛠️ Useful gcloud Commands (Optional)

List active account:
```bash
gcloud auth list
```

List project ID:
```bash
gcloud config list project
```

---

## 🌍 Understanding Regions and Zones

- Regions are geographic locations (e.g., us-central1)
- Zones are subdivisions within regions (e.g., us-central1-a)
- VM instances and disks are zonal resources
- Static IPs are regional resources

---

## 📍 Example Regions & Zones

| Region         | Zones                  |
| -------------- | ---------------------- |
| Western US     | us-west1-a, us-west1-b |
| Central US     | us-central1-a, b, d, f |
| Eastern US     | us-east1-b, c, d       |
| Western Europe | europe-west1-b, c, d   |
| Eastern Asia   | asia-east1-a, b, c     |

---

## ⚙️ Set Your Default Region and Zone

Run the following commands in Cloud Shell:
```bash
gcloud config set compute/zone "Zone"
export ZONE=$(gcloud config get compute/zone)

gcloud config set compute/region "Region"
export REGION=$(gcloud config get compute/region)
```

---

# 🧩 Task 1. Create Custom Network with Cloud Shell

In this task, you will create a **custom VPC network** and manually define its subnetworks using **Cloud Shell** and the **gcloud CLI**.

---

## 🌐 Create the Custom Network

Create a network named **taw-custom-network** and specify that you will manually add subnetworks by using the `--subnet-mode custom` flag.

```bash
gcloud compute networks create taw-custom-network --subnet-mode custom
```

## ✅ Output
```text
NAME                MODE    IPV4_RANGE  GATEWAY_IPV4
taw-custom-network  custom
```
> ⚠️ Note:
> Instances on this network will not be reachable until firewall rules are created.

Example firewall rules (shown by the system):
```bash
$ gcloud compute firewall-rules create <firewall_name> \
  --network taw-custom-network \
  --allow tcp,udp,icmp \
  --source-ranges <ip_range>

$ gcloud compute firewall-rules create <firewall_name> \
  --network taw-custom-network \
  --allow tcp:22,tcp:3389,icmp
```

## 🧱 Create Subnetworks

Now create three subnetworks, each with a unique IP range.
In each command, specify:
- 🌍 The region
- 🌐 The network it belongs to
- 📡 The IP address range

## ➕ Subnet 1 (10.0.0.0/16)
```bash
gcloud compute networks subnets create subnet-Region \
   --network taw-custom-network \
   --region Region \
   --range 10.0.0.0/16
```

## Output:
```text
Created [https://www.googleapis.com/compute/v1/projects/cloud-network-module-101/regions/Region/subnetworks/subnet-Region].
NAME           REGION   NETWORK              RANGE
subnet-Region  Region   taw-custom-network   10.0.0.0/16
```

## ➕ Subnet 2 (10.1.0.0/16)
```bash
gcloud compute networks subnets create subnet-Region \
   --network taw-custom-network \
   --region Region \
   --range 10.1.0.0/16
```

## Output:
```bash
Created [https://www.googleapis.com/compute/v1/projects/cloud-network-module-101/regions/Region/subnetworks/subnet-Region].
NAME           REGION   NETWORK              RANGE
subnet-Region  Region   taw-custom-network   10.1.0.0/16
```

## ➕ Subnet 3 (10.2.0.0/16)
```bash
gcloud compute networks subnets create subnet-Region \
   --network taw-custom-network \
   --region Region \
   --range 10.2.0.0/16
```

## Output:
```text
Created [https://www.googleapis.com/compute/v1/projects/cloud-network-module-101/regions/Region/subnetworks/subnet-Region].
NAME           REGION   NETWORK              RANGE
subnet-Region  Region   taw-custom-network   10.2.0.0/16
```

## 📋 Verify the Subnetworks

List all subnetworks created under taw-custom-network:
```bash
gcloud compute networks subnets list \
   --network taw-custom-network
```

## ✅ Output
```bash
NAME           REGION   NETWORK              RANGE
subnet-Region  Region   taw-custom-network   10.1.0.0/16
subnet-Region  Region   taw-custom-network   10.2.0.0/16
subnet-Region  Region   taw-custom-network   10.0.0.0/16
```

---

# 🔐 Task 2. Add Firewall Rules

To allow access to **VM instances**, you must apply **firewall rules**. In this task, you will use **network tags** to control which VMs the firewall rules apply to.

---

## 🏷️ Understanding Network Tags

Network tags help manage firewall rules across groups of VM instances.

Example:
- Tag multiple VMs with **`web-server`**
- Create one firewall rule that allows **HTTP traffic**
- All VMs with that tag automatically inherit the rule

✅ This simplifies management and improves flexibility.  
📌 Tags are also visible in the **metadata server**, so applications can reference them.

---

## 🌐 Allow HTTP Traffic (Ingress)

Start by allowing **HTTP (TCP port 80)** traffic from the Internet to VMs tagged with **`http`**.

### ➕ Create HTTP Firewall Rule

```bash
gcloud compute firewall-rules create nw101-allow-http \
--allow tcp:80 \
--network taw-custom-network \
--source-ranges 0.0.0.0/0 \
--target-tags http
```

### ✅ Output (Summary)
- Name: nw101-allow-http
- Network: taw-custom-network
- Direction: Ingress
- Priority: 1000
- Allowed: tcp:80

---

## ➕ Create Additional Firewall Rules

Now create firewall rules for ICMP, internal traffic, SSH, and RDP.

### 📡 ICMP (Ping)

Allows ICMP traffic to VMs tagged with rules.
```bash
gcloud compute firewall-rules create "nw101-allow-icmp" \
--allow icmp \
--network "taw-custom-network" \
--target-tags rules
```

### 🔁 Internal Communication

Allows full internal communication between subnetworks.
```bash
gcloud compute firewall-rules create "nw101-allow-internal" \
--allow tcp:0-65535,udp:0-65535,icmp \
--network "taw-custom-network" \
--source-ranges "10.0.0.0/16","10.2.0.0/16","10.1.0.0/16"
```

### 🔑 SSH Access

Allows SSH (TCP port 22) to VMs tagged with ssh.
```bash
gcloud compute firewall-rules create "nw101-allow-ssh" \
--allow tcp:22 \
--network "taw-custom-network" \
--target-tags "ssh"
```

---

### 🔍 Review Firewall Rules

Use the Cloud Console to verify your firewall rules:

📍 Navigation menu → VPC network → Firewall rules

You should see all newly created rules associated with taw-custom-network.

---

### 🧭 Note on Routes

- Routes are automatically created when subnetworks exist
- They enable traffic between subnetworks and the Internet
- ❌ Default routes cannot be modified
- ➕ Custom routes can be added for VPNs, gateways, or specific architectures

🧠 Routes + Firewalls work together:
- Routes decide where traffic goes
- Firewalls decide whether traffic is allowed
When you create VMs, assign the appropriate network tags so the correct firewall rules apply.

---

## 🎉 Task Completed

You have successfully used **gcloud commands** to:

- 🌐 Create a **custom VPC network**
- 🧩 Create **three subnetworks** in different regions
- 🔥 Apply a variety of **firewall rules**
- 🔓 Control and allow access to your **VM instances** using network tags

Great job building a secure and well-structured network architecture using Google Cloud! 🚀
