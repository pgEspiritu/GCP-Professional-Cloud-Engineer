# 🌐 Deploy and Troubleshoot a Website — Challenge Lab

## 📘 Overview

This is a **challenge lab** where you are given a scenario and a set of tasks to complete **without step-by-step instructions**. You are expected to apply the skills learned from previous labs to determine how to complete each task.

An **automated scoring system** will provide feedback based on whether your tasks are completed correctly.

### Key Expectations
- No new Google Cloud concepts are taught
- You must rely on prior knowledge and troubleshooting skills
- Reading error messages and adjusting configurations is part of the challenge
- To score **100%**, all tasks must be completed within the allotted time

This lab is recommended for learners preparing for the **Google Cloud Certified Professional Cloud Architect** exam.  
Are you ready for the challenge? 💪

---

## ⚙️ Setup and Requirements

### Before You Click **Start Lab**
- Labs are **timed** and **cannot be paused**
- The timer starts as soon as you click **Start Lab**
- Google Cloud resources are available only for the duration of the lab

This is a **hands-on lab** using a real Google Cloud environment (not a simulation).  
Temporary credentials are provided so you can sign in and work during the lab session.

### What You Need
- 🌐 Access to a standard web browser (**Chrome recommended**)
- 🕵️ Use an **Incognito or private browser window** to avoid account conflicts
- ⏱️ Sufficient uninterrupted time to finish the lab
- ⚠️ Use **only the student account** provided to avoid unexpected charges

---

## 🧩 Challenge Scenario

Your company is preparing to launch a **brand-new product** 🚀.  
To support the launch, a new website has been created—but the developer left before deploying it.

The website is ready. The infrastructure is not.

---

## 🎯 Challenge

Your task is to **deploy the website in the public cloud** by completing the required tasks.

For this exercise:
- You will deploy a **basic Apache web server**
- The server acts as a **placeholder** for the new product website

---

## 🖥️ Running a Basic Apache Web Server

A **Compute Engine virtual machine** behaves like a standard Linux server.

In this lab, you will:
- Create and manage a VM instance
- Install and run a simple **Apache web server**
- Use it as a temporary stand-in for the new product site

This task helps reinforce the fundamentals of running and troubleshooting web servers on virtual machines.

---

## ✅ Task 1: Create a Linux VM Instance (CLI)

### 🎯 Goal
Create a Linux **Compute Engine** virtual machine named **`prd-mkt-2xr`** in the specified zone.

---

### 🔹 Step 1: Define Task-Specific Variables
```bash
export VM_NAME=prd-mkt-2xr
export ZONE=us-central1-c
```

---

### 🔹 Step 2: Create the Linux VM Instance

```bash
gcloud compute instances create $VM_NAME \
  --zone=$ZONE \
  --machine-type=e2-micro \
  --image-family=debian-11 \
  --image-project=debian-cloud \
  --tags=http-server
```

---

### 🔹 Step 3: Verify the VM

```bash
gcloud compute instances list --filter="name=$VM_NAME"
```
Confirm that the instance:
- Exists
- Is running
- Is located in us-central1-c

---

## ✅ Task 2: Enable Public Access to the VM Instance (CLI)

### 🎯 Goal
Allow public HTTP access to the VM so users can reach the website.

---

### 🔹 Step 1: Create an HTTP Firewall Rule
Create a firewall rule that allows inbound traffic on **TCP port 80**.

```bash
gcloud compute firewall-rules create allow-http \
  --direction=INGRESS \
  --priority=1000 \
  --network=default \
  --action=ALLOW \
  --rules=tcp:80 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=http-server
```

### 🔹 Step 2: Verify the Firewall Rule

```bash
gcloud compute firewall-rules list --filter="name=allow-http"
```
Confirm:
- Port 80 is allowed
- Target tag is http-server

---

### 📝 Notes
- The VM created in Task 1 already has the http-server network tag
- This firewall rule enables public users to access the web server over HTTP

---

## 🚀 Task 3: Running a Basic Apache Web Server (CLI)

### 🎯 Goal
Install and run a simple **Apache web server** on the VM to serve a placeholder website.

VM details (from previous tasks):
- **VM name:** `prd-mkt-2xr`
- **Zone:** `us-central1-c`

---

## 🔹 Step 1: SSH into the VM
```bash
gcloud compute ssh prd-mkt-2xr --zone=us-central1-c
```

---

## 🔹 Step 2: Update the Package Index

```bash
sudo apt-get update
```

---

## 🔹 Step 3: Install Apache Web Server

```bash
sudo apt-get install -y apache2
```

---

## 🔹 Step 4: Start and Enable Apache

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

Verify Apache is running:
```bash
sudo systemctl status apache2
```

---


## ✅ Solution — Task 4: Test Your Server 🌐

### 🎯 Goal
Verify that the VM is correctly serving web traffic over its **external IP** and displays the **“Hello World!”** page.

---

## 🔹 Step 1: Get the External IP Address
Run the following command in Cloud Shell:

```bash
gcloud compute instances describe prd-mkt-2xr \
  --zone=us-central1-c \
  --format="get(networkInterfaces[0].accessConfigs[0].natIP)"
```
> Copy the returned IP address.

---

## 🔹 Step 2: Access the Website

Open a web browser and navigate to:
```bash
http://EXTERNAL_IP
```

---

## 🔹 Step 3: Validate the Result

You should see a page similar to:
```nginx
Hello World!
```
> (or your Apache placeholder default page)

This confirms:
- ✅ Apache is running
- ✅ Firewall rule allows HTTP (port 80)
- ✅ VM is serving traffic publicly

---

## Troubleshooting
Receiving a Connection Refused error:
- Your VM instance is not publicly accessible because the VM instance does not have the proper tag that allows Compute Engine to apply the appropriate firewall rules, or your project does not have a firewall rule that allows traffic to your instance's external IP address.
- You are trying to access the VM using an https address. Check that your URL is http:// EXTERNAL_IP and not https:// EXTERNAL_IP.

---

## 🎉 Task Completed! ✅

You have successfully:

- Created a Linux VM instance (`prd-mkt-2xr`)  
- Enabled public access via firewall rules  
- Installed and started an Apache web server  
- Verified that the default Apache page is served over the VM's external IP  

Your **new website is now live** on Google Cloud! 🌐🚀
