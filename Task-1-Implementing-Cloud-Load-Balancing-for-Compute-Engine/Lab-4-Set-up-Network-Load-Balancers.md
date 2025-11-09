# 4. Set Up Network Load Balancers

## Overview
In this lab, you will set up a **passthrough Network Load Balancer (NLB)** using Compute Engine VMs.  
An L4 NLB handles traffic by IP/port without inspecting packet content.

**You will:**
- Configure region/zone
- Create web servers
- Configure load balancing
- Create forwarding rule to distribute traffic

---

## Setup
- Use the **temporary student account** provided.  
- Open the **Google Cloud Console → Activate Cloud Shell**.  
- Confirm active account and project:
```bash
gcloud auth list
gcloud config list project
```

![Lab 4 - Set Up Network Load Balancers.1](images/Lab-4.1.png)
![Lab 4 - Set Up Network Load Balancers.2](images/Lab-4.2.png)

---

### Task 1. Set Default Region and Zone
```bash
gcloud config set compute/region REGION
gcloud config set compute/zone ZONE
```

![Lab 4 - Set Up Network Load Balancers.3](images/Lab-4.3.png)

---

### Task 2. Create Multiple Web Servers

1. Create 3 instances with Apache installed.
a. www1
  ```bash
  gcloud compute instances create www1 \
  --zone=ZONE --tags=network-lb-tag --machine-type=e2-small \
  --image-family=debian-11 --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
    apt-get update
    apt-get install apache2 -y
    service apache2 restart
    echo "<h3>Web Server: www1</h3>" > /var/www/html/index.html'
  ```

![Lab 4 - Set Up Network Load Balancers.4](images/Lab-4.4.png)

b. www2
  ```bash
  gcloud compute instances create www2 \
  --zone=ZONE --tags=network-lb-tag --machine-type=e2-small \
  --image-family=debian-11 --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
    apt-get update
    apt-get install apache2 -y
    service apache2 restart
    echo "<h3>Web Server: www2</h3>" > /var/www/html/index.html'
  ```

![Lab 4 - Set Up Network Load Balancers.5](images/Lab-4.5.png)

c. ww3
  ```bash
  gcloud compute instances create www3 \
  --zone=ZONE --tags=network-lb-tag --machine-type=e2-small \
  --image-family=debian-11 --image-project=debian-cloud \
  --metadata=startup-script='#!/bin/bash
    apt-get update
    apt-get install apache2 -y
    service apache2 restart
    echo "<h3>Web Server: www3</h3>" > /var/www/html/index.html'
  ```

![Lab 4 - Set Up Network Load Balancers.6](images/Lab-4.6.png)

Allow HTTP traffic
```bash
gcloud compute firewall-rules create www-firewall-network-lb \
  --target-tags network-lb-tag --allow tcp:80
```

![Lab 4 - Set Up Network Load Balancers.7](images/Lab-4.7.png)

Verify instances
```bash
gcloud compute instances list
curl http://[EXTERNAL_IP]
```

![Lab 4 - Set Up Network Load Balancers.8](images/Lab-4.8.png)
![Lab 4 - Set Up Network Load Balancers.9](images/Lab-4.9.png)

---

### Task 3. Configure Load Balancer

Create static IP
```bash
gcloud compute addresses create network-lb-ip-1 --region REGION
```

Create health check
```bash
gcloud compute http-health-checks create basic-check
```

---

### Task 4. Create Target Pool and Forwarding Rule

Create target pool
```bash
gcloud compute target-pools create www-pool \
  --region REGION --http-health-check basic-check
```

Add instances
```bash
gcloud compute target-pools add-instances www-pool \
  --instances www1,www2,www3
```

Create forwarding rule
```bash
gcloud compute forwarding-rules create www-rule \
  --region REGION --ports 80 \
  --address network-lb-ip-1 \
  --target-pool www-pool
```

![Lab 4 - Set Up Network Load Balancers.10](images/Lab-4.10.png)

---

### Task 5. Test Load Balancer

Get external IP
```bash
gcloud compute forwarding-rules describe www-rule --region REGION
```

Save and show IP
```bash
IPADDRESS=$(gcloud compute forwarding-rules describe www-rule --region REGION --format="json" | jq -r .IPAddress)
echo $IPADDRESS
```

Send traffic
```bash
while true; do curl -m1 $IPADDRESS; done
```
- The responses alternate among `www1`, `www2`, and `www3`.
- Press Ctrl + C to stop.

![Lab 4 - Set Up Network Load Balancers.11](images/Lab-4.11.png)
![Lab 4 - Set Up Network Load Balancers.12](images/Lab-4.12.png)

---

## Task Completed
- Created 3 Apache web servers
- Configured a Network Load Balancer
- Verified load distribution using a forwarding rule
