# 🚀 Infrastructure as Code with Terraform

## 📘 Overview

Terraform is the **infrastructure as code** offering from HashiCorp. It is a tool for **building, changing, and managing infrastructure** in a safe, repeatable way. Operators and infrastructure teams can use Terraform to manage environments with a configuration language called the **HashiCorp Configuration Language (HCL)** for human-readable, automated deployments.

Infrastructure as code is the process of **managing infrastructure in files** rather than manually configuring resources in a UI. A resource can be any piece of infrastructure in a given environment, such as a virtual machine, security group, network interface, etc.

At a high level, Terraform allows operators to:

- Use **HCL** to author files containing definitions of desired resources on almost any provider (AWS, Google Cloud, GitHub, Docker, etc.)  
- Automate the creation of those resources at the time of `apply`

---

### 🛠️ Simple Workflow for Deployment

A simple workflow adheres closely to the following steps:

1. **Scope** - Confirm what resources need to be created for a given project.  
2. **Author** - Create the configuration file in HCL based on the scoped parameters.  
3. **Initialize** - Run:
```bash
terraform init
```
in the project directory with the configuration files. This downloads the correct provider plug-ins for the project.
4. Plan & Apply - Run:
```bash
terraform plan
```
to verify the creation process, then:
```bash
terraform apply
```
to create real resources as well as the state file that tracks the differences between your configuration files and actual deployment environment.

---

## 🎯 Objectives

In this lab, you will learn to:
- Build, change, and destroy infrastructure with Terraform
- Create resource dependencies with Terraform
- Provision infrastructure with Terraform

---

## ⚙️ Setup and Requirements
Before You Start the Lab
- Read all instructions carefully. Labs are timed and cannot be paused. The timer starts when you click Start Lab.
- This lab runs in a real cloud environment, not a simulation. Temporary credentials are provided for the duration of the lab.

Requirements
- Access to a standard internet browser (Chrome recommended)
- Use an Incognito/private browser window to prevent conflicts between personal and student accounts
- Time to complete the lab (cannot pause once started)
- Use only the provided student account; using a personal account may incur charges

---

### 🖥️ How to Start Your Lab and Sign in to Google Cloud Console

1. Click Start Lab. If payment is required, select your method.
2. Lab Details pane includes:
- Open Google Cloud console button
- Time remaining
- Temporary credentials
3. Click Open Google Cloud console (or right-click → Open in Incognito).

Tip: Arrange tabs side-by-side for convenience.

### 🔑 Sign In Credentials

If the Choose an account dialog appears, click Use Another Account.

Username:
```bash
"Username"
```

Password:
```bash
"Password"
```

> ⚠️ Important: Use only the credentials provided by the lab. Do not use your personal Google Cloud account to avoid extra charges.

### ✅ Click Through

- Accept terms and conditions
- Do not add recovery options or 2FA (temporary account)
- Do not sign up for free trials

After a few moments, the Google Cloud console will open.
> Note: Use the Navigation menu or Search field to access Google Cloud products.


### 🖥️ Activate Cloud Shell

Cloud Shell is a VM loaded with development tools, offering a persistent 5GB home directory and command-line access to Google Cloud resources.

1. Click Activate Cloud Shell at the top of the console
2. Follow the prompts:
  - Continue through the info window
  - Authorize Cloud Shell to use your credentials
Once connected, you are authenticated and your project is set to PROJECT_ID:
```bash
Your Cloud Platform project in this session is set to "PROJECT_ID"
```

### 🔧 Optional gcloud Commands

List the active account:
```bash
gcloud auth list
```

Set active account:
```bash
gcloud config set account `ACCOUNT`
```

List the project ID:
```bash
gcloud config list project
```

For full gcloud documentation, refer to the [gcloud CLI overview guide](https://cloud.google.com/sdk/gcloud)

---

## 🏗️ Task 1: Build Infrastructure

Terraform comes pre-installed in **Cloud Shell**. With Terraform already installed, you can dive right in and create some infrastructure.

---

### 1️⃣ Create Configuration File

Start by creating your example configuration file named `main.tf`. Terraform recognizes files ending in `.tf` or `.tf.json` and loads them automatically.

```bash
touch main.tf
```
Click the Open Editor button on the toolbar of Cloud Shell.
> Tip: You can switch between Cloud Shell and the editor using the Open Editor and Open Terminal icons, or open the editor in a separate tab with Open in new window.

### 2️⃣ Add Terraform Configuration

In the editor, add the following content to main.tf:
```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "3.5.0"
    }
  }
}

provider "google" {
  project = "PROJECT ID"
  region  = "REGION"
  zone    = "ZONE"
}

resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
```

> Note: For Terraform 0.12, remove the terraform {} block.

### 🔹 Terraform Block

The terraform {} block tells Terraform which provider to download from the Terraform Registry.
  - source = "hashicorp/google" is shorthand for registry.terraform.io/hashicorp/google.
  - The version argument is optional but recommended to avoid breaking changes from new provider versions.
  - Without specifying a version, Terraform downloads the latest provider automatically.

> For more info, see the [Terraform Provider Requirements](https://www.terraform.io/docs/language/providers/requirements.html)


🔹 Providers

The provider block configures the provider, in this case google.
  - A provider is responsible for creating and managing resources.
  - Multiple provider blocks can exist if you manage resources from different providers.

---

### ⚡ Initialize Terraform

Before applying your configuration, run:
```bash
terraform init
```
This initializes local settings and downloads the required provider.

### 🚀 Create Resources

Apply your configuration by running:
```bash
terraform apply
```
- Terraform will show a plan with a + next to the resource to indicate creation.
- (known after apply) means the value will be determined once the resource is created.

If the plan looks correct, approve it by typing:
```bash
yes
```

Terraform will then create the resource. Example output:
```yaml
google_compute_network.vpc_network: Creating...
google_compute_network.vpc_network: Still creating... [10s elapsed]
...
google_compute_network.vpc_network: Creation complete after 58s [id=terraform-network]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

### 🔍 Verify Resources
- In the Google Cloud Console, navigate to VPC network. You should see terraform-network provisioned.
- In Cloud Shell, inspect the current state:
```bash
terraform show
```
> These values can be used later to configure other resources or outputs.

---

## ## 🔄 Task 2: Change the Infrastructure

In the previous task, you created basic infrastructure with Terraform: a **VPC network**.  
In this task, you'll **modify your configuration** and see how Terraform handles changes.

> Terraform builds an execution plan that **only modifies what is necessary** to reach your desired state.  
> Using Terraform to change infrastructure allows you to **version control configurations and state**, so you can track how infrastructure evolves over time.

---

### ➕ Add Resources

You can add new resources by updating your Terraform configuration and running:

```bash
terraform apply
```

In the editor, add a compute instance resource to `main.tf`:
```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "terraform-instance"
  machine_type = "e2-micro"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-12"
    }
  }

  network_interface {
    network = google_compute_network.vpc_network.name
    access_config {
    }
  }
}
```

Notes:
- `name` and `machine_type` are simple strings.
- `boot_disk` and `network_interface` are more complex blocks.
- Your compute instance uses Debian and connects to the VPC network created earlier.
- The `access_config` block (even empty) ensures the instance is internet-accessible.

### ⚡ Apply the Changes

Run:
```bash
terraform apply
```

- Approve the plan by typing:
```bash
yes
```

- Terraform will create the google_compute_instance resource in Google Cloud.
- The `~` prefix in Terraform output means the resource is updated in-place.


### 🧑‍💻 Enable Gemini Code Assist in Cloud Shell IDE

Gemini Code Assist provides AI-powered guidance directly in your Cloud Shell editor.

1. Enable Gemini API:
```bash
gcloud services enable cloudaicompanion.googleapis.com
```

2. Click Open Editor in Cloud Shell.
3. Go to Settings → Gemini Code Assist → Enable.
4. Click Cloud Code - No Project in the status bar and authorize the plugin.
5. Make sure your Google Cloud Project (Project ID) displays in the Cloud Code status bar.


### ✨ Modify Resources Using Gemini Code Assist

- Add a tags argument to your vm_instance resource in main.tf using Gemini:

Prompt to Gemini:
```pgsql
In the configuration file "main.tf", update the "vm_instance" resource by adding a "tags" argument. Set the value to ["web", "dev"].
```
- Press ENTER to apply the change.
- In the Gemini Diff view, click Accept.

Updated `vm_instance` resource:
```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "terraform-instance"
  machine_type = "e2-micro"
  tags         = ["web", "dev"]
  # ...
}
```

### ⚡ Apply the Updated Configuration

Run:
```bash
terraform apply
```
- Approve the plan with `yes`.
- Terraform updates the instance in-place and adds the tags.

## Make Destructive Changes & Destroy Infrastructure

A **destructive change** requires the provider to **replace an existing resource** rather than updating it in-place.  
This usually occurs when the cloud provider does not support updating a resource in the way described by your configuration.

> Example: Changing the disk image of a compute instance.

---

### 🔹 Update Disk Image (Destructive Change)

You will edit the `boot_disk` block inside the `vm_instance` resource in `main.tf` using **Gemini Code Assist Smart Actions**.

1. Open the **Cloud Shell Editor**.  
2. Click the **Gemini Code Assist: Smart Actions** icon on the toolbar.  
3. Paste the following prompt:
```bash
In the configuration file "main.tf", update the image in the boot_disk block of the "vm_instance" resource to "cos-cloud/cos-stable".
```
4. Press **ENTER** to apply the change.  
5. In the **Gemini Diff view**, click **Accept**.  

Updated `boot_disk` block:

```hcl
boot_disk {
  initialize_params {
    image = "cos-cloud/cos-stable"
  }
}
```

### ⚡ Apply the Destructive Change

Run:
```bash
terraform apply
```

- The `-/+` prefix indicates Terraform will destroy and recreate the resource instead of updating it in-place.
- The `~` prefix shows attributes that can be updated in-place.
- Terraform handles the replacement automatically and clearly shows what will be destroyed and recreated.

> After approval, Terraform will first destroy the existing instance, then create a new one with the updated disk image.
You can inspect the new state with
```bash
terraform show
```

### 🗑️ Destroy Infrastructure

Now you will completely destroy the Terraform-managed infrastructure.

1. Run:
```bash
terraform destroy
```

2. Approve the plan by typing:
```bash
yes
```
- The - prefix indicates resources will be destroyed.
- Terraform determines the correct order for destruction: e.g., it deletes the instance before deleting the VPC network.
- Terraform automatically handles dependency graphs and parallel operations where safe.
> Destroying infrastructure is rare in production but useful for development, testing, or staging environments.

---

## 🔗 Task 3: Create Resource Dependencies

In this task, you learn how to **share information between resources** and manage **resource dependencies** in Terraform.

> Real-world infrastructure contains multiple resources and resource types, sometimes spanning multiple providers.  
> Terraform can automatically determine the correct order of operations based on resource references.

---

### ⚡ Recreate Existing Resources

Run the following command to ensure your **network** and **VM instance** are created:

```bash
terraform apply
```
- Respond with yes to confirm the plan.

### ➕ Assign a Static IP Address

Add the following resource to main.tf to allocate a reserved static IP:
```hcl
resource "google_compute_address" "vm_static_ip" {
  name = "terraform-static-ip"
}
```
- This is similar to adding a VM instance, but creates a google_compute_address resource.
- The resource allocates a reserved IP address in your project.

### 🔍 Plan Changes

Run:
```bash
terraform plan
```

Sample output shows what Terraform plans to create:
```csharp
# google_compute_address.vm_static_ip will be created
+ resource "google_compute_address" "vm_static_ip" {
    + address      = (known after apply)
    + address_type = "EXTERNAL"
    + name         = "terraform-static-ip"
    ...
  }
Plan: 1 to add, 0 to change, 0 to destroy.
```
> Note: The plan command only shows potential changes; it does not apply them.

### 🖧 Update VM Network Interface

Update the `network_interface` block in your VM instance to attach the static IP:
```hcl
network_interface {
  network = google_compute_network.vpc_network.self_link
  access_config {
    nat_ip = google_compute_address.vm_static_ip.address
  }
}
```
- The `nat_ip` attribute references the static IP resource.
- Terraform now knows that vm_static_ip must be created before vm_instance.
- Properties of the static IP are saved in the Terraform state.

### 💾 Save and Apply the Plan

1. Save the plan to ensure reproducibility:
```bash
terraform plan -out static_ip
```

2. Apply the saved plan:
```bash
terraform apply "static_ip"
```

- Terraform first creates the static IP, then updates the VM instance.
- This interpolation expression allows Terraform to infer dependencies automatically.


---

## 🔗 Explore Implicit and Explicit Dependencies

Terraform can **automatically infer resource dependencies** using **interpolation expressions**.  
For example, referencing:

```hcl
google_compute_address.vm_static_ip.address
```

creates an implicit dependency on the `google_compute_address` resource named `vm_static_ip`.
Terraform uses this to determine the correct order of creation and updates.

### 📝 Implicit Dependencies
- Implicit dependencies via interpolation expressions are the primary method to inform Terraform about relationships between resources.
- They should be used whenever possible.

### 🏗️ Explicit Dependencies
Some dependencies are not visible to Terraform.
- In these cases, use the depends_on argument to explicitly declare dependencies.
- Example: An application running on a VM that depends on a Cloud Storage bucket.
Add the following resources to main.tf:
```hcl
# New resource for the storage bucket our application will use.
resource "google_storage_bucket" "example_bucket" {
  name     = "<UNIQUE-BUCKET-NAME>"
  location = "US"

  website {
    main_page_suffix = "index.html"
    not_found_page   = "404.html"
  }
}

# Create a new instance that uses the bucket
resource "google_compute_instance" "another_instance" {
  # Ensures this VM is created only after the storage bucket
  depends_on = [google_storage_bucket.example_bucket]

  name         = "terraform-instance-2"
  machine_type = "e2-micro"

  boot_disk {
    initialize_params {
      image = "cos-cloud/cos-stable"
    }
  }

  network_interface {
    network = google_compute_network.vpc_network.self_link
    access_config {
    }
  }
}
```

> ⚠️ Note: Storage bucket names must be globally unique. Replace <UNIQUE-BUCKET-NAME> with a unique name (e.g., your name + date).

### 🗂️ Organizing Resources
- The order of resource definitions in a .tf file does not affect how Terraform applies changes.
- Organize files in a way that is logical and maintainable for your team.


### ⚡ Apply the Changes
1. Run Terraform plan:
```bash
terraform plan
```

2. Apply the changes:
```bash
terraform apply
```
- Terraform will create the storage bucket first, then the dependent VM instance.

### 🧹 Cleanup
- Remove these new resources from your configuration.
- Run terraform apply again to destroy them, as they are not used further in this lab.


---

## ⚙️ Task 6: Provision Infrastructure

The compute instance you launched so far uses a **Google image**, but has **no additional software or configuration** applied.  

Terraform can use **provisioners** to upload files, run scripts, or trigger configuration management tools. This allows you to automatically configure instances after creation.

> For custom OS images, Google Cloud + Packer can be used, but in this task we focus on Terraform provisioners.

---

### 📝 Define a Provisioner

Modify the `vm_instance` resource to include a `local-exec` provisioner:

```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "terraform-instance"
  machine_type = "e2-micro"
  tags         = ["web", "dev"]

  provisioner "local-exec" {
    command = "echo ${google_compute_instance.vm_instance.name}:  ${google_compute_instance.vm_instance.network_interface[0].access_config[0].nat_ip} >> ip_address.txt"
  }

  # ...
}
```

- local-exec runs commands on the machine running Terraform (not the VM).
- Multiple provisioners can be added for multiple provisioning steps.
- Uses string interpolation to access the first network interface and access_config ([0]).

### ⚡ Apply the Provisioner

Run:
```bash
terraform apply
```

- Initially, Terraform may show:
```pgsql
Terraform found nothing to do
```

- No ip_address.txt file will be created because provisioners only run when the resource is first created.
