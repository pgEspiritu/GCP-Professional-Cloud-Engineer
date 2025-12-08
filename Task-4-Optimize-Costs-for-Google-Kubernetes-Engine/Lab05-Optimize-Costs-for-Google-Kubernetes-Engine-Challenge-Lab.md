# 🚀 Optimize Costs for Google Kubernetes Engine: Challenge Lab  
**⏱ Duration:** 1 hour 30 minutes  
**💰 Credits:** 5  
**📈 Level:** Intermediate  
**🧪 Lab Code:** GSP343  

---

## 🧩 Introduction

In this **challenge lab**, you're given a **scenario** and a **set of tasks**.  
Unlike standard labs, you **won’t receive step-by-step instructions** — you must apply what you've learned throughout the course to figure out how to complete each task. 💡

An **automated scoring system** will indicate whether your tasks have been completed correctly.

This lab does **not** introduce new Google Cloud concepts. Instead, you are expected to:

- Modify default values ⚙️  
- Troubleshoot and interpret error messages 🛠️  
- Use your previous knowledge to solve problems 🔍  

To score **100%**, you must complete **all tasks** within the allowed time. ⏳✔️  
Only proceed if you're comfortable working independently with Google Cloud and Google Kubernetes Engine.

---

## 🛠️ Setup and Requirements

### Before clicking **Start Lab** ▶️

- Labs are **timed** and **cannot be paused**. Once started, the timer begins counting down and resources are provisioned for your temporary environment.
- You will perform tasks in a **real Google Cloud environment** — *not a simulation*.
- You will receive **temporary student credentials** to access Google Cloud for the duration of the lab.

### ✅ Requirements

- A standard internet browser (✨ **Chrome recommended**).
- Use an **Incognito / Private window** to avoid conflicts with your personal Google account.  
  ⚠️ This prevents accidental charges to personal billing accounts.
- Ensure you have enough time — labs cannot be paused once started.
- Use **only the student account** provided for the lab.

---

## 📘 Challenge Scenario

You are the **lead Google Kubernetes Engine (GKE) admin** for the team managing the **OnlineBoutique** e-commerce site. 🛍️  
You're preparing to deploy your app to GKE while ensuring:

- 💸 **Costs remain optimized**, and  
- ⚡ **Performance stays high**.

Your responsibilities:

- Deploy the **OnlineBoutique** application to GKE  
- Apply a set of **recommended cost-optimization configurations**

### 📏 Deployment Guidelines

Follow these rules while deploying:

- ➤ Create the cluster in **`us-central1-c`**  
- ➤ Use naming format: **`team-resource-number`**  
  - Example: `onlineboutique-cluster-987`
- ➤ Initial cluster should use machine type **`e2-standard-2`** (2 vCPU, 8 GB RAM)  
- ➤ Set the cluster to use the **rapid release channel** 🚀

---

# 🧩 Task 1 — Step-by-step Solution: Create a cluster and deploy your app

> **Goal:** Create a zonal GKE cluster named `onlineboutique-cluster-987` in `us-central1-c` (2 nodes, `e2-standard-2`, rapid release channel), create `dev` and `prod` namespaces, and deploy the OnlineBoutique app into `dev`.

---

## 🔐 Before you start (lab environment)
1. Open an **Incognito / Private** browser window and sign in using the **student account** provided by the lab.  
   ⚠️ Do **not** use your personal Google account.

2. Make sure the **Cloud Shell** or a machine with `gcloud` and `kubectl` available is ready. The Cloud Shell is recommended in the lab.

---

## 1️⃣ Set your project & zone (Cloud Shell)

Run these commands (replace `YOUR_PROJECT_ID` only if your lab requires it — often the lab auto-sets the project):

Set default zone to us-central1-c
```bash
gcloud config set compute/zone us-central1-c
```

## 2️⃣ Create the GKE cluster (zonal, 2 nodes, rapid channel)
```bash
gcloud container clusters create onlineboutique-cluster-987 \
  --zone us-central1-c \
  --num-nodes 2 \
  --machine-type e2-standard-2 \
  --release-channel rapid \
  --verbosity=info
```

Notes:
- --num-nodes 2 creates 2 nodes in the single zone.
- --release-channel rapid sets the rapid release channel.
If your lab environment requires IP allocation flags (rare for challenge labs), the console will show errors with suggested flags — follow those suggestions.

## 3️⃣Get cluster credentials (so kubectl talks to your cluster)
```bash
gcloud container clusters get-credentials onlineboutique-cluster-987 --zone us-central1-c
```

Confirm kubectl can reach the cluster:
```bash
kubectl get nodes
```
Expect: 2 nodes listed and READY

## 4️⃣ Create namespaces: dev and prod
```bash
kubectl create namespace dev
kubectl create namespace prod
```

Verify
```bash
kubectl get namespaces
```
> Expect: dev and prod listed along with others (kube-system, default, etc.)

## 5️⃣ Clone the OnlineBoutique repo and deploy to `dev`
```bash
git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
cd microservices-demo
kubectl apply -f ./release/kubernetes-manifests.yaml --namespace dev
```

## 6️⃣ Verify resources are created and pods are running

Watch pods come up (give a few minutes for images to pull and pods to become READY):
```bash
kubectl get pods -n dev --watch
```
> You should see many pods (frontend, productcatalogservice, cartservice, etc.). Some pods may restart during initial pulls; wait until most show READY and STATUS is Running.

## 7️⃣ Find the external IP for the frontend
The frontend service in this manifest is usually frontend-external (Service type LoadBalancer). Get its external IP:
```bash
kubectl get svc frontend-external -n dev
```

Output looks like:
```pgsql
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
frontend-external  LoadBalancer   10.x.x.x       34.x.x.x         80:xxxxx/TCP   2m
```
> If `EXTERNAL-IP` shows `<pending>`, wait a minute and re-run the command. When ready, copy the external IP.

## 8️⃣ Verify the OnlineBoutique store in your browser
Open http://<EXTERNAL-IP>/ in your browser (use the IP you obtained).
You should see the OnlineBoutique storefront UI.

---

# 🧩 Task 2 — Migrate to an Optimized Node Pool

After analyzing your cluster resources, you observe:

- 🧠 Your workloads leave **plenty of unused RAM**, meaning a node type with less memory will work.
- ⚙️ Scaling most deployments only adds **~100m CPU per replica**, so a machine type with less CPU is also sufficient.
- 📉 Therefore, a **smaller and more cost-efficient machine type** can handle your workload.

Your task:

- Create a new node pool named **`optimized-pool-5299`**  
- Use machine type **`custom-2-3584`** (2 vCPU, 3.5 GB RAM)  
- Use **2 nodes**  
- Migrate the workloads by draining `default-pool`  
- Delete the old node pool after workloads have moved

## 1️⃣ Create the optimized node pool

Run:

```bash
gcloud container node-pools create optimized-pool-5299 \
  --cluster=onlineboutique-cluster-987 \
  --machine-type=custom-2-3584 \
  --num-nodes=2 \
  --zone=us-central1-c
```

Verify creation:
```bash
gcloud container node-pools list \
  --cluster=onlineboutique-cluster-987 \
  --zone=us-central1-c
```

## 2️⃣ Confirm nodes are ready
```bash
kubectl get nodes -o wide
```

You should now see two new nodes labeled with:
```pgsql
gke-onlineboutique-cluster-987-optimized-pool-5299-xxxxx
```
They must show STATUS: Ready.

## 3️⃣ Cordon the old node pool (default-pool)

This prevents scheduling new pods onto old nodes.

Identify nodes in the default pool:
```bash
kubectl get nodes -l cloud.google.com/gke-nodepool=default-pool
```

Cordon each node:
```bash
kubectl cordon <node-name>
```

Repeat for all nodes in default-pool.

## 4️⃣ Drain the old node pool

This forces pods to migrate to the new node pool.
Use the following command to safely evict pods without deleting services:
```bash
kubectl drain <node-name> \
  --ignore-daemonsets \
  --delete-emptydir-data \
  --force
```
Run this for each node in the default pool.

✔️ As pods are drained, Kubernetes automatically re-schedules them onto nodes in optimized-pool-5299.

## 5️⃣ Verify pods have moved to the new node pool

Run:
```bash
kubectl get pods -n dev -o wide
```

Check the NODE column → all pods should now be running on nodes from:
```bash
optimized-pool-5299
```

## 6️⃣ Delete the old node pool

Once workloads are migrated safely:
```bash
gcloud container node-pools delete default-pool \
  --cluster=onlineboutique-cluster-987 \
  --zone=us-central1-c \
  --quiet
```

Verify only the optimized pool remains:
```bash
gcloud container node-pools list \
  --cluster=onlineboutique-cluster-987 \
  --zone=us-central1-c
```

