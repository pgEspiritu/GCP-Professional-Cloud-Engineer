# 📘 Manage Terraform State (GSP752)

experiment • Lab  
⏱️ 1 hour • 💰 5 Credits • 📊 Intermediate  
ℹ️ This lab may incorporate AI tools.  
Developed with Hashicorp. Your information may be shared with the lab sponsor if you opted in.

---

## 🧭 Overview

Terraform stores infrastructure state to map real resources to configuration, track metadata, and improve performance.  
Default state file: `terraform.tfstate` (local). Remote storage works better for teams.

Terraform creates plans using the state. Before operations, Terraform refreshes state to match real infrastructure.

---

## 🎯 Objectives

You will learn to:

- Create a local backend  
- Create a Cloud Storage backend  
- Refresh Terraform state  
- Import a Terraform configuration  
- Manage imported configuration  

---

## 🛠️ Setup and Requirements

### Before starting the lab

This lab uses real Google Cloud resources with temporary credentials.

You need:

- A web browser (Chrome recommended)  
- Incognito/private mode (prevents conflicts with your personal account)  
- Time to complete the lab (cannot pause)  
- Use only the student account to avoid charges  

---

## 🚀 Start the Lab & Sign In

1. Click **Start Lab**.  
2. View the **Lab Details** section for:
   - Open Google Cloud Console  
   - Time remaining  
   - Temporary username/password  
3. Click **Open Google Cloud console**  
4. If prompted, select **Use Another Account**  
5. Enter the provided username and password:
```text
"Username"
```

```text
"Password"
```


6. Accept terms  
7. Skip recovery options and 2FA  
8. Do not start free trials  

You will reach the Google Cloud Console.

---

## 💻 Activate Cloud Shell

Cloud Shell provides a VM with tools and a persistent 5GB home directory.

1. Click **Activate Cloud Shell**  
2. Continue through prompts  
3. Authorize Cloud Shell  

You should see:
```text
Your Cloud Platform project in this session is set to "PROJECT_ID"
```


`gcloud` is preinstalled.

### Optional Commands

List active account:
```bash
gcloud auth list
```

Output example:
```sql
ACTIVE: *
ACCOUNT: "ACCOUNT"
```


Set active account:
```bash
gcloud config set account ACCOUNT
```


List project ID:
```bash
gcloud config list project
```


Output:
```bash
[core]
project = "PROJECT_ID"
```

---

# 📚 Purpose of Terraform State

State is required for Terraform to function. While some ask if Terraform can skip state and inspect cloud resources on every run, doing so would shift significant complexity elsewhere. The following explains why Terraform requires state.

---

## 🔗 Mapping to the Real World

Terraform needs a database-like system to map its configuration to real infrastructure.

When your configuration has:
```nginx
resource "google_compute_instance" "foo"
```


Terraform needs to know that instance `i-abcd1234` corresponds to that resource.

- Each remote object must map to **only one** resource instance.  
- Objects imported from outside Terraform must be mapped correctly.  
- If a single remote object maps to multiple instances, Terraform may behave unpredictably.

---

## 🧩 Metadata

Terraform tracks metadata such as resource dependencies.

- Terraform normally reads the configuration to determine dependency order.  
- When you remove a resource from configuration, Terraform still needs to know how to delete it.  
- If a resource exists in state but not in the config, Terraform will plan to destroy it.  
- Since the config no longer lists it, dependency ordering must come from the state.

Terraform stores metadata such as:

- Dependency relationships  
- Provider configuration references (useful with multiple aliased providers)

This avoids Terraform needing to understand ordering rules for every cloud and every resource type.

---

## 🚀 Performance

Terraform stores cached attribute values for all resources in the state. This improves performance.

During `terraform plan`, Terraform must know the current state of resources.

### For small infrastructures:
- Terraform queries all resources directly from providers each run  
- This is the default behavior  

### For large infrastructures:
- Querying every resource is slow  
- Many clouds can't query bulk resources  
- API rate limits restrict how fast Terraform can query  

Large users often use:
```yaml
-refresh=false
-target
```


In these cases, cached state becomes the source of truth.

---

## 🔄 Synchronization

By default, Terraform stores the state locally in the working directory.  
This is fine for single-user workflows, but not for teams.

### Remote state solves this:

- Ensures all users access the same state  
- Prevents applying changes to different resources  
- Supports remote locking  
- Ensures each run uses the most updated state

---

## 🔐 State Locking

If a backend supports locking, Terraform locks the state for operations that write state.

- Prevents simultaneous modifications  
- Happens automatically  
- If locking fails, Terraform stops  
- Can disable with `-lock` (not recommended)

If lock acquisition is slow, Terraform prints a message.  
If no message appears, locking is still happening.

Not all backends support locking — check backend documentation.

---

## 🗂️ Workspaces

Each Terraform configuration uses a backend that determines:

- Where persistent data (state) is stored  
- How operations are run  

The persistent data belongs to a **workspace**.

### Default Behavior:

- Backends start with a single workspace: `default`  
- Only one Terraform state exists initially  

### Some backends support multiple workspaces:

- Allows multiple states for the same configuration  
- Does not require a new backend  
- Does not require different authentication credentials  

This enables deploying multiple distinct environments from the same config.

---

# 🧩 Task 1: Work with Backends

A backend in Terraform determines how state is loaded and how an operation such as apply is executed. This abstraction enables non-local file state storage, remote execution, etc.

---

## 🌟 Benefits of Backends

- Working in a team: Backends can store their state remotely and protect that state with locks to prevent corruption. Some backends, such as Terraform Cloud, even automatically store a history of all state revisions.
- Keeping sensitive information off disk: State is retrieved from backends on demand and only stored in memory.
- Performing remote operations: Some backends support remote operations, which enable the operation to execute remotely.
- Backends are optional: Terraform works without them, but they solve pain points at scale.

Even if you intend to use the local backend, it is useful to learn about backends.

---

# ⚙️ Enable Gemini Code Assist in Cloud Shell

## 1️⃣ Enable the API
```bash
gcloud services enable cloudaicompanion.googleapis.com
```

## 2️⃣ Open the Cloud Shell Editor

Click Open Editor on the Cloud Shell toolbar.

## 3️⃣ Enable Gemini Code Assist

- Click the Settings icon.
- Search for Gemini Code Assist.
- Ensure the checkbox is selected.

## 4️⃣ Configure Cloud Code

- Click Cloud Code – No Project on the status bar.
- Authorize the plugin.
- Select your Project ID.
- Confirm it appears in the status bar.

---

# 📦 Add a Local Backend

When configuring a backend for the first time, Terraform prompts to migrate existing state.
It is recommended to manually back up terraform.tfstate.

## ✏️ Create main.tf
```bash
touch main.tf
```

Open the file in the Cloud Shell Editor.

## ✍️ Add Cloud Storage Bucket Resource
```hcl
provider "google" {
  project     = "# REPLACE WITH YOUR PROJECT ID"
  region      = "REGION"
}

resource "google_storage_bucket" "test-bucket-for-state" {
  name        = "# REPLACE WITH YOUR PROJECT ID"
  location    = "US"
  uniform_bucket_level_access = true
}
```

Save the file.

## 🤖 Use Gemini Code Assist

Click the Smart Actions icon.
Paste:
```sql
Update the main.tf configuration file with the following changes:

* In the Google provider block, set the project to PROJECT_ID.
* In the "test-bucket-for-state" resource block, update the name of the bucket to BUCKET_NAME.
```

Accept the changes.

Your file should resemble:
```hcl
provider "google" {
  project     = "PROJECT_ID"
  region      = "REGION"
}

resource "google_storage_bucket" "test-bucket-for-state" {
  name        = "BUCKET_NAME"
  location    = "US"
  uniform_bucket_level_access = true
}
```

## 🔧 Add Local Backend Block

Append:
```hcl
terraform {
  backend "local" {
    path = "terraform/state/terraform.tfstate"
  }
}
```

## 🚀 Initialize and Apply Terraform
Initialize Terraform
```bash
terraform init
```

Apply Configuration
```bash
terraform apply
```

Enter `yes` when prompted.

A new state file should appear in `terraform/state`.

## 🔍 Inspect Terraform State
```bash
terraform show
```

> Your `google_storage_bucket.test-bucket-for-state` resource should display.

---

# ☁️ Add a Cloud Storage Backend

A **Cloud Storage backend** stores the Terraform state as an object inside a Cloud Storage bucket. This backend also supports **state locking**, which prevents multiple users from writing state at the same time and corrupting it.

State locking occurs automatically for all operations that might write state. If locking fails, Terraform stops.

---

## 🔄 Replace Local Backend With GCS Backend

Navigate back to your `main.tf` file and replace the existing local backend with:

```hcl
terraform {
  backend "gcs" {
    bucket  = "BUCKET_NAME"
    prefix  = "terraform/state"
  }
}
```

> 🔎 Note: Ensure the bucket name matches the bucket you created (from your `google_storage_bucket` resource).

## 🚀 Reinitialize Terraform With State Migration
```bash
terraform init -migrate-state
```

Enter `yes` when prompted.

## 📁 Verify State in Cloud Storage

In the Cloud Console:
1. Go to Cloud Storage > Buckets
2. Open your bucket
3. Navigate to:
`terraform/state/default.tfstate`
Your state is now stored remotely!

## 🔄 Refresh the State

The terraform refresh command reconciles Terraform state with real-world infrastructure.

### 1️⃣ Add a Label to the Bucket

In the Cloud Console:
- Select your bucket
- Click the Labels tab
- Click Add Label
- Set:
  - Key: key
  - Value: value
- Click Save

### 2️⃣ Refresh State in Cloud Shell
```bash
terraform refresh
```
### 3️⃣ View Updated State
```bash
terraform show
```

You should see:
```ini
"key" = "value"
```

## 🧹 Clean Up Your Workspace

To destroy your infrastructure, you must first revert to the local backend so Terraform can delete the Cloud Storage bucket.

### 🔧 Revert Backend to Local

Replace your backend with:
```hcl
terraform {
  backend "local" {
    path = "terraform/state/terraform.tfstate"
  }
}
```

Reinitialize and migrate state back locally:
```bash
terraform init -migrate-state
```

Enter yes when prompted.

---

## 🤖 Update Bucket Resource With Gemini Code Assist

Use Gemini Code Assist to modify the bucket resource.

Prompt:
```pgsql
Update the google_storage_bucket resource named "test-bucket-for-state" in the main.tf file. Add the force_destroy = true argument to its configuration.
```

Accept the diff.

Your updated resource should now look like:
```hcl
resource "google_storage_bucket" "test-bucket-for-state" {
  name        = "BUCKET_NAME"
  location    = "US"
  uniform_bucket_level_access = true
  force_destroy = true
}
```

### ✔️ Apply Changes
```bash
terraform apply
```
Enter `yes` at the prompt.


### 💥 Destroy Infrastructure
```bash
terraform destroy
```
- Enter yes when prompted.
- Your workspace is now clean.

---

# 🧩 Task 2 — Import a Terraform Configuration

In this section, you import an existing Docker container and image into an empty Terraform workspace. This teaches you strategies and considerations for importing real-world infrastructure into Terraform.

---

## 📌 Default Terraform Workflow

Terraform normally manages infrastructure **end-to-end**:

1. **Write** a Terraform configuration that defines the infrastructure.  
2. **Review** the Terraform plan to ensure it matches expectations.  
3. **Apply** the configuration — Terraform creates infrastructure *and* the Terraform state.

After creation, you can:

- Update the configuration  
- Run `terraform plan` and `terraform apply` to change infrastructure  
- Destroy the infrastructure when no longer needed  

This assumes Terraform **creates** everything.

---

## 🧲 When Infrastructure Already Exists

Sometimes infrastructure already exists and wasn't created with Terraform.  
To bring these resources under Terraform management, you use:

### **`terraform import`**

However, importing is a **multi-step process** because the import command **does not create configuration** — it only maps real infrastructure into the state.

---

## 🧵 Five Steps to Import Existing Infrastructure

1. Identify the infrastructure to import.  
2. Import the infrastructure into Terraform state.  
3. Write Terraform configuration that matches the real infrastructure.  
4. Review the Terraform plan to ensure the config matches the state.  
5. Apply the configuration to update the Terraform state.

---

⚠️ **Warning:**  
Importing infrastructure directly manipulates state.  
Always **back up**:

- `terraform.tfstate`  
- `.terraform/` directory  

before using `terraform import` in real projects.

---

# 🐳 Create a Docker Container

Run this command to create a container named **hashicorp-learn** using the latest NGINX image:

```bash
docker run --name hashicorp-learn --detach --publish 8080:80 nginx:latest
```
