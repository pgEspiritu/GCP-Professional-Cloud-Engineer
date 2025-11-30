# 🏷️ Labels in Google Cloud

Projects and folders provide levels of segregation for resources, but sometimes you need **more granularity** — that's where **labels** come in.

Labels are a utility for organizing Google Cloud resources. They are **key-value pairs** that you can attach to resources like VMs, disks, snapshots, and images.

You can create and manage labels using:
- 🌐 Google Cloud Console  
- 💻 `gcloud` CLI  
- 🔧 Resource Manager API  

Each resource can have **up to 64 labels**.

---

## 🖇️ Example: Environment Labels
You could create a label such as:

```text
environment = production
```

or


Using these labels, you can:
- 🔍 Search and filter resources  
- 📋 List production resources for inventory  
- 📊 Analyze costs  
- 🛠️ Run automated scripts on grouped resources  

---

## 🏷️ Example Labels on an Instance
A typical VM may have multiple labels such as:

- `team=marketing`  
- `component=frontend`  
- `environment=prod`  
- `state=inuse`

---

# 🧰 Recommended Uses for Labels

## 👥 1. Team or Cost Center  
Distinguish ownership for cost accounting or budgeting.

- `team:marketing`  
- `team:research`

## 🧩 2. Components  
Identify which part of the system the resource belongs to.

- `component:redis`  
- `component:frontend`

## 🌐 3. Environment or Stage  
Highlight lifecycle position.

- `environment:prod`  
- `environment:test`

## 👤 4. Owner or Contact  
Define responsibility.

- `owner:gaurav`  
- `contact:opm`

## 🔄 5. Resource State  
Track lifecycle or clean-up needs.

- `state:inuse`  
- `state:readyfordeletion`

---

# ⚠️ Labels vs Network Tags

❗ **Don’t confuse labels with network tags.**

### 🏷️ Labels  
- Key-value pairs  
- Used to organize resources  
- Can propagate to billing  
- Apply to many resource types  

### 🔖 Network Tags  
- User-defined strings  
- Apply **only** to Compute Engine instances  
- Used mainly for networking  
  - Applying **firewall rules**  
  - Defining **custom routes**

---

For more information about using labels, refer to the **Course Resources**.
