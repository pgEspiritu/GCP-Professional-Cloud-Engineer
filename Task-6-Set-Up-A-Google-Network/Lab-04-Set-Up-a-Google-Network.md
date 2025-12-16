# 🌐 Set Up a Google Cloud Network: Challenge Lab

## 📘 Overview

In this **challenge lab**, you are given a **scenario and a set of tasks** without step-by-step instructions. You are expected to use the skills learned in previous labs to complete the objectives independently.

An **automated scoring system** will provide feedback on whether your tasks are completed correctly.

⚠️ **Important reminders:**
- No new Google Cloud concepts are taught.
- You must rely on prior knowledge, experimentation, and troubleshooting.
- To score **100%**, all tasks must be completed **within the allotted time**.

🎯 This lab is recommended for learners who have enrolled in the **Set up a Google Cloud Network** skill badge.

Are you ready for the challenge? 💪

---

## ⚙️ Setup

### 🔔 Before You Click **Start Lab**

Please read the following carefully:

- ⏳ Labs are **timed** and **cannot be paused**.
- The timer starts as soon as you click **Start Lab**.
- Google Cloud resources are available **only for the duration of the lab**.

### ☁️ Real Cloud Environment

This is a **hands-on lab** using a real Google Cloud environment—not a simulation. You will receive **temporary credentials** to access Google Cloud resources.

### ✅ What You Need

- 🌍 Access to a standard web browser (**Chrome recommended**)
- 🕶️ Use an **Incognito or Private window** (recommended)  
  > Prevents conflicts with your personal Google Cloud account and avoids accidental charges.
- ⏱️ Sufficient uninterrupted time to complete the lab
- 🔐 Use **only the provided student account**  

---

## 🧩 Challenge Scenario

You are tasked with setting up a **Virtual Private Cloud (VPC) network** in Google Cloud Platform (GCP) and ensuring **connectivity between virtual machines (VMs)** in different subnets. You will also configure **firewall rules** to manage access and test network connectivity.

---

## 📋 Tasks to Complete

1. 🌐 **Create a VPC network** with **two subnets**.
2. 🔒 **Configure firewall rules** to allow traffic between VMs.
3. 🖥️ **Launch two VMs in each subnet**.
4. 📡 **Verify connectivity** between the VMs using the configured protocols.

> ⚠️ You will need to plan your network, configure firewalls, and validate connectivity **without step-by-step instructions**.

---

## 🧭 High-Level Approach

- **VPC & Subnets:** Create a custom VPC and define two subnets with appropriate IP ranges.  
- **Firewall Rules:** Open required ports (e.g., ICMP, SSH) for VM-to-VM communication.  
- **VM Deployment:** Launch VMs in each subnet and assign internal IPs.  
- **Connectivity Tests:** Use `ping`, `ssh`, or other protocols to verify communication between VMs.

---

💡 **Lab Outcome:**  
By completing this lab, you will demonstrate your ability to **create a GCP network**, deploy **VMs in different subnets**, and **configure connectivity** between them—all essential skills for cloud networking. 🚀

---

## 🛜 Task 1: Create Networks (VPC with Two Subnets)

### 🎯 Objective
Create a **VPC network** with **two subnets** and configure firewall rules to allow connectivity between resources.

---

### 📌 Requirements

#### 🌐 VPC Network
- **Name:** `network-name` (replace with your desired name)
- **Subnet mode:** Custom
- **Dynamic routing mode:** Regional

#### 📍 Subnet A
- **Name:** `subnet-a-name`
- **Region:** `network-region-1`
- **IP stack:** IPv4 (single-stack)
- **IPv4 range:** `10.10.10.0/24`

#### 📍 Subnet B
- **Name:** `subnet-b-name`
- **Region:** `network-region-2`
- **IP stack:** IPv4 (single-stack)
- **IPv4 range:** `10.10.20.0/24`

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace `network-name`, `subnet-a-name`, `subnet-b-name`, `network-region-1`, and `network-region-2` with the lab-provided names and regions.

---

### 1️⃣ Create the Custom VPC Network

```bash
gcloud compute networks create network-name \
  --subnet-mode=custom \
  --bgp-routing-mode=regional
```

---

### 2️⃣ Create Subnet A

```bash
gcloud compute networks subnets create subnet-a-name \
  --network=network-name \
  --region=network-region-1 \
  --range=10.10.10.0/24 \
  --stack-type=IPV4
```

### 3️⃣ Create Subnet B

```bash
gcloud compute networks subnets create subnet-b-name \
  --network=network-name \
  --region=network-region-2 \
  --range=10.10.20.0/24 \
  --stack-type=IPV4
```

---

### 🔍 Verification

List subnets in the network:
```bash
gcloud compute networks subnets list \
  --filter="network:network-name"
```
> You should see both `subnet-a-name` and `subnet-b-name` with the correct IP ranges.

---

### ✅ Expected Result

- ✅ VPC network network-name created in custom subnet mode
- ✅ Two subnets configured correctly:
  - subnet-a-name → 10.10.10.0/24 in network-region-1
  - subnet-b-name → 10.10.20.0/24 in network-region-2
- ✅ Ready to configure firewall rules for inter-subnet connectivity

---

## 🔒 Task 2: Add Firewall Rules

### 🎯 Objective
Create firewall rules to allow **SSH, RDP, and ICMP traffic** on the VPC network to enable connectivity and network diagnostics.

---

### 📌 Requirements

#### Firewall Rule 1: SSH Access
- **Name:** `firewall-rule-1`
- **Network:** `network-name`
- **Priority:** 1000
- **Direction:** Ingress
- **Action:** Allow
- **Targets:** All instances in the network
- **Source IPv4 ranges:** `0.0.0.0/0`
- **Protocol & Port:** TCP:22 (SSH)

#### Firewall Rule 2: RDP Access
- **Name:** `firewall-rule-2`
- **Network:** `network-name`
- **Priority:** 65535
- **Direction:** Ingress
- **Action:** Allow
- **Targets:** All instances in the network
- **Source IPv4 ranges:** `0.0.0.0/24`
- **Protocol & Port:** TCP:3389 (RDP)

#### Firewall Rule 3: ICMP (Ping)
- **Name:** `firewall-rule-3`
- **Network:** `network-name`
- **Priority:** 1000
- **Direction:** Ingress
- **Action:** Allow
- **Targets:** All instances in the network
- **Source IPv4 ranges:** `10.10.10.0/24`, `10.10.20.0/24`
- **Protocol:** ICMP

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace placeholders (`network-name`, `firewall-rule-1`, etc.) with the actual names used in your lab.

---

### 1️⃣ Create Firewall Rule for SSH

```bash
gcloud compute firewall-rules create firewall-rule-1 \
  --network=network-name \
  --priority=1000 \
  --direction=INGRESS \
  --action=ALLOW \
  --rules=tcp:22 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=all-instances
```

---

### 2️⃣ Create Firewall Rule for RDP

```bash
gcloud compute firewall-rules create firewall-rule-2 \
  --network=network-name \
  --priority=65535 \
  --direction=INGRESS \
  --action=ALLOW \
  --rules=tcp:3389 \
  --source-ranges=0.0.0.0/24 \
  --target-tags=all-instances
```

---

### 3️⃣ Create Firewall Rule for ICMP

```bash
gcloud compute firewall-rules create firewall-rule-3 \
  --network=network-name \
  --priority=1000 \
  --direction=INGRESS \
  --action=ALLOW \
  --rules=icmp \
  --source-ranges=10.10.10.0/24,10.10.20.0/24 \
  --target-tags=all-instances
```

---

### 🔍 Verification

List firewall rules in the network:
```bash
gcloud compute firewall-rules list \
  --filter="network:network-name"
```
> Check that all three rules exist with correct protocols, ports, priorities, and source ranges.

---

### ✅ Expected Result

- ✅ firewall-rule-1 allows SSH from anywhere
- ✅ firewall-rule-2 allows RDP from 0.0.0.0/24
- ✅ firewall-rule-3 allows ICMP between the two subnets
- ✅ VMs can communicate securely and can be diagnosed using ping or SSH/RDP

---

## 🖥️ Task 3: Add VMs to Your Network

### 🎯 Objective
Create virtual machines in each subnet, assign network tags for firewall rules, and verify connectivity using **ICMP (ping)**.

---

### 📌 Requirements

| VM Name      | Subnet         | Zone    |
|-------------|----------------|--------|
| us-test-01  | subnet-a-name  | ZONE   |
| us-test-02  | subnet-b-name  | ZONE   |

- VMs must use network tags required by the firewall rules.
- Connectivity is tested via **ping (ICMP)**.

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace placeholders (`subnet-a-name`, `subnet-b-name`, `ZONE`) with your actual lab values.

---

### 1️⃣ Create VM in Subnet A

```bash
gcloud compute instances create us-test-01 \
  --zone=ZONE \
  --machine-type=e2-micro \
  --subnet=subnet-a-name \
  --tags=all-instances \
  --image-family=debian-12 \
  --image-project=debian-cloud
```

---

### 2️⃣ Create VM in Subnet B

```bash
gcloud compute instances create us-test-02 \
  --zone=ZONE \
  --machine-type=e2-micro \
  --subnet=subnet-b-name \
  --tags=all-instances \
  --image-family=debian-12 \
  --image-project=debian-cloud
```

---

### 3️⃣ Verify SSH Access to the VMs

Open an SSH session to `us-test-01`:
```bash
gcloud compute ssh us-test-01 --zone=ZONE
```

---

### 4️⃣ Test Connectivity Between VMs Using ICMP

Get the internal IP address of us-test-02:
```bash
gcloud compute instances describe us-test-02 \
  --zone=ZONE \
  --format='get(networkInterfaces[0].networkIP)'
```

Ping `us-test-02` from `us-test-01`:
```bash
ping -c 3 <us-test-02-internal-ip-address>
```
> This tests ICMP connectivity between the two VMs.

---

### 5️⃣ Measure Latency Across Regions (Optional)

If the VMs are in different regions, you can test latency:
```bash
ping -c 3 us-test-02.ZONE
```
> Replace ZONE with the zone of `us-test-02`.

---

### 🔍 Verification

- ✅ VMs are created in the correct subnets and zones
- ✅ Network tags match firewall rules (all-instances)
- ✅ Ping from us-test-01 to us-test-02 succeeds
- ✅ Latency can be observed between instances

---

### ✅ Expected Result

- Two VMs (us-test-01 and us-test-02) running in different subnets
- SSH access to each VM is functional
- ICMP (ping) tests show successful communication between VMs 🌐

---

