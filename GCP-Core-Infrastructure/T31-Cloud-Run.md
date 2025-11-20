# 🚀 Cloud Run

## ☁️ What is Cloud Run?

**Cloud Run** is a *fully managed, serverless compute platform* that runs **stateless containers** triggered by:

- 🌐 Web requests  
- 🔔 Pub/Sub events  

Cloud Run is built on **Knative**, which itself runs on Kubernetes, and can be deployed:

- ✔ Fully managed on Google Cloud  
- ✔ On Google Kubernetes Engine (GKE)  
- ✔ Anywhere Knative runs  

---

## ⚡ Key Benefits of Cloud Run

### 🟣 Serverless
- No infrastructure management.
- You focus **entirely on application code**.

### 🟢 Fast Scaling
- Scales to **zero** when idle.
- Scales **instantly** on demand.
- Billing is based on usage down to **100 milliseconds**.

### 🔐 Secure and Managed
- Cloud Run handles:
  - HTTPS endpoints  
  - Certificate management  
  - Autoscaling  
  - Monitoring  

---

## 🛠 Cloud Run Developer Workflow (3 Steps)

1. **Write your application**  
   - Use any language you want.  
   - Must start a server that listens for requests.

2. **Build & package into a container image**  
   - Docker or any container build tool.

3. **Push the image to Artifact Registry**  
   - Cloud Run deploys your image from there.

After deployment, Cloud Run gives you a **unique HTTPS URL** 🌍.

Cloud Run automatically:
- Starts containers when requests arrive  
- Stops them when idle  
- Handles auto-scaling  

---

## 💡 Two Ways to Deploy

### 1️⃣ **Container-based workflow**
- You build and manage your own container images.

### 2️⃣ **Source-based workflow**
- You deploy *source code* directly.
- Cloud Run uses **Buildpacks** to:
  - Build your code  
  - Package it into a secure container image  
  - Deploy automatically  

Great if you want simplicity and consistent builds.

---

## 🔐 HTTPS Handling

Cloud Run manages **TLS/SSL** for you automatically.  
You only handle **HTTP logic**, and Cloud Run takes care of encryption and certificates.

---

## 💵 Pricing Model

You only pay for:

- CPU time  
- Memory usage  
- During:
  - Request processing  
  - Container startup/shutdown  
- Plus a small fee for every **1 million requests**

💡 **If your app receives no requests → you pay nothing.**

---

## 🧑‍💻 Supported Languages

Cloud Run can run *any* 64-bit Linux binary, including:

- Java  
- Python  
- Node.js  
- PHP  
- Go  
- C++  

And even uncommon languages like:

- COBOL  
- Haskell  
- Perl  

As long as your app **listens for web requests**, it works on Cloud Run.

