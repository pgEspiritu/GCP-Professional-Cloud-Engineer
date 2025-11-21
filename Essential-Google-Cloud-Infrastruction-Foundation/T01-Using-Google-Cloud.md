# 🌥️ Ways to Interact with Google Cloud

Google Cloud provides **four main ways** to interact with its services.

## 🖥️ Google Cloud Console (Web UI)

- A **web-based graphical interface** accessed at `console.cloud.google.com`.
- Lets you view and manage resources like Virtual Machine instances.
- Example: View VM details directly in the UI.

---

## 💻 Cloud Shell & Google Cloud CLI (Terminal)

### 🌐 Cloud Shell
- A **browser-based interactive shell** inside the Google Cloud Console.
- Runs on a temporary VM with **5 GB persistent storage**.
- Comes with the **Google Cloud CLI pre-installed**.

### 🔧 Google Cloud CLI (gcloud)
- A command-line tool for interacting with Google Cloud.
- Example:
  ```bash
  gcloud compute instances list
  ```

### 🧑‍💻 Google Cloud APIs & Client Libraries
📦 App APIs
- Provide access to Google Cloud services.
- Optimized for languages like Python, Node.js, and more.

🛠️ Admin APIs
- Used for resource management.
- Helpful when building automation tools.

### 📱 Cloud Mobile App

Available on Android and iOS.

You can:
- Start, stop, and SSH into Compute Engine instances.
- View VM logs.
- Monitor key metrics (CPU, network, requests per second, errors).
- Receive alerts and handle incidents.
- Access up-to-date billing info and enable budget alerts.
