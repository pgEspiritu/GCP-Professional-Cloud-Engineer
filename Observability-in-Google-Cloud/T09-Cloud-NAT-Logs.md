# Cloud NAT Logs

- **Cloud NAT**: Managed Network Address Translation service allowing VMs without public IPs to access the internet for updates, patching, and provisioning.
- **Benefits**:
  - Reduces the need for external IPs.
  - Automatically scales NAT IPs.
  - Supports managed instance groups, including autoscaling.
  - Distributed, software-defined service, not tied to a single gateway.
- **Configuration**:
  - NAT gateway is configured on a **Cloud Router**, which holds NAT parameters.

## Logging Features
- Logs TCP and UDP connections and errors.
- Can log:
  - **Connection creation** using Cloud NAT.
  - **Egress packet drops** due to unavailable NAT ports.
- Logs can be filtered to log either or both events.
- Maximum log rate: **50-100 events per vCPU** before filtering.

## Accessing Logs
- Use **Logs Explorer**.
- Filter by **Cloud NAT Gateway resource** and optionally by region or gateway.
