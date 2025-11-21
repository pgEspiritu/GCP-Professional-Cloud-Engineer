# 🛣️ Routes and Firewall Rules in Google Cloud

---

## 🔹 Routing Overview

- Every network in GCP has **default routes**:
  - Enable VMs to communicate **within the network**, even across subnets.
  - Default route directs packets **outside the network** 🌐.
- Custom routes can **override defaults**, but:
  - Firewall rules must also allow the packet 🔒.

- **Default network firewall rules**:
  - Allow all instances to communicate internally.
- **Manually created networks**:
  - Require **custom firewall rules**.

---

## 📋 How Routes Work

- Routes match packets by **destination IP addresses**.
- A route is created:
  - When a **network** is created.
  - When a **subnet** is created.
- Routes may apply to:
  - Specific instances (based on network and instance tags).
  - All instances in a network (if no tags specified).
- Compute Engine creates **read-only routing tables** for each VM instance.
- Each VM is connected to a **massively scalable virtual router** 🖧:
  - Determines **next hop** for outgoing packets.

---

## 🔥 Firewall Rules Overview

- Protect VM instances from **unapproved inbound (ingress) and outbound (egress) connections**.
- Every VPC network functions as a **distributed firewall**.
- Rules are **applied at the instance level**.
- **Stateful firewall**:
  - Once a session is allowed, traffic is allowed in **both directions** 🔄.

- If all firewall rules are deleted:
  - **Default implied rules**:
    - Deny all ingress.
    - Allow all egress.

---

## ⚙️ Firewall Rule Components

1. **Direction**:
   - Inbound → matched against ingress rules.
   - Outbound → matched against egress rules.
2. **Source or Destination**:
   - Ingress → source of the connection.
   - Egress → destination of the connection.
3. **Protocol & Port**:
   - Can restrict to specific protocols or combinations of protocols and ports.
4. **Action**:
   - Allow ✅ or Deny ❌ packets matching the rule.
5. **Priority**:
   - Determines evaluation order; **first match applies**.
6. **Rule Assignment**:
   - Default: all instances.
   - Can assign to specific instances only.

---

## 🔹 Use Cases

### Egress Rules 🌐

- Control **outgoing connections** from your GCP network.
- **Allow rules** → permit specific protocols, ports, and IPs.
- **Deny rules** → block undesired connections.
- **Destination ranges** can protect:
  - External hosts 🌍
  - Internal VM instances within specific subnets 🏠

### Ingress Rules 🛡️

- Protect VMs from **incoming connections**.
- **Allow rules** → permit specific protocols, ports, and source IP ranges.
- Can restrict rules to:
  - Specific **source IP CIDR ranges**.
  - Internal GCP ranges.
- Controls connections from:
  - External networks 🌎
  - Other VMs in the same network 🖥️

---

💡 **Key Points**

- Routes and firewall rules **work together**: a packet must match **both**.
- Firewall rules are **stateful**, so sessions allow bidirectional traffic.
- Use **CIDR ranges** to define precise traffic controls.
