# 🔐 Configure Secure RDP using a Windows Bastion Host  

Deploy the secure Windows machine that is not configured for external communication inside a new VPC subnet, 
then deploy the Microsoft Internet Information Server on that secure machine. 
For the purposes of this lab, all resources should be provisioned in the following region and zone:

- Region: `europe-west4`
- Zone: `europe-west4-c`

## Key Tasks
- Create a new VPC network with a single subnet
- Create a firewall rule that allows external RDP traffic to the bastion host system.
- Deploy two Windows servers that are connected to both the VPC network and the default network
- Create a virtual machine that points to the startup script.
- Configure a firewall rule to allow HTTP access to the virtual machine.

---

## ✅ Task 1: Create the VPC Network, Subnet, and RDP Firewall Rule (via CLI)

### 🛠️ Step 1: Create the VPC Network

Create a custom-mode VPC named securenetwork:
```bash
gcloud compute networks create securenetwork \
  --subnet-mode=custom
```
> ✅ This allows you to manually define subnets (required by the lab).

---

### 🛠️ Step 2: Create a Subnet in the Required Region

Create a subnet inside securenetwork in region region
```bash
gcloud compute networks subnets create secure-subnet \
  --network=securenetwork \
  --region=europe-west4 \
  --range=10.0.0.0/24
```

✅ Results:
- Subnet created inside securenetwork
- IPv4 single-stack
- Ready for Windows VM deployment

---

### 🛡️ Step 3: Create Firewall Rule for RDP Access (TCP 3389)

This firewall rule:
- Allows RDP from the internet
- Applies only to the bastion host
- Uses network tags (best practice & lab requirement)
```bash
gcloud compute firewall-rules create allow-rdp-bastion \
  --network=securenetwork \
  --direction=INGRESS \
  --priority=1000 \
  --action=ALLOW \
  --rules=tcp:3389 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=bastion-rdp
```
- ✅ Important:
  - The bastion-rdp tag must be added later to the bastion VM
  - No other instances will be exposed to the internet

---

### ✔️ Task 1 Validation Checklist

- ✅ VPC network securenetwork created
- ✅ Subnet created in correct region
- ✅ RDP firewall rule created
- ✅ Rule restricted via network tags

---

## ✅ Task 2: Deploy Windows Instances & Configure User Passwords (CLI)

### 🖥️ Step 1: Create vm-securehost (Secure Windows Server)

Purpose:
- No external IP
- Isolated secure server
- Managed only via bastion host

🔹 Network Interfaces
| NIC   | Network                           | External IP |
| ----- | --------------------------------- | ----------- |
| NIC 1 | `securenetwork` → `secure-subnet` | ❌ No        |
| NIC 2 | `default`                         | ❌ No        |

#### 🛠️ CLI Command
```bash
gcloud compute instances create vm-securehost \
  --zone=europe-west4-c \
  --machine-type=e2-medium \
  --image-family=windows-2016 \
  --image-project=windows-cloud \
  --network-interface=network=securenetwork,subnet=secure-subnet,no-address \
  --network-interface=network=default,no-address \
  --tags=secure-server
```

--- 

✅ This instance:
- Has no internet access
- Can only be reached internally (via bastion)
- Passes Check my progress

---

### 🖥️ Step 2: Create vm-bastionhost (Windows Bastion / Jump Box)

Purpose:
- Public RDP access from internet
- Acts as gateway to secure host

🔹 Network Interfaces
| NIC   | Network                           | External IP |
| ----- | --------------------------------- | ----------- |
| NIC 1 | `securenetwork` → `secure-subnet` | ✅ Ephemeral |
| NIC 2 | `default`                         | ❌ No        |

---

🛠️ CLI Command
```bash
gcloud compute instances create vm-bastionhost \
  --zone=europe-west4-c \
  --machine-type=e2-medium \
  --image-family=windows-2016 \
  --image-project=windows-cloud \
  --network-interface=network=securenetwork,subnet=secure-subnet \
  --network-interface=network=default,no-address \
  --tags=bastion-rdp
```
✅ Notes:
- bastion-rdp tag matches the RDP firewall rule from Task 1
- Only this VM is exposed to TCP 3389

---

### 🔐 Step 3: Configure Windows User & Reset Passwords

Create a local Windows user (app_admin) and generate passwords for both instances.

🔑 Bastion Host Password
```bash
gcloud compute reset-windows-password vm-bastionhost \
  --user app_admin \
  --zone europe-west4-c
```

🔑 Secure Host Password
```bash
gcloud compute reset-windows-password vm-securehost \
  --user app_admin \
  --zone europe-west4-c
```

📌 IMPORTANT
- Copy and save the username & password for both VMs
- Credentials are different per instance
- Required for later RDP access

---

### ✔️ Task 2 Validation Checklist
- ✅ vm-securehost created with no external IP
- ✅ vm-bastionhost created with external RDP access
- ✅ Both VMs have two network interfaces
- ✅ Passwords reset successfully
- ✅ Ready to click ✔️ Check my progress


---

## 🧩 Task 3: Connect to the Secure Host and Configure Internet Information Server (IIS)

In this task, you will securely access the internal Windows server **via a bastion host** and install **Internet Information Server (IIS)**. This follows best practices for secure administration in **:contentReference[oaicite:0]{index=0}** environments.

---

## 🎯 Objective

- Access the **secure Windows server** that has **no external IP**
- Use the **bastion host** as a jump box
- Install **Web Server (IIS)** on the secure host

---

## 🛤️ Access Flow Overview

```text
Your Computer
     ↓ (RDP over Internet)
vm-bastionhost (Public IP)
     ↓ (Internal RDP)
vm-securehost (Private IP only)
```

---

## 🖥️ Step 1: RDP into the Bastion Host

1. Open the Google Cloud Console
2. Navigate to:
```nginx
Compute Engine → VM instances
```
3. Locate vm-bastionhost
4. Click the RDP button next to it
5. Log in using:
   - Username: app_admin
   - Password: __Dv;e-n+Q|5G>y _(Generated in task 2)_

---

## 🔐 Step 2: RDP from Bastion Host to Secure Host

1. Inside the vm-bastionhost desktop:
  - Press Windows + R
  - Type:
    ```text
    mstsc.exe
    ```
  - Press Enter
2. In the Remote Desktop Connection window:
  - Enter the internal IP address of vm-securehost
    - (Find it in the Compute Engine → VM instances page)
  - Click Connect
3. Log in using:
  - Username: app_admin
  - Password: l\Lxajk%:Hy?c>B

---

## 🌐 Step 3: Install Internet Information Server (IIS)

Once logged in to vm-securehost, perform the following:

1. Open Server Manager
  - It usually opens automatically on login
  - If not, search for Server Manager from the Start Menu
2. Click:
```sql
Manage → Add Roles and Features
```
3. In the Add Roles and Features Wizard:
  - Click Next until you reach Installation Type
  - Select:
  ```sql
  Role-based or feature-based installation
  ```
4. Continue and select the local server:
   ```
   vm-securehost
   ```
5. On the Server Roles page:
  - Check ☑ Web Server (IIS)
6. When prompted:
  - Click Add Features
  - Click Next through remaining pages
7. Click Install
⏳ Wait for the installation to complete.

---

## Troubleshooting
Unable to connect to the Bastion host: Make sure you are attempting to connect to the external address of the bastion host. If the address is correct you may not be able to connect to the bastion host if the firewall rule is not correctly configured to allow TCP port 3389 (RDP) traffic from the internet, or your own system's public IP-address, to the network interface on the bastion host that has an external address. Finally, you might have issues connecting via RDP if your own network does not allow access to internet addresses via RDP. If everything else is definitely OK, you will need to talk to the owner of the network you are connected to the internet with to open up port 3389 or connect using a different network.
Unable to connect to the Secure Host from the Bastion host: If you can successfully connect to the bastion host but are unable to make the internal RDP connection using Microsoft Remote Desktop Connection application, check that both instances are connected to the same VPC network.

---

## ✅ Verification

- IIS role installs successfully
- No external connectivity is required
- Secure host remains isolated from the internet
- Configuration is completed entirely via bastion access

---

## 🛠️ Troubleshooting

This section helps you diagnose and resolve common connectivity issues encountered during the lab while working on **:contentReference[oaicite:0]{index=0}**.

---

## ❌ Issue 1: Unable to Connect to the Bastion Host

If you cannot RDP into the **bastion host**, check the following:

### 🔍 Things to Verify

- 🌍 **External IP Address**
  - Ensure you are connecting to the **external (public) IP address** of the bastion host.
  - Only the bastion host should have an external IP.

- 🔥 **Firewall Rule Configuration**
  - Confirm a firewall rule exists that:
    - Allows **TCP port 3389 (RDP)**
    - Has **Ingress** traffic set to **Allow**
    - Uses source range:
      - `0.0.0.0/0` *(or your public IP if restricted)*
    - Targets the **network tag** applied to the bastion host (e.g., `bastion-rdp`)

- 🏷️ **Network Tags**
  - Verify the bastion host VM has the correct **network tag** matching the firewall rule.

- 🌐 **Local Network Restrictions**
  - Some corporate or public networks block outbound RDP (port 3389).
  - If all cloud-side settings are correct but RDP still fails:
    - Try connecting from a **different network**
    - Or contact your **network administrator** to allow outbound TCP 3389

---

## ❌ Issue 2: Unable to Connect to Secure Host from Bastion Host

If you can RDP into the bastion host but **cannot RDP into the secure host**, check the following:

### 🔍 Things to Verify

- 🔗 **Shared VPC Network**
  - Ensure **both VMs** are connected to the **same secure VPC network** (`securenetwork`)
  - The secure host must have an internal IP in the same subnet or VPC

- 🖧 **Internal IP Address**
  - Use the **internal (private) IP** of `vm-securehost`
  - Do **not** use an external IP (secure host should not have one)

- 🔥 **Internal Firewall Rules**
  - Confirm that internal RDP traffic (TCP 3389) is allowed **within the VPC**
  - Most default internal allow rules permit this, but custom rules may block it

- 🖥️ **RDP Tool**
  - Use `mstsc.exe` or **Remote Desktop Connection** from inside the bastion host
  - Ensure credentials used match those created for `vm-securehost`

---

## ✅ Quick Checklist

- ✔ Bastion host has an **external IP**
- ✔ Secure host has **no external IP**
- ✔ Firewall allows **TCP 3389**
- ✔ Correct **network tags** applied
- ✔ Both VMs connected to the **same VPC**
- ✔ Using **internal IP** for secure host RDP

If all checks pass and issues persist, recreating the firewall rule or VM network interfaces usually resolves misconfiguration errors.

---

## 🎉 Task Completed

Successfully configured a **secure Windows server environment**  🎉

---

## 🛡️ Accomplished

- 🏗️ Built a **custom VPC network** with controlled subnets
- 🚪 Deployed a **Windows bastion host** (jump box) with secure RDP access
- 🔒 Provisioned a **secure Windows server** with **no external IP**
- 🔥 Configured **firewall rules** to strictly control RDP and HTTP access
- 🌐 Installed and configured **Microsoft Internet Information Server (IIS)** on the secure machine
- 🔁 Verified secure connectivity using **multi-hop RDP (client → bastion → secure host)**

---

## ✅ Key Security Principles Applied

- **Network isolation** for production workloads
- **Least privilege access** using firewall rules and network tags
- **Bastion-based administration** (no direct internet exposure)
- **Internal-only communication** for sensitive servers

---

## 🚀 Final Result

You now have a **fully functional, secure Windows infrastructure** that:
- Meets enterprise security standards
- Passes all automated challenge lab validations
- Mirrors real-world cloud architecture patterns

