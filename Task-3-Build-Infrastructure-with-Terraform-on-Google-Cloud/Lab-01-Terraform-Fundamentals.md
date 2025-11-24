# Terraform Fundamentals ☁️🛠️

## Introduction 📝

In this lab, you use **Terraform** and **Gemini Code Assist**—an AI-powered collaborator in Google Cloud—to provision a virtual machine (VM) instance.

Terraform enables you to safely and predictably create, change, and improve infrastructure. It codifies APIs into declarative configuration files that can be shared, reviewed, versioned, and treated as code.

---

## Objectives 🎯

In this lab, you will learn how to:

- Get started with Terraform in Google Cloud  
- Install Terraform from installation binaries  
- Use Gemini Code Assist to create a Terraform configuration for a VM instance  

---

## Setup ⚙️

Before you click the **Start Lab** button:

- Labs are timed and cannot be paused. The timer starts when you click **Start Lab**.  
- You will work in a **real Google Cloud environment**, not a simulation. Temporary credentials are provided.  
- Requirements:
  - Standard internet browser (Chrome recommended)  
  - Incognito/Private mode to prevent conflicts with your personal Google account  
  - Enough time to finish the lab in one session  
  - Use **only the student account** provided to avoid extra charges  

---

## Getting Started with Google Cloud 🌐

1. Click **Start Lab** and then **Open Google Cloud Console**  
2. Use the temporary **Username** and **Password** provided in the Lab Details pane  
3. Accept the terms and conditions  
4. Do **not** add recovery options or enable 2FA  
5. Once signed in, click the **Navigation menu** or **Search** field to access Google Cloud products  

### Activate Cloud Shell 🖥️

Cloud Shell provides a persistent 5GB home directory and command-line access to Google Cloud.

1. Click **Activate Cloud Shell**  
2. Continue through the Cloud Shell info window  
3. Authorize Cloud Shell to use your credentials  
4. Check your project ID:

```bash
gcloud config list project
```

---

## What is Terraform? 📦

Terraform is an Infrastructure as Code (IaC) tool to build, change, and manage infrastructure safely.
- Infrastructure as Code: Version and treat infrastructure as code.
- Execution Plans: See what Terraform will do before applying changes.
- Resource Graph: Understand dependencies and create resources efficiently.
- Change Automation: Apply complex changes with minimal manual intervention.

---

### Task 1: Verify Terraform Installation ✅

Terraform comes pre-installed in Cloud Shell.
```bash
terraform
```

you should see:
```sql
Usage: terraform [global options] <subcommand> [args]

Main commands:
  init        Prepare your working directory
  validate    Check configuration
  plan        Show changes
  apply       Create/update infrastructure
  destroy     Destroy resources
  ...
```

---

### Task 2: Build the Infrastructure 🏗️
Enable Gemini Code Assist
```bash
gcloud services enable cloudaicompanion.googleapis.com
```

1. Open the Cloud Shell Editor
2. Go to Settings → Gemini Code Assist → Enable
3. Authorize the plugin and select your Project ID


Create a Terraform Configuration File
```bash
touch instance.tf
```

Open instance.tf in the editor and use Gemini Code Assist to generate the VM configuration:
```yaml
/generate Generate the Terraform configuration for a Google Compute Engine VM:
* Project ID: Project ID
* VM Name: terraform
* Machine Type: e2-medium
* Deployment Zone: zone
* Boot Disk: Debian 12
* Network: Default network
```

Accept the generated code. Example:
```hcl
resource "google_compute_instance" "default" {
  project      = "Project ID"
  zone         = "zone"
  name         = "terraform"
  machine_type = "e2-medium"
  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-12"
    }
  }
  network_interface {
    network = "default"
  }
}
```

Initialize Terraform
```bash
terraform init
```

Create an Execution Plan
```bash
terraform plan
```

Apply the Configuration
```bash
terraform apply
```
Type yes to confirm. Terraform will provision your VM.


Inspect State 📝
Terraform stores metadata in `terraform.tfstate`.
```bash
terraform show
```

Example output:
```hcl
# google_compute_instance.default:
resource "google_compute_instance" "default" {
  name         = "terraform"
  machine_type = "e2-medium"
  ...
}
```

Cleanup 🧹
To remove all created resources:
```bash
terraform destroy
```

---

## 🎉 Task Completed
- Installed Terraform
- Used Gemini Code Assist to generate a Terraform configuration
- Provisioned a VM instance on Google Cloud
- Explored Terraform execution plans and state files
