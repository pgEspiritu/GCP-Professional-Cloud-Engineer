# 📘 Understanding and Combining GKE Autoscaling Strategies  

## 📝 Overview

Google Kubernetes Engine has horizontal and vertical solutions for automatically scaling your pods as well as your infrastructure. When it comes to cost-optimization, these tools become extremely useful in ensuring that your workloads are being run as efficiently as possible and that you're only paying for what you're using.

In this lab, will set up and observe Horizontal Pod Autoscaling and Vertical Pod Autoscaling for pod-level scaling and Cluster Autoscaler (horizontal infrastructure solution) and Node Auto Provisioning (vertical infrastructure solution) for node-level scaling. First you'll use these autoscaling tools to save as many resources as possible and shrink your cluster's size during a period of low demand. Then you will increase the demands of your cluster and observe how autoscaling maintains availability.

---

## 🎯 Objectives

In this lab, you will learn how to:

- 📉 Decrease number of replicas for a Deployment with Horizontal Pod Autoscaler  
- 🔧 Decrease CPU request of a Deployment with Vertical Pod Autoscaler  
- 🧹 Decrease number of nodes used in cluster with Cluster Autoscaler  
- 🤖 Automatically create an optimized node pool for workload with Node Auto Provisioning  
- 📈 Test the autoscaling behavior against a spike in demand  
- 📦 Overprovision your cluster with Pause Pods  

---

## 🛠️ Setup and Requirements

### Before you click the **Start Lab** button

- Read these instructions. Labs are timed and you cannot pause them.  
- The timer starts when you click **Start Lab**, which provisions your temporary Google Cloud credentials.

This hands-on lab lets you do the lab activities in a real cloud environment using temporary credentials.

To complete this lab, you need:

- 🌐 Access to a standard internet browser (Chrome recommended)  
- 🕵️ Use an **Incognito window** to avoid account conflicts  
- ⏳ Enough time — labs cannot be paused  
- 🎓 Use only the **student account** provided  

---

## 🚀 Start your lab and sign in

1. Click **Start Lab**  
2. Open Google Cloud console  
3. Sign in using the **Username** and **Password** provided  
4. Accept terms  
5. Do *not* set recovery options or 2FA  
6. Do *not* sign up for free trial  

After a few moments, the Google Cloud console opens.

---

## 💻 Activate Cloud Shell

Cloud Shell is a virtual machine loaded with dev tools and a persistent home directory.

1. Click **Activate Cloud Shell**  
2. Continue through prompts  
3. Authorize Cloud Shell  

You should see:
```text
Your Cloud Platform project in this session is set to "PROJECT_ID"
```


List active account:
```bash
gcloud auth list
```


List project ID:
```bash
gcloud config list project
```


---

## 🧪 Provision Testing Environment

Set your default zone:
```bash
gcloud config set compute/zone zone
```


Create a three-node cluster with Vertical Pod Autoscaling enabled:
```bash
gcloud container clusters create scaling-demo --num-nodes=3 --enable-vertical-pod-autoscaling
```


---

## 🐳 Deploy php-apache test workload

Create the deployment manifest:
```bash
cat << EOF > php-apache.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-apache
spec:
  selector:
    matchLabels:
      run: php-apache
  replicas: 3
  template:
    metadata:
      labels:
        run: php-apache
    spec:
      containers:
      - name: php-apache
        image: k8s.gcr.io/hpa-example
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
---
apiVersion: v1
kind: Service
metadata:
  name: php-apache
  labels:
    run: php-apache
spec:
  ports:
  - port: 80
  selector:
    run: php-apache
EOF
```


Apply the manifest:
```bash
kubectl apply -f php-apache.yaml
```

---

# 🧩 Task 1: Scale Pods with Horizontal Pod Autoscaling (HPA)

Horizontal Pod Autoscaling changes the shape of your Kubernetes workload by automatically increasing or decreasing the number of pods in response to the workload's CPU or memory consumption, or in response to custom metrics reported from within Kubernetes or external metrics from sources outside of your cluster.

---

## 🔍 Inspect your cluster's deployments

In Cloud Shell, run:
```bash
kubectl get deployment
```


You should see the `php-apache` deployment with **3/3 pods running**:
```nginx
NAME READY UP-TO-DATE AVAILABLE AGE
php-apache 3/3 3 3 91s
```


> 📝 **Note:**  
> If you don't see 3 available pods, wait a minute and try again.  
> If you see 1/1 pods, enough time has passed for your deployment to scale down.

---

## ⚙️ Apply Horizontal Pod Autoscaling

Run:
```bash
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

This command creates an HPA that maintains **1–10 replicas** of the `php-apache` pods.  
The `--cpu-percent=50` flag sets a **target average CPU usage of 50%** across all pods.

---

## 📊 Check HPA status

Run:
```bash
kubectl get hpa
```


You should see something like:
```ini
NAME REFERENCE TARGETS MINPODS MAXPODS REPLICAS AGE
php-apache Deployment/php-apache 1%/50% 1 10 3 1m
```


- `1%/50%` → current CPU usage vs target CPU  
- This is expected because the php-apache app currently has **no active traffic**  
- Replicas will start at **3**, but the autoscaler will reduce them based on low load

> 🕒 **Note:** If you see `<unknown>/50%`, wait 1 minute and run the command again.

HPA typically takes **5–10 minutes** to scale up/down depending on activity.

You will inspect the autoscaling results later in the lab.

---

## 💡 Additional Note

Although this lab uses `cpu-percent` as the metric, HPA supports **custom metrics**, allowing scaling based on logs, external metrics, or application-specific indicators.

---

# 🧩 Task 2: Scale Size of Pods with Vertical Pod Autoscaling (VPA)

Vertical Pod Autoscaling frees you from having to think about what CPU and memory request values to specify for a container.  
VPA can **recommend** resource values or **automatically update** them.

> ⚠️ **Important Note:**  
> VPA should **not** be used alongside HPA on **CPU or memory** because both systems attempt to respond to the same metrics and will conflict.  
> However, VPA on CPU/memory **can** be used with HPA on **custom metrics**.

VPA has already been enabled on the `scaling-demo` cluster.

---

## 🔍 Verify that VPA is enabled

Run:
```bash
gcloud container clusters describe scaling-demo | grep ^verticalPodAutoscaling -A 1
```


You should see:
```ini
enabled: true
```


> 📝 You can enable VPA on an existing cluster with:  
> `gcloud container clusters update scaling-demo --enable-vertical-pod-autoscaling`

---

## 🚀 Deploy `hello-server` for the VPA demonstration

Apply the deployment:
```bash
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```


Check deployment:
```bash
kubectl get deployment hello-server
```


---

## 🧩 Assign CPU resource requests

Set CPU request to **450m**:
```bash
kubectl set resources deployment hello-server --requests=cpu=450m
```


Inspect the container configuration:
```bash
kubectl describe pod hello-server | sed -n "/Containers:$/,/Conditions:/p"
```


Check:

- Under **Requests**, CPU should show **450m**

---

## 📝 Create a Vertical Pod Autoscaler manifest

Create the file:
```yaml
cat << EOF > hello-vpa.yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: hello-server-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       hello-server
  updatePolicy:
    updateMode: "Off"
EOF
```


### 🔧 VPA Update Policies

| Mode | Behavior |
|------|----------|
| **Off** | Only provides recommendations; manual apply |
| **Initial** | Applies recommendations only during pod creation |
| **Auto** | Continuously deletes/recreates pods to apply updated recommendations |

Apply the manifest:
```bash
kubectl apply -f hello-vpa.yaml
```


---

## 📊 Inspect VPA Recommendations

Wait 1 minute, then run:
```bash
kubectl describe vpa hello-server-vpa
```


Scroll to **Container Recommendations**.

You’ll see values for:

- **Lower Bound** — triggers scale down  
- **Target** — expected pod size  
- **Uncapped Target** — unrestricted target  
- **Upper Bound** — triggers scale up  

You’ll likely see that the recommended CPU is **25m**, lower than your assigned **450m**.

> 📝 **Note:**  
> VPA recommendations are based on historical usage.  
> Real-world best practice = wait **24 hours** before applying.

---

## 🔄 Change VPA update policy to Auto

To observe dynamic resizing:
```bash
sed -i 's/Off/Auto/g' hello-vpa.yaml
kubectl apply -f hello-vpa.yaml
```


---

## 📈 Scale `hello-server` to allow pod replacements

VPA will not delete the **last active pod** to avoid downtime.  
You need **at least 2 pods**.

Scale deployment:
```bash
kubectl scale deployment hello-server --replicas=2
```


---

## 👀 Watch for VPA pod replacements

Observe pod events:
```bash
kubectl get pods -w
```


Watch until you see pods entering **Terminating** or **Pending** state.

Example signs:
```bash
hello-server-abc123 Terminating
hello-server-def456 Pending
```


This indicates VPA is **deleting and recreating pods** with new resource sizes.

Press **Ctrl + C** once observed.

---

# 🧩 Task 3: HPA Results

By this point, your **Horizontal Pod Autoscaler (HPA)** has most likely scaled your `php-apache` deployment down.

---

## 🔍 Check HPA status

Run:
```bash
kubectl get hpa
```

Look at the **Replicas** column.  
You should now see that the `php-apache` deployment has been scaled down to **1 pod**.

> 🕒 **Note:** If you still see **3 replicas**, wait a few more minutes for the autoscaler to react.

---

## 📉 How HPA is optimizing resources

The HPA identifies that the application is currently idle and reduces pod count, freeing unused compute resources.

If traffic increases later, HPA will **scale the deployment back up** automatically to maintain performance and availability.

---

## 💡 Best Practice for Availability

If high availability is critical:

- Set a **minimum number of pods higher than 1**
- This ensures there is a buffer during scale-up time

---

## 💰 Cost Optimization Insight

A properly tuned HPA ensures:

- 🔄 High availability during demand spikes  
- 💸 Efficient cost usage during quiet periods  
- 📉 You only pay for the resources your application *actually needs*  

This makes HPA a powerful tool for **cost-efficient Kubernetes operations**.

---

# 🧩 Task 4: VPA Results

Now, the **Vertical Pod Autoscaler (VPA)** should have resized your pods in the `hello-server` deployment.

---

## 🔍 Inspect resized pods

Run:
```bash
kubectl describe pod hello-server | sed -n "/Containers:$/,/Conditions:/p"
```


Find the **Requests:** field.

You should now see something similar to:
```ini
Requests:
cpu: 25m
memory: 262144k
```


This means the VPA has recreated the pods with its **target utilization** recommendations:

- Lower CPU request (from **450m → 25m**)  
- Newly calculated memory request

---

## ⚠️ Note on delayed VPA updates

If you still see **450m CPU** on either pod, manually apply the CPU request:
```bash
kubectl set resources deployment hello-server --requests=cpu=25m
```


Sometimes VPA in *Auto* mode:

- Takes longer to act  
- Provides inaccurate bounds when not enough historical data exists  

To save time in the lab, applying VPA recommendations *manually* is acceptable—similar to using VPA in **Off** mode.

---

## 💡 Why VPA is valuable for optimization

Setting an initial CPU request of **450m** was more than the container required.  
By reducing it to **25m**, you:

- Free unnecessary CPU capacity  
- Reduce node pressure  
- Potentially reduce the total number of nodes needed  
- Lower compute costs  

---

## 🔄 Behavior of VPA in Auto mode

With **updateMode: Auto**, VPA will continuously:

- Delete & recreate pods  
- Adjust CPU/memory requests based on usage trends  
- Scale up to handle heavier traffic  
- Scale down during quiet periods  

This is excellent for gradual changes in demand but can:

- Introduce brief downtime during pod replacements  
- Reduce availability during sudden heavy load spikes

---

## 🛡️ Best Practice

For most production workloads:

- Use **updateMode: Off**  
- Manually apply recommendations  
- Maintain cluster availability  
- Still benefit from right-sizing resources

---

# 🧩 Task 5 — Cluster Autoscaler (GKE)

The Cluster Autoscaler (CA) automatically adds or removes nodes from your GKE node pool depending on cluster demand.
- ✔ High demand → more nodes
- ✔ Low demand → fewer nodes → lower cost

## ⚙️ 1. Enable Autoscaling on the Cluster
```bash
gcloud beta container clusters update scaling-demo \
  --enable-autoscaling --min-nodes 1 --max-nodes 5
```
This may take several minutes.

## 🧮 2. Choose an Autoscaling Profile

Profiles influence how aggressively the autoscaler removes nodes
| Profile                  | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| **Balanced** (default)   | Keeps some spare capacity.                                                    |
| **Optimize-utilization** | Aggressively removes nodes to increase utilization. Best for batch workloads. |

Switch to aggressive scaling:
```bash
gcloud beta container clusters update scaling-demo \
  --autoscaling-profile optimize-utilization
```

## 🖥️ 3. Observe Current Node Utilization

In GCP Console:
➡️ Kubernetes Engine → Clusters → scaling-demo → Nodes tab

Typical values example (your values may differ):
- CPU Requested: 1555m
- CPU Allocatable: 2820m
- Available CPU: ~1265m

This means the cluster can run on 2 nodes, but is currently using 3 nodes.

Why no scale-down yet?
👉 kube-system pods are spread across nodes and cannot be freely rescheduled.

## 🛡️ 4. Allow Rescheduling via Pod Disruption Budgets (PDBs)

PDBs tell Kubernetes how many pods can be temporarily disrupted.

Apply PDBs for key system pods:
```bash
kubectl create poddisruptionbudget kube-dns-pdb --namespace=kube-system --selector k8s-app=kube-dns --max-unavailable 1
kubectl create poddisruptionbudget prometheus-pdb --namespace=kube-system --selector k8s-app=prometheus-to-sd --max-unavailable 1
kubectl create poddisruptionbudget kube-proxy-pdb --namespace=kube-system --selector component=kube-proxy --max-unavailable 1
kubectl create poddisruptionbudget metrics-agent-pdb --namespace=kube-system --selector k8s-app=gke-metrics-agent --max-unavailable 1
kubectl create poddisruptionbudget metrics-server-pdb --namespace=kube-system --selector k8s-app=metrics-server --max-unavailable 1
kubectl create poddisruptionbudget fluentd-pdb --namespace=kube-system --selector k8s-app=fluentd-gke --max-unavailable 1
kubectl create poddisruptionbudget backend-pdb --namespace=kube-system --selector k8s-app=glbc --max-unavailable 1
kubectl create poddisruptionbudget kube-dns-autoscaler-pdb --namespace=kube-system --selector k8s-app=kube-dns-autoscaler --max-unavailable 1
kubectl create poddisruptionbudget stackdriver-pdb --namespace=kube-system --selector app=stackdriver-metadata-agent --max-unavailable 1
kubectl create poddisruptionbudget event-pdb --namespace=kube-system --selector k8s-app=event-exporter --max-unavailable 1
```
These make it safe for CA to reschedule and remove nodes.

## 🟢 5. Wait for Cluster to Scale Down

Within 1–2 minutes, your cluster should drop from 3 → 2 nodes.
Check repeatedly:
```bash
kubectl get nodes
```
Once you see 2 nodes, scaling has succeeded 🎉

## 📉 6. Cost & Resource Optimization Summary

By setting:
- Vertical Pod Autoscaler (VPA) → reduces pod CPU/memory requests
- Horizontal Pod Autoscaler (HPA) → manages replicas
- Cluster Autoscaler (CA) → removes unnecessary nodes

Your cluster:
- ✨ Packs workloads more efficiently
- ✨ Removes idle nodes
- ✨ Reduces cloud compute costs
- ✨ Still maintains high availability
This triple-combination is the foundation of cost-optimized Kubernetes.

---

# 🏗️ Task 6 — Node Auto Provisioning (NAP)

Node Auto Provisioning (NAP) automatically creates new node pools with machine types that best fit current workload demand.

This is different from Cluster Autoscaler:
| Feature                          | Behavior                                                         |
| -------------------------------- | ---------------------------------------------------------------- |
| **Cluster Autoscaler (CA)**      | Only adds/removes nodes *inside existing node pools*             |
| **Node Auto Provisioning (NAP)** | Creates brand new node pools with the “right-sized” machine type |
NAP makes the cluster smarter, not just bigger.

## ⚙️ 1. Enable Node Auto Provisioning

Run:
```bash
gcloud container clusters update scaling-demo \
    --enable-autoprovisioning \
    --min-cpu 1 \
    --min-memory 2 \
    --max-cpu 45 \
    --max-memory 160
```

🔍 What these flags mean:
| Flag                        | Purpose                                     |
| --------------------------- | ------------------------------------------- |
| `--enable-autoprovisioning` | Turns on NAP                                |
| `--min-cpu / max-cpu`       | Total vCPU allowed across NAP-created nodes |
| `--min-memory / max-memory` | Total RAM allowed across NAP-created nodes  |
These limits apply cluster-wide, not per node.

## 🧠 Understanding NAP Behavior
- ✨ NAP chooses machine types automatically (e2-medium, n2-standard, c2, etc.)
- ✨ NAP creates brand new node pools dynamically
- ✨ NAP considers your workload resource requests (CPU, memory)
- ✨ NAP will delete unused auto-provisioned node pools during low demand

Unlike CA alone, which can only scale existing pools, NAP can scale the shape of your cluster, not just its size.

## 🕒 2. Expect Delayed or No Action (For Now)

GKE may not create new node pools immediately.

Why?
Because your current cluster workload probably still fits inside your 2 remaining nodes.

🌱 NAP only triggers when resource requests exceed available capacity.
In the next tasks, you will deliberately increase workload demand so you can observe:
- Cluster Autoscaler adding nodes
- NAP creating new node pools
- VPA adjusting pod resource requests
- How GKE balances everything automatically

---

🧪 Task 7: Test with Larger Demand

So far, you've seen how HPA, VPA, and the Cluster Autoscaler help reduce costs during low demand.
Now let’s test how they behave during high demand.

## 🔥 Step 1 — Generate Heavy Load

Open a new Cloud Shell tab (press the ➕ icon).

Run an infinite load-generating loop:
```bash
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```
This will continuously hammer the php-apache service.

## 📈 Step 2 — Monitor HPA Under Load

Return to your original Cloud Shell tab and run:
```bash
kubectl get hpa
```

Re-run every few seconds until the CPU target rises above 100%.
This indicates the HPA will begin scaling up pods.

## 📦 Step 3 — Watch Deployment Scaling

While load is running, monitor your deployment:
```bash
kubectl get deployment php-apache
```
Or use the Nodes tab in Cloud Console to visualize scaling events.

## 🚀 What Will Happen

After a few minutes, you will observe:

✅ 1. HPA scales up php-apache pods
To handle the CPU pressure.

✅ 2. Cluster Autoscaler provisions new nodes
Because your node pool no longer has capacity for more pods.

✅ 3. Node Auto Provisioning creates a new node pool
NAP builds new node pools with optimized machine types.

In this scenario, expect:

- High-CPU, low-memory node types
(because your load generator stresses CPU heavily)

- Wait until:
  - php-apache reaches 7 replicas, and
  - Your Nodes tab shows additional nodes from autoscaling + NAP.

## 🛑 Step 4 — Stop the Load Generator

Return to the tab where the load test is running and press:
```mathematica
Ctrl + C
```
This stops the infinite loop.

Your cluster will gradually scale back down:
- HPA reduces pod replicas
- Cluster Autoscaler removes unneeded nodes
- NAP may also remove node pools if no longer required

## 🎯 Result

Your cluster successfully scaled up to meet higher demand!
However, note:
⏳ Scaling takes time.
For latency-sensitive production workloads, autoscaling delay can reduce availability during sudden spikes.

---

# ⚡ Task 8: Optimize Larger Loads

When scaling up for higher demand:
- Horizontal Pod Autoscaler (HPA) adds new pods.
- Vertical Pod Autoscaler (VPA) resizes pods based on your settings.
- If there’s room on an existing node, new pods can start immediately without pulling the image.
- If a new node is needed, Cluster Autoscaler provisions it, downloads images, and starts the pods.
- Node Auto Provisioning (NAP) may create a new node pool, adding more setup time.
> Tip: Smaller container images improve pod cold start times, reducing delays during autoscaling.

## 🛠 Step 1 — Plan Overprovisioning

Autoscaling latency can affect your app’s availability. To handle spikes efficiently, consider over-provisioning.

Use this formula to determine the safety buffer:
```formula
Safety Buffer = 1 + traffic growth / (1 − buffer)
```

Example:
- Buffer: 15%
- Estimated traffic growth: 30%

```mathematica
(1−0.15)/(1+0.3)=0.65
```
This gives a 65% overprovisioning to maintain availability without overspending.

## 🛠 Step 2 — Create Pause Pods

Pause Pods are low-priority pods that reserve cluster capacity. They are removed first when higher-priority workloads need resources.
Create a manifest: 
```yaml
cat << EOF > pause-pod.yaml
---
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: overprovisioning
value: -1
globalDefault: false
description: "Priority class used by overprovisioning."
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: overprovisioning
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      run: overprovisioning
  template:
    metadata:
      labels:
        run: overprovisioning
    spec:
      priorityClassName: overprovisioning
      containers:
      - name: reserve-resources
        image: k8s.gcr.io/pause
        resources:
          requests:
            cpu: 1
            memory: 4Gi
EOF
```

Apply it to your cluster:
```sh
kubectl apply -f pause-pod.yaml
```

## 📊 Step 3 — Observe Effects
1. Wait a minute.
2. Refresh the Nodes tab in your scaling-demo cluster.
3. Notice a new node, likely in a new node pool, created to host the pause pod.

When your real application needs extra resources:
- The pause pod will move to a different node.
- Your application pod can be scheduled immediately on the freed-up node.
> Best practice: No more than one pause pod per node to prevent wasted resources.


---

# In this lab, you successfully configured a Google Kubernetes Engine (GKE) cluster to automatically and efficiently scale based on demand.

You applied four key autoscaling tools:
- **Horizontal Pod Autoscaling (HPA)**: Automatically scales the number of pods in a deployment based on CPU/memory or custom metrics.
- **Vertical Pod Autoscaling (VPA)**: Automatically adjusts the CPU and memory requests of pods to match actual usage.
- **Cluster Autoscaler**: Adds or removes nodes in your cluster depending on pod scheduling needs.
- **Node Auto Provisioning (NAP)**: Automatically creates new node pools optimized for your workloads’ resource requirements.
	​
