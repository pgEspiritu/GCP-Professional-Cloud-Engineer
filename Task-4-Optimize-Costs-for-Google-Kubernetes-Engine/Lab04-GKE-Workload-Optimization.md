# 🚀 GKE Workload Optimization — Lab Guide  
**Experiment • 1 hour 30 minutes • Intermediate**  
**Lab ID:** GSP769  

---

# 📘 Overview

One of the many benefits of using Google Cloud is its billing model that charges you only for the resources you use. With that in mind, it’s important to allocate reasonable resources for your apps and infrastructure—and optimize them continuously.

With Google Kubernetes Engine (GKE), you have many tools and strategies to reduce resource usage while improving application availability.

This lab walks you through concepts and tools that increase **resource efficiency**, **application reliability**, and **overall cost optimization**.

---

# 🎯 Objectives

In this lab, you will learn how to:

- Create a **container-native load balancer** using Ingress  
- Load test an application  
- Configure **liveness** and **readiness probes**  
- Create a **Pod Disruption Budget (PDB)**  

---

# 🧰 Setup and Requirements

## Before clicking **Start Lab**
- Read all instructions (lab timer cannot be paused).  
- You will be given **temporary Google Cloud credentials**.  
- Use only the provided student account (to avoid personal billing).  
- Use **Incognito Mode** to prevent browser conflicts.

This hands-on lab uses a real Google Cloud environment.

---

# ▶️ Start the Lab & Sign In

1. Click **Start Lab**.  
2. View the **Lab Details** panel (temporary Username & Password).  
3. Click **Open Google Cloud Console** (open in Incognito).  
4. When prompted:  
   - Select **Use Another Account**  
   - Enter the provided **Username**, then **Password**  
   - Accept terms  
   - Skip recovery options  
   - Do NOT sign up for free trials  

After login, the Google Cloud Console opens.

---

# 💻 Activate Cloud Shell

Cloud Shell provides a VM with tools pre-installed (including gcloud CLI).

1. Click **Activate Cloud Shell**.  
2. Approve the prompts.  
3. You're automatically authenticated with the lab project.

Verify active account:

```sh
gcloud auth list
```

Verify project:
```bash
gcloud config list project
```

# 🛠 Provision Lab Environment

Set your default compute zone:
```bash
gcloud config set compute/zone ZONE
```

Create a 3-node GKE cluster:
```bash
gcloud container clusters create test-cluster --num-nodes=3 --enable-ip-alias
```
> `--enable-ip-alias` enables alias IPs required for container-native load balancing via Ingress.

# 📦 Deploy the gb-frontend Application (Single Pod)

Create the Pod manifest:
```bash
cat << EOF > gb_frontend_pod.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: gb-frontend
  name: gb-frontend
spec:
  containers:
  - name: gb-frontend
    image: gcr.io/google-samples/gb-frontend-amd64:v5
    resources:
      requests:
        cpu: 100m
        memory: 256Mi
    ports:
    - containerPort: 80
EOF
```

Apply it:
```bash
kubectl apply -f gb_frontend_pod.yaml
```
> Note: Scoring for this task may take 1–2 minutes.

---

# 🧩 Task 1 — Container-Native Load Balancing Through Ingress

Container-native load balancing allows Google Cloud Load Balancers to target **Kubernetes Pods directly**, ensuring efficient and even traffic distribution.

---

## 📌 Why Container-Native Load Balancing?

Without container-native load balancing, incoming traffic flows like this:

1. Load balancer  
2. Node instance group  
3. iptables routing  
4. Pod (may be on a different node)

With container-native load balancing:

✔ Load balancer can route **directly to Pods**  
✔ Fewer network hops  
✔ Lower latency  
✔ Less network overhead  
✔ Even traffic distribution  
✔ App-level health checks

This requires your cluster to be **VPC-native**, which was enabled earlier using:
```bash
--enable-ip-alias
```


---

## 🛠 Create a ClusterIP Service (Enabled for NEGs)

This service allows GKE to create **Network Endpoint Groups (NEGs)** for pods.

Create `gb_frontend_cluster_ip.yaml`:

```yaml
cat << EOF > gb_frontend_cluster_ip.yaml
apiVersion: v1
kind: Service
metadata:
  name: gb-frontend-svc
  annotations:
    cloud.google.com/neg: '{"ingress": true}'
spec:
  type: ClusterIP
  selector:
    app: gb-frontend
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
EOF
```

Apply:
```bash
kubectl apply -f gb_frontend_cluster_ip.yaml
```

## 🌐 Create the Ingress Resource

Create gb_frontend_ingress.yaml:
```yaml
cat << EOF > gb_frontend_ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: gb-frontend-ingress
spec:
  defaultBackend:
    service:
      name: gb-frontend-svc
      port:
        number: 80
EOF
```

Apply:
```bash
kubectl apply -f gb_frontend_ingress.yaml
```

## 🏗 What Happens Next?

Creating the ingress automatically provisions:
- An HTTP(S) Load Balancer
- One NEG (Network Endpoint Group) per cluster zone
- A backend service that manages LB traffic distribution
- A health check for your Pods
- The ingress will be assigned an external IP after a few minutes.

## 🔍 Check Backend Service Health

First, get the backend service name:
```bash
BACKEND_SERVICE=$(gcloud compute backend-services list | grep NAME | cut -d ' ' -f2)
```

Check health:
```bash
gcloud compute backend-services get-health $BACKEND_SERVICE --global
```

Expected healthy status:
```yaml
backend: https://www.googleapis.com/compute/v1/projects/<project>/zones/<zone>/networkEndpointGroups/<neg-name>
status:
  healthStatus:
  - healthState: HEALTHY
    instance: https://www.googleapis.com/compute/v1/projects/<project>/zones/<zone>/instances/<instance>
    ipAddress: 10.8.0.6
    port: 80
```

> ⚠️ Note:
> GCLB health checks are not the same as Kubernetes readiness/liveness probes.
> They operate at the load balancer level and use special Google-controlled health check IPs.


## 🌍 Access the Application

Retrieve the ingress external IP:
```bash
kubectl get ingress gb-frontend-ingress
```
Open the external IP in your browser to view the application.

---

# 🧪 Task 2 — Load Testing an Application

Understanding your application’s capacity is essential for choosing the right **resource requests/limits** and designing a smart **autoscaling strategy**.

To measure how much load your **single pod** can handle, you will use **Locust**, an open-source load-testing framework.

---

## 📥 Step 1 — Download Locust Image Files

```sh
gsutil -m cp -r gs://spls/gsp769/locust-image .
```
The locust-image directory contains Locust configuration files.

## 🏗 Step 2 — Build & Push the Locust Docker Image

Build and store Locust in Container Registry:
```bash
gcloud builds submit \
    --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/locust-tasks:latest locust-image
```

Verify it exists:
```bash
gcloud container images list
```

Expected output example:
```ini
Only listing images in gcr.io/qwiklabs-gcp-01-343cd312530e.
```
