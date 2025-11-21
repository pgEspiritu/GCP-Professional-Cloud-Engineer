# 🌐 IP Addresses in Google Cloud

Let's explore how **IP addresses** work in Google Cloud for virtual machines (VMs) and services.

---

## 🖥️ Each VM has Two IP Addresses

1. **Internal IP Address** 🏠
   - Assigned **automatically via DHCP** within the network.
   - Every VM and dependent services (e.g., App Engine, GKE) get an internal IP.
   - **DNS registration:**  
     - Each VM name is registered with an **internal DNS** that translates symbolic names to internal IPs.
     - **Scoped to the network:** Only resolves names within the same network.
   - Usage: For **internal communication** between VMs and services.

2. **External IP Address** 🌍
   - Optional: only required if VM is **externally facing**.
   - Two types:
     - **Ephemeral:** Assigned from a pool temporarily.
     - **Static:** Reserved external IP that does not change.
   - **Billing:**  
     - Reserved static IPs **not in use** incur **higher charges**.
     - Static or ephemeral IPs **in use** are billed at standard rates.
   - Advanced usage: You can bring your own **publicly routable IP prefixes**:
     - Must be **/24 block or larger**  
     - Can advertise these IPs on the Internet

---

## 🔹 Summary

| IP Type       | Scope             | Assignment     | Usage                        | Notes                             |
|---------------|-----------------|----------------|-------------------------------|----------------------------------|
| Internal 🏠   | Network-local    | DHCP           | Internal communication       | DNS resolves VM names internally |
| External 🌍   | Internet-facing  | Ephemeral/Static | Access from outside the network | Static unused IPs cost more      |

---

## 💡 Key Points
- Internal IPs are **automatic** and essential for GCP services.
- External IPs are **optional** and can be ephemeral or static.
- Bringing your own IP addresses requires owning **/24 block or larger**.
