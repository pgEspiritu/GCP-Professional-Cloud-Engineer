# 🌐 Choosing a Google Cloud Load Balancer

Selecting the right load balancer depends on **traffic type, protocol, application requirements, and regional/global deployment needs**.

---

## 1️⃣ IPv6 Support

- **Supported by:**  
  - HTTP(S) Load Balancing  
  - SSL Proxy Load Balancing  
  - TCP Proxy Load Balancing  

- **Functionality:**  
  - IPv6 client requests are terminated at the load balancer  
  - Requests are proxied over IPv4 to backend instances  
  - Responses are translated back to IPv6 for the client  
  - Backends appear as IPv6 applications to IPv6 clients

---

## 2️⃣ Traffic Type Considerations

| Load Balancer Type | Use Case / Traffic Type |
|------------------|-----------------------|
| **Application Load Balancer (HTTP(S))** | Flexible feature set for HTTP(S) traffic, global distribution, URL-based routing |
| **Proxy Network Load Balancer (TCP / SSL)** | TLS offload, TCP proxy, multi-region backends, external traffic |
| **Passthrough Network Load Balancer** | Preserve client source IP, lower overhead (no proxy), supports UDP, ESP, ICMP, external traffic |

---

## 3️⃣ Deployment Considerations

- **External vs Internal:** Does your application need to be internet-facing or private to a VPC?  
- **Global vs Regional:** Do your backends need global reach or regional placement?  
- **Managed vs Unmanaged:**  
  - **MANAGED** indicates the load balancer is a fully managed service on **Google Front Ends** or **Envoy proxies**  
  - Requests are routed automatically to Google Front Ends or Envoy proxies  

---

## 4️⃣ Additional Notes

- The **load-balancing scheme** is defined in the **forwarding rule** and **backend service**  
- Determines whether the load balancer handles **internal or external traffic**  
- For more information on **Network Service Tiers**, refer to Google Cloud documentation

---

> ⚡ Summary: Choose your load balancer based on traffic type (HTTP(S), TCP, UDP), client connectivity (IPv4/IPv6), application location (internal/external), and backend deployment (regional/global).
