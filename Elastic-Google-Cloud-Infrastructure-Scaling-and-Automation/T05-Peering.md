# 🌐 Cloud Peering Services

Google Cloud offers **Peering services** for direct connectivity to Google and Google Cloud properties:  

- **Direct Peering**  
- **Carrier Peering**  

These services provide **public IP access** to Google services but **do not include an SLA**.

---

## 1️⃣ Direct Peering

- **Definition:** Direct connection between your business network and Google's network 🔗  
- **Use case:** Exchange internet traffic and access all Google Cloud services  
- **Setup:**  
  - Exchange **BGP routes** between your network and Google  
  - Connect at one of Google’s **Edge Points of Presence (PoPs)**  
- **Capacity:** 10 Gbps per link  
- **Requirements:** Must meet Google’s peering requirements (see Peering DB entries)  
- **Google PoPs:**  
  - Present at **90+ Internet exchanges**  
  - Present at **100+ interconnection facilities** worldwide 🌍  

> Direct peering allows full access to Google Cloud products but **does not have an SLA**.  

---

## 2️⃣ Carrier Peering

- **Definition:** Connect to Google’s public infrastructure via a **service provider** 🤝  
- **Use case:** When you cannot reach a GCP PoP or meet direct peering requirements  
- **Setup:** Work with a **carrier partner** to establish the connection and satisfy their requirements  
- **Capacity:** Varies depending on the service provider  
- **SLA:** None  

---

## 3️⃣ Peering Comparison

| Option           | Capacity       | Requirements                           | SLA       | Use case |
|-----------------|----------------|---------------------------------------|-----------|----------|
| **Direct Peering**  | 10 Gbps/link  | Connection to GCP Edge PoP             | None      | Full Google services access if near PoP |
| **Carrier Peering** | Varies       | Partner service provider requirements  | None      | Access Google public services when PoP unreachable |

> 💡 Both options provide **public IP address access** to all Google services. The main differences are **capacity** and **connection requirements**.
