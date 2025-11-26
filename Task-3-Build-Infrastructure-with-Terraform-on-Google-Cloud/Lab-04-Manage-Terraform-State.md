# 📘 Manage Terraform State 

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

![Lab-4-image-1](images/Lab-4-image-1.png)

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

![Lab-4-image-2](images/Lab-4-image-2.png)

---

⚠️ **Warning:**  
Importing infrastructure directly manipulates state.  
Always **back up**:

- `terraform.tfstate`  
- `.terraform/` directory  

before using `terraform import` in real projects.

---

## 🐳 Create a Docker Container

Run this command to create a container named **hashicorp-learn** using the latest NGINX image:

```bash
docker run --name hashicorp-learn --detach --publish 8080:80 nginx:latest
```

Verify the container is running:
```bash
docker ps
```

Click **Web Preview** → Preview on port **8080** to view the NGINX default page.

Now you have a running Docker image and container to import.

## 📥 Import the Container into Terraform

Clone the example repository:
```bash
git clone https://github.com/hashicorp/learn-terraform-import.git
```

Enter the directory:
```bash
cd learn-terraform-import
```
This folder contains:
   - main.tf → Docker provider config
   - docker.tf → Configuration for managing your container
Initialize Terraform:
```bash
terraform init
```

If you get
Error: Failed to query available provider packages

run: `terraform init -upgrade`

## 🔧 Update the Docker Provider

Open main.tf and comment out/delete the `host` argument:
```hcl
provider "docker" {
  # host = "npipe:////.//pipe//docker_engine"
}
```
(This is a workaround for an initialization issue.)

## 📝 Add an Empty Resource to docker.tf

In docker.tf, add:
```h
resource "docker_container" "web" {}
```

## 🔍 Get the Container ID

List running containers:
```bash
docker ps
```

Get the full container ID:
```bash
docker inspect -f {{.ID}} hashicorp-learn
```

## 📦 Import the Docker Container Into Terraform

Run:
```bash
terraform import docker_container.web $(docker inspect -f {{.ID}} hashicorp-learn)
```

Terraform import requires:
- Terraform resource address:
`docker_container.web`
- Real container ID:
`the SHA256 ID from Docker`

## 🔎 Verify the Import
```bash
terraform show
```

You should now see the container’s attributes stored in Terraform state.

➡️ Note: Terraform import does not generate configuration.
You must write the corresponding resource configuration manually.

---

## 🛠️ Create the Terraform Configuration

Before you can manage the Docker container with Terraform, you need to create the Terraform configuration.

---

### 🔹 Step 1: Run Terraform Plan

```bash
terraform plan
```
> ⚠️ Note: Terraform will show errors for missing required arguments (image and name). It cannot generate a plan for a resource missing required attributes.

### 🔹 Step 2: Update Configuration to Match Imported State

There are two approaches:
1. Accept the current state as-is
   - Fast, but may create a verbose configuration including unnecessary attributes.
2. Select required attributes only
   - More manageable configuration, but requires knowing which attributes are needed.
For this lab, we will use the current state.

### 🔹 Step 3: Copy Terraform State to `docker.tf`
```bash
terraform show -no-color > docker.tf
```
> ⚠️ Note: This replaces the entire contents of `docker.tf.` For existing resources, you may need to edit and merge manually.
Inspect `docker.tf` — it now contains all attributes from the state.

### 🔹 Step 4: Run Terraform Plan Again
```bash
terraform plan
```
Terraform may display:
   - Warnings about deprecated arguments (e.g., links)
   - Read-only arguments (ip_address, network_data, gateway, ip_prefix_length, id)
These read-only arguments are stored in state but cannot be set via configuration. Optional arguments with default values are also included.

### 🔹 Step 5: Remove Optional Attributes
Keep only required attributes: `image`, `name`, and `ports`.
Your configuration should look like this:
```hcl
resource "docker_container" "web" {
    image = "sha256:87a94228f133e2da99cb16d653cd1373c5b4e8689956386c1c12b60a20421a02"
    name  = "hashicorp-learn"
    ports {
        external = 8080
        internal = 80
        ip       = "0.0.0.0"
        protocol = "tcp"
    }
}
```
> 📚 Consult the Docker provider documentation for each argument and how to handle plan warnings/errors.

### 🔹 Step 6: Verify the Plan
```bash
terraform plan
```

The plan should now execute successfully. Terraform will indicate updates to attributes such as:
- attach
- logs
- must_run
- start
These attributes are default values assigned by the Docker provider and will not affect the running container.

### 🔹 Step 7: Apply the Configuration
```bash
terraform apply
```
Enter `yes` at the prompt to confirm.

Now, your configuration, Terraform state, and Docker container are fully in sync. You can manage the container using Terraform as usual.

---

## Create a Docker Image Resource

Some resources can be managed by Terraform **without using `terraform import`**, such as Docker images, which are identified by a unique ID or tag.

### 🔹 Step 1: Retrieve the Image Tag

The `docker_container.web` resource uses the SHA256 image ID. To get the human-readable tag:

```bash
docker image inspect <IMAGE-ID> -f {{.RepoTags}}
```
Replace `<IMAGE-ID>` with the ID from `docker.tf`.

### 🔹 Step 2: Add the Image Resource

Add this to your `docker.tf` file:
```hcl
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```
> ⚠️ Note: Do not replace the image in `docker_container.web` yet. Terraform will destroy and recreate the container if the image ID does not match the state.

### 🔹 Step 3: Create the Image in Terraform State
```bash
terraform apply
```
Enter `yes` to confirm.
Terraform now has a resource for the image in its state.

### 🔹 Step 4: Reference the Image in the Container

Use Gemini Code Assist to update the container resource:

Prompt:
```pgsql
Update the image argument in the docker_container resource named web within the docker.tf file. Set its value to docker_image.nginx.image_id.
```

After accepting the changes, `docker.tf` should look like:
```hcl
resource "docker_container" "web" {
    image = docker_image.nginx.image_id
    name  = "hashicorp-learn"
    ports {
        external = 8080
        internal = 80
        ip       = "0.0.0.0"
        protocol = "tcp"
    }
}
```

### 🔹 Step 5: Apply Changes
```bash
terraform apply
```
Enter `yes`.
Since `docker_image.nginx` matches the original image ID, no changes occur.
> ⚠️ If the image ID has changed since the container was created, Terraform will destroy and recreate it.

### ⚙️ Manage the Container

Change the container’s external port from 8080 → 8081:
```hcl
resource "docker_container" "web" {
  name  = "hashicorp-learn"
  image = docker_image.nginx.image_id

  ports {
    external = 8081
    internal = 80
    ip       = "0.0.0.0"
    protocol = "tcp"
  }
}
```

Apply the change:
```bash
terraform apply
```
Enter `yes.`
Terraform destroys and recreates the container with the new port.

Verify the new container:
```bash
docker ps
```
Notice the container ID has changed.

### 🗑️ Destroy the Infrastructure

Destroy both the container and the image:
```bash
terraform destroy
```
Enter yes.

Verify removal:
```bash
docker ps --filter "name=hashicorp-learn"
```

> ⚠️ Note: Terraform manages the full lifecycle. Because the image was part of the configuration, it is also removed. If another container used the same image, destroy would fail.

---

# ⚠️ Limitations and Considerations for Terraform Import

When importing resources into Terraform, keep the following in mind:

---

## 🔹 Terraform Import Limitations

- Terraform only knows the **current state** as reported by the provider. It does **not** know:
  - Whether the infrastructure is working correctly  
  - The original intent behind the infrastructure  
  - Any manual changes made outside Terraform (e.g., Docker container filesystem changes)

- Importing involves **manual steps**, which can be error-prone if the importer lacks context.

- Importing directly manipulates the Terraform **state file**.  
  - ⚠️ Always **backup** `terraform.tfstate` before importing.

- Terraform import does **not automatically detect relationships** between resources.

- Default attributes that do not need to be set are **not automatically detected**.

- **Not all providers and resources** support Terraform import.

- Importing does **not guarantee the resource can be safely destroyed or recreated**.  
  - Imported resources may depend on other unmanaged infrastructure or configurations.

---

## 🔹 Best Practices

- Follow **Infrastructure as Code (IaC)** principles, like immutable infrastructure, to avoid issues.
- Resources created manually are unlikely to follow IaC best practices.
- Consider using tools such as **Terraformer** to automate manual steps, though these are **not officially supported by HashiCorp**.

---

# 🎉 Task Completed
- Manage **Terraform backends and state** using Gemini Code Assist  
- Create **local** and **Cloud Storage backends** for your state file  
- **Refresh Terraform state** and import existing configuration  
- Edit configuration to **fully manage a Docker container** with Terraform



