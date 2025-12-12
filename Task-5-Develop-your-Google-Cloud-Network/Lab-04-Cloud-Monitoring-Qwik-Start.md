# Cloud Monitoring: Qwik Start 🧪  
**Lab — 20 minutes — 1 Credit**  
**Level:** Introductory  
**GSP089**

---

## 📝 Overview
Cloud Monitoring provides visibility into the performance, uptime, and overall health of cloud-powered applications. Cloud Monitoring collects metrics, events, and metadata from Google Cloud, Amazon Web Services, hosted uptime probes, application instrumentation, and a variety of common application components including Cassandra, Nginx, Apache Web Server, Elasticsearch, and many others. Cloud Monitoring ingests that data and generates insights via dashboards, charts, and alerts. Cloud Monitoring alerting helps you collaborate by integrating with Slack, PagerDuty, HipChat, Campfire, and more.

In this lab you'll install monitoring and logging agents to collect information from your instance, which could include metrics and logs from 3rd party apps.

---

## 🎯 Objectives
In this lab, you learn how to:

- Monitor a Compute Engine virtual machine (VM) instance with Cloud Monitoring.
- Install monitoring and logging agents for your VM.

---

## 🛠 Setup and Requirements

### Before you click the **Start Lab** button
Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click **Start Lab**, shows how long Google Cloud resources are made available to you.

This hands-on lab lets you do the lab activities in a real cloud environment. It gives you new, temporary credentials to sign in and access Google Cloud for the duration of the lab.

To complete this lab, you need:

- Access to a standard internet browser (Chrome recommended).  
- **Use an Incognito or private browser window** to prevent conflicts with your personal Google account.  
- Time to complete the lab—once started, you **cannot pause** the lab.  
- Use **only the student account** provided to you. Using your own account may result in charges.

---

## 🚀 How to Start Your Lab and Sign in to Google Cloud Console

1. Click **Start Lab**.  
2. The Lab Details pane will show:
   - **Open Google Cloud console** button  
   - Time remaining  
   - Temporary **Username** and **Password**  
3. Click **Open Google Cloud console** (open in Incognito if using Chrome).  
4. On the Google Sign-In page:
   - Click **Use Another Account** if prompted  
   - Enter the **Username**  
   - Enter the **Password**

> ⚠️ Important:  
> You must use the provided student credentials.  
> Do **not** use your own Google Cloud account.

5. Accept terms, skip recovery options, skip 2FA setup, ignore free trial prompts.

After a moment, the console will open.

Use the Navigation menu or Search bar to find services.

---

## 💻 Activate Cloud Shell

Cloud Shell is a VM loaded with development tools and a persistent 5GB home directory.

1. Click **Activate Cloud Shell** at the top of the console.  
2. Continue through the dialogs:
   - Continue  
   - Authorize  
3. You will see output like:
```ini
Your Cloud Platform project in this session is set to "PROJECT_ID"
```


### Optional Commands

List active account:
```bash
gcloud auth list
```


List project ID:
```bash
gcloud config list project
```


---

## 🌍 Set Your Region and Zone

Compute Engine resources live in **regions** and **zones**.

Run the following in Cloud Shell:
```bash
gcloud config set compute/zone "ZONE"
export ZONE=$(gcloud config get compute/zone)

gcloud config set compute/region "REGION"
export REGION=$(gcloud config get compute/region)
```

---

# 🧩 Task 1. Create a Compute Engine Instance

1. In the Cloud console, open the **Navigation menu (☰)** and go to:  
   **Compute Engine > VM Instances**  
2. Click **Create instance**.

---

## 🖥 Configure the VM

### **Machine configuration**
Fill in the fields as follows (leave all others at default):

| Field   | Value |
|---------|--------|
| **Name** | `lamp-1-vm` |
| **Region** | `<REGION>` |
| **Zone** | `<ZONE>` |
| **Series** | `E2` |
| **Machine** | `e2-medium` |

---

### **OS and Storage**
Under **Boot Disk**, select:

- **Debian GNU/Linux 12 (bookworm)**

---

### **Networking**
Under **Firewall**, check:

- **Allow HTTP traffic**

---

## 🚀 Create the VM
After configuring all sections:

1. Scroll down.  
2. Click **Create** to launch the VM instance.  
3. Wait a couple of minutes — a **green checkmark** will appear when the VM is ready.

---

# 🧩 Task 2. Add Apache2 HTTP Server to Your Instance

## 🚀 Connect to VM
1. In the Console, go to **VM Instances**.
2. Click **SSH** next to **lamp-1-vm** to open a terminal.

---

## 🏗 Install Apache2 & PHP

Run the following commands:

```bash
sudo apt-get update
sudo apt-get install apache2 php7.0
```
> Note: If php7.0 cannot be installed, use php5.

When prompted to continue, enter Y.

Restart Apache:
```bash
sudo service apache2 restart
```

---

## 🌐 Verify Apache Default Page
1. Return to the Cloud Console → VM Instances.
2. Click the External IP of lamp-1-vm.
3. You should see the Apache2 default page.
> If the External IP column is hidden:
> Click Column Display Options → enable External IP → OK.

---

## 🧭 Create a Monitoring Metrics Scope
1. Go to Navigation menu > View All Products > Observability > Monitoring.
2. When the Monitoring Overview page loads, your metrics scope is ready.

---

## 📡 Install the Monitoring & Logging Agents
Agents gather system and application metrics and send them to Cloud Monitoring and Cloud Logging.

### ⭐ Install Cloud Monitoring Agent

Run these commands in the VM's SSH terminal:
```bash
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```
Press Y if prompted.

### ⭐ Install Cloud Logging Agent

Check the agent status:
```bash
sudo systemctl status google-cloud-ops-agent"*"
```
Press q to exit.

Update packages:
```bash
sudo apt-get update
```

---

# 🛠️ Task 3. Create an Uptime Check

Uptime checks verify that a resource is accessible and responding correctly. Follow the steps below to create an uptime check for your VM.

---

## 🌐 Create an Uptime Check

1. In the Cloud Console, open the **Navigation menu** → **Uptime checks**.
2. Click **Create Uptime Check**.

---

### 🔧 Configuration

#### **Protocol**
- Select **HTTP**

#### **Resource Type**
- Select **Instance**

#### **Instance**
- Choose **lamp-1-vm**

#### **Check Frequency**
- Select **1 minute**

Click **Continue**.

---

### 📝 Response Validation
- Accept the default values  
Click **Continue**.

---

### 🔔 Alert & Notification
- Accept the defaults  
Click **Continue**.

---

### 🏷 Title
- Enter: **Lamp Uptime Check**

---

## ✅ Test Your Uptime Check
Click **Test** — wait for the **green check mark** to confirm connectivity.

Then click **Create**.

> ⏳ The uptime check may take a few minutes to become active. Continue with the lab while it initializes.

---

# 🧪 Task 4. Create an Alerting Policy

Use **Cloud Monitoring** to create one or more alerting policies.

1. In the left menu, click **Alerting**, and then click **+Create Policy**.

2. Click on **Select a metric** dropdown.  
   - Uncheck **Active**.  
   - Type **Network traffic** in *Filter by resource and metric name*.  
   - Click on **VM instance > Interface**.  
   - Select **Network traffic (agent.googleapis.com/interface/traffic)** and click **Apply**.  
   - Leave all other fields at their default value.

3. Click **Next**.

4. Set the following:
   - **Threshold position:** Above threshold  
   - **Threshold value:** 500  
   - **Advanced Options → Retest window:** 1 min  

   Then click **Next**.

5. Click the dropdown arrow next to **Notification Channels**, then click **Manage Notification Channels**.

6. A *Notification channels* page will open in a new tab.  
   - Scroll down and click **ADD NEW** for **Email**.  
   - In the *Create Email Channel* dialog box, enter your **personal email address** and a **Display name**.  
   - Click **Save**.

7. Go back to the previous **Create alerting policy** tab.

8. Click **Notification Channels** again, then click the **Refresh** icon to load your newly created display name.

9. Click **Notification Channels** again if needed, select your **Display name**, and click **OK**.

10. Add a message in **Documentation**, which will be included in the emailed alert.

11. Set the **Alert name** to:  
    **Inbound Traffic Alert**

12. Click **Next**.

13. Review the alert and click **Create Policy**.

🎉 **You've created an alert!**  
While waiting for the system to trigger an alert, proceed to create a dashboard and chart, then explore **Cloud Logging**.

---

# 📊 Task 5. Create a Dashboard and Chart

You can display the metrics collected by **Cloud Monitoring** in your own charts and dashboards. In this section, you create the charts for the lab metrics and a custom dashboard.

---

### 🖥️ Create the Custom Dashboard

1. In the left menu, select **Dashboards**, and then click **+Create Custom Dashboard**.
2. Name the dashboard: **Cloud Monitoring LAMP Qwik Start Dashboard**.

---

### 📈 Add the First Chart

1. Click **+ ADD WIDGET**.
2. Select the **Line** option under *Visualization*.
3. Name the widget: **CPU Load**.
4. Click **Select a metric** dropdown.  
   - Uncheck **Active**.  
   - Type **CPU load (1m)** in *Filter by resource and metric name*.  
   - Click **VM instance > Cpu**.  
   - Select **CPU load (1m)** and click **Apply**.  
   - Leave all other fields at default.
5. Refresh the tab to view the graph.

---

### 📡 Add the Second Chart

1. Click **+ ADD WIDGET** and select the **Line** option again.
2. Name the widget: **Received Packets**.
3. Click **Select a metric** dropdown.  
   - Uncheck **Active**.  
   - Type **Received packets** in *Filter by resource and metric name*.  
   - Click **VM instance > Instance**.  
   - Select **Received packets** and click **Apply**.
4. Refresh the tab to view the graph.
5. Leave all other fields at their default values.  
   You should now see the chart data.

---

# 📜 Task 6. View Your Logs

Cloud Monitoring and Cloud Logging are closely integrated. In this task, you will check the logs for your lab.

---

### 🔍 Open Logs Explorer

1. Select **Navigation menu > Logging > Logs Explorer**.
2. Select the logs you want to see — in this case, choose the logs for the **lamp-1-vm** instance created at the start of the lab:
   - Click on **All Resource**.
   - Select **VM Instance > lamp-1-vm** in the Resource drop-down.
   - Click **Apply**.
3. In the results section, you can now view the logs for your VM instance.

---

### 🔄 Observe Logs When Starting & Stopping the VM

To clearly see how Cloud Monitoring and Cloud Logging respond to instance changes, you will perform actions in one window and observe logs in another.

---

### 🪟 Prepare Your Windows

1. Open the **Compute Engine** window in a **new browser window**:
   - Navigation menu > **Compute Engine**
   - Right-click **VM instances** > *Open link in new window*
2. Move the **Logs Explorer** window beside the **Compute Engine** window.  
   This makes it easier to watch log entries update in real time.

---

### 🛑 Stop the VM

1. In the Compute Engine window, select the **lamp-1-vm** instance.
2. Click the **three vertical dots** on the right side of the screen.
3. Click **Stop**, then confirm.
4. Wait a few minutes for the instance to stop.
5. Watch the log entries in **Logs Explorer** for stop-related messages.

---

### ▶️ Start the VM

1. In the VM instance details window, click the **three vertical dots** again.
2. Select **Start/resume**, then confirm.
3. Wait a few minutes for the instance to start.
4. Observe the log messages as the VM boots up.

---

# 📡 Task 7. Check the Uptime Check Results and Triggered Alerts

---

### 🔎 View Uptime Checks

1. In the **Cloud Logging** window, select  
   **Navigation menu > Monitoring > Uptime checks**.
2. This page shows all active uptime checks and their status across different regions.
3. You should see **Lamp Uptime Check** listed.
4. Since you recently restarted your instance, the regions may show a **failed** status.
5. Wait up to **5 minutes** for the regions to become active.
6. Reload the browser window as needed until all regions show as active.
7. Click the uptime check name: **Lamp Uptime Check**.

---

### ⏳ Wait for Region Status to Update

- Because the VM was restarted, it may take several minutes for the uptime check to return to normal.
- Continue reloading the browser to verify when the regions become active.

---

### 🚨 Check if Alerts Were Triggered

1. In the left menu, click **Alerting**.
2. The **Alerting** window shows **incidents** and **events** triggered by your policies.
3. Check your **email inbox** — you should see **Cloud Monitoring Alerts**.

---

### 🧹 Cleanup: Remove Email Notifications

- It is recommended to **remove the email notification** from your alerting policy.
- Lab resources may stay active for some time, which can cause more alert emails to be sent.


---

## 🎉 Task Completed

You have successfully **set up and monitored a VM** using **Cloud Monitoring**.  
You also created an **uptime check**, an **alerting policy**, and built a **dashboard and chart** to visualize key metrics.  

Additionally, you explored **Cloud Logging** and observed how it reflects changes made to your VM instance in real-time.

Great job completing this lab! 🚀
