# Managing Deployments Using Kubernetes Engine

## 🕒 Lab Overview
**Duration:** 25 minutes  
**Credits:** 5  
**Level:** Intermediate  

DevOps practices often involve multiple deployments to manage scenarios such as **continuous deployment**, **blue-green deployments**, and **canary deployments**. This lab teaches you how to **scale and manage containers** to accomplish these scenarios using Kubernetes.

---

## 🎯 Objectives
In this lab, you will learn how to:

- Use the `kubectl` tool
- Create deployment YAML files
- Launch, update, and scale deployments
- Understand deployment styles and update deployments

---

## 🔑 Prerequisites
To maximize learning:

- Completed the following Google Cloud Skills Boost labs:
  - Introduction to Docker
  - Hello Node Kubernetes
- Linux System Administration skills
- Understanding of DevOps, CI/CD, and continuous deployment concepts

---

## 💡 Introduction to Deployments
Heterogeneous deployments connect **multiple environments or regions** for technical or operational needs. They may be:

- **Hybrid:** On-premises + public cloud
- **Multi-cloud:** Multiple public cloud environments
- **Public-private:** Combination of public cloud + private infrastructure

### Challenges of single-environment deployments:

- Maxed out resources
- Limited geographic reach
- Limited availability
- Vendor lock-in
- Inflexible resources

Heterogeneous deployments address these challenges but must be **repeatable, deterministic, and well-architected**.

**Common scenarios:**

- Multi-cloud deployments
- Fronting on-premises data
- CI/CD processes

---

## ⚙️ Setup and Requirements
Before starting the lab:

- Use a standard browser (Chrome recommended)
- Open in **Incognito/private window**
- Only use the provided **student account** (not your personal Google Cloud account)
- Labs are **timed** and cannot be paused

---

## 🔑 Lab Access
1. Click **Start Lab**.
2. Use the temporary credentials provided:
   - **Username**: Copy from Lab Details
   - **Password**: Copy from Lab Details
3. Accept terms and skip recovery options/2FA.
4. Open **Google Cloud Console**.

---

## ☁️ Activate Cloud Shell
Cloud Shell provides command-line access with a persistent 5GB home directory.

1. Click **Activate Cloud Shell** at the top of the console.
2. Authorize Cloud Shell to use credentials for Google Cloud API.
3. Verify project:
```bash
gcloud config list project
```
4. Set the active account if needed:
```bash
gcloud config set account ACCOUNT
```

---

## 🌐 Set the Zone

Set your working Google Cloud zone:
```bash
gcloud config set compute/zone ZONE
```

---

## 💻 Get Sample Code

Download lab resources:
```bash
gcloud storage cp -r gs://spls/gsp053/kubernetes .
cd kubernetes
```

## 🏗️ Create a Cluster

Create a 3-node Kubernetes cluster:
```bash
gcloud container clusters create bootcamp \
  --machine-type e2-small \
  --num-nodes 3 \
  --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
```
> ⚠️ Note: Cluster creation may take a few minutes.

---

# 🧩 Task 1. Learn about the deployment object

To get started, take a look at the **deployment object**.

The `explain` command in `kubectl` can tell us about the deployment object:

```bash
kubectl explain deployment
```

You can also see all of the fields using the `--recursive` option:
```bash
kubectl explain deployment --recursive
```

You can use the explain command as you go through the lab to help you understand the structure of a deployment object and what the individual fields do:
```bash
kubectl explain deployment.metadata.name
```

---

# 🚀 Task 2. Create a deployment

Create the **fortune-app** deployment. Examine the deployment configuration file:

```bash
cat deployments/fortune-app-blue.yaml
```

Output:
```yaml
# orchestrate-with-kubernetes/kubernetes/deployments/fortune-app-blue.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fortune-app-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: fortune-app
  template:
    metadata:
      labels:
        app: fortune-app
        track: stable
        version: "1.0.0"
    spec:
      containers:
        - name: fortune-app
          # The new, centralized image path
          image: "us-central1-docker.pkg.dev/qwiklabs-resources/spl-lab-apps/fortune-service:1.0.0"
          ports:
            - name: http
              containerPort: 8080
...
```
> 🔎 This deployment creates three replicas and uses version 1.0.0 of the `fortune-service` container.

---

## 📦 Create the deployment

Create the deployment using `kubectl:`
```bash
kubectl create -f deployments/fortune-app-blue.yaml
```

Verify that the deployment was created:
```bash
kubectl get deployments
```

Once created, Kubernetes automatically generates a ReplicaSet. Verify it with:
```bash
kubectl get replicasets
```

You should see a ReplicaSet named similar to:
```ini
fortune-app-blue-xxxxxxx
```

View the Pods created by the deployment:
```bash
kubectl get pods
```

---

## 🌐 Expose the deployment

Create a Service to expose the `fortune-app` deployment externally:
```bash
kubectl create -f services/fortune-app.yaml
```

Retrieve the external IP of the service:
```bash
kubectl get services fortune-app
```
> ⏳ Note: The `EXTERNAL-IP` may take a few seconds to appear. Re-run the command until it is populated.

Access the `/version` endpoint:
```bash
curl http://<EXTERNAL-IP>/version
```

You should receive the following response:
```json
{"version":"1.0.0"}
```

You can also use a one-liner with output templating:
```bash
curl http://`kubectl get svc fortune-app -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
```

---

## 📈 Scale a deployment

Now scale the deployment by adjusting the replicas count.

Increase replicas to 5:
```bash
kubectl scale deployment fortune-app-blue --replicas=5
```
> ⏳ Note: It may take a minute for the new Pods to start.

Verify that 5 Pods are running:
```bash
kubectl get pods | grep fortune-app-blue | wc -l
```

Scale the deployment back down to 3 replicas:
```bash
kubectl scale deployment fortune-app-blue --replicas=3
```

Confirm the correct number of Pods:
```bash
kubectl get pods | grep fortune-app-blue | wc -l
```
> 🎯 You have successfully learned how to create, expose, and scale Kubernetes deployments.

---

# 🔄 Task 3. Rolling update

Kubernetes **Deployments** support updating container images using a **rolling update** mechanism, allowing zero-downtime upgrades.

---

## 🚀 Trigger a rolling update

To start a rolling update, edit the existing deployment. Kubernetes will detect the change and roll it out gradually.

```bash
kubectl edit deployment fortune-app-blue
```

In the editor:

✏️ Press i to enter insert mode.

---

### 1️⃣ Update the image version

Find this line:
```yaml
image: "us-central1-docker.pkg.dev/qwiklabs-resources/spl-lab-apps/fortune-service:1.0.0"
```

Change it to:
```yaml
image: "us-central1-docker.pkg.dev/qwiklabs-resources/spl-lab-apps/fortune-service:2.0.0"
```

---

### 2️⃣ Update the environment variable

Locate the env section and update the APP_VERSION variable:

- From:
```yaml
value: "1.0.0"
```

- To:
```yaml
value: "2.0.0"
```
💾 Save and exit by pressing Esc, typing :wq, then pressing Enter.

> This action triggers the rolling update and records the deployment history.

---

### 📦 Observe the rollout

View the new ReplicaSet created by Kubernetes:
```bash
kubectl get replicaset
```

Check the rollout history:
```bash
kubectl rollout history deployment/fortune-app-blue
```

---

### ⏸️ Pause a rolling update

Pause the rollout mid-way:
```bash
kubectl rollout pause deployment/fortune-app-blue
```

Check the rollout status:
```bash
kubectl rollout status deployment/fortune-app-blue
```
> ⚠️ Note: You may see a message saying the deployment was successfully rolled out.
> This only confirms the pause command succeeded—it does not mean the update is complete.

Verify the versions running on each Pod. You should see a mix of 1.0.0 and 2.0.0, confirming the rollout is paused:
```bash
for p in $(kubectl get pods -l app=fortune-app -o=jsonpath='{.items[*].metadata.name}'); do echo $p && curl -s http://$(kubectl get pod $p -o=jsonpath='{.status.podIP}')/version; echo; done
````
Press Ctrl + C to exit the loop.

---

## ▶️ Resume a rolling update

Continue the rollout:
```bash
kubectl rollout resume deployment/fortune-app-blue
```

When complete, verify the status:
```bash
kubectl rollout status deployment/fortune-app-blue
```

---

## ⏪ Roll back an update

Assume a bug is detected in version 2.0.0.

Roll back to the previous version:
```bash
kubectl rollout undo deployment/fortune-app-blue
```
⏳ Note: The rollback may take a few moments.

Confirm that all Pods are back to version 1.0.0:
```bash
curl http://`kubectl get svc fortune-app -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
```
> 🎉 Great job! You’ve successfully practiced rolling updates, pausing, resuming, and rolling back Kubernetes deployments.

---

# 🐤 Task 4. Canary deployments

When you want to safely test a new version in production with only a **subset of users**, you can use a **canary deployment**.

---

## 🚀 Create a canary deployment

First, inspect the canary deployment configuration file:

```bash
cat deployments/fortune-app-canary.yaml
```

Now create the canary deployment:
```bash
kubectl create -f deployments/fortune-app-canary.yaml
```

---

## 📦 Verify deployments

After creating the canary deployment, you should now have two deployments running.

Check them with:
```bash
kubectl get deployments
```

ℹ️ The fortune-app Service uses the selector:
```yaml
app: fortune-app
```
This selector matches Pods from both:
- fortune-app-blue (production)
- fortune-app-canary (canary)
As a result, traffic is shared between them.

---

## 🔍 Verify the canary behavior

Send multiple requests to the service to see which version responds:
```bash
for i in {1..10}; do curl -s http://`kubectl get svc fortune-app -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version; echo;
done
```
🔁 Run this command several times.

You should observe:
- Most responses return version 1.0.0
- A small number of responses return version 2.0.0
> ✅ This confirms that the canary deployment is receiving only a portion of production traffic.

---

# 🔵🟢 Task 5. Blue-green deployments

In a **blue-green deployment**, you run two separate versions of an application and switch traffic **all at once** by updating the Service selector.

---

## 🌐 The service (Blue – v1.0.0)

First, update the Service so it points **only to the blue version (1.0.0)**:

```bash
kubectl apply -f services/fortune-app-blue-service.yaml
```

---

## 🚀 Deploy the Green version (v2.0.0)

Create the new green deployment for version 2.0.0:
```bash
kubectl create -f deployments/fortune-app-green.yaml
```

Once the green deployment is running, confirm that traffic is still serving version 1.0.0:
```bash
curl http://`kubectl get svc fortune-app -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
```

---

## 🔁 Switch traffic to Green

Now update the Service to point to the green deployment:
```bash
kubectl apply -f services/fortune-app-green-service.yaml
```
After the Service update, traffic switches immediately.

Verify that version 2.0.0 is now always served:
```bash
curl http://`kubectl get svc fortune-app -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
```

---

## ⏪ Blue-green rollback

If you need to roll back, simply re-apply the Service for the blue deployment:
```bash
kubectl apply -f services/fortune-app-blue-service.yaml
```

Verify that traffic has switched back to version 1.0.0:
```bash
curl http://`kubectl get svc fortune-app -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
```

---

# 🎉 Task Completed

You’ve successfully worked hands-on with the **kubectl** command-line tool and explored multiple **deployment strategies** using YAML configuration files.

### 🚀 What you accomplished
- Used **kubectl** to manage Kubernetes resources  
- Created and applied **deployment YAML files**  
- Launched, updated, and **scaled deployments**  
- Practiced different **deployment styles** (rolling updates, canary, blue-green)

👏 Great job! You now have a stronger understanding of how to manage and evolve applications in **Google Kubernetes Engine** using real-world DevOps practices.

