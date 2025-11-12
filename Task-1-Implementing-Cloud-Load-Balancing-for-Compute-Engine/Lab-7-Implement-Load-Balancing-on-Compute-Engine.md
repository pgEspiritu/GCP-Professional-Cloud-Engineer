# 🌐 Implement Load Balancing on Compute Engine: Challenge Lab  

## 🧾 Overview

In this challenge lab, you act as a **junior cloud engineer** assigned to provide network functionality to Compute Engine virtual machines (VMs) on a Google Cloud Virtual Private Cloud (VPC) network.

You’ll apply what you learned from earlier labs to:
- 🖥 Create multiple web server instances  
- ⚙️ Configure a **Network Load Balancer (NLB)**  
- 🌍 Create and test an **HTTP Load Balancer (L7)**  

You must build everything manually and verify results by sending traffic to your load balancers.

---

## ⚙️ Setup and Requirements

Before starting:
- Use **us-central1** region and **us-central1-f** zone unless stated otherwise.  
- Use the **default VPC network** for all instances.  
- Use the **provided student account** (no personal account).  

---

## 🧩 Challenge Scenario

You’ll create 3 VM instances, configure firewall rules, set up a **Network Load Balancer**, and finally deploy an **HTTP Load Balancer** to distribute traffic between instances.

---

## 🚀 Task 1. Create Multiple Web Server Instances

### 🎯 Objective
Create 3 Compute Engine VM instances, install Apache on each, and configure a firewall to allow HTTP traffic.

### 🧠 Configuration

| Property | Value |
|-----------|--------|
| VM Instance - 1 | web1 |
| VM Instance - 2 | web2 |
| VM Instance - 3 | web3 |
| Region | us-central1 |
| Zone | us-central1-f |
| Series | E2 |
| Machine type | e2-small |
| Tags | network-lb-tag |
| Image family | debian-12 |
| Image project | debian-cloud |

---

### 🧰 Commands

```bash
# Create web1
gcloud compute instances create web1 \
  --zone=us-central1-f \
  --machine-type=e2-small \
  --tags=network-lb-tag \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update
apt-get install apache2 -y
service apache2 restart
echo "<h3>Web Server: web1</h3>" | tee /var/www/html/index.html'

# Create web2
gcloud compute instances create web2 \
  --zone=us-central1-f \
  --machine-type=e2-small \
  --tags=network-lb-tag \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update
apt-get install apache2 -y
service apache2 restart
echo "<h3>Web Server: web2</h3>" | tee /var/www/html/index.html'

# Create web3
gcloud compute instances create web3 \
  --zone=us-central1-f \
  --machine-type=e2-small \
  --tags=network-lb-tag \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update
apt-get install apache2 -y
service apache2 restart
echo "<h3>Web Server: web3</h3>" | tee /var/www/html/index.html'
```

🔥 Allow HTTP Traffic

```bash
gcloud compute firewall-rules create www-firewall-network-lb \
  --target-tags=network-lb-Expected output:ag \
  --allow=tcp:80
```

✅ Verification

```bash
gcloud compute instances list
```

Expected output:
```nginx
NAME   ZONE            MACHINE_TYPE  INTERNAL_IP  EXTERNAL_IP     STATUS
web1   us-central1-f   e2-small      10.128.0.2   34.136.xxx.xxx  RUNNING
web2   us-central1-f   e2-small      10.128.0.3   34.136.xxx.xxx  RUNNING
web3   us-central1-f   e2-small      10.128.0.4   34.136.xxx.xxx  RUNNING
```

Check Apache server response:
```bash
curl http://[EXTERNAL_IP_OF_web1]
curl http://[EXTERNAL_IP_OF_web2]
curl http://[EXTERNAL_IP_OF_web3]
```

Output:
```bash
<h3>Web Server: web1</h3>
<h3>Web Server: web2</h3>
<h3>Web Server: web3</h3>
```

---

### 🌉 Task 2. Configure the Load Balancing Service (Network Load Balancer)

🎯 Objective
Create a Network Load Balancer to distribute traffic among the three instances.

1️⃣ Create an HTTP Health Check named basic-check
```bash
gcloud compute http-health-checks create basic-check \
  --port 80
```

- This creates a global HTTP health check that target pools can use.
- The health check will verify that Apache is responding on port 80.

2️⃣ Create the Target Pool with the HTTP Health Check
```bash
gcloud compute target-pools create www-pool \
  --region=us-central1 \
  --http-health-check basic-check
```

3️⃣ Add your instances to the Target Pool
```bash
gcloud compute target-pools add-instances www-pool \
  --instances web1,web2,web3 \
  --region=us-central1
```

4️⃣ Verify Health Status
```bash
gcloud compute target-pools get-health www-pool --region=us-central1
```
- After 1–2 minutes, you should see:
```bash
instance: web1
healthState: HEALTHY
instance: web2
healthState: HEALTHY
instance: web3
healthState: HEALTHY
```

5️⃣ Create the forwarding rule
```bash
gcloud compute forwarding-rules create www-rule \
  --region=us-central1 \
  --ports=80 \
  --address=network-lb-ip-1 \
  --target-pool=www-pool
```

---

### ☁️ Task 3. Create an HTTP Load Balancer

🎯 Objective
Deploy a Layer 7 (HTTP) Load Balancer with a backend managed instance group and health checks.

🧱 1. Create Instance Template
```bash
gcloud compute instance-templates create lb-backend-template \
  --region=us-central1 \
  --network=default \
  --machine-type=e2-medium \
  --tags=allow-health-check \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
apt-get update
apt-get install apache2 -y
service apache2 restart
echo "<h3>Page served from: $(hostname)</h3>" | tee /var/www/html/index.html'
```

🧩 2. Create Managed Instance Group
```bash
gcloud compute instance-groups managed create lb-backend-group \
  --base-instance-name=lb-backend \
  --size=2 \
  --template=lb-backend-template \
  --zone=us-central1-f
```

🔒 3. Create Firewall for Health 
```bash
gcloud compute firewall-rules create fw-allow-health-check \
  --network=default \
  --action=ALLOW \
  --direction=INGRESS \
  --source-ranges=130.211.0.0/22,35.191.0.0/16 \
  --target-tags=allow-health-check \
  --rules=tcp:80
```

❤️ 4. Create Health Check
```bash
gcloud compute health-checks create http http-basic-check --port 80
```

🔙 5. Create Backend Service
```bash
gcloud compute backend-services create web-backend-service \
  --protocol=HTTP \
  --health-checks=http-basic-check \
  --global
```

Attach the instance group:
```bash
gcloud compute backend-services add-backend web-backend-service \
  --instance-group=lb-backend-group \
  --instance-group-zone=us-central1-f \
  --global
```

🗺️ 6. Create URL Map and Proxy
```bash
gcloud compute url-maps create web-map-http \
  --default-service web-backend-service

gcloud compute target-http-proxies create http-lb-proxy \
  --url-map web-map-http
```

📡 7. Create Forwarding Rule
```bash
gcloud compute addresses create lb-ipv4-1 --ip-version=IPV4 --global

gcloud compute forwarding-rules create http-content-rule \
  --address=lb-ipv4-1 \
  --global \
  --target-http-proxy=http-lb-proxy \
  --ports=80
```

✅ Verification

Get load balancer IP:
```bash
gcloud compute forwarding-rules describe http-content-rule --global
```

Output:
```bash
IPAddress: 34.149.xxx.xxx
```

Test:
```bash
curl http://34.149.xxx.xxx/
```

Output:
```csharp
<h3>Page served from: lb-backend-group-xxxx</h3>
```

---

## Task Completed
- ✅ Created a VPC and 3 Compute Engine instances
- ✅ Configured a Network Load Balancer
- ✅ Deployed and tested an HTTP Load Balancer
