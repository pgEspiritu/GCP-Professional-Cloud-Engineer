# 🔥 Firewall Rules Logging

Another essential part of knowing what's happening at the VPC network level is knowing what the firewall rules are doing.  
VPC firewall rules let you allow or deny connections to or from your virtual machine (VM) instances based on a configuration that you specify.

Enabled VPC firewall rules are always enforced and protect your instances regardless of their configuration and operating system, even if they didn’t start. 🛡️

Firewall Rules Logging lets you audit, verify, and analyze the effects of your firewall rules.

It can help answer questions like:
- ❓ Did my firewall rules cause that application outage?
- 📊 How many connections match the rule I just created?
- 🚦 Are my firewall rules stopping (or allowing) the correct traffic?

By default, Firewall Rules Logging is disabled.  
You can enable it on a per-rule basis.

In the slide screenshot example, you’re editing the firewall rule named **enable-rdp**.  
Selecting the radio button will enable firewall rules.

⚠️ Note: Firewall Rules Logging can only record TCP and UDP connections.  
For other protocols, use Packet Mirroring.

⚠️ Caution: Firewall Rules Logging can generate a lot of data, which might have a cost implication.

Firewall Rules Logging can also be activated on existing firewall rules using the CLI:

```bash
gcloud compute firewall-rules update [NAME] --enable-logging
gcloud compute firewall-rules update [NAME] --no-enable-logging
```

Like all Google Cloud logs, use Logs Explorer to view logs in real time or configure exports.
To filter for firewall logs and network policy firewall logs, below the Compute Engine resource, select firewall.

Many are familiar with classic segmentation or gateway-centric firewalls.

In a traditional private network scenario, a firewall sits at the boundary between the private network and the internet.
A segmentation firewall is designed to segment and secure the protected network from an outside insecure network.

Google Cloud VPC firewalls are micro-segmentation firewalls.
These firewalls function like many micro-firewalls, each operating at the NIC of every VM connected to the VPC.
The micro-firewalls can grant or deny any configured incoming or outgoing traffic.

### 🐞 Troubleshooting Example

Imagine two different web servers.
After some configuration changes by a DevOps team, the web servers can no longer access the shared application server.

If the connectivity issue is firewall-related, there are two possibilities:

1. ❌ A firewall rule is actively blocking the incoming connections.
2. 🚫 Network traffic is blocked by default, and a rule is missing or misconfigured.
Logging all denied connections would generate too much data, so instead:

#### ✅ Step 1: Create a temporary high-priority allow rule
Enable Cloud Logging so entries are visible.
Traffic suddenly gets through → it’s firewall-related.

#### 🔍 Step 2: Examine the log entries
Look at the existing rule that should be allowing the traffic.
The rule is based on a network tag: webserver
The actual VMs are using: web-server
This mismatch is the root cause of the issue. 🔧
