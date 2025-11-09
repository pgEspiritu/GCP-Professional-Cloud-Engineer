# ☁️ Getting Started with Cloud Shell and gcloud

## 🌐 Overview

Cloud Shell provides command-line access to Google Cloud resources.  
It’s a **Debian VM** with a **persistent 5GB home directory** and pre-installed **gcloud** tools.  

In this lab, you will:
- Practice gcloud commands  
- Connect to compute services  

---

## ⚙️ Setup and Requirements

### Before You Start
- Use **Incognito/Private** window  
- Use **temporary student account** only  
- Lab **cannot be paused**  

### Starting the Lab
1. Click **Start Lab → Open Google Cloud Console**  
2. Sign in with the provided **Username** & **Password**  
3. Accept terms, skip recovery options, and avoid free trials  

---

## 🧰 Activate Cloud Shell

1. Click **Activate Cloud Shell** (💻 icon)  
2. Continue and **Authorize** credentials  

**Check Project ID:**
```bash
gcloud config list project
```

Output:
```ini
[core]
project = "PROJECT_ID"
```

Optional Commands:
```bash
gcloud auth list
```

Output:
```vbnet
ACTIVE: *
ACCOUNT: "ACCOUNT"
```

```bash
gcloud config set account `ACCOUNT`
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.1](images/Lab-3.1.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.2](images/Lab-3.2.png)

---

### 🏗️ Task 1: Configuring Your Environment
1. Set Region & Zone
```bash
gcloud config set compute/region REGION
gcloud config set compute/zone ZONE
```

2.  Check settings
```bash
gcloud config get-value compute/region
gcloud config get-value compute/zone
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.3](images/Lab-3.3.png)

3. Project Information:
```bash
gcloud config get-value project
gcloud compute project-info describe --project $(gcloud config get-value project)
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.4](images/Lab-3.4.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.5](images/Lab-3.5.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.6](images/Lab-3.6.png)

4. Environment Variables
```bash
export PROJECT_ID=$(gcloud config get-value project)
export ZONE=$(gcloud config get-value compute/zone)
echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.7](images/Lab-3.7.png)

---

### 🖥️ Task 2: Creating a VM
```bash
gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE
```

Output Example:
```nginx
NAME     ZONE   MACHINE_TYPE  INTERNAL_IP  EXTERNAL_IP   STATUS
gcelab2  ZONE   e2-medium     10.128.0.2   34.67.152.90 RUNNING
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.8](images/Lab-3.8.png)

Help & Info:
```bash
gcloud compute instances create --help
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.9](images/Lab-3.9.png)

```bash
gcloud -h
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.10](images/Lab-3.10.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.11](images/Lab-3.11.png)

```bash
gcloud config --help
```

```bash
gcloud help config
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.12](images/Lab-3.12.png)

```bash  
gcloud config list
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.13](images/Lab-3.13.png)

```bash
gcloud config list --all
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.14](images/Lab-3.14.png)

```bash
gcloud components list
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.15](images/Lab-3.15.png)

---

### 🔍 Task 3: Filtering Output
1. List all instances:
```bash
gcloud compute instances list
```

2. Filter by name:
```bash
gcloud compute instances list --filter="name=('gcelab2')"
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.16](images/Lab-3.16.png)

3. List firewall rules:
```bash
gcloud compute firewall-rules list
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.17](images/Lab-3.17.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.18](images/Lab-3.18.png)

```bash
gcloud compute firewall-rules list --filter="network='default'"
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.19](images/Lab-3.19.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.20](images/Lab-3.20.png)

```bash
gcloud compute firewall-rules list --filter="NETWORK:'default' AND ALLOW:'icmp'"
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.21](images/Lab-3.21.png)

---

### 🔐 Task 4: Connect to VM & Install Nginx
SSH to VM:
```bash
gcloud compute ssh gcelab2 --zone $ZONE
```

Follow prompts to generate SSH keys.

![Lab 3 - Getting Started with Cloud Shell and gcloud.22](images/Lab-3.22.png)
![Lab 3 - Getting Started with Cloud Shell and gcloud.23](images/Lab-3.23.png)

Install nginx:
```bash
sudo apt update && sudo apt install -y nginx
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.24](images/Lab-3.24.png)


Exit SSH:
```bash
exit
```

---

### 🔥 Task 5: Update Firewall

Add VM tags:
```bash
gcloud compute instances add-tags gcelab2 --tags http-server,https-server
```

Allow HTTP traffic:
```bash
gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
```

Verify HTTP firewall rule:
```bash
gcloud compute firewall-rules list --filter=ALLOW:'80'
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.25](images/Lab-3.25.png)

Test HTTP access:
```bash
curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.26](images/Lab-3.26.png)

---

### 📄 Task 6: Viewing System Logs
List available logs:
```bash
gcloud logging logs list
gcloud logging logs list --filter="compute"
```

Read logs:
```bash
gcloud logging read "resource.type=gce_instance" --limit 5
gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5
```

![Lab 3 - Getting Started with Cloud Shell and gcloud.27](images/Lab-3.27.png)

---

## Task Completed
- Launch Cloud Shell
- Run gcloud commands
- Create and manage VMs
- Configure firewall rules
- View logs
