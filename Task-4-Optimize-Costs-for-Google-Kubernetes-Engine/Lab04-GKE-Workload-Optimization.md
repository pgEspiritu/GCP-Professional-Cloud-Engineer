# 🚀 GKE Workload Optimization — Lab Guide  


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


## 🚀 Step 3 — Deploy Locust (Main + Workers)

Download and apply the manifest:
```bash
gsutil cp gs://spls/gsp769/locust_deploy_v2.yaml .
sed 's/${GOOGLE_CLOUD_PROJECT}/'$GOOGLE_CLOUD_PROJECT'/g' locust_deploy_v2.yaml | kubectl apply -f -
```

This creates:
- 1 Locust main pod
- 5 Locust worker pods

## 🌐 Step 4 — Access the Locust UI

Get the external IP of the Locust LoadBalancer:
```bash
kubectl get service locust-main
```
If <pending>, wait a moment and rerun.

Open the UI in a browser:
```bash
http://EXTERNAL_IP:8089
```
You will see the Locust dashboard.

## 🏋️ Step 5 — Load Test (Baseline)

In Locust UI, enter:
- Number of users: 200
- Spawn rate: 20
Click Start swarming.

After a few seconds you should see:
- Status: Running with 200 users
- Approx. 150 RPS (Requests Per Second)

## 📊 Step 6 — Observe Pod Resource Usage

In Cloud Console:
1. Go to Kubernetes Engine → Workloads
2. Click your gb-frontend pod
3. View the CPU & Memory graphs
> 👉 Tip: Click the three dots on the chart → Expand chart legend

Expected behavior:
- CPU: 0.04 to 0.06 cores (40–60% of request)
- Memory: Around 80Mi, well below the 256Mi request
This is your per-pod capacity under typical load.

## ⚡ Step 7 — Test a Sudden Spike (Burst Load)

In Locust, click Edit:
- Users: 900
- Spawn rate: 300
Click Start swarming

This generates 700 new users in 2–3 seconds.

Observe the pod metrics again in the Console.

Expected behavior:
- CPU spikes to ~0.07 cores (70% of request)
- Memory remains around 80Mi

This indicates your application:
- Is more CPU-bound than memory-bound
- Can likely reduce memory requests
- Would benefit from HPA based on CPU
- Could use Cluster Autoscaler to scale nodes as demand rises

---

# 🩺 Task 3 — Readiness and Liveness Probes

Kubernetes provides **liveness probes** and **readiness probes** to evaluate the health and availability of a containerized application.  
These checks help Kubernetes automatically restart unhealthy containers and ensure that only fully-ready pods receive traffic.

---

# 🔁 Part 1 — Liveness Probe

A **liveness probe** continuously checks whether a container is still functioning correctly.  
If the probe fails, Kubernetes **restarts the container automatically**.

In this example, we create a pod whose liveness probe checks for the existence of a file `/tmp/alive`.

## 📄 Create the liveness probe manifest

```sh
cat << EOF > liveness-demo.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    demo: liveness-probe
  name: liveness-demo-pod
spec:
  containers:
  - name: liveness-demo-pod
    image: quay.io/centos/centos:stream9
    args:
    - /bin/sh
    - -c
    - touch /tmp/alive; sleep infinity
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/alive
      initialDelaySeconds: 5
      periodSeconds: 10
EOF
```

Apply the manifest:
```bash
kubectl apply -f liveness-demo.yaml
```

📝 Notes
- initialDelaySeconds — how long to wait before the first probe
- periodSeconds — how often the probe runs
- A startupProbe can optionally be used to avoid premature failures for slow-starting apps

## 🔍 Verify liveness probe behavior

Check pod events:
```bash
kubectl describe pod liveness-demo-pod
```
You will initially see only creation and startup events.

❌ Trigger a liveness probe failure
Delete the file being checked:
```bash
kubectl exec liveness-demo-pod -- rm /tmp/alive
```

Check events again:
```bash
kubectl describe pod liveness-demo-pod
```

You should now see:
- Liveness probe failures
- Kubernetes killing and restarting the container
- New pull/restart events

Example:
```bash
Warning  Unhealthy   Liveness probe failed: cat: /tmp/alive: No such file or directory
Normal   Killing     Container failed liveness probe, will be restarted
```

🧠 Other types of liveness probes
- HTTP probe — checks for valid HTTP responses
- TCP probe — checks if a TCP connection can be established

🟢 Part 2 — Readiness Probe

A readiness probe determines whether a container is ready to receive traffic.
If the probe fails, the pod remains running but is removed from service endpoints.

In this example, the probe waits for a file /tmp/healthz before signaling readiness.

## 📄 Create the readiness probe + LoadBalancer Service
```bash
cat << EOF > readiness-demo.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    demo: readiness-probe
  name: readiness-demo-pod
spec:
  containers:
  - name: readiness-demo-pod
    image: nginx
    ports:
    - containerPort: 80
    readinessProbe:
      exec:
        command:
        - cat
        - /tmp/healthz
      initialDelaySeconds: 5
      periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: readiness-demo-svc
  labels:
    demo: readiness-probe
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  selector:
    demo: readiness-probe
EOF
```

Apply:
```bash
kubectl apply -f readiness-demo.yaml
```

## 🌐 Test the service

Retrieve the external IP:
```bash
kubectl get service readiness-demo-svc
```
Open the IP in a browser — it will fail because the readiness probe is failing.

## 🔍 Check readiness probe status
```bash
kubectl describe pod readiness-demo-pod
```

You will see repeated failures:
```ini
Warning  Unhealthy  Readiness probe failed: cat: /tmp/healthz: No such file or directory
```
Readiness probe failures do not restart the pod.

## ✔ Make the pod “Ready”

Create the expected file:
```bash
kubectl exec readiness-demo-pod -- touch /tmp/healthz
```

Check readiness conditions:
```bash
kubectl describe pod readiness-demo-pod | grep ^Conditions -A 5
```

You should now see:
```bash
Ready   True
ContainersReady True
```
Refresh the browser — you should now see the Welcome to nginx! page.

## 🧠 Why readiness probes matter

Readiness probes:
- Prevent traffic from reaching unready pods
- Ensure load balancers only send requests to healthy backends
- Protect applications that require warm-up, cache loading, DB connections, etc.

A good readiness probe example:
- Check database connectivity
- Check cache initialization
- Validate important config files

---

# 🛡️ Task 4 — Pod Disruption Budgets (PDBs)

Pod Disruption Budgets help protect your application's availability during voluntary disruptions, such as:
- Node draining
- Deployment updates
- Admin actions (deleting/rolling updates)
- Moving pods between nodes
PDBs ensure that a minimum number of pods stay running so your app remains accessible.

## ✅ Step 1: Replace Single Pod With Deployment

Delete your single pod:
```bash
kubectl delete pod gb-frontend
```

Create deployment of 5 replicas:
```bash
kubectl apply -f gb_frontend_deployment.yaml
```

## 🔄 Step 2: Drain Nodes Without a PDB (App Will Go Down)

Drain all nodes in the default node pool:
```bash
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=default-pool -o=name); do
  kubectl drain --force --ignore-daemonsets --grace-period=10 "$node";
done
```

Check deployment status:
```bash
kubectl describe deployment gb-frontend | grep ^Replicas
```

Expected result without PDB:
```ini
5 desired | 5 updated | 5 total | 0 available | 5 unavailable
```
➡️ Application is DOWN because Kubernetes freely evicted all pods.

## 🔧 Step 3: Uncordon Nodes

Bring nodes back:
```bash
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=default-pool -o=name); do
  kubectl uncordon "$node";
done
```

Confirm all pods recover:
```bash
kubectl describe deployment gb-frontend | grep ^Replicas
```

Expected:
```ini
5 desired | 5 updated | 5 total | 5 available | 0 unavailable
```

## 🛡️ Step 4: Create a Pod Disruption Budget

Require minimum 4 pods available:
```bash
kubectl create poddisruptionbudget gb-pdb --selector run=gb-frontend --min-available 4
```

## 🔄 Step 5: Drain Nodes Again (Now PDB Protects Availability)
```bash
for node in $(kubectl get nodes -l cloud.google.com/gke-nodepool=default-pool -o=name); do
  kubectl drain --timeout=30s --ignore-daemonsets --grace-period=10 "$node";
done
```

You’ll see Kubernetes evict only one pod, then refuse to evict more:
```vbnet
error when evicting pod ... Cannot evict pod as it would violate the pod's disruption budget.
```
Press CTRL+C to stop the loop.

Check deployment status:
```bash
kubectl describe deployment gb-frontend | grep ^Replicas
```
➡️ Because min-available is 4, Kubernetes stops evicting pods to protect availability.

---

# Task Completed

You successfully created a container-native load balancer using Ingress, enabling more efficient and intelligent load balancing for your GKE applications.

Throughout this lab you also:
- 🔍 Ran a load test and observed baseline CPU and memory usage
- 📈 Monitored how the application responds to traffic spikes
- ❤️ Configured liveness and readiness probes to ensure the app behaves reliably
- 🛡️ Implemented a Pod Disruption Budget (PDB) to protect availability during voluntary disruptions

By combining these tools and techniques, you’ve improved the efficiency, resiliency, and cost-effectiveness of applications running on GKE. You now understand how to minimize unnecessary network traffic, define meaningful health indicators, and maintain high availability—even during updates or disruptions.
