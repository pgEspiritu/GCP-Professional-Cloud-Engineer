# 📦 Scale Out and Update a Containerized Application on a Kubernetes Cluster: Challenge Lab

**Lab ID:** GSP305  
**Level:** Intermediate  
**Duration:** ~15 minutes  
**Credits:** 5  

This challenge lab is performed on **:contentReference[oaicite:0]{index=0}** and focuses on **updating and scaling containerized applications** running on Kubernetes.

---

## 📘 Overview

In a **challenge lab**, you are given a real-world scenario and a set of objectives instead of step-by-step instructions. You are expected to rely on previously learned skills, experiment safely, and troubleshoot issues on your own. An **automated scoring system** will validate whether each requirement has been completed correctly.

⚠️ Important notes:
- No new Google Cloud concepts are taught in this lab
- You are expected to read error messages and adjust configurations independently
- All tasks must be completed **within the time limit** to score 100%

This lab is recommended for learners preparing for the **Google Cloud Certified Professional Cloud Architect** exam.

---

## 🎯 Challenge Scenario

You are taking over ownership of an existing **test environment** that already uses a **containerized microservices architecture**. An updated version of a test web application has been provided, and your responsibility is to manage and update this application running on Kubernetes.

The environment uses:
- A Kubernetes cluster named **`echo-cluster`**
- A deployment named **`echo-web`**
- A containerized test application named **`echo-app`**
- All resources deployed in **ZONE**

You will begin with an existing deployment running **version v1** of the application and then update it to a newer version provided by the development team.

---

## 🔎 Pre-Deployment Validation

Before proceeding, verify the following prerequisites in the **Google Cloud Console**:

### 🪣 Cloud Storage
- Navigate to:
```text
Cloud Storage → Buckets
```

- Confirm that the file **`echo-web-v2.tar.gz`** exists in the specified bucket

### ☸️ Kubernetes Engine
- Navigate to:
```text
Kubernetes Engine → Clusters
```

- Confirm that **`echo-cluster`** exists and shows a **green checkmark**, indicating it is running and healthy

---

## 🚀 Initial Application State

The initial version of the application (`v1`) is deployed using the following commands (already provided as reference):

```bash
gcloud container clusters get-credentials echo-cluster --zone=ZONE
kubectl create deployment echo-web --image=gcr.io/qwiklabs-resources/echo-app:v1
kubectl expose deployment echo-web --type=LoadBalancer --port 80 --target-port 8000
```

At this point:
- The application is running on Kubernetes
- It is exposed externally via a LoadBalancer
- Traffic is served on port 80, mapped to container port 8000

---

## 🧠 Challenge

You are responsible for:
- Updating the running application from v1 to v2
- Scaling the application to support multiple instances
- Verifying that all instances are running correctly
💡 You will apply Kubernetes deployment update and scaling techniques to complete the challenge.

---

## 🐳 Task 1: Build and Deploy the Updated Application (v2)

In this step, you will work with the **updated version (v2)** of the sample application. The new version adds a **version number** to the application output. You will download the archive from Cloud Storage, build a new Docker image, and tag it as **v2** so it can later be used to update the running deployment on **:contentReference[oaicite:0]{index=0}**.

---

### 🎯 Objective

- Download the updated application archive (`echo-web-v2.tar.gz`)
- Extract the application source and Dockerfile
- Build a Docker image
- Tag the image with **`v2`**

---

### 📦 Step 1: Download the Updated Application Archive

The archive is stored in a Cloud Storage bucket provided by the lab.

```bash
gsutil cp gs://bucket-name/echo-web-v2.tar.gz .
```
> 📌 Replace bucket-name with the exact bucket name shown in the lab instructions.

---

### 📂 Step 2: Extract the Archive

Unpack the application files and move into the directory:
```bash
tar -xzf echo-web-v2.tar.gz
```
✅ This directory should now contain:
- Dockerfile
- Updated application source files (v2)

---

### 🧱 Step 3: Build the Docker Image with Tag v2

Build the Docker image and tag it as v2 so it can be used for the application update.
```bash
docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v2 .
```

✔️ Verification

Confirm the image was built successfully:
```bash
docker images | grep echo-app
```

You should see both versions listed, similar to:
```bash
gcr.io/PROJECT_ID/echo-app   v1
gcr.io/PROJECT_ID/echo-app   v2
```

---

### ✅ Result

- 📥 Updated application archive downloaded
- 📂 Application source extracted
- 🐳 Docker image built successfully
- 🏷️ Image tagged as echo-app:v2


---

## 📤 Task 2: Push the Updated Image to Container Registry

After building the updated Docker image (`v2`), you must push it to **Google Container Registry (GCR)** using the required **`gcr.io`** hostname. This makes the image available for deployment to your Kubernetes cluster on **:contentReference[oaicite:0]{index=0}**.

---

###   🎯 Objective

- Push the **`echo-app:v2`** Docker image
- Use the **gcr.io** Container Registry hostname
- Ensure the image is available for Kubernetes updates

---

### 🔐 Step 1: (Optional) Ensure Docker Is Authenticated to GCR

Cloud Shell is usually preconfigured, but it’s safe to run:

```bash
gcloud auth configure-docker gcr.io
```

---

### 🚀 Step 2: Push the v2 Image to Container Registry

Push the updated image to GCR:
```bash
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v2
```
⏳ Wait until the push completes successfully.

---

### 🔍 Step 3: Verify the Image in Container Registry

List images in your project’s registry:
```bash
gcloud container images list \
  --repository=gcr.io/$GOOGLE_CLOUD_PROJECT
```

Verify the available tags:
```bash
gcloud container images list-tags \
  gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app
```
You should see both v1 and v2 tags.

---

## ✅ Result

- 🐳 Updated Docker image echo-app:v2 pushed to Container Registry
- 🏷️ Image available alongside v1
- ☸️ Ready to update the running Kubernetes deployment

---

## 🚀 Task 3: Deploy the Updated Application to the Kubernetes Cluster

In this step, you will update the running **echo-web** deployment to use the new **v2** container image and ensure the application remains accessible externally on **port 80**.

This deployment runs on **:contentReference[oaicite:0]{index=0} Kubernetes Engine (GKE)**.

---

## 🎯 Objective

- Update the existing deployment **echo-web** to use `echo-app:v2`
- Keep the service exposed on **port 80**
- Ensure the application is accessible from outside the cluster

---

### 🔐 Step 1: Connect to the Kubernetes Cluster

Make sure your local `kubectl` is configured for the correct cluster and zone:

```bash
gcloud container clusters get-credentials echo-cluster --zone=ZONE
```

---

### 🔄 Step 2: Update the Deployment to Use v2 Image

Update the container image in the existing deployment:
```bash
kubectl set image deployment/echo-web \
  echo-web=gcr.io/$GOOGLE_CLOUD_PROJECT/echo-app:v2
```
This performs a rolling update, replacing v1 pods with v2 pods without downtime.

---

### 📊 Step 3: Verify the Deployment Status

Check that the rollout completed successfully:
```bash
kubectl rollout status deployment/echo-web
```

Confirm the running pods:
```bash
kubectl get pods
```
All pods should show STATUS: Running.

---

### 🌐 Step 4: Verify External Access on Port 80

Confirm the service is still exposed:
```bash
kubectl get services
```
Look for:
- TYPE: LoadBalancer
- PORT(S): 80:xxxxx/TCP
- EXTERNAL-IP: assigned (may take a minute)

Once available, test the application:
```bash
curl http://EXTERNAL-IP
```
> ✅ The output should now include the v2 version identifier, confirming the update.

---

##  ✅ Result
- 🔁 Deployment echo-web updated to v2
- 🌍 Application exposed externally on port 80
- ☸️ Rolling update completed successfully
- 🧪 Version change verified via browser or curl

---

## 📈 Task 4: Scale Out the Application to 2 Replicas

In this step, you will increase the number of running instances (pods) of the **echo-web** application to improve availability and demonstrate horizontal scaling in **:contentReference[oaicite:0]{index=0} Kubernetes Engine (GKE)**.

---

## 🎯 Objective

- Scale the **echo-web** deployment to **2 replicas**
- Verify that both replicas are running successfully

---

### ⚙️ Step 1: Scale the Deployment

Run the following command in Cloud Shell:

```bash
kubectl scale deployment echo-web --replicas=2
```

---

### 🔍 Step 2: Verify the Scaling Operation

Check the deployment status:
```bash
kubectl get deployment echo-web
```
Expected output:
- READY: 2/2
- AVAILABLE: 2

Verify the pods:
```bash
kubectl get pods
```
You should see two pods in the Running state.

---

### 🌐 Step 3: (Optional) Confirm Load Balancing

To confirm traffic is being load-balanced across replicas, access the service multiple times:
```bash
curl http://EXTERNAL-IP
```
Repeated responses may show different pod identifiers, confirming traffic distribution.

---

## ✅ Result
- 📦 Application successfully scaled to 2 replicas
- ☸️ All pods running and healthy
- 🌍 Service continues to respond on port 80

---

## ✅ Task 5: Confirm the Application Is Running

In this step, you will verify that the **echo-web** application is running correctly and responding to requests from outside the cluster using its external IP address on **:contentReference[oaicite:0]{index=0} Kubernetes Engine (GKE)**.

---

## 🌐 Step 1: Get the External IP Address

Run the following command to retrieve the service details:

```bash
kubectl get services
```

Look for the EXTERNAL-IP associated with the echo-web service.
> ⏳ If the external IP shows <pending>, wait a few moments and run the command again.

---

## 🔍 Step 2: Test the Application

Use a browser or curl to access the application:
```bash
curl http://EXTERNAL-IP
```

Or open the following in your web browser:
```cpp
http://EXTERNAL-IP
```

---

## 🧪 Expected Result
- The application responds successfully
- Output includes system information and the application version (v2)
- Confirms the service is reachable on port 80 and backed by running pods

---

## 🎉 Result
- 🌍 Application is accessible externally
- ⚙️ Service and pods are running correctly
- 📦 Updated version is successfully deployed and responding

---

## 🛠️ Troubleshooting: 504 Gateway Timeout Error

A **504 Gateway Timeout** error typically indicates that the LoadBalancer cannot reach the backend pods of your application. This can happen even if the pods are running, and is often related to **port misconfigurations** or initialization delays.

---

### 🔍 Possible Causes

1. **Application Initialization Delay**
   - The pods may not have finished starting yet
   - The LoadBalancer will return 504 until the backend is ready

2. **Port Mismatch**
   - The Dockerfile sets the container to listen on **TCP port 8000**
   - Possible mismatches include:
     - The **Kubernetes deployment container port** differs from 8000
     - The **service targetPort** does not map to container port 8000
     - External port configured incorrectly on the LoadBalancer or service

---

### 🛠️ Resolution Steps

1. **Check Pod Status**
```bash
kubectl get pods
kubectl describe pod <pod-name>
```

2. Verify Service Configuration
```bash
kubectl get svc echo-web -o yaml
```
- Ensure targetPort: 8000 matches the container port
- Ensure port: 80 is set for external access

3. Check Deployment Image
```bash
kubectl get deployment echo-web -o yaml | grep image
```

4. Test Pod Directly
```bash
kubectl port-forward pod/<pod-name> 8080:8000
curl http://localhost:8080
```
- Confirms container responds correctly on port 8000

5. Rollout Restart (Optional)
```bash
kubectl rollout restart deployment echo-web
kubectl rollout status deployment echo-web
```
- Restarts pods and ensures new configuration is applied

---

# 🎉 Task Completed

- 🐳 Deployed a **containerized application** (`echo-web`) to a Kubernetes cluster (`echo-cluster`)
- 🔄 Updated the application from **v1 to v2**
- 📈 Scaled the deployment to **2 replicas**
- 🌍 Verified external access to the application via LoadBalancer

---

## 🏁 Lab Summary

By completing this lab, you demonstrated key Kubernetes and container management skills:

- Building and tagging Docker images
- Pushing images to **Google Container Registry**
- Deploying applications to **GKE**
- Updating running deployments with new container versions
- Scaling deployments horizontally
- Troubleshooting common connectivity and port issues

## ✨ **Result:**  
Your application is now running **updated and scaled** on Kubernetes, fully accessible externally, and ready for production-style workloads 🚀
