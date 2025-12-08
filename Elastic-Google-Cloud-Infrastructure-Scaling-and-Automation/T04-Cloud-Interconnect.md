# 🌐 Cloud Interconnect Services

Google Cloud offers multiple **interconnect options** to connect your on-premises network to Google’s network. These services provide higher bandwidth, lower latency, and internal IP access compared to VPN.

---

## 1️⃣ Dedicated Interconnect

- **Definition:** Direct **physical connection** between your on-premises network and Google’s network 🔗  
- **Use case:** Transfer large amounts of data cost-effectively  
- **Requirements:**  
  - Provision a **cross-connect** between Google’s network and your router in a supported **colocation facility** 🏢  
  - Configure a **BGP session** over the interconnect between Cloud Router and on-premises router  
- **SLA:** Can be configured for **99.9%** or **99.99%** uptime ✅  
- **Capacity:** 10 Gbps or 100 Gbps per link  
- **Scaling:** Up to 8 links for 10 Gbps multiples, up to 2 links for 100 Gbps multiples  
- **Documentation:** [Dedicated Interconnect SLA & redundancy](https://cloud.google.com/interconnect/docs/concepts/dedicated-overview#redundancy)  
- **Colocation facilities:** [Locations list](https://cloud.google.com/interconnect/docs/concepts/colocation-facilities)

---

## 2️⃣ Partner Interconnect

- **Definition:** Connects your on-premises network to Google Cloud through a **supported service provider** 🤝  
- **Use case:** For locations far from Google colocation facilities or smaller data needs  
- **Requirements:**  
  - Work with a **service provider** that has existing connections to Google  
  - Establish a **BGP session** between Cloud Router and on-premises router  
- **SLA:** Can be configured for **99.9%** or **99.99%** uptime ✅  
- **Capacity:** 50 Mbps to 50 Gbps per connection  
- **Documentation:** [Partner Interconnect SLA & redundancy](https://cloud.google.com/interconnect/docs/concepts/partner-overview#redundancy)  

---

## 3️⃣ Cross-Cloud Interconnect

- **Definition:** High-bandwidth dedicated connectivity between Google Cloud and **another cloud provider** 🌩️  
- **Supported providers:** AWS, Microsoft Azure, Oracle Cloud, Alibaba Cloud  
- **Use case:** Multi-cloud strategy, site-to-site data transfer, encrypted traffic  
- **Capacity:** 10 Gbps or 100 Gbps  
- **Setup:**  
  - Identify supported locations for Google connections  
  - Purchase **primary and redundant ports** from both Google and the other cloud provider  
- **Limitations:** Google only supports connectivity up to the other provider’s network; uptime and support are the responsibility of the other provider  

---

## 4️⃣ Comparison of Connectivity Options

| Option                     | Capacity                  | Requirements                                         | Use case |
|-----------------------------|--------------------------|-----------------------------------------------------|----------|
| **Cloud VPN**               | 1.5–3 Gbps per tunnel     | VPN device on-premises                               | Low cost, experimental workloads |
| **Dedicated Interconnect**  | 10–100 Gbps per link      | Google colocation facility                            | Enterprise-grade, high throughput |
| **Partner Interconnect**    | 50 Mbps–50 Gbps           | Service provider                                     | Medium-to-high throughput where colocation is not feasible |
| **Cross-Cloud Interconnect**| 10–100 Gbps               | Supported cloud provider                              | Multi-cloud connectivity |

---

## 5️⃣ Recommendations

- **Cloud VPN:** For lower cost, lower bandwidth, or experimentation  
- **Dedicated / Partner Interconnect:** For enterprise-grade connections with high throughput  
- **Cross-Cloud Interconnect:** To connect Google Cloud with another cloud provider  
- **Direct / Carrier Peering:** Only for specific use cases; Google recommends using **Cloud Interconnect** in most scenarios ✅
