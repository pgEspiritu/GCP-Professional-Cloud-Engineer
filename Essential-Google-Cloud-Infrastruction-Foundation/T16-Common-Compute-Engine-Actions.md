# ⚙️ Common Compute Engine Actions

Now that we’ve covered **compute, images, and disk options**, let’s explore some common actions in Google Cloud Compute Engine. 🖥️☁️

---

## 📝 Metadata Server

- Every VM stores metadata on a **metadata server** 🗂️  
- Useful with **startup and shutdown scripts** 🏁🛑
- Example: Retrieve the VM's **external IP** programmatically to configure a database 🌐
- Default metadata keys are the same for all VMs, making scripts **reusable** 🔁
- Recommended: Store startup/shutdown scripts in **Cloud Storage** 📦

---

## 🔄 Moving a VM to a New Zone

- Reasons to move:
  - 🌍 Geographical requirements  
  - ⚠️ Zone deprecation
- VM can be moved if:
  - VM is **TERMINATED**  
  - VM is a **Shielded VM with UEFI firmware**  
- Moving within the same **region**:
  1. Shut down VM  
  2. Use `gcloud compute instances move` command  
  3. Restart VM  
  4. Update any references (e.g., target pools) 🔗

- Moving to a **different region** (manual process):
  1. Create **snapshots** of all persistent disks 📸  
  2. Create new disks in the destination zone from snapshots  
  3. Create new VM and attach the new disks  
  4. Assign a static IP  
  5. Update references  
  6. Delete original VM, disks, and snapshots 🗑️

---

## 📸 Snapshots

Snapshots are **incremental backups** stored in **Cloud Storage**. They are useful for:

- ✅ Backing up critical data  
- ✅ Migrating data between zones  
- ✅ Transferring data to a different disk type (e.g., HDD → SSD) ⚡

### Key Notes

- Only available for **persistent disks** (not local SSDs)  
- Incremental and **automatically compressed**  
- Lower cost and faster than creating a full disk image  
- Can be scheduled for **automatic backups** ⏰  
- Can restore to a **new persistent disk**, enabling zone migration

> For step-by-step snapshot creation, refer to the **Course Resources PDF** 📚

---

## 💽 Resizing Persistent Disks

- Increasing storage **improves I/O performance** 📈  
- Can resize **while attached to a running VM**  
- **Cannot shrink disks** — plan capacity accordingly ⚠️  

---

