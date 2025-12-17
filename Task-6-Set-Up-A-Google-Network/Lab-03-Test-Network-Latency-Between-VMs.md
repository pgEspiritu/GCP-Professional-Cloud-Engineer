## 🧪 Test Network Latency Between VMs

## 📘 Overview
When using a **Virtual Private Cloud (VPC)** network, your **VMs and subnetworks can be deployed anywhere**—across regions and zones 🌍.  
This lab focuses on **network connectivity and performance**, showing how a **multi-region, multi-zone VPC** improves availability, scalability, and speed.

Through hands-on practice, you’ll observe how Google Cloud networking enables:
- 🔁 **Automatic traffic rerouting** during regional or zonal outages
- ⚡ **Lower latency** by placing resources closer to users
- 🧠 **Intelligent routing** based on network conditions and location

You will use the **gcloud CLI** to add VMs to an existing network, then **test connectivity and latency** between them.

---

## ✅ Prerequisites
- Familiarity with **custom VPC networks** and **firewall rules** is recommended  
- (Optional) Complete **Create a Custom Network and Apply Firewall Rules** lab beforehand

---

## 🎯 What you’ll learn
- 🖥️ Add **VM instances** to an existing VPC subnet  
- 🔗 **Verify connectivity** between VMs  
- ⏱️ **Measure and compare latency** across zones  

---

## ⚙️ Setup and Requirements
Before starting the lab:
- 🌐 Use a **modern browser** (Chrome recommended)
- 🕵️‍♂️ Open the lab in **Incognito / Private mode**
- ⏳ Labs are **timed and cannot be paused**
- 🔐 Use **only the provided temporary credentials** to avoid charges

---

## 🚀 Getting Started
1. Click **Start Lab**
2. Open the **Google Cloud Console** using the provided button
3. Sign in with the **temporary username and password**
4. Accept terms and skip recovery options or free trials
5. Once logged in, access services via the **Navigation menu** or **Search bar**

---

## ☁️ Activate Cloud Shell
Cloud Shell provides a ready-to-use VM with `gcloud` installed.

- Click **Activate Cloud Shell** ☁️
- Authorize access when prompted
- Confirm your session is set to the correct **PROJECT_ID**

(Optional checks):
```bash
gcloud auth list
gcloud config list project
```

---

## 🌍 Set Your Region and Zone

Configure default compute settings for the lab:
```bash
gcloud config set compute/zone "Zone"
export ZONE=$(gcloud config get compute/zone)

gcloud config set compute/region "Region"
export REGION=$(gcloud config get compute/region)
```

![Lab 3.1](images/Lab-3.1.png)

---

## 🧪 Task 1. Connect VMs and Check Latency

In this task, you will create **three virtual machines (VMs)**—one in each subnet/zone—and verify **connectivity and network latency** between them.  
Each VM will use **network tags** required by the firewall rules to allow traffic 🔐.

---

## 🖥️ Create VM Instances

### ▶️ Create `us-test-01`
Run the following command to create a VM in the first subnet:

```bash
gcloud compute instances create us-test-01 \
  --subnet subnet-Region \
  --zone ZONE \
  --machine-type e2-standard-2 \
  --tags ssh,http,rules
```
> 📌 Important: Take note of the External IP address. You’ll need it later.

Example output:
```ini
Example output:
```

---

### ▶️ Create us-test-02 and us-test-03

Create two more VMs in their corresponding subnets:
```bash
gcloud compute instances create us-test-02 \
  --subnet subnet-REGION \
  --zone ZONE \
  --machine-type e2-standard-2 \
  --tags ssh,http,rules
```

& 

```bash
gcloud compute instances create us-test-03 \
  --subnet subnet-REGION \
  --zone ZONE \
  --machine-type e2-standard-2 \
  --tags ssh,http,rules
```

![Lab 3.4](images/Lab-3.4.png)
![Lab 3.5](images/Lab-3.5.png)

---

### 🔗 Verify VM Connectivity
▶️ SSH into us-test-01
1. Go to Compute Engine > VM instances
2. Click SSH next to us-test-01

![Lab 3.6](images/Lab-3.6.png)

---

### ▶️ Test connectivity with ping

From the SSH window of us-test-01, ping the other VMs using their external IPs.
```bash
ping -c 3 <us-test-02-external-ip-address>
```
```bash
ping -c 3 <us-test-03-external-ip-address>
```
> 📌 You can find external IPs in the Compute Engine VM list.


Example output:
```ini
64 bytes from 35.187.149.67: icmp_seq=1 ttl=76 time=152 ms
64 bytes from 35.187.149.67: icmp_seq=2 ttl=76 time=152 ms
64 bytes from 35.187.149.67: icmp_seq=3 ttl=76 time=152 ms
```
> ✔️ This confirms ICMP traffic is allowed by your firewall rules.

![Lab 3.7](images/Lab-3.7.png)

---

## ⏱️ Measure Network Latency

Latency is the time it takes for a packet to travel to another host and back (Round Trip Time / RTT), measured in milliseconds (ms).

From the SSH session on us-test-01, run:
```bash
ping -c 3 us-test-02.ZONE
```

Example output:
```ini
PING us-test-02.ZONE.c.cloud-network-module-101.internal (10.2.0.2)
64 bytes from 10.2.0.2: icmp_seq=1 ttl=64 time=105 ms
64 bytes from 10.2.0.2: icmp_seq=2 ttl=64 time=104 ms
64 bytes from 10.2.0.2: icmp_seq=3 ttl=64 time=104 ms
```
> 📌 The reported time is the RTT (Round Trip Time) between regions.

![Lab 3.8](images/Lab-3.8.png)

### 🧠 Key Concepts to Think About
- 🌍 How does latency differ between regions?
- ⚡ What’s special about connectivity between us-test-02 and us-test-03?
- 🔁 How does Google’s global network reduce cross-region latency?

### 🧾 Note: Internal DNS in Google Cloud
- Each VM uses a metadata server that acts as a DNS resolver
- VM names are automatically resolvable inside the VPC
- Internal FQDN format:
```text
hostName.[ZONE].c.[PROJECT_ID].internal
```
> ✅ You can always connect to another VM using this internal DNS name, even without external IPs.

---

## 🧭 Task 2. Traceroute and Performance Testing

**Traceroute** is a network diagnostic tool used to trace the path packets take between two hosts 🌐.  
It’s commonly requested by network and support engineers to help identify routing or latency issues.

---

## 📌 How Traceroute Works (Concept)
- Traceroute displays all **Layer 3 (routing layer)** hops between a source and destination.
- It sends packets with an increasing **TTL (Time To Live)** value:
  - Starts at TTL = 1, then 2, 3, and so on.
  - Each router decreases the TTL by 1.
  - When TTL reaches 0, the router discards the packet and sends back an **ICMP TTL Exceeded** message.
- This prevents infinite routing loops 🔁.
- Once the packet reaches the destination, the destination responds and traceroute completes.

🔎 **Packet types by OS**:
- **Linux**: Uses UDP packets to a high, unused port → destination responds with *ICMP Port Unreachable*.
- **Windows / mtr**: Uses ICMP Echo Requests → destination replies with *ICMP Echo Reply*.

---

## 🖥️ Set Up Traceroute Testing

For this task:
- Use **us-test-01** and **us-test-02**
- SSH into both VMs

---

## 🛠️ Install Network & Performance Tools
In the SSH session of **us-test-01**, install the required tools:

```bash
sudo apt-get update
sudo apt-get -y install traceroute mtr tcpdump iperf whois host dnsutils siege
```

![Lab 3.9](images/Lab-3.9.png)
![Lab 3.10](images/Lab-3.10.png)

### 🌍 Run Traceroute to an External Site

Test traceroute using a public destination:
```bash
traceroute www.icann.org
```

### 📊 Understanding the output:
- Column 1 → Hop number
- Column 2 → IP address or hostname of the hop
- Remaining columns → Round Trip Time (RTT) for packets to that hop
Each line represents a router the packet traverses.

### 🔁 Try More Traceroutes

Run traceroute to different destinations and from different VMs to compare paths and latency:

- 🖥️ VMs in the same or different regions
(eu1-vm, asia1-vm, w2-vm)
- 🌐 Public websites:
```bash
traceroute www.wikipedia.org
traceroute www.adcash.com
```
- 🐎 Fun test (increase max TTL):
```bash
traceroute -m 255 bad.horse
```
- ➕ Any other destination you want to explore

⛔ To stop traceroute at any time, press Ctrl + C.

---

### 🤔 Things to Think About
- How do traceroutes differ between regions?
- Do some hops hide their IPs or show * * *?
- Are paths shorter when destinations are geographically closer?
- How consistent are RTT values across hops?

![Lab 3.11](images/Lab-3.11.png)
![Lab 3.12](images/Lab-3.12.png)
![Lab 3.13](images/Lab-3.13.png)

---

## 🚀 Task 3. Use iperf to Test Performance

**iperf** is a network testing tool used to measure **throughput** and **latency** between two hosts 📊.  
To run an iperf test, one VM must act as the **server** and the other as the **client**.

⚠️ **Cost Note**  
The following commands transfer **gigabytes of data across regions**, which may incur **Internet egress charges**.  
If you are not on an allowlisted or free-trial project, you may skip or lightly review this section.  
*(Expected cost: < $1 USD)*

---

## 🖥️ Install Tools on us-test-02

SSH into **us-test-02** and install the required performance tools:

```bash
sudo apt-get update
sudo apt-get -y install traceroute mtr tcpdump iperf whois host dnsutils siege
```

### 🖧 Run iperf (Basic TCP Test)
Start the iperf server (us-test-01)

SSH into us-test-01 and run:
```bash
iperf -s
```

![Lab 3.14](images/Lab-3.14.png)

Run the iperf client (us-test-02)

From us-test-02, run:
```bash
iperf -c us-test-01.ZONE
```

![Lab 3.15](images/Lab-3.15.png)
![Lab 3.16](images/Lab-3.16.png)

📈 Sample Output
```bash
[  3]  0.0-10.0 sec   298 MBytes   249 Mbits/sec
```

![Lab 3.17](images/Lab-3.17.png)

- Transfer → Total data sent
- Bandwidth → Measured throughput
> When finished, stop the server on us-test-01 with Ctrl + C.


---

### 🏙️ Performance Between VMs in the Same Region

Now deploy another VM (us-test-04) in a different zone but same region as us-test-01.
Within a region, bandwidth is limited by the 2 Gbit/s per core egress cap.

Create us-test-04

In Cloud Shell:
```bash
gcloud compute instances create us-test-04 \
--subnet subnet-REGION \
--zone ZONE \
--tags ssh,http
```

![Lab 3.18](images/Lab-3.18.png)

Install tools on us-test-04

SSH into us-test-04 and run:
```bash
sudo apt-get update
sudo apt-get -y install traceroute mtr tcpdump iperf whois host dnsutils siege
```

![Lab 3.19](images/Lab-3.19.png)

---

### 🌍 Improving Performance Between Regions

Between regions, bandwidth is typically lower due to:
- TCP window size limits
- Single-stream TCP performance
To increase throughput, try UDP or parallel TCP streams.

### 📡 UDP iperf Test
Start UDP server (us-test-02)
```bash
iperf -s -u
```

![Lab 3.20](images/Lab-3.20.png)

Run UDP client (us-test-01)
```bash
iperf -c us-test-02.ZONE -u -b 2G
```
> 🚀 This often achieves higher throughput than a single TCP stream.

![Lab 3.21](images/Lab-3.21.png)


back to us-test-02

![Lab 3.22](images/Lab-3.22.png)

---

### 🔀 Parallel TCP Streams Test
Start server (us-test-01)
```bash
iperf -s
```

![Lab 3.23](images/Lab-3.23.png)

Run client with parallel streams (us-test-02)
```bash
iperf -c us-test-01.ZONE -P 20
```
> 📊 The combined bandwidth should be close to the maximum achievable for the network path.

![Lab 3.24](images/Lab-3.24.png)
![Lab 3.25](images/Lab-3.25.png)

back to us-test-01

![Lab 3.26](images/Lab-3.26.png)
![Lab 3.27](images/Lab-3.27.png)

---

### 🧠 Key Takeaways

- A single TCP stream rarely achieves maximum bandwidth.
- Performance is affected by:
  - TCP window size
  - TCP slow start
  - Latency between regions
- Using parallel TCP sessions or UDP improves throughput.
- Tools like bbcp can optimize large file transfers via parallelization.
📘 For deeper understanding, see TCP/IP Illustrated.

---

## 🎉 Task Completed 

You have learned how to use **gcloud** to build virtual machines on an existing network 🖥️🌐, and how to use a variety of tools to test **connectivity** and **network latency** 📡⏱️.
