# 🌐 Multiple VPC Networks (GSP211)

## 🔎 Overview

Virtual Private Cloud (VPC) networks allow you to maintain **isolated environments** within a larger cloud architecture. This gives you **granular control** over network access, data protection, and application security.

In this lab, you will create multiple VPC networks and VM instances, then test connectivity between them. Two custom mode networks—**managementnet** and **privatenet**—will be created along with firewall rules and VMs, based on this architecture:

> **Note:** The **mynetwork** VPC, including firewall rules and two VM instances (*mynet-vm-1* and *mynet-vm-2*), is **already created** for you.

---

## 🎯 Objectives

You will learn how to:

- Create **custom mode** VPC networks with firewall rules  
- Create **Compute Engine VM instances**  
- Explore **network connectivity** across VPCs  
- Create a VM instance with **multiple network interfaces**

---

## 🧰 Setup and Requirements

### Before starting the lab

Labs are timed. Once started, the timer cannot be paused. Temporary credentials are provided—use them **only** for this lab.

### What you need

- A standard internet browser (**Chrome recommended**)  
- Use **Incognito Mode** to avoid conflicts with personal Google accounts  
- Enough time to complete the lab in one run  
- **Use only the provided student account** to avoid charges  

---

## 🚀 How to Start Your Lab & Sign In

1. Click **Start Lab**.  
2. In the Lab Details pane, locate:
   - **Open Google Cloud console** button  
   - Lab timer  
   - Temporary **Username** and **Password**  
3. Click **Open Google Cloud console**  
   - If using Chrome → *Right-click > Open link in Incognito Window*  
4. On the sign-in page:
   - Click **Use another account** if prompted  
   - Copy/paste the provided **Username**, click **Next**  
   - Copy/paste the provided **Password**, click **Next**  
5. Accept terms  
6. Skip recovery options, 2FA, and free trial  
7. The Google Cloud console will load  
8. Use the **Navigation Menu** or **Search bar** to access services

---

## 💻 Activate Cloud Shell

Cloud Shell provides:

- A preconfigured VM  
- A persistent 5GB home directory  
- Built-in dev tools  
- Direct access to your Google Cloud resources  

### Steps:

1. Click **Activate Cloud Shell** at the top right  
2. Continue through prompts  
3. Authorize Cloud Shell  
4. Once connected, your project is automatically set:
```text
Your Cloud Platform project in this session is set to "PROJECT_ID"
```


### Useful Commands

List active accounts:
```bash
gcloud auth list
```


List the active project:
```bash
gcloud config list project
```

Both commands will display the temporary credentials provided for the lab.

---

# 🧪 Task 1: Create Custom Mode VPC Networks with Firewall Rules

Create two custom networks—**managementnet** and **privatenet**—and configure firewall rules to allow **SSH**, **ICMP**, and **RDP** ingress traffic.

---

## 🛠️ Create the `managementnet` Network (Cloud Console)

1. In the Cloud console, go to:  
   **Navigation menu → VPC network → VPC networks**

   You will see the existing **default** and **mynetwork** VPCs.

2. Click **Create VPC Network**.

3. Configure the network:
   - **Name:** `managementnet`
   - **Subnet creation mode:** `Custom`

4. Add a subnet:
   | Property | Value |
   |----------|--------|
   | Name | `managementsubnet-1` |
   | Region | `<Region_1>` |
   | IPv4 range | `10.130.0.0/20` |

5. Click **Done**.

6. Click **EQUIVALENT COMMAND LINE** to view the gcloud equivalent, then click **Close**.

7. Click **Create**.

### ✅ Test Completed Task
Use **Check my progress** to confirm creation of **managementnet**.

---

## 🛠️ Create the `privatenet` Network (Cloud Shell)

Run the following commands:

### Create VPC network:
```bash
gcloud compute networks create privatenet --subnet-mode=custom
```

Create subnets:
```bash
gcloud compute networks subnets create privatesubnet-1 --network=privatenet --region=Region_1 --range=172.16.0.0/24
gcloud compute networks subnets create privatesubnet-2 --network=privatenet --region=Region_2 --range=172.20.0.0/20
```

---

### 🔍 List VPC Networks
```bash
gcloud compute networks list
```

Expected output example:
```ini
NAME: default
SUBNET_MODE: AUTO
...

NAME: managementnet
SUBNET_MODE: CUSTOM
...

NAME: privatenet
SUBNET_MODE: CUSTOM
...
```
> Note:
> `default` and `mynetwork` = Auto mode (subnets created in all regions)
> `managementnet` and `privatenet` = Custom mode (you manually create subnets)

---

### 🔍 List VPC Subnets
```bash
gcloud compute networks subnets list --sort-by=NETWORK
```

Expected output example:
```ini
NAME: default
REGION: Region_1
NETWORK: default
RANGE: 10.128.0.0/20
...
```

---

### 🔥 Create Firewall Rules for managementnet (Cloud Console)

1. In Cloud console:
Navigation menu → VPC network → Firewall
2. Click + Create Firewall Rule
3. Configure:
| Property           | Value                              |
| ------------------ | ---------------------------------- |
| Name               | `managementnet-allow-icmp-ssh-rdp` |
| Network            | `managementnet`                    |
| Targets            | All instances in the network       |
| Source filter      | IPv4 Ranges                        |
| Source IPv4 ranges | `0.0.0.0/0`                        |
| Protocols & ports  | `tcp:22`, `tcp:3389`, `icmp`       |
> Important: Include `/0` in `0.0.0.0/0.`
4. Click EQUIVALENT COMMAND LINE (for reference), then Close.
5. Click Create.

---

### 🔥 Create Firewall Rules for privatenet (Cloud Shell)

Run:
```bash
gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0
```

Expected output:
```ini
Creating firewall...done.
NAME: privatenet-allow-icmp-ssh-rdp
NETWORK: privatenet
DIRECTION: INGRESS
PRIORITY: 1000
ALLOW: icmp,tcp:22,tcp:3389
DISABLED: False
```

---

### 🔍 List All Firewall Rules
```bash
gcloud compute firewall-rules list --sort-by=NETWORK
```

Expected sample output:
```bash
NAME: default-allow-icmp
NETWORK: default
DIRECTION: INGRESS
ALLOW: icmp
...

NAME: privatenet-allow-icmp-ssh-rdp
NETWORK: privatenet
ALLOW: icmp,tcp:22,tcp:3389
...
```

> Note:
- Custom networks (privatenet, managementnet) combine multiple protocols in one rule
- Auto networks (default, mynetwork) may separate protocols across rules

In the Cloud console, navigate again to:
Navigation menu → VPC network → Firewall
to view all firewall rules.

---

# 🧪 Task 2: Create VM Instances

Create two VM instances:

- **managementnet-vm-1** in `managementsubnet-1`
- **privatenet-vm-1** in `privatesubnet-1`

---

## 🖥️ Create `managementnet-vm-1` (Cloud Console)

1. In the Cloud console, navigate to:  
   **Navigation menu → Compute Engine → VM instances**

   You will see **mynet-vm-1** and **mynet-vm-2**, which are pre-created.

2. Click **Create Instance**.

3. Under **Machine configuration**, set:

| Property | Value |
|----------|--------|
| Name | `managementnet-vm-1` |
| Region | `US_Region` |
| Zone | `US_Zone` |
| Series | `E2` |
| Machine Type | `e2-micro` |

4. Click **Networking**.

5. Under **Network interfaces**, edit and set:

| Property | Value |
|----------|--------|
| Network | `managementnet` |
| Subnetwork | `managementsubnet-1` |

6. Click **Done**.

7. Click **EQUIVALENT CODE** to view gcloud equivalent, then **Close**.

8. Click **Create**.

### ✅ Test Completed Task
Use **Check my progress** to confirm the VM instance was created.

---

## 🖥️ Create `privatenet-vm-1` (Cloud Shell)

Run the following command:

```bash
gcloud compute instances create privatenet-vm-1 --zone= --machine-type=e2-micro --subnet=privatesubnet-1
```

Expected output:
```ini
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-972c7275ce91/zones/""/instances/privatenet-vm-1].
NAME: privatenet-vm-1
ZONE:
MACHINE_TYPE: e2-micro
PREEMPTIBLE:
INTERNAL_IP: 172.16.0.2
EXTERNAL_IP: 34.135.195.199
STATUS: RUNNING
```

---

### 🔍 List All VM Instances

Run:
```bash
gcloud compute instances list --sort-by=ZONE
```

Sample output:
```ini
NAME: mynet-vm-2
ZONE:
MACHINE_TYPE: e2-micro
INTERNAL_IP: 10.164.0.2
EXTERNAL_IP: 34.147.23.235
STATUS: RUNNING

NAME: mynet-vm-1
ZONE:
MACHINE_TYPE: e2-micro
INTERNAL_IP: 10.128.0.2
EXTERNAL_IP: 35.232.221.58
STATUS: RUNNING
...
```

---

### 📊 View VM Networking in Console

1. In the console:
Navigation menu → Compute Engine → VM instances

2. Click Column display options, then check Network → click OK.
There are now:
- 3 instances in Region_1
- 1 instance in Region_2

These instances are spread across three different networks:
managementnet, mynetwork, and privatenet, with no two VMs sharing both zone and network.

---

# 🌐 Task 3: Explore Connectivity Between VM Instances

In this task, you test how VM instances communicate using **external** and **internal** IP addresses, and observe how **VPC networks** affect connectivity.

---

## 🌍 Ping External IP Addresses

You will confirm whether VM instances are reachable from the public internet.

1. In the Cloud console, navigate to:  
   **Navigation menu → Compute Engine → VM instances**

2. Note the **external IPs** of:
   - **mynet-vm-2**
   - **managementnet-vm-1**
   - **privatenet-vm-1**

3. From the VM **mynet-vm-1**, click **SSH** to open a terminal.

---

### ✅ Test 1: Ping `mynet-vm-2`

```bash
ping -c 3 'Enter mynet-vm-2 external IP here'
```
✔️ This should work!


### ✅ Test 2: Ping managementnet-vm-1

```bash
ping -c 3 'Enter managementnet-vm-1 external IP here'
````
✔️ This should work!

### ✅ Test 3: Ping privatenet-vm-1

```bash
ping -c 3 'Enter privatenet-vm-1 external IP here'
```
✔️ This should work!

> 💡 Observation:
You can ping all external IPs regardless of zone or VPC network.
This is allowed because:
  - External IPs are public-facing.
  - You previously configured ICMP firewall rules that allow external pings.

---

### 🔒 Ping Internal IP Addresses

Now test internal communication between VM instances.
1. In the Cloud console, again note the internal IPs of:
  - mynet-vm-2
  - managementnet-vm-1
  - privatenet-vm-1

2. Return to the SSH session of mynet-vm-1.

---

### 🟢 Test 1: Ping internal IP of mynet-vm-2

```bash
ping -c 3 'Enter mynet-vm-2 internal IP here'
```
✔️ This should work!

> 💡 Even though the VM instances are in different zones, regions, or continents, they are in the same VPC network (mynetwork), so internal communication is allowed.

---

### 🔴 Test 2: Ping internal IP of managementnet-vm-1

```bash
ping -c 3 'Enter managementnet-vm-1 internal IP here'
```
❌ This should NOT work → Expect 100% packet loss

---

### 🔴 Test 3: Ping internal IP of privatenet-vm-1

```bash
ping -c 3 'Enter privatenet-vm-1 internal IP here'
```
❌ This should NOT work → Expect 100% packet loss

💡 Reason:
These VMs are in different VPC networks.
VPC networks are isolated private networking domains by default.
Internal communication is blocked unless you configure:
  - VPC Peering
  - VPN tunnels
  - Shared VPC

📌 Reminder:
For this task, consider:
- region_1 = Region_1
- region_2 = Region_2

---

## 🧪 Task 4: Create a VM Instance with Multiple Network Interfaces

Every instance in a VPC network has a default network interface. You can create additional network interfaces attached to your VMs. Multiple network interfaces enable you to create configurations where an instance connects directly to several VPC networks (up to 8 interfaces, depending on the instance type).

The CIDR ranges of the subnets must not overlap — this is required to attach multiple NICs.

---

### 🚀 Create the VM Instance with Multiple Network Interfaces
🔧 Steps to Create vm-appliance
1. Navigate to ☰ Navigation menu > Compute Engine > VM instances.
2. Click Create Instance.
3. Set the following under Machine configuration:
| Property         | Value           |
| ---------------- | --------------- |
| **Name**         | `vm-appliance`  |
| **Region**       | `US_Region`     |
| **Zone**         | `US_Zone`       |
| **Series**       | `E2`            |
| **Machine Type** | `e2-standard-4` |
> 💡 Note: e2-standard-4 allows up to 4 network interfaces.
4. Click Networking → Edit Network interfaces.

🔌 NIC #1

| Property       | Value           |
| -------------- | --------------- |
| **Network**    | privatenet      |
| **Subnetwork** | privatesubnet-1 |
Click Done.

🔌 NIC #2

Click Add a network interface.
| Property       | Value              |
| -------------- | ------------------ |
| **Network**    | managementnet      |
| **Subnetwork** | managementsubnet-1 |
Click Done.

🔌 NIC #3

Click Add a network interface.
| Property       | Value     |
| -------------- | --------- |
| **Network**    | mynetwork |
| **Subnetwork** | mynetwork |
Click Done.

Finally, click Create.

---

### 🔍 Explore the Network Interface Details
🖥️ In the Cloud Console
Go to Compute Engine > VM Instances → click nic0 on vm-appliance.

✔️ Verify:

NIC 0
- Attached to privatesubnet-1
- Internal IP within 172.16.0.0/24
- Correct firewall rules

NIC 1
- Attached to managementsubnet-1
- Internal IP within 10.130.0.0/20

NIC 2
- Attached to mynetwork
- Internal IP within 10.128.0.0/20

> 📝 Each NIC gets its own internal IP so the VM can communicate with each network independently.

---

### 🖥️ In the VM Terminal

SSH into vm-appliance, then run:
```bash
sudo ifconfig
```

You should see three interfaces similar to:
```ini
eth0 ... inet 172.16.0.3
eth1 ... inet 10.130.0.3
eth2 ... inet 10.128.0.3
```

---

### 🌐 Explore Network Interface Connectivity

Ping VM instances on each subnet to verify connectivity.
First, note the internal IPs of:
- privatenet-vm-1
- managementnet-vm-1
- mynet-vm-1
- mynet-vm-2

---

### 🟢 Test Connectivity (Working Cases)

📌 Ping privatenet-vm-1 (by IP)
```bash
ping -c 3 <privatenet-vm-1 IP>
```
✔️ Works!

📌 Ping privatenet-vm-1 (by name)
```bash
ping -c 3 privatenet-vm-1
```
✔️ Works — internal DNS resolves to NIC0.

📌 Ping managementnet-vm-1
```bash
ping -c 3 <managementnet-vm-1 IP>
```
✔️ Works!

📌 Ping mynet-vm-1
```bash
ping -c 3 <mynet-vm-1 IP>
```
✔️ Works!

---

🔴 Ping mynet-vm-2 (Fails)
```bash
ping -c 3 <mynet-vm-2 IP>
```
❌ Does not work.

❗ Why it fails
The VM’s routing table only includes:
- NIC0 subnet (172.16.0.0/24)
- NIC1 subnet (10.130.0.0/20)
- NIC2 subnet (10.128.0.0/20)
- Default route via NIC0 (eth0)
> Since mynet-vm-2 is in 10.132.0.0/20, that subnet is not in the table, so traffic incorrectly exits via eth0.

---

### 📘 View Routing Table

Run:
```bash
ip route
```

Expected output:
```ini
default via 172.16.0.1 dev eth0
10.128.0.0/20 via 10.128.0.1 dev eth2
10.128.0.1 dev eth2 scope link
10.130.0.0/20 via 10.130.0.1 dev eth1
10.130.0.1 dev eth1 scope link
172.16.0.0/24 via 172.16.0.1 dev eth0
172.16.0.1 dev eth0 scope link
```

🧠 Key Insight
- Primary NIC (eth0) always gets the default route.
- Traffic to unknown subnets leaves via eth0.
- To route more intelligently, use policy routing.

📚 See Configuring policy routing in Google Cloud docs.

---

## 🎉 Task Completed

In this lab, you created a VM instance with three network interfaces and verified internal connectivity for VM instances that are on the subnets attached to the multiple-interface VM. 🖧🖧🖧

You also explored the default network along with its:
   - 🌐 Subnets
   - 🛣️ Routes
   - 🔥 Firewall rules
After that, you created and tested connectivity for a new auto mode VPC network. 🚀
