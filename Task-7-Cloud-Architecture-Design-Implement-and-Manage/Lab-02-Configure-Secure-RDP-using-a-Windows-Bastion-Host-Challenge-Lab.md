# 🔐 Configure Secure RDP using a Windows Bastion Host  

Deploy the secure Windows machine that is not configured for external communication inside a new VPC subnet, 
then deploy the Microsoft Internet Information Server on that secure machine. 
For the purposes of this lab, all resources should be provisioned in the following region and zone:
- Region: region
- Zone: zone

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
  --region=region \
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
  --zone=zone \
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
  --zone=zone \
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
  --zone zone
```

🔑 Secure Host Password
```bash
gcloud compute reset-windows-password vm-securehost \
  --user app_admin \
  --zone zone
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
