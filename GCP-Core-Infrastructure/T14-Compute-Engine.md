# 🖥️ Compute Engine

Earlier in the course, we explored **Infrastructure as a Service (IaaS)**.  
Now, let’s dive into **Google Cloud’s IaaS solution: Compute Engine**! 🚀

---

## 🚀 What Is Compute Engine?

**Compute Engine** lets users create and run **virtual machines (VMs)** on Google’s high-performance infrastructure.

### Benefits:
- No upfront investments needed 💰  
- Thousands of virtual CPUs available  
- Consistent, fast performance ⚡  
- Full operating system control  

Each VM behaves like a full physical server—you choose:
- CPU & RAM  
- Storage type & size  
- Operating system (Linux, Windows, or custom images)

---

## 🛠️ How to Create a VM

You can create VM instances via:

- **Google Cloud Console** (web interface)  
- **Google Cloud CLI**  
- **Compute Engine API**

Supports:
- Google-provided Linux & Windows images  
- Custom images  
- Other OS images you build yourself

---

## 🏪 Cloud Marketplace

A fast way to get started is via **Cloud Marketplace**, offering ready-made solutions from Google and third-party vendors.

### Features:
- No need to manually configure software, VMs, storage, or networking  
- Most software packages are free (normal resource charges still apply)  
- Some licensed products include usage fees (with cost estimates shown)

---

## 💳 Pricing & Billing

### ⏱️ Per-Second Billing
- Billed **per second** (minimum: 1 minute)

### 💡 Sustained Use Discounts
Automatically applied when a VM runs more than **25% of the month**—the longer it runs, the more you save.

### 📉 Committed Use Discounts
Save up to **57%** when committing to:
- 1-year or  
- 3-year terms  
for a specific amount of vCPUs and memory.

---

## 🏷️ Preemptible & Spot VMs

Great for workloads that can be interrupted, such as batch jobs or analytics.

### 💸 Save up to 90%
Preemptible and Spot VMs are **much cheaper** because Google may stop your VM when resources are needed elsewhere.

### Differences:
| Feature | Preemptible VM | Spot VM |
|--------|----------------|---------|
| Max runtime | 24 hours ⏳ | No max runtime 🚫⏱️ |
| Price | Same | Same |
| Features | Fewer | More flexible |

**Note:** Your app must tolerate interruptions.

---

## 💾 Storage Performance

Compute Engine gives **high throughput to persistent disks** by default—no special machine type required. 🚀

---

## ⚙️ Custom Machine Types

You only pay for what you need!  
Choose:
- Exact number of vCPUs  
- Exact memory amount  

Or use Google’s predefined machine types.

Customizing lets you optimize performance *and* cost. 💡

---
