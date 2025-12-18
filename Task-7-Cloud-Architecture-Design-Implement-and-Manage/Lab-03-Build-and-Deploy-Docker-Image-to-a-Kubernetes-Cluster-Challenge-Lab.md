# 🐳 Build and Deploy a Docker Image to a Kubernetes Cluster: Challenge Lab

**Lab ID:** GSP304  
**Level:** Intermediate  
**Duration:** ~20 minutes  
**Credits:** 5  

This challenge lab is completed on **:contentReference[oaicite:0]{index=0}** and focuses on building, pushing, and deploying a containerized application using Docker and Kubernetes.

---

## 📘 Overview

In a **challenge lab**, you are given a scenario and objectives instead of step-by-step UI instructions. You are expected to apply prior knowledge, troubleshoot errors, and complete all tasks within the allotted time to achieve **100% completion**.

This lab validates your ability to:
- Build Docker images
- Push images to Container Registry
- Deploy applications to Google Kubernetes Engine (GKE)

---

## 🎯 Challenge Scenario

Your development team provided a simple Go application named **echo-web** along with a `Dockerfile`.  
Your task is to:

1. Build a Docker image
2. Push it to **Container Registry**
3. Create a Kubernetes cluster
4. Deploy the application to the cluster

⚠️ **Strict Naming Requirements (Lab Validation Depends on This):**
- **Container image name:** `echo-app`
- **Cluster name:** `echo-cluster`
- **Deployment name:** `echo-web`
- **Cluster zone:** `ZONE`

---

## ☸️ Task 1: Create a Kubernetes Cluster

In this task, you will create a **small, cost-effective Kubernetes cluster** to host the test application. The lab environment has **limited capacity**, so the cluster must strictly follow the required specifications to pass validation on **:contentReference[oaicite:0]{index=0}**.

---

## 🎯 Objective

- Create a Kubernetes cluster named **`echo-cluster`**
- Use **exactly 2 nodes**
- Use machine type **`e2-standard-2`**
- Deploy the cluster in **ZONE**

---

## 🛠️ How to Do It (CLI)

Run the following command in **Cloud Shell**:

```bash
gcloud container clusters create echo-cluster \
  --zone ZONE \
  --num-nodes 2 \
  --machine-type e2-standard-2
```

---

## 🔑 Configure kubectl Access

After the cluster is created, configure kubectl so it can communicate with the cluster:
```bash
gcloud container clusters get-credentials echo-cluster \
  --zone ZONE
```

---

## ✔️ Verification

Confirm the cluster and nodes are running:
```bash
kubectl get nodes
```
> You should see 2 nodes in the Ready state.

---

## ✅ Task 1 Completed

- ☸️ Kubernetes cluster echo-cluster created
- 🖥️ 2 worker nodes using e2-standard-2
- 📍 Cluster deployed in the correct ZONE
- 🔗 kubectl successfully connected

---

## 🐳 Task 2: Build a Tagged Docker Image

In this task, you will download the sample application archive, extract it, and build a **Docker image tagged as `v1`**. This image will later be pushed and deployed on a Kubernetes cluster running on **:contentReference[oaicite:0]{index=0}**.

---

## 🎯 Objective

- Download the application archive from Cloud Storage
- Extract the application files
- Build a Docker image
- Tag the image as **`v1`**

---

## 📦 Step 1: Download the Application Archive

The archive is stored in a Cloud Storage bucket **owned by your lab project**.

### 🛠️ How to Do It (CLI)

```bash
gsutil cp gs://$GOOGLE_CLOUD_PROJECT/echo-web.tar.gz .
```

---

### 📂 Step 2: Extract the Archive

Unpack the application files:
```bash
tar -xzf echo-web.tar.gz
cd echo-web
```
✅ This directory should contain:
- Dockerfile
- Application source files (Go app)

---

### 🧱 Step 3: Build the Docker Image (Tagged as v1)

Build the Docker image and tag it correctly so it can be used later by Kubernetes.

🛠️ How to Do It (CLI)
```bash
docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v1 .
```

---

### ✔️ Verification

Confirm the image was built successfully:
```bash
docker images | grep echo-app
```

You should see an image similar to:
```ini
gcr.io/PROJECT_ID/echo-app   v1   <IMAGE_ID>
```

---

## ✅ Task 2 Completed

- 📥 Downloaded echo-web.tar.gz from Cloud Storage
- 📂 Extracted application and Docker context
- 🐳 Built Docker image echo-app
- 🏷️ Tagged image as v1

---

## 📤 Task 3: Push the Image to Google Container Registry

In this task, you will push the Docker image you built in the previous step to **Google Container Registry (GCR)** using the required **`gcr.io` hostname** on **:contentReference[oaicite:0]{index=0}**.

---

## 🎯 Objective

- Push the Docker image to **Google Container Registry**
- Use the **gcr.io** hostname
- Ensure the image is available for Kubernetes deployment
- Application listens on **TCP port 8000** (used later during service exposure)

---

## 🔐 Step 1: (If Needed) Authenticate Docker to GCR

Cloud Shell usually has this configured, but you can run this safely to ensure authentication:

```bash
gcloud auth configure-docker gcr.io
```

---

## 📦 Step 2: Push the Tagged Image

Push the v1-tagged image to Container Registry:
```bash
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v1
```
> ⏳ Wait for the push to complete.

---

## ✔️ Verification

Confirm the image exists in Container Registry:
```bash
gcloud container images list \
  --repository=gcr.io/$GOOGLE_CLOUD_PROJECT
```

You should see:
```bash
gcr.io/PROJECT_ID/echo-app
```

Optional: list tags
```bash
gcloud container images list-tags \
  gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app
```

---

## ✅ Task 3 Completed

- 🐳 Docker image pushed to Google Container Registry
- 🏷️ Image tag v1 available
- ☸️ Ready for Kubernetes deployment
- 🔌 Application configured for TCP port 8000

---

## 🚀 Task 4: Deploy the Application to the Kubernetes Cluster

In this task, you will deploy the Docker image to your Kubernetes cluster **echo-cluster** and expose it as a service that responds on **port 80**, even though the container listens on **port 8000**. This ensures the application is accessible as a standard web service on **:contentReference[oaicite:0]{index=0}**.

---

## 🎯 Objective

- Deploy the Docker image `echo-app:v1` to Kubernetes
- Use the deployment name **echo-web**
- Expose the service to respond on **port 80**
- Verify the deployment and service are working

---

## 🧑‍💻 Step 1: Create the Deployment

Create a Kubernetes deployment using your pushed image:

```bash
kubectl create deployment echo-web \
  --image=gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v1
```
> ✅ This creates a deployment named echo-web in the default namespace.

---

## 🧑‍💻 Step 2: Expose the Deployment as a Service

Expose the deployment via a LoadBalancer service so it responds to HTTP requests on port 80:
```bash
kubectl expose deployment echo-web \
  --type=LoadBalancer \
  --port 80 \
  --target-port 8000
```
- port 80: external port for HTTP requests
- target-port 8000: internal container port

---

## 🔎 Step 3: Verify the Deployment

Check that the deployment and pods are running:
```bash
kubectl get deployments
kubectl get pods
```
> You should see echo-web deployment with 1 or more pods in Ready state.

---

## 🌐 Step 4: Verify the Service

Check the service and note the EXTERNAL-IP:
```bash
kubectl get services
```

Example output:
```text
NAME       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
echo-web   LoadBalancer   10.0.0.12      34.68.123.45   80:XXXXX/TCP   5m
```
- Open the EXTERNAL-IP in a browser to confirm the application responds
- The application should show Echo-app responses, indicating successful deployment

---

## ✅ Task 4 Completed

- ☸️ Deployment echo-web created
- 🐳 Docker image echo-app:v1 running in Kubernetes
- 🌐 Service exposed on port 80 with LoadBalancer
- 🔍 Application verified to respond to HTTP requests


---

## 🛠️ Troubleshooting

This section covers common issues you may encounter when deploying the **echo-web** application on **Google Kubernetes Engine** and how to resolve them.

---

## ❌ Issue 1: Receiving a 504 Gateway Timeout Error

A 504 error usually occurs when the LoadBalancer cannot reach the backend pods.

### 🔍 Possible Causes

- The **application container has not fully initialized**
  - Give the pods a few minutes to start
- **Port mismatch**
  - The Docker container listens on **TCP port 8000** (as set in the Dockerfile)
  - The Kubernetes service must **map external port 80 to target port 8000**
- **Service not pointing to the correct pods**
  - Check the deployment name matches the service selector
  - Ensure pods are **Ready**:
    ```bash
    kubectl get pods
    ```

### 🛠️ Resolution Steps

1. Verify service configuration:

```bash
kubectl get svc echo-web -o yaml
```

2. Confirm targetPort: 8000 and port: 80

3. Check pod status and logs:
```bash
kubectl get pods
kubectl logs <pod-name>
```

4. If pods are failing, delete the deployment and recreate:
```bash
kubectl delete deployment echo-web
kubectl create deployment echo-web --image=gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v1
kubectl expose deployment echo-web --type=LoadBalancer --port 80 --target-port 8000
```

## ❌ Issue 2: Not Receiving Assessment Score for Last Objectives  

### 🔍 Possible Cause
- The Kubernetes cluster may have been created in a different zone than the lab-specified ZONE.
- Lab assessments require exact names and zone settings.

### 🛠️ Resolution Steps

1. Confirm cluster zone:
```bash
gcloud container clusters list
```

2. If zone is incorrect, delete the cluster and recreate in the correct zone:
```bash
gcloud container clusters delete echo-cluster --zone <wrong-zone>
gcloud container clusters create echo-cluster --zone ZONE --num-nodes 2 --machine-type e2-standard-2
gcloud container clusters get-credentials echo-cluster --zone ZONE
```

3. Re-deploy the Docker image and service as instructed.

---

### ✅ Quick Checklist

- Cluster created in ZONE
- Deployment name: echo-web
- Service maps port 80 → targetPort 8000
- Pods are in Ready state
- External IP is assigned
- Application responds via browser

---

## ## 🎉 Congratulations!

Congratulations! In this lab, you successfully **deployed a sample application to a Kubernetes cluster** on **:contentReference[oaicite:0]{index=0}**.

---

## 🏁 Task Completed

Completed the following tasks:

- ☸️ **Created a Kubernetes cluster** (`echo-cluster`) with 2 `e2-standard-2` nodes
- 🐳 **Built a Docker image** (`echo-app:v1`) from the provided application source
- 📤 **Pushed the image to Google Container Registry**
- 🚀 **Deployed the application** (`echo-web`) to Kubernetes
- 🌐 **Exposed the application via LoadBalancer** on port 80, mapping to container port 8000

---

## ✅ Key Skills
- Containerization with **Docker**
- Image management and pushing to **GCR**
- Kubernetes cluster creation and management
- Deployment and service exposure in **Kubernetes**
- Troubleshooting connectivity and deployment issues

---

🎯 **Result:**  
The application is now running in a Kubernetes cluster and accessible externally via the service’s LoadBalancer IP.  
You’ve demonstrated a full **CI/CD-style workflow** for deploying containerized applications in Google Cloud. 🚀
