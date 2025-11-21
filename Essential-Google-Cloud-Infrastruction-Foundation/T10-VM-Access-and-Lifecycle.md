# 🖥️ VM Access and Lifecycle in Google Cloud

---

## 🔐 VM Access

### 👤 Access Permissions
- The **creator of a VM** automatically has **full root privileges**.

### 🐧 Linux VM Access
- Creator has **SSH access**.
- Can grant SSH access to other users via **Google Cloud Console**.

### 🪟 Windows VM Access
- Creator generates a **username & password** through the Console.
- Users connect using **RDP (Remote Desktop Protocol)**.

### 🔥 Required Firewall Rules
- SSH → `tcp:22`
- RDP → `tcp:3389`
- *If using the default VPC network, these rules already exist.*

---

## 🔄 VM Lifecycle Overview

A VM moves through several states during its lifetime:

### 1. 🟦 Provisioning
- Occurs when you click **Create VM**.
- Resources (CPU, RAM, disk) are **reserved**.
- VM is not yet running.

### 2. 🟨 Staging
- System prepares the VM:
  - Assigns IP addresses
  - Boots system image
  - Boots operating system

### 3. 🟩 Running
- Startup scripts execute.
- SSH/RDP becomes available.
- While running, you can:
  - 🟢 **Live migrate** the VM within the same zone
  - 📦 **Snapshot** persistent disk
  - 📤 **Export** system image
  - 🔧 Modify metadata
  - 🌐 Move VM to another **zone**

### 🟣 Live Migration
- Keeps VM running during Google maintenance.
- Prevents downtime.
- Default maintenance behavior unless changed.

---

## 🔧 Actions While Running or Stopped

### Actions while *running*:
- Live migration  
- Change metadata  
- Snapshot disks

### Actions requiring a *stopped* VM:
- Increase CPU/RAM  
- Some machine-type changes

---

## 🟥 Terminated State
- VM is powered off.
- ❌ You do NOT pay for:
  - CPU  
  - Memory  
- 💰 You STILL pay for:
  - Attached persistent disks  
  - Reserved IP addresses  
- While terminated, you can:
  - Change machine type
  - Modify settings  
- ⚠️ You CANNOT change:
  - The VM's OS image

---

## 🔁 Resetting a VM
- Like pressing a **physical reset button**.
- Memory contents wiped.
- VM stays in **Running** state.

---

## 🛠️ Repairing State
- VM enters this state when:
  - It hits an internal error  
  - Host machine becomes unavailable  
- VM is **unusable** during repair.
- 🆓 Not billed while repairing.
- ❗ Not covered by SLA.
- If successful → returns to a normal lifecycle state.

---

## 😴 Suspend & Resume
- Suspended VM keeps:
  - Memory state (saved to disk)
- You can:
  - Resume VM  
  - Delete VM  

---

## ⏳ Shutdown Behavior
- Normal stop/reboot/delete → shutdown takes **~90 seconds**
- For **preemptible VM**:
  - Google sends a soft shutdown  
  - If still running after 30 sec → Google sends **ACPI G3 Mechanical Off**  
  - Important for writing shutdown scripts

---

# ⚙️ Availability Policies

### 🛡️ On-Host Maintenance
How the VM behaves during maintenance:
- **Live migrate** (default)
- **Terminate** instead of migrating

### 🔄 Automatic Restart
- VM automatically restarts after crash/maintenance  
- Can be turned off

### 📝 Policies can be changed:
- During VM creation  
- While VM is running  

---

# 🩹 OS Patch Management

Long-running VMs need patching for security.

Google Cloud provides **centralized patch management**:

## Patch Management Components

### 1. 📊 Patch Compliance Reporting
- Shows patch status for:
  - Windows VMs  
  - Linux distributions  
- Provides recommendations

### 2. 🛠️ Patch Deployment
- Automates OS and software updates
- Runs **patch jobs** on selected VM groups

---

## ✔️ What You Can Do with Patch Management

- 📝 **Create patch approvals**
  - Select which OS updates to apply  
- 📅 **Flexible scheduling**
  - One-time or recurring  
- 🔧 **Advanced configuration**
  - Pre- and post-patch scripts  
- 🗂️ **Centralized management**
  - View and manage patch jobs from one place  

---

# 💰 Billing Notes

### When VM is terminated:
- ❌ Not billed for CPU or memory  
- 💾 Still billed for disks  
- 🌐 Still billed for reserved IPs

### Reminder:
- Some settings require a stopped VM.
- Availability policies can be adjusted while VM is running.

---

# ✅ Summary (Quick Emoji Cheatsheet)

- 🟦 Provisioning → Reserving resources  
- 🟨 Staging → Preparing VM  
- 🟩 Running → VM active  
- 🟥 Terminated → VM stopped (disk & IP still billed)  
- 🟣 Live migration → No downtime  
- 🛠️ Repairing → Temporary unusable  
- 😴 Suspended → Saved state  
- 🔁 Reset → Hard reboot  
