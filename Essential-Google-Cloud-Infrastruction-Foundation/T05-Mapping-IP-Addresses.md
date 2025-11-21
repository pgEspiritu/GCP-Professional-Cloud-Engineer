# 🌐 IP Addresses, DNS, and Alias IPs in Google Cloud

## 🖥️ External IPs Are Transparent

- Regardless of **ephemeral** or **static** IPs 🌍:
  - The **VM OS does not see the external IP**.
  - The external IP is mapped **transparently by VPC** to the internal IP.
- Example: Running `ifconfig` inside a VM only shows the **internal IP** 🏠.

---

## 🔹 DNS Resolution

### Internal DNS 🏠

- Google Cloud provides **two types** of internal DNS names:
  1. **Zonal DNS** – scoped to a zone. Recommended ✅ for reliability.
  2. **Global (Project-wide) DNS** – spans the entire project.

- Each VM instance has:
  - **Hostname:** same as instance name.
  - **Internal FQDN:** fully qualified domain name (format shown in console).

- Notes:
  - Recreating an instance can **change its internal IP**, but the DNS name always points to the instance.
  - **Metadata server** on each instance handles DNS queries:
    - Local network queries: resolved internally.
    - External queries: routed to Google’s public DNS servers.

---

### External DNS 🌍

- VMs with external IPs allow connections from **outside the project**.
- Public DNS records for these instances are **not automatically published**.
- Admins can use existing DNS servers or **Cloud DNS**.

---

## ☁️ Cloud DNS

- Managed, authoritative **Domain Name System (DNS)** service.
- Features:
  - Scalable and reliable.
  - Runs on Google’s **global Anycast network**.
  - **High availability** – 100% SLA.
  - Easy to manage millions of DNS records via:
    - UI
    - CLI
    - API

- Purpose: Translates domain names (e.g., `google.com`) into IP addresses efficiently and reliably.

---

## 🔹 Alias IP Ranges

- Assign a **range of internal IPs** to a VM's **network interface**.
- Useful when multiple services run on a single VM.
- Benefits:
  - Multiple IPs per VM without additional network interfaces.
  - Each service/container can have its own IP.
- Alias IP ranges are drawn from:
  - Primary subnet CIDR
  - Secondary subnet CIDR

- Example: Primary and secondary CIDR ranges mapped to VM alias IPs 🖧

---

## 💡 Key Points

1. External IPs are **invisible to the VM**; VPC handles the mapping.
2. Use **zonal internal DNS** for reliability.
3. Cloud DNS is **highly available and managed**, reducing DNS management overhead.
4. Alias IP ranges provide **flexibility for multiple services** on a single VM.

