# 🏗️ Infrastructure as Code (IaC) with Terraform on Google Cloud

Google Cloud offers multiple ways to create resources:
- **Cloud Console:** Recommended for beginners or those who prefer a UI.
- **Cloud Shell:** CLI-based, faster for users comfortable with commands.
- **Terraform:** Declarative Infrastructure as Code for reproducible deployments.

---

## 1️⃣ What is Infrastructure as Code?

- Enables **quick provisioning and removal** of infrastructure.
- Supports **on-demand deployments** integrated into CI/CD pipelines.
- Automates infrastructure provisioning and manages deployment complexity in code.
- Changes are centralized, enabling **replication of dev/test environments** and easy cleanup.

> ⚡ Benefits: Consistent, repeatable deployments; easy teardown; flexible infrastructure management.

---

## 2️⃣ Terraform Overview

- Terraform is a **tool for Infrastructure as Code (IaC)**.
- Google Cloud supports Terraform for provisioning:
  - Compute Engine instances
  - Containers
  - Storage buckets
  - Networking (VPCs, firewall rules, VPNs, Cloud Routers, Load Balancers)
- Uses **declarative configuration files** to describe resources.
- Written in **HashiCorp Configuration Language (HCL)**:
  - **Blocks**: Represent objects (e.g., resources, providers)
  - **Arguments**: Assign values to names
  - **Expressions**: Values assigned to identifiers
- Supports **modularization** for reusable components across deployments.
- Deployments are **parallelized** for efficiency.
- Terraform uses **Google Cloud APIs** under the hood.

> ⚡ Advantage: Declarative approach specifies *what* to create, not *how*.

---

## 3️⃣ Terraform Configuration

A Terraform configuration is a set of files that describe the infrastructure. Typical blocks include:

| Block Type     | Purpose                                                                 |
|----------------|------------------------------------------------------------------------|
| **provider**   | Specifies the cloud provider (e.g., Google Cloud) and region           |
| **resource**   | Defines infrastructure objects (VMs, storage buckets, networks, etc.)  |
| **output**     | Specifies output variables from the configuration                      |

- Configurations can span **multiple files and directories** for larger deployments.

---

## 4️⃣ Example: HTTP Network with Firewall

### `main.tf` Overview
```hcl
# Define provider
provider "google" {
  project = "my-gcp-project"
  region  = "us-central1"
}

# Create auto mode network
resource "google_compute_network" "vpc_network" {
  name                    = "my-auto-network"
  auto_create_subnetworks = true
  mtu                     = 1460
}

# Create firewall rule
resource "google_compute_firewall" "http_firewall" {
  name    = "allow-http"
  network = google_compute_network.vpc_network.name
  allow {
    protocol = "tcp"
    ports    = ["80", "8080"]
  }
}
```

## 5️⃣ Terraform Workflow

1. Initialize configuration
```bash
terraform init
```
- Downloads required provider plugins.
- Creates bookkeeping files.

2. Preview execution plan
```bash
terraform plan
```
- Checks actions needed to achieve the desired state.
- No changes applied yet.

3. Apply configuration
```bash
terraform apply
```
- Creates all defined infrastructure in the configuration.
- Infrastructure is now accessible.
> ⚡ Terraform allows repeatable deployments and easy teardown with a single command.


## 6️⃣ Key Advantages of Terraform on Google Cloud
- Declarative, not imperative
- Supports almost all GCP resources
- Repeatable, consistent deployments
- Modular and reusable configurations
- Parallelized resource provisioning
