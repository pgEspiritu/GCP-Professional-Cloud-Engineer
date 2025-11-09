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

