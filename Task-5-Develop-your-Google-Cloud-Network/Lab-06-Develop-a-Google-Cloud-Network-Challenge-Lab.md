# 🧪 Develop Your Google Cloud Network: Challenge Lab

## 📘 Introduction

In a **challenge lab**, you’re given a **scenario** and a **set of tasks**. Instead of following step-by-step instructions, you will rely on the **skills learned from previous labs** in the course to determine how to complete each task on your own 💡.

An **automated scoring system** will provide feedback on whether you have completed the tasks correctly.

⚠️ **Important Notes:**
- You will **not** be taught new Google Cloud concepts in this lab.
- You are expected to **apply and extend** your existing knowledge.
- You may need to:
  - Change default values  
  - Read and interpret error messages  
  - Research and troubleshoot issues independently  

🎯 To score **100%**, you must successfully complete **all tasks within the allotted time**.

This lab is recommended for learners who have completed the **Develop your Google Cloud Network** skill badge.

👉 **Are you up for the challenge?** 🚀

---

## ⚙️ Setup

### 🛑 Before You Click **Start Lab**
Please read these instructions carefully:

- ⏱ Labs are **timed** and **cannot be paused**.
- The timer starts immediately after clicking **Start Lab**.
- Google Cloud resources are available **only during the lab time**.

### 🌐 Lab Environment
This is a **hands-on lab** using a **real Google Cloud environment**, not a simulation or demo.

You will be provided with:
- Temporary Google Cloud credentials
- Access limited to the duration of the lab

### ✅ What You Need
- 🌍 Access to a standard internet browser (**Chrome recommended**)
- 🕵️ Use an **Incognito or Private window** (recommended)  
  - Prevents conflicts with your personal Google Cloud account
- ⏳ Enough uninterrupted time to finish the lab

⚠️ **Important:**  
Use **only the student account** provided. Using a personal Google Cloud account may result in **unexpected charges**.

---

## 🎯 Challenge Scenario

You are a **Cloud Engineer at Jooli Inc.**, recently trained in **Google Cloud and Kubernetes** ☁️⚙️.

A new team named **Griffin** has asked for your help to complete the setup of their environment. Some work has already been started, but **critical components are still missing** and require your expertise.

📌 You are expected to already have the skills needed — **no step-by-step guides will be provided**.

---

## 📋 Required Tasks (Overview)

You need to complete the following:

1. 🏗 Create a **development VPC** with **three subnets** manually  
2. 🏭 Create a **production VPC** with **three subnets** manually  
3. 🔐 Create a **bastion host** connected to **both VPCs**  
4. 🗄 Create a **development Cloud SQL instance** and prepare the **WordPress environment**  
5. ☸️ Create a **Kubernetes cluster** in the **development VPC**  
6. ⚙️ Prepare the Kubernetes cluster for WordPress  
7. 🌐 Deploy **WordPress** using the supplied configuration  
8. 📊 Enable **monitoring** for the cluster  
9. 👥 Provide access for an **additional engineer**

> 🛠 *Detailed “How to do it” steps will be provided per task in the next sections.*

---

## 🏢 Jooli Inc. Standards

Please follow these company standards carefully:

- 📍 Create all resources in the specified **REGION** and **ZONE**, unless stated otherwise
- 🌐 Use the **project VPCs**
- 🏷 Follow naming conventions:  
  - Format: `team-resource`  
  - Example: `kraken-webserver1`
- 💰 Use **cost-effective resource sizes**
  - Default recommendation: **e2-medium**
  - Excessive resource usage may result in **project termination** ⚠️

---

## 🧩 Your Mission

The Griffin team plans to use **WordPress** and needs a **development environment** set up properly.

Some components are already in place, but the rest depends on **your expertise**.

💻 As soon as you open your laptop, you receive this request.

🍀 **Good luck, engineer — the challenge begins now!**

---

## 🛜 Task 1: Create Development VPC Manually (Using CLI)

### 🎯 Objective
Create a **development VPC** named `griffin-dev-vpc` with **two custom subnets** using the **Google Cloud CLI (gcloud)**.

---

### 📌 Requirements

#### 🌐 VPC
- **Name:** `griffin-dev-vpc`
- **Subnet mode:** Custom

#### 📍 Subnets
| Subnet Name          | CIDR Block        |
|----------------------|-------------------|
| griffin-dev-wp       | 192.168.16.0/20   |
| griffin-dev-mgmt     | 192.168.32.0/20   |

> ⚠️ All resources must be created in the assigned **REGION**.

---


## 🧭 How to Do It (CLI Method)

### 🧩 Step 0: Set Default Zone and Region 🌍

Configure the default zone and region for the project to ensure all resources are created in the correct location.
```bash
gcloud config set compute/zone "us-east1-c"
export ZONE=$(gcloud config get compute/zone)

gcloud config set compute/region "us-east1"
export REGION=$(gcloud config get compute/region)
```

---

### 1️⃣ Create the Custom VPC

```bash
gcloud compute networks create griffin-dev-vpc \
  --subnet-mode=custom
```

### 2️⃣ Create the WordPress Subnet

```bash
gcloud compute networks subnets create griffin-dev-wp \
  --network=griffin-dev-vpc \
  --region=$REGION \
  --range=192.168.16.0/20
```

### 3️⃣ Create the Management Subnet

```bash
gcloud compute networks subnets create griffin-dev-mgmt \
  --network=griffin-dev-vpc \
  --region=$REGION \
  --range=192.168.32.0/20
```

### 🔍 Verification (Optional but Recommended)

```bash
gcloud compute networks subnets list \
  --filter="network:griffin-dev-vpc"
```

---

## 🏭 Task 2: Create Production VPC Manually (Using CLI)

### 🎯 Objective
Create a **production VPC** named `griffin-prod-vpc` with **two custom subnets** using the **Google Cloud CLI (gcloud)**.

---

### 📌 Requirements

#### 🌐 VPC
- **Name:** `griffin-prod-vpc`
- **Subnet mode:** Custom

#### 📍 Subnets
| Subnet Name           | CIDR Block        |
|-----------------------|-------------------|
| griffin-prod-wp       | 192.168.48.0/20   |
| griffin-prod-mgmt     | 192.168.64.0/20   |

> ⚠️ All resources must be created in the assigned **REGION**.

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace `REGION` with the lab-provided region (for example: `us-central1`).

---

### 1️⃣ Create the Custom VPC

```bash
gcloud compute networks create griffin-prod-vpc \
  --subnet-mode=custom
```

### 2️⃣ Create the WordPress Subnet

```bash
gcloud compute networks subnets create griffin-prod-wp \
  --network=griffin-prod-vpc \
  --region=$REGION \
  --range=192.168.48.0/20
```

### 3️⃣ Create the Management Subnet

```bash
gcloud compute networks subnets create griffin-prod-mgmt \
  --network=griffin-prod-vpc \
  --region=$REGION \
  --range=192.168.64.0/20
```

### 🔍 Verification (Optional but Recommended)

```bash
gcloud compute networks subnets list \
  --filter="network:griffin-prod-vpc"
```

---

## 🧱 Task 3: Create Bastion Host (Using CLI)

### 🎯 Objective
Create a **bastion host VM** with **two network interfaces**:
- One connected to the **development management subnet**
- One connected to the **production management subnet**

The bastion host must be reachable via **SSH**.

---

### 📌 Requirements

#### 🖥️ Compute Instance
- **Purpose:** Secure administrative access (bastion host)
- **Network Interfaces:**
  - `griffin-dev-vpc` → `griffin-dev-mgmt`
  - `griffin-prod-vpc` → `griffin-prod-mgmt`
- **Machine type:** `e2-medium` (cost-effective standard)
- **Zone:** Assigned **ZONE**

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace `ZONE` and `REGION` with the lab-provided values  
> 💡 Replace `PROJECT_ID` only if explicitly required

---

### 1️⃣ Create Firewall Rule to Allow SSH (If Not Already Present)

Allow SSH access to the bastion host.

For dev
```bash
gcloud compute firewall-rules create allow-ssh-1 \
  --direction=INGRESS \
  --priority=1000 \
  --network=griffin-dev-vpc \
  --action=ALLOW \
  --rules=tcp:22 \
  --source-ranges=0.0.0.0/0
```

For prod
```bash
gcloud compute firewall-rules create allow-ssh-2 \
  --direction=INGRESS \
  --priority=1000 \
  --network=griffin-prod-vpc \
  --action=ALLOW \
  --rules=tcp:22 \
  --source-ranges=0.0.0.0/0
```

### 2️⃣ Create the Bastion Host with Two Network Interfaces

```bash
gcloud compute instances create griffin-bastion \
  --zone=$ZONE \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --network-interface=network=griffin-dev-vpc,subnet=griffin-dev-mgmt \
  --network-interface=network=griffin-prod-vpc,subnet=griffin-prod-mgmt \
  --tags=bastion
```
> ⚠️ The first network interface receives the external IP by default, enabling SSH access.

### 3️⃣ Verify the Bastion Host

Check that the VM is running and has two NICs:
```bash
gcloud compute instances describe griffin-bastion \
  --zone=$ZONE
```
Look for:
- Two networkInterfaces
- One external IP on the first interface

### 4️⃣ SSH into the Bastion Host

```bash
gcloud compute ssh griffin-bastion --zone=$ZONE
```
> ✅ Successful login confirms SSH access is working.

---

## 🗄️ Task 4: Create and Configure Cloud SQL Instance (Using CLI)

### 🎯 Objective
Create a **MySQL Cloud SQL instance** named `griffin-dev-db` in the assigned **REGION**, then connect to it and prepare the **WordPress database and user**.

---

### 📌 Requirements

#### ☁️ Cloud SQL Instance
- **Name:** `griffin-dev-db`
- **Database engine:** MySQL
- **Region:** Assigned **REGION**
- **Purpose:** WordPress backend database

#### 🧑‍💻 Database Configuration
- **Database name:** `wordpress`
- **Username:** `wp_user`
- **Password:** `stormwind_rules`

> ⚠️ These credentials will be required again in **Task 6**.

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace `REGION` with the lab-provided region (for example: `us-central1`).

---

### 1️⃣ Create the Cloud SQL MySQL Instance

```bash
gcloud sql instances create griffin-dev-db \
  --database-version=MYSQL_8_0 \
  --region=$REGION \
  --tier=db-f1-micro \
  --storage-type=SSD \
  --storage-size=10GB \
  --availability-type=zonal
```
> 💡 `db-f1-micro` is cost-effective and sufficient for lab workloads.

### 2️⃣ Set the Root Password (If Prompted)

```bash
gcloud sql users set-password root \
  --host=% \
  --instance=griffin-dev-db \
  --password=stormwind_rules
```

### 3️⃣ Connect to the Cloud SQL Instance

Use Cloud SQL Proxy via gcloud:
```bash
gcloud sql connect griffin-dev-db --user=root
```
> 🔐 Enter the root password when prompted.


### 4️⃣ Prepare the WordPress Database

Run the following SQL commands inside the MySQL prompt:
```sql
CREATE DATABASE wordpress;
CREATE USER "wp_user"@"%" IDENTIFIED BY "stormwind_rules";
GRANT ALL PRIVILEGES ON wordpress.* TO "wp_user"@"%";
FLUSH PRIVILEGES;
```

Exit MySQL:
```sql
EXIT;
```

### 🔍 Verification (Optional)

Reconnect using the WordPress user:
```bash
gcloud sql connect griffin-dev-db --user=wp_user
```

Then verify:
```sql
SHOW DATABASES;
```

---

## ☸️ Task 5: Create Kubernetes Cluster (Using CLI)

### 🎯 Objective
Create a **Google Kubernetes Engine (GKE) cluster** named `griffin-dev` with **2 nodes**, deployed in the **development WordPress subnet**.

---

### 📌 Requirements

#### ☁️ GKE Cluster
- **Cluster name:** `griffin-dev`
- **Number of nodes:** `2`
- **Machine type:** `e2-standard-4`
- **Zone:** Assigned **ZONE**

#### 🌐 Networking
- **VPC:** `griffin-dev-vpc`
- **Subnet:** `griffin-dev-wp`

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace `ZONE` and `REGION` with the lab-provided values.

---

### 1️⃣ Create the GKE Cluster

```bash
gcloud container clusters create griffin-dev \
  --zone=$ZONE \
  --num-nodes=2 \
  --machine-type=e2-standard-4 \
  --network=griffin-dev-vpc \
  --subnetwork=griffin-dev-wp
```
> ⚠️ This command creates a zonal cluster with default settings suitable for the lab.

### 2️⃣ Get Cluster Credentials

Configure kubectl to access the new cluster:
```bash
gcloud container clusters get-credentials griffin-dev \
  --zone=$ZONE
```

### 3️⃣ Verify Cluster and Nodes

Check cluster status:
```bash
kubectl cluster-info
```

Check nodes:
```bash
kubectl get nodes
```

---

## ⚙️ Task 6: Prepare the Kubernetes Cluster (Using CLI)

### 🎯 Objective
Prepare the **GKE cluster** for WordPress by:
- Copying the required Kubernetes configuration files
- Creating **secrets** for MySQL credentials
- Configuring **persistent storage** for WordPress
- Adding a **service account key** for Cloud SQL access

---

### 📌 Requirements Recap

- **MySQL credentials (from Task 4):**
  - Username: `wp_user`
  - Password: `stormwind_rules`
- **Kubernetes config source:** `gs://spls/gsp321/wp-k8s`
- **Service account:** `cloud-sql-proxy`
- **Cluster:** `griffin-dev`

---

### 🧭 How to Do It (CLI Method)

> ⚠️ Ensure `kubectl` is already configured for the `griffin-dev` cluster  
> (completed in Task 5).

---

### 1️⃣ Copy WordPress Kubernetes Files from Cloud Storage

```bash
gsutil cp -r gs://spls/gsp321/wp-k8s .
```

Change into the directory:
```bash
cd wp-k8s
```

### 2️⃣ Edit the Environment Configuration File

Open the file:
```bash
nano wp-env.yaml
```

Update the values to match your database credentials:
```yaml
- name: WORDPRESS_DB_USER
  value: wp_user

- name: WORDPRESS_DB_PASSWORD
  value: stormwind_rules
```
> 💡 Ensure both values are correct before saving.

Save and exit (CTRL + O, ENTER, CTRL + X).

### 3️⃣ Create the WordPress Secrets and Volume Configuration

Apply the configuration:
```bash
kubectl apply -f wp-env.yaml
```
This step:
- Creates Kubernetes secrets for WordPress
- Defines a persistent volume / claim for WordPress files

### 4️⃣ Create a Service Account Key for Cloud SQL Proxy

Generate the service account key:
```bash
gcloud iam service-accounts keys create key.json \
  --iam-account=cloud-sql-proxy@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com
```

### 5️⃣ Add the Service Account Key as a Kubernetes Secret

```bash
kubectl create secret generic cloudsql-instance-credentials \
  --from-file=key.json
```

### 🔍 Verification (Optional)

Check secrets:
```bash
kubectl get secrets
```

Check persistent volume claims:
```bash
kubectl get pvc
```

---

## 📰 Task 7: Create a WordPress Deployment (Using CLI)

### 🎯 Objective
Deploy **WordPress** to the Kubernetes cluster by:
- Updating the deployment configuration with the correct **Cloud SQL instance connection name**
- Creating the **WordPress deployment**
- Exposing WordPress using a **LoadBalancer service**
- Verifying that the WordPress installer is accessible

---

### 📌 Prerequisites

- ✅ Cloud SQL instance `griffin-dev-db` is running
- ✅ Kubernetes secrets and volumes are configured (Task 6)
- ✅ Files copied from `gs://spls/gsp321/wp-k8s`
- ✅ `kubectl` is configured for the `griffin-dev` cluster

---

### 🧭 How to Do It (CLI Method)

---

### 1️⃣ Get the Cloud SQL Instance Connection Name

```bash
gcloud sql instances describe griffin-dev-db \
  --format="value(connectionName)"
```

📌 Output format:
```ini
PROJECT_ID:REGION:griffin-dev-db
```
Copy this value—you will need it in the next step.

### 2️⃣ Edit the WordPress Deployment Configuration

Open the deployment file:
```bash
nano wp-deployment.yaml
```

Find the placeholder:
```yaml
YOUR_SQL_INSTANCE
```

Replace it with the actual instance connection name, for example:
```yaml
PROJECT_ID:REGION:griffin-dev-db
```
> ⚠️ Make sure there are no extra spaces or typos.

Save and exit (CTRL + O, ENTER, CTRL + X).

### 3️⃣ Create the WordPress Deployment

Apply the deployment configuration:
```bash
kubectl apply -f wp-deployment.yaml
```

Verify that the pod is running:
```bash
kubectl get pods
```
> Wait until the WordPress pod shows STATUS: Running.

### 4️⃣ Create the WordPress Service

Expose WordPress using a LoadBalancer service:
```bash
kubectl apply -f wp-service.yaml
```

### 5️⃣ Access the WordPress Site

Get the external IP address:
```bash
kubectl get svc
```
Look for the EXTERNAL-IP of the WordPress service.

### 🌍 Open a browser and navigate to:
```bash
http://EXTERNAL-IP
```

---

## 📈 Task 8: Enable Monitoring (Create Uptime Check)

### 🎯 Objective
Enable monitoring for the **WordPress development site** by creating an **uptime check** that verifies the site is reachable.

---

### 📌 Prerequisites

- ✅ WordPress service is running
- ✅ LoadBalancer service has an **EXTERNAL-IP**
- ✅ WordPress site is accessible via browser

---

### 🧭 How to Do It (GPU Method)

> 💡 Replace `EXTERNAL_IP` with the LoadBalancer IP from Task 7  
> 💡 Replace `REGION` if required by your lab

---

#### ✅ Recommended Method: Google Cloud Console (Guaranteed to Pass)

1. Open Navigation Menu ☰
2. Go to Monitoring
3. Click Uptime checks
4. Click Create Uptime Check

| Setting           | Value                        |
| ----------------- | ---------------------------- |
| **Title**         | `WordPress Dev Uptime Check` |
| **Check type**    | `HTTP`                       |
| **Protocol**      | `HTTP`                       |
| **Resource type** | `URL`                        |
| **Hostname**      | `<WordPress EXTERNAL-IP>`    |
| **Path**          | `/`                          |
| **Port**          | `80`                         |
| **Frequency**     | 1 minute                     |
| **Regions**       | Leave default                |

👉 Use the EXTERNAL-IP from:
```bash
kubectl get svc wordpress
```

---

## 👤 Task 9: Provide Access for an Additional Engineer

### 🎯 Objective
Grant **project access** to an additional engineer by assigning the **Editor role** to their Google account.

---

### 📌 Prerequisites

- ✅ You have **Owner** or **IAM Admin** permissions in the project
- ✅ The additional engineer's **user account email** is available

---

### 🧭 How to Do It (CLI Method)

> 💡 Replace `ENGINEER_EMAIL` with the email of the additional engineer  
> 💡 Replace `PROJECT_ID` with your Google Cloud project ID

---

### 1️⃣ Grant the Editor Role

```bash
gcloud projects add-iam-policy-binding qwiklabs-gcp-01-fd82afb89c9b \
  --member="user:student-01-f3dd7812fcbe@qwiklabs.net" \
  --role="roles/editor"
```

### 2️⃣ Verify Access

Get the IAM policy for the project:
```bash
gcloud projects get-iam-policy PROJECT_ID
```

Check that the additional engineer appears with:
```makefile
role: roles/editor
member: user:student-01-f3dd7812fcbe@qwiklabs.net
```

---

# ✅ Task Completed

Here’s a summary of the tasks you completed in the **Develop Your Google Cloud Network Challenge Lab**:

1. **Development VPC Creation** 🛜  
   - Created `griffin-dev-vpc` with two subnets:
     - `griffin-dev-wp` (192.168.16.0/20)  
     - `griffin-dev-mgmt` (192.168.32.0/20)

2. **Production VPC Creation** 🏭  
   - Created `griffin-prod-vpc` with two subnets:
     - `griffin-prod-wp` (192.168.48.0/20)  
     - `griffin-prod-mgmt` (192.168.64.0/20)

3. **Bastion Host Deployment** 🧱  
   - Deployed `griffin-bastion` with two network interfaces:
     - Connected to `griffin-dev-mgmt`  
     - Connected to `griffin-prod-mgmt`  
   - Verified SSH access

4. **Cloud SQL Instance Creation & Configuration** 🗄️  
   - Created MySQL instance `griffin-dev-db`  
   - Configured `wordpress` database and `wp_user` credentials  
   - Ensured credentials ready for WordPress deployment

5. **Kubernetes Cluster Creation** ☸️  
   - Created `griffin-dev` GKE cluster with 2 nodes (`e2-standard-4`)  
   - Cluster deployed in `griffin-dev-wp` subnet

6. **Kubernetes Cluster Preparation** ⚙️  
   - Copied WordPress Kubernetes manifests from `gs://spls/gsp321/wp-k8s`  
   - Created secrets for MySQL credentials  
   - Configured persistent volumes  
   - Added Cloud SQL service account key as a secret

7. **WordPress Deployment** 📰  
   - Edited `wp-deployment.yaml` with Cloud SQL instance connection name  
   - Deployed WordPress pods and service  
   - Verified LoadBalancer exposes WordPress installer page

8. **Monitoring Enabled** 📈  
   - Created uptime check for WordPress development site  
   - Verified the site is reachable and monitored

9. **Additional Engineer Access Granted** 👤  
   - Assigned **Editor role** to a second user account for project access  

---

💡 **Lab Outcome:**  
You successfully set up a **development and production network**, deployed a **secure bastion host**, provisioned **Cloud SQL and GKE resources**, deployed **WordPress**, enabled **monitoring**, and configured **team access**—demonstrating a full end-to-end Google Cloud network and application setup.
