# 🧩 Interact with Terraform Modules  

## 📘 Overview

As your infrastructure grows, Terraform configurations can become complex. While Terraform places no limits on file size or directory complexity, organizing everything in a single directory leads to problems:

- ❌ Difficult navigation  
- ❌ Risky updates that cause unintended side effects  
- ❌ Repeated, duplicated configuration (dev/staging/prod)  
- ❌ Hard-to-maintain copy/paste across teams and projects  

### ✔️ Terraform Modules solve these problems:

- **📁 Organize configuration** → Break infrastructure into logical components  
- **🧱 Encapsulate configuration** → Prevent accidental changes between components  
- **🔁 Re-use configuration** → Use shared, reusable modules internally or from the public registry  
- **🔐 Ensure consistency** → Enforce best practices across your org  
- **🚀 Reduce errors** → Avoid misconfigurations of complex resources (e.g., Cloud Storage buckets)

---

## 🎯 Objectives

In this lab, you will:

- ✅ Use a module from the Terraform Registry  
- ✅ Build your own module  

---

## 🛠️ Setup and Requirements

Before clicking **Start Lab**, remember:

- Labs are **timed**, cannot be paused  
- You will use **temporary student credentials**  
- Use **Incognito/Private Mode** to avoid conflicts  

### ✔️ You need:

- A standard browser (Chrome recommended)  
- Time to complete the lab in one session  
- Use only the **provided student account** (NOT your own GCP account)

---

## **▶️ How to Start the Lab and Sign in to Google Cloud Console**

1. Click **Start Lab**  
2. A left panel appears showing:
   - **Open Google Cloud console**
   - **Lab timer**
   - **Temporary Username & Password**

3. Click **Open Google Cloud console**  
   - If using Chrome → Right-click → *Open Link in Incognito Window*

4. At the login screen:  
   - Choose **Use another account** if prompted  
   - Copy/paste the **Username**  
   - Click **Next**  
   - Copy/paste the **Password**  
   - Click **Next**

### Important Notes
🚫 Do *not* add recovery email or 2FA  
🚫 Do *not* use your own Google account  
🚫 Do *not* start a free trial  

---

## 🖥️ Activate Cloud Shell

Cloud Shell provides a pre-configured VM with:

- 5GB persistent home folder  
- gcloud CLI  
- Development tools installed  

### Steps:

1. Click **Activate Cloud Shell** ▶️  
2. Continue through prompts  
3. Authorize Cloud Shell  
4. When connected, your session shows:
```text
Your Cloud Platform project in this session is set to "PROJECT_ID"
```

### Useful commands:

Check active account:  
```bash
gcloud auth list
```

Check current project:
```bash
gcloud config list project
```

## **📘 What is a Terraform Module?**

A **Terraform module** is a set of Terraform configuration files in a **single directory**.  
Even a simple configuration with just one `.tf` file is considered a module.

When you run Terraform commands in that directory, it is called the **root module**.

📁 **Example minimal module:**
├── LICENSE
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf


Running Terraform inside this directory treats it as the **root module**.

---

### **📥 Calling Modules**

Terraform processes configuration **only from the current directory**, but you can call other modules using **module blocks**.

When Terraform sees:

```hcl
module "something" {
  source = "./path"
}
```

…it loads and processes the module from that location.
> 👉 A module loaded inside another configuration is called a child module.

### **🌐 Local and Remote Modules**

Terraform can load modules from:
- 📁 Local filesystem
- 🌍 Terraform Registry
- 🌀 Version control systems
- 🔗 HTTP URLs
- ☁️ Terraform Cloud or Enterprise private registries

### 📐 Module Best Practices

Terraform modules are like libraries or packages in programming languages.

Best practices:

- ✔️ 1. Plan for modules early
Even small projects benefit from modular organization.

- ✔️ 2. Use local modules for organization
Encapsulation keeps your infrastructure maintainable as it grows.

- ✔️ 3. Use public modules from the Registry
Reuse trusted modules instead of reinventing the wheel.

- ✔️ 4. Publish and share modules
Teams can maintain consistent infrastructure by sharing private or public modules.

You will learn how to publish modules in a later lab.

---

## **🧩 Task 1. Use Modules from the Registry**

In this section, you use modules from the [Terraform Registry](https://registry.terraform.io/) to provision an example environment in Google Cloud. The concepts you use here are applicable to any modules from any source. 🌐

---

Open the Terraform Registry page for the Terraform Network module in a new browser tab or window. The page looks like this:  
[**Terraform Registry page**](https://registry.terraform.io/modules/terraform-google-modules/network/google/3.3.0)📘

The page includes information about the module and a link to the source repository. The right side of the page includes a dropdown interface to select the module version and instructions for using the module to provision infrastructure.

When you call a module, the **source** argument is required. In this example, Terraform searches for a module in the Terraform Registry that matches the given string. You could also use a URL or local file path for the source of your modules. Refer to the [Terraform documentation](https://developer.hashicorp.com/terraform/language/modules/configuration) for a list of possible module sources.

The other argument shown here is the **version**. For supported sources, the version lets you define what version or versions of the module are loaded. In this lab, you specify an exact version number for the modules you use. You can read about more ways to specify versions in the module documentation.

Other arguments to module blocks are treated as input variables to the modules.

---

## **🛠️ Create a Terraform configuration**

To start, run the following commands in Cloud Shell to clone the example simple project from the Google Terraform modules GitHub repository and switch to the v6.0.1 branch:

```bash
git clone https://github.com/terraform-google-modules/terraform-google-network
cd terraform-google-network
git checkout tags/v6.0.1 -b v6.0.1
```

This ensures that you're using the correct version number.

---

On the Cloud Shell toolbar, click Open Editor.

In the Editor, navigate to:
```bash
terraform-google-network/examples/simple_project
```

and open the main.tf file. Your main.tf configuration should look like this:
```hcl
module "test-vpc-module" {
  source       = "terraform-google-modules/network/google"
  version      = "~> 6.0"
  project_id   = var.project_id 
  network_name = "my-custom-mode-network"
  mtu          = 1460

  subnets = [
    {
      subnet_name   = "subnet-01"
      subnet_ip     = "10.10.10.0/24"
      subnet_region = "us-west1"
    },
    {
      subnet_name           = "subnet-02"
      subnet_ip             = "10.10.20.0/24"
      subnet_region         = "us-west1"
      subnet_private_access = "true"
      subnet_flow_logs      = "true"
    },
    {
      subnet_name               = "subnet-03"
      subnet_ip                 = "10.10.30.0/24"
      subnet_region             = "us-west1"
      subnet_flow_logs          = "true"
      subnet_flow_logs_interval = "INTERVAL_10_MIN"
      subnet_flow_logs_sampling = 0.7
      subnet_flow_logs_metadata = "INCLUDE_ALL_METADATA"
      subnet_flow_logs_filter   = "false"
    }
  ]
}
```

This configuration includes one important block:

`module "test-vpc-module"` defines a Virtual Private Cloud (VPC), which provides networking services for the rest of your infrastructure. ☁️


### **🔧 Set values for module input variables**

Some input variables are required, which means that the module doesn't provide a default value; an explicit value must be provided for Terraform to run correctly.

Within the `test-vpc-module` block, review the input variables you are setting. Each of these input variables is documented in the Terraform Registry.
The required inputs for this module are:
- network_name: The name of the network being created
- project_id: The ID of the project where this VPC is created
- subnets: The list of subnets being created

To use most modules, you must pass input variables to the module configuration. The configuration that calls a module is responsible for setting its input values.

On the Terraform Registry page for the Google Cloud network module, an Inputs tab describes all of the input variables that module supports.

---

### **🤖 Enable Gemini Code Assist in the Cloud Shell IDE**

You can use Gemini Code Assist in Cloud Shell to receive AI-powered help while coding.

Enable the Gemini for Google Cloud API:
```bash
gcloud services enable cloudaicompanion.googleapis.com
```
Click Open Editor on the Cloud Shell toolbar.
> 📝 Note: You can switch between Cloud Shell and the Editor by clicking Open Editor or Open Terminal.

In the left pane, click the **`Settings`** icon and search for Gemini Code Assist.
Ensure the checkbox for **`Gemini Code Assist: Enable is selected`**.
Click **`Cloud Code – No Project`** in the status bar.

Authorize the plugin.
If no project is selected, click **Select a Google Cloud Project** and choose your **Project ID.**
Verify that your project displays in the Cloud Code status bar.

---

### **🏷️ Define root input variables**

Using input variables with modules works the same as in any Terraform configuration.
To help you be more productive, Gemini Code Assist will help you edit your Terraform code.
Navigate to `variables.tf` and click the Gemini Code Assist: Smart Actions icon.

Paste this prompt:
```pgsql
In the configuration file variables.tf, update the variable block "project_id" resource by adding a "default" argument. Set the value to PROJECT_ID.
```
Press **ENTER**, then click **Accept**.

Your updated variable should look like:
```hcl
variable "project_id" {
  description = "The project ID to host the network in"
  default     = "PROJECT_ID"
}
```

Now define the `network_name` variable.

Paste this prompt:
```pgsql
In the configuration file variables.tf, define the variable "network_name" and set the value "example-vpc" to the "default" argument.
```

Press ENTER and click Accept.

The result:
```hcl
variable "network_name" {
  description = "The name of the network to be created"
  default     = "example-vpc"
}
```

---

Navigate back to `main.tf`.

You will now use Gemini Code Assist to update:
- the network name → var.network_name
- subnet regions → REGION

Paste this prompt:
```bash
Update the "test-vpc-module" module within the main.tf configuration file. Modify the network name from "my-custom-mode-network" to var.network_name and change the subnet region from us-west1 to REGION.
```

Your updated module should look like this:
```hcl
module "test-vpc-module" {
  source       = "terraform-google-modules/network/google"
  version      = "~> 6.0"
  project_id   = var.project_id # Replace this with your project ID in quotes
  network_name = var.network_name
  mtu          = 1460

  subnets = [
    {
      subnet_name   = "subnet-01"
      subnet_ip     = "10.10.10.0/24"
      subnet_region = "REGION"
    },
    {
      subnet_name           = "subnet-02"
      subnet_ip             = "10.10.20.0/24"
      subnet_region         = "REGION"
      subnet_private_access = "true"
      subnet_flow_logs      = "true"
    },
    {
      subnet_name               = "subnet-03"
      subnet_ip                 = "10.10.30.0/24"
      subnet_region             = "REGION"
      subnet_flow_logs          = "true"
      subnet_flow_logs_interval = "INTERVAL_10_MIN"
      subnet_flow_logs_sampling = 0.7
      subnet_flow_logs_metadata = "INCLUDE_ALL_METADATA"
      subnet_flow_logs_filter   = "false"
    }
  ]
}
```

## **🧩 Define Root Output Values**

Modules can also have **output values**, defined using the `output` keyword.  
You can access module outputs using:

```pgsql
module.<MODULE NAME>.<OUTPUT NAME>
```

``lua

These outputs are displayed in the **Outputs** tab of the module in the Terraform Registry.

Module outputs are typically:
- 🔗 Passed to other components of your configuration  
- 📤 Exposed as **root module outputs**

---

### 📄 Verify `outputs.tf` Contents

Your `outputs.tf` file should include the following definitions:


These outputs are displayed in the **Outputs** tab of the module in the Terraform Registry.

Module outputs are typically:
- 🔗 Passed to other components of your configuration  
- 📤 Exposed as **root module outputs**

---

### 📄 Verify `outputs.tf` Contents

Your `outputs.tf` file should include the following definitions:

```hcl
output "network_name" {
  value       = module.test-vpc-module.network_name
  description = "The name of the VPC being created"
}

output "network_self_link" {
  value       = module.test-vpc-module.network_self_link
  description = "The URI of the VPC being created"
}

output "project_id" {
  value       = module.test-vpc-module.project_id
  description = "VPC project id"
}

output "subnets_names" {
  value       = module.test-vpc-module.subnets_names
  description = "The names of the subnets being created"
}

output "subnets_ips" {
  value       = module.test-vpc-module.subnets_ips
  description = "The IP and cidrs of the subnets being created"
}

output "subnets_regions" {
  value       = module.test-vpc-module.subnets_regions
  description = "The region where subnets will be created"
}

output "subnets_private_access" {
  value       = module.test-vpc-module.subnets_private_access
  description = "Whether the subnets will have access to Google API's without a public IP"
}

output "subnets_flow_logs" {
  value       = module.test-vpc-module.subnets_flow_logs
  description = "Whether the subnets will have VPC flow logs enabled"
}

output "subnets_secondary_ranges" {
  value       = module.test-vpc-module.subnets_secondary_ranges
  description = "The secondary ranges associated with these subnets"
}

output "route_names" {
  value       = module.test-vpc-module.route_names
  description = "The routes associated with this VPC"
}
```

### **🚀 Provision Infrastructure**

Navigate to your example project directory:
```bash
cd ~/terraform-google-network/examples/simple_project
```

Initialize Terraform:
```bash
terraform init
```

Apply your configuration:
```bash
terraform apply
```

Respond yes when prompted.

**📤 Example Output**

Your output will look similar to:
```makefile
Outputs:

network_name = "example-vpc"
network_self_link = "https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-01-a68489b0625b/global/networks/example-vpc"
project_id = "PROJECT_ID"
route_names = []
subnets_flow_logs = [
  false,
  true,
  true,
]
subnets_ips = [
  "10.10.10.0/24",
  "10.10.20.0/24",
  "10.10.30.0/24",
]
subnets_names = [
  "subnet-01",
  "subnet-02",
  "subnet-03",
]
...
...
```

### **🧠 Understanding How Modules Work**

When using a module for the first time, you must run:
- terraform init, or
- terraform get

Terraform installs modules under:
```bash
.terraform/modules
```
> 📌 For local modules, Terraform creates a symlink to the module folder, so updates apply immediately without re-running `terraform get`.

### **🧹 Clean Up Your Infrastructure**

Destroy the resources you created:
```bash
terraform destroy
```
Respond yes to confirm.

Remove the module folder:
```bash
cd ~
rm -rd terraform-google-network -f
```

✨ After this, click Check my progress in your lab environment.

---

## 🏗️ Task 2: Build a Module

In the previous task, you used a module from the **Terraform Registry** to create a VPC network in Google Cloud. While knowing how to *use* modules is essential, being able to *build* your own modules is a core skill for any Terraform practitioner.

Terraform treats **every configuration** as a module. When you run `terraform` commands, the directory containing your configuration is considered the **root module**.

In this task, you will create your own module to manage **Compute Storage buckets** for hosting static websites.

---

## 📁 Module Structure

Terraform treats any **local directory** referenced in the `source` argument of a `module` block as a module.

A typical module file structure looks like this:

├── LICENSE
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf


> 💡 None of these files are required for Terraform to use the module.  
> A module can contain just **one `.tf` file**, but this structure keeps things organized and reusable.

---

## 📄 Purpose of Each File

### 🧾 LICENSE
- Describes the open-source license for your module.
- Not used by Terraform, but important when sharing modules publicly.

### 📘 README.md
- Markdown documentation explaining how to use the module.
- Displayed automatically on GitHub or the Terraform Registry.
- Terraform itself does **not** read this file.

### ⚙️ main.tf
- The core configuration for your module.
- Can be split into multiple `.tf` files depending on your organization style.

### 🧮 variables.tf
- Contains input variable definitions.
- Required variables (no default) must be provided by the module user.
- Optional variables (with default) can be overridden by the user.

### 📤 outputs.tf
- Defines what information your module will return to the calling configuration.
- Useful for passing values like bucket URLs, names, IDs, etc.

---

## 🚫 Files You Should NOT Distribute

Terraform generates some files during execution that **must not** be distributed with your module:

### ❌ `terraform.tfstate` & `terraform.tfstate.backup`
- Contain your state file and sensitive infrastructure info.
- Must never be shared.

### ❌ `.terraform/` Directory
- Contains downloaded modules and provider plugins.
- Specific to your local environment.

### ❌ `*.tfvars`
- Used for setting variable values.
- Not needed when distributing a module, unless you're also using it as a standalone configuration.

> 💡 If you use Git for version control, configure your `.gitignore` to exclude these files.  
> Example: GitHub’s Terraform `.gitignore` template.

---

## **🧩 Create a Module**

In this task, you will build your own Terraform module that provisions **Cloud Storage buckets** configured for static website hosting.

---

## 📁 Create the Module Directory Structure

Navigate to your home directory and create your **root module** and **module folder**:

```bash
cd ~
touch main.tf
mkdir -p modules/gcs-static-website-bucket
```

Create the module files:
```bash
cd modules/gcs-static-website-bucket
touch website.tf variables.tf outputs.tf
```

📝 Add README.md

Create a README.md file inside gcs-static-website-bucket:
```bash
tee -a README.md <<EOF
# GCS static website bucket

This module provisions Cloud Storage buckets configured for static website hosting.
EOF
```

📄 Add LICENSE File

🔖 This lab uses the Apache 2.0 license.
```bash
tee -a LICENSE <<EOF
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
EOF
```

Your module directory now looks like:
```cpp
main.tf
modules/
└── gcs-static-website-bucket
    ├── LICENSE
    ├── README.md
    ├── website.tf
    ├── outputs.tf
    └── variables.tf
```

☁️ Add Cloud Storage Bucket Resource (website.tf)

Inside `website.tf`, add:
```hcl
resource "google_storage_bucket" "bucket" {
  name               = var.name
  project            = var.project_id
  location           = var.location
  storage_class      = var.storage_class
  labels             = var.labels
  force_destroy      = var.force_destroy
  uniform_bucket_level_access = true

  versioning {
    enabled = var.versioning
  }

  dynamic "retention_policy" {
    for_each = var.retention_policy == null ? [] : [var.retention_policy]
    content {
      is_locked        = var.retention_policy.is_locked
      retention_period = var.retention_policy.retention_period
    }
  }

  dynamic "encryption" {
    for_each = var.encryption == null ? [] : [var.encryption]
    content {
      default_kms_key_name = var.encryption.default_kms_key_name
    }
  }

  dynamic "lifecycle_rule" {
    for_each = var.lifecycle_rules
    content {
      action {
        type          = lifecycle_rule.value.action.type
        storage_class = lookup(lifecycle_rule.value.action, "storage_class", null)
      }
      condition {
        age                   = lookup(lifecycle_rule.value.condition, "age", null)
        created_before        = lookup(lifecycle_rule.value.condition, "created_before", null)
        with_state            = lookup(lifecycle_rule.value.condition, "with_state", null)
        matches_storage_class = lookup(lifecycle_rule.value.condition, "matches_storage_class", null)
        num_newer_versions    = lookup(lifecycle_rule.value.condition, "num_newer_versions", null)
      }
    }
  }
}
```

📥 Define Module Inputs (variables.tf)

Add all module variables:
```hcl
variable "name" {
  description = "The name of the bucket."
  type        = string
}

variable "project_id" {
  description = "The ID of the project to create the bucket in."
  type        = string
}

variable "location" {
  description = "The location of the bucket."
  type        = string
}

variable "storage_class" {
  description = "The Storage Class of the new bucket."
  type        = string
  default     = null
}

variable "labels" {
  description = "A set of key/value label pairs to assign to the bucket."
  type        = map(string)
  default     = null
}

variable "bucket_policy_only" {
  description = "Enables Bucket Policy Only access to a bucket."
  type        = bool
  default     = true
}

variable "versioning" {
  description = "While set to true, versioning is fully enabled for this bucket."
  type        = bool
  default     = true
}

variable "force_destroy" {
  description = "Delete bucket contents on destroy."
  type        = bool
  default     = true
}

variable "iam_members" {
  description = "IAM members for the bucket."
  type = list(object({
    role   = string
    member = string
  }))
  default = []
}

variable "retention_policy" {
  description = "Bucket object retention policy."
  type = object({
    is_locked        = bool
    retention_period = number
  })
  default = null
}

variable "encryption" {
  description = "KMS encryption configuration."
  type = object({
    default_kms_key_name = string
  })
  default = null
}

variable "lifecycle_rules" {
  description = "Bucket lifecycle rules."
  type = list(object({
    action    = any
    condition = any
  }))
  default = []
}
```

📤 Define Module Output (outputs.tf)
```hcl
output "bucket" {
  description = "The created storage bucket"
  value       = google_storage_bucket.bucket
}
```

📦 Reference the Module in Root main.tf

Return to your `root directory` and edit `main.tf`:
```h
module "gcs-static-website-bucket" {
  source = "./modules/gcs-static-website-bucket"

  name       = var.name
  project_id = var.project_id
  location   = "REGION"

  lifecycle_rules = [{
    action = {
      type = "Delete"
    }
    condition = {
      age        = 365
      with_state = "ANY"
    }
  }]
}
```

📤 Create Root Module Output
```bash
cd ~
touch outputs.tf
```

Add:
```hcl
output "bucket-name" {
  description = "Bucket names."
  value       = "module.gcs-static-website-bucket.bucket"
}
```

🧮 Create Root Module Variables
```bash
touch variables.tf
```

Add:
```hcl
variable "project_id" {
  description = "The ID of the project in which to provision resources."
  type        = string
  default     = "PROJECT_ID"
}

variable "name" {
  description = "Name of the buckets to create."
  type        = string
  default     = "PROJECT_ID"
}
```
> ⚠️ Reminder: Storage bucket names must be globally unique. Use a combination of your name + date to avoid conflicts.

---

### 📤 Upload Files to the Bucket

You now have a module that creates a static website bucket — but it’s empty.  
Upload sample website files so that the website displays content. 🌐

⬇️ Download Sample Website Files

```bash
cd ~
curl https://raw.githubusercontent.com/hashicorp/learn-terraform-modules/master/modules/aws-s3-static-website-bucket/www/index.html > index.html
curl https://raw.githubusercontent.com/hashicorp/learn-terraform-modules/master/modules/aws-s3-static-website-bucket/www/error.html > error.html
```

📁 Upload Files to Your Storage Bucket

Replace PROJECT_ID with your actual bucket name.
```bash
gsutil cp *.html gs://PROJECT_ID
```

🌍 View the Website

Open:
```bash
https://storage.cloud.google.com/YOUR-BUCKET-NAME/index.html
```
You should see:
> "Nothing to see here."

🧹 Clean Up the Website and Infrastructure

Destroy all Terraform-managed resources:
```bash
terraform destroy
```
Type yes when prompted.

### 🎉 Task Completed
- 🧱 Explored Terraform module fundamentals
- 📦 Used a pre-existing module from the Registry
- 🛠️ Built your own module for hosting a static website
- 📥 Defined module inputs, outputs, and variables
- 🧭 Followed best practices for reusable infrastructure code
