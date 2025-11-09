# 🌐 Set Up Application Load Balancers

## 🧭 Overview  
In this lab, you create an HTTP Load Balancer and configure Cloud Armor security policies to protect your web application.  

You'll:  
- Deploy multiple web servers on Compute Engine  
- Configure a global HTTP Load Balancer  
- Add a Cloud Armor policy  
- Test your setup  

---

## ⚙️ Setup and Requirements  

### 🧑‍💻 Before You Begin  
- Use an **Incognito** or **Private** window.  
- Use the **student credentials** provided by the lab — not your personal Google account.  
- Once started, the lab timer cannot be paused.  

### 🚀 Start the Lab  
1. Click **Start Lab**.  
2. Copy the provided **Username** and **Password**.  
3. Click **Open Google Cloud Console** → open in **Incognito window** if using Chrome.  
4. Sign in using the credentials provided.  
5. Accept terms, skip recovery options, and do **not** enable 2FA or trials.  

---

## 💻 Activate Cloud Shell  
1. Click **Activate Cloud Shell** at the top of the console.  
2. Click **Continue** and **Authorize**.  
3. Confirm the project ID is set:  
```bash
Your Cloud Platform project in this session is set to "PROJECT_ID"
```

4. (Optional) Verify authentication and project:
```bash
gcloud auth list
gcloud config list project
```

---

### 🏗️ Task 1: Create Web Server Instances

1. In Cloud Shell, create startup script:
```bash
cat << EOF > startup.sh
#!/bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
echo "<h1>Web Server: $(hostname)</h1>" | tee /var/www/html/index.html
EOF
```

2. Create 2 VM instances in the same region:
```bash
gcloud compute instances create web-1 \
  --zone=ZONE \
  --tags=webserver \
  --metadata-from-file startup-script=startup.sh

gcloud compute instances create web-2 \
  --zone=ZONE \
  --tags=webserver \
  --metadata-from-file startup-script=startup.sh
```

3. Confirm instances are running:
```bash
gcloud compute instances list
```

---

### 🧱 Task 2: Create a Firewall Rule

1. Allow traffic to the web servers:
```bash
gcloud compute firewall-rules create allow-http \
  --direction=INGRESS \
  --priority=1000 \
  --network=default \
  --action=ALLOW \
  --rules=tcp:80 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=webserver
```
> ✅ Task Check: Firewall rule successfully created.

---

### ⚖️ Task 3: Configure HTTP Load Balancer

1. Create a health check:
```bash
gcloud compute health-checks create http http-basic-check \
  --port 80
```

2. Create a backend service:
```bash
gcloud compute backend-services create web-backend \
  --protocol HTTP \
  --port-name http \
  --health-checks http-basic-check \
  --global
```

3. Add instance group:
```bash
gcloud compute instance-groups unmanaged create web-group --zone=ZONE
gcloud compute instance-groups unmanaged add-instances web-group \
  --instances=web-1,web-2 \
  --zone=ZONE
gcloud compute backend-services add-backend web-backend \
  --instance-group web-group \
  --instance-group-zone=ZONE \
  --global
```

4. Create a URL map:
```bash
gcloud compute url-maps create web-map \
  --default-service web-backend
```

5. Create a target HTTP proxy:
```bash
gcloud compute target-http-proxies create http-lb-proxy \
  --url-map web-map
```

6. Create a forwarding rule:
```bash
gcloud compute forwarding-rules create http-forwarding-rule \
  --global \
  --target-http-proxy=http-lb-proxy \
  --ports=80
```

> ✅ Task Check: Load Balancer created successfully.

---

### 🛡️ Task 4: Configure Cloud Armor Policy

1. Create a policy:
```bash
gcloud compute security-policies create web-policy \
  --description "Block unwanted traffic"
```

2. Add a rule to deny traffic from a specific IP (example):
```bash
gcloud compute security-policies rules create 1000 \
  --security-policy web-policy \
  --expression "inIpRange(origin.ip, '192.0.2.0/24')" \
  --action deny-403 \
  --description "Block subnet 192.0.2.0/24"
```

3. Attach policy to backend:
```bash
gcloud compute backend-services update web-backend \
  --security-policy web-policy \
  --global
```

> ✅ Task Check: Cloud Armor applied successfully.

---

### 🔍 Task 5: Test Load Balancer

1. Get the external IP:
```bash
gcloud compute forwarding-rules list
```

2. Visit http://EXTERNAL_IP in a browser — it should display one of the server names.

3. Refresh to see load balancing between instances.

---

## Task Completed
- ✅ Created web server
- ✅ Configured a global HTTP Load Balancer
- ✅ Added a Cloud Armor policy
- ✅ Tested your deployment
