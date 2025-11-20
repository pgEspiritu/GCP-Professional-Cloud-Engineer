# 🌤️ Development in the Cloud: Event-Driven Applications with Cloud Run Functions

## 📸 Example: Image Upload Workflow

Consider an application where users upload images.  
When a new image is uploaded, several tasks may need to happen:

- 🖼 Convert the image to a standard format  
- 🧩 Generate thumbnails in multiple sizes  
- 📦 Store the processed files in a repository  

You *could* build this directly into your main application, but then you’d need to allocate compute resources **all the time**, even if uploads happen only once in a while.

---

## ⚡ Enter Cloud Run Functions

**Cloud Run functions** let you write **single-purpose**, event-driven functions that run automatically when triggered.

### ✨ Key Characteristics

- 🪶 **Lightweight**  
- ⚙️ **Event-based**  
- 🔄 **Asynchronous**  
- ☁️ **Serverless (no infrastructure to manage)**  
- 🎯 **Single-purpose logic**  

Cloud Run functions respond to cloud events, such as:

- A new file in **Cloud Storage**  
- A message in **Pub/Sub**  
- An incoming **HTTP request**  

You only pay for the **exact time your code runs**, billed to the nearest **100 ms**.

---

## 🧱 Build Workflows from Functions

Cloud Run functions enable you to chain together small, focused tasks to construct entire application workflows—for example:

- Transform an image  
- Update a database  
- Notify a user  
- Trigger another service  

This keeps applications modular, efficient, and highly scalable.

---

## 🧑‍💻 Supported Languages

Cloud Run functions support source code written in:

- Node.js  
- Python  
- Go  
- Java  
- .NET Core  
- Ruby  
- PHP  

For specific runtime versions, see the **Cloud Run runtimes documentation**.

---

## 🔔 Supported Event Triggers

Cloud Run functions can be triggered by:

### 🔄 **Asynchronous Events**
- 📂 **Cloud Storage** events (e.g., file upload, delete)  
- 📮 **Pub/Sub** messages  

### 🌐 **Synchronous Events**
- **HTTP invocations**  

---

Cloud Run functions make it easier to build small, efficient pieces of cloud-native logic that automatically respond to events—without managing servers, scaling, or runtime environments.
