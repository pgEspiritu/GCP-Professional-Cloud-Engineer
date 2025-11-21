# 💰 Google Cloud Network Pricing

---

## 🔹 Understanding Network Charges

- Important to know **when GCP charges for network traffic**.
- Reference: **Compute Engine documentation** 📄

---

## 🌐 Egress Traffic

- **Ingress (traffic coming into GCP)** is generally **not charged** unless:
  - Traffic goes through a **resource** (e.g., load balancer) that processes egress.
- **Egress (traffic leaving a VM)**:
  - **Same zone via internal IP** → free ✅
  - **To Google products** (YouTube, Maps, Drive) or **other GCP services in the same region** → free ✅
  - **Between zones in the same region** → charged 💸
  - **Through external IP** → charged 💸
  - **Between regions** → charged 💸

- Note: **External IP traffic** cannot determine VM zones, so it may be treated like inter-zone egress.

---

## 🆔 External IP Pricing

- Charges apply for **static and ephemeral external IP addresses**.
- **Static external IP not assigned to a resource** → higher rate ⚠️
- Preemptible VMs with external IP → lower charges compared to standard VMs.
- Pricing is subject to change; always refer to official documentation 📚.

---

## 🧮 GCP Pricing Calculator

- Web-based tool to **estimate costs** for a collection of GCP resources.
- Specify:
  - Instance type 🖥️
  - Region 🌍
  - Monthly egress traffic 📦
- Calculator provides **total estimated cost**.
- You can:
  - Adjust **currency** 💵
  - Adjust **time frame** ⏳
  - Save the estimate or **email** it for future reference ✉️

- Recommended: Always use the pricing calculator to plan your network and avoid surprises.

---

💡 **Tips**

- Traffic within a region or internal IP communication is mostly free.
- External IP and cross-region traffic incur charges.
- Pricing varies for preemptible vs. standard VMs.
- Always double-check with **GCP documentation** or **pricing calculator**.
