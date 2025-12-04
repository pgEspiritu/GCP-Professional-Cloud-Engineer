# 📘 Understanding and Combining GKE Autoscaling Strategies  
### 🧪 Lab • GSP768  
⏱️ *1 hour 30 minutes* • 🎯 *Intermediate* • 💳 *5 Credits*

---

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

In the next sections, you will explore additional ways to optimize your resource usage using:

- **Cluster Autoscaler (CA)**
- **Node Auto Provisioning (NAP)**

Stay tuned! 🚀


