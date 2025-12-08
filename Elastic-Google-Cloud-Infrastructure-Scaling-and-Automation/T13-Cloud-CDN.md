# 🌐 Cloud CDN

Cloud CDN (Content Delivery Network) **caches HTTP(S) content at Google's edge locations** to reduce latency and improve performance for users globally. It works seamlessly with **HTTP(S) Load Balancing**.

---

## 1️⃣ Key Features

- Caches **static and dynamic content** close to users 🌍  
- Uses **Google’s global edge network** for faster content delivery ⚡  
- **Reduces load** on backend instances  
- Supports **HTTP/HTTPS** traffic  
- Works with **backend services and backend buckets** used by HTTP(S) Load Balancers  

---

## 2️⃣ How it Works

1. User requests content from your application 🌐  
2. Cloud CDN checks if the requested content is cached at the nearest **edge location**  
   - If cached → content is served immediately ✅  
   - If not cached → request is forwarded to the **origin backend** (instance group or backend bucket)  
3. Response is cached at the edge for **subsequent requests**  

---

## 3️⃣ Configuration Highlights

- Enable CDN on **backend services** in your HTTP(S) Load Balancer  
- Control cache behavior with:  
  - **Cache keys** (URL, query string, headers, etc.)  
  - **TTL (Time-to-Live)** values  
  - **Cache modes** (Cache all content, Cache static content, Bypass cache)  
- Integrates with **signed URLs and signed cookies** for secure content delivery 🔐  

---

## 4️⃣ Benefits

- ⚡ **Faster load times** for users worldwide  
- 💰 **Reduced backend load and costs**  
- 🌍 **Global reach** using Google’s edge locations  
- 🔄 **Automatic cache invalidation** and refresh  

> ⚡ More on Cloud CDN: see Google Cloud documentation for caching strategies and edge location details.
