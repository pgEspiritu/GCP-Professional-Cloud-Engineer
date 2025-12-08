# Optimize Costs for Google Kubernetes Engine: Challenge Lab

**Experiment Lab**  
**Schedule:** 1 hour 30 minutes  
**Credits:** 5  
**Difficulty:** Intermediate  
**Course:** GSP343  

> This lab may incorporate AI tools to support your learning.

---

## Introduction

In a **challenge lab**, you’re given a scenario and a set of tasks. Instead of following step-by-step instructions, you will use the skills learned from the labs in the course to figure out how to complete the tasks on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

When you take a challenge lab, you will **not be taught new Google Cloud concepts**. You are expected to **extend your learned skills**, like changing default values and reading and researching error messages to fix your own mistakes.

To score **100%**, you must successfully complete all tasks within the time period!

> This lab is only recommended for students who are enrolled in the *Optimize Costs for Google Kubernetes Engine* course.

---

## Setup and Requirements

### Before you click the Start Lab button
- Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click **Start Lab**, shows how long Google Cloud resources are made available to you.
- This hands-on lab lets you do the lab activities in a **real cloud environment**, not in a simulation or demo environment. It provides new, temporary credentials you use to sign in and access Google Cloud for the duration of the lab.

### To complete this lab, you need:
- Access to a standard internet browser (**Chrome recommended**).  
  > **Note:** Use an Incognito (recommended) or private browser window to run this lab. This prevents conflicts between your personal account and the student account, which may cause extra charges incurred to your personal account.
- Time to complete the lab—remember, once you start, you cannot pause a lab.  
  > **Note:** Use only the student account for this lab. If you use a different Google Cloud account, you may incur charges to that account.

---

## Challenge Scenario

You are the lead **Google Kubernetes Engine admin** on a team that manages the online shop for **OnlineBoutique**.

You are ready to deploy your team's site to GKE but you are still looking for ways to make sure that you're able to **keep costs down and performance up**.

You will be responsible for deploying the **OnlineBoutique app to GKE** and making some configuration changes that have been recommended for **cost optimization**.

### Guidelines for Deployment
- Create the cluster in the **us-east1-c** zone.
- The naming scheme is **team-resource-number**, e.g., a cluster could be named `onlineboutique-cluster-651`.
- For your initial cluster, start with **machine size e2-standard-2 (2 vCPU, 8G memory)**.
- Set your cluster to use the **rapid release-channel**.

---


## Task 1: Create a Cluster and Deploy Your App

Before you can deploy the application, you'll need to:

1. **Create a cluster** in the `us-east1-c` zone.
2. **Name it** `onlineboutique-cluster-651`.
3. **Start small** with a zonal cluster of only **two (2) nodes**.

### Step 0: Set Default Zone
Set the default zone for your gcloud session:

```bash
gcloud config set compute/zone us-east1-c
```

### Step 1: Create the Cluster

Create a small zonal cluster with 2 nodes and machine type e2-standard-2 using the rapid release channel:
```bash
gcloud container clusters create onlineboutique-cluster-651 \
  --num-nodes=2 \
  --machine-type=e2-standard-2 \
  --release-channel=rapid
```

### Step 2: Create Namespaces
Before deploying the shop, set up namespaces to separate resources for the two environments: `dev` and `prod`.

```bash
kubectl create namespace dev
kubectl create namespace prod
```

Step 2: Deploy the Application

Deploy the OnlineBoutique application to the dev namespace using the following commands:
```bash
git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
cd microservices-demo
kubectl apply -f ./release/kubernetes-manifests.yaml --namespace dev
```

Step 4: Verify Deployment

Check that your OnlineBoutique store is up and running by getting the external IP of the frontend service:
```bash
kubectl get svc -n dev frontend-external
```

Note: Navigate to the returned EXTERNAL-IP in your browser to see the store running.


---

## Task 2: Migrate to an Optimized Node Pool

After successfully deploying the app to the `dev` namespace, inspect the node details to determine resource usage:

```bash
kubectl get nodes -o wide
kubectl describe node <node-name>
```

Based on your observations:
- There is plenty of leftover RAM from current deployments → smaller RAM nodes can be used.
- Most deployments require only 100m CPU per additional pod → smaller CPU nodes can be considered.
- Consider the scaling requirements of your deployments.

### Step 1: Create an Optimized Node Pool

Create a new node pool named optimized-pool-9216 with the following specifications:
- Machine type: custom-2-3584 (2 vCPU, 3.5 GB RAM)
- Number of nodes: 2
```bash
gcloud container node-pools create optimized-pool-9216 \
  --cluster=onlineboutique-cluster-651 \
  --machine-type=custom-2-3584 \
  --num-nodes=2
```

### Step 2: Migrate Deployments to the New Node Pool

1. Cordon the default node pool to prevent new pods from being scheduled:
```bash
kubectl cordon <default-node-name>
```

2. Drain the default node pool to safely evict existing pods:
```bash
kubectl drain <default-node-name> --ignore-daemonsets --delete-emptydir-data
```
Note: Replace <default-node-name> with the names of nodes in the default pool. This will safely migrate workloads to the new optimized-pool-9216.

3. Step 3: Delete the Default Node Pool

Once all deployments have been safely migrated to the optimized pool:
```bash
gcloud container node-pools delete default-pool \
  --cluster=onlineboutique-cluster-651
```
This ensures you are only running the optimized node pool, reducing costs while maintaining performance.

---

## Task 3: Apply a Frontend Update

The dev team wants to push a last-minute update to the frontend without causing downtime. Follow these steps:

---

### Step 1: Set a Pod Disruption Budget (PDB)

Create a Pod Disruption Budget for the frontend deployment to ensure high availability during updates:

```bash
kubectl apply -f - <<EOF
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: onlineboutique-frontend-pdb
  namespace: dev
spec:
  minAvailable: 1
  selector:
    matchLabels:
      app: frontend
EOF
```
> This ensures that at least 1 pod is always available during updates or maintenance.


## Step 2: Update the Frontend Deployment Image

The dev team has provided an updated Docker image:
```bash
gcr.io/qwiklabs-resources/onlineboutique-frontend:v2.1
```

Edit the frontend deployment to use this new image and set the imagePullPolicy to Always:
```bash
kubectl set image deployment/frontend frontend=gcr.io/qwiklabs-resources/onlineboutique-frontend:v2.1 --namespace dev
kubectl patch deployment frontend -n dev --patch '{"spec":{"template":{"spec":{"containers":[{"name":"frontend","imagePullPolicy":"Always"}]}}}}'
```

Step 3: Verify the Update

Check that the frontend pods are running the updated image:
```bash
kubectl get pods -n dev -o wide
kubectl describe deployment frontend -n dev | grep Image
```
> Note: The Pod Disruption Budget ensures that at least one pod remains available while the rollout occurs, preventing downtime.
