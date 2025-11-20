# 📨 Pub/Sub: Qwik Start – Python  

## 🧭 Overview
Google Cloud **Pub/Sub** allows applications to exchange messages **reliably**, **quickly**, and **asynchronously**.  
A **publisher** sends messages to a **topic**, and a **subscriber** pulls messages from a **subscription** linked to that topic.

🕒 Messages are stored for up to **7 days** if undelivered.

In this lab, you will learn how to publish and retrieve Pub/Sub messages using **Python**.

---

## 🎯 What You'll Do
- 📚 Learn the basics of Pub/Sub  
- 🧵 Create, delete, and list Pub/Sub **topics** and **subscriptions**  
- ✉️ Publish messages to a topic  
- 📥 Use a pull subscriber to read individual messages  

---

## 🛠️ Setup and Requirements

### ⚠️ Before You Click **Start Lab**
- ⏳ Labs are timed—you cannot pause  
- 🧪 You receive temporary credentials for the real Google Cloud environment  
- 🌐 Use Chrome in **Incognito Mode** to avoid account conflicts  

### ✔️ Requirements
- Internet browser  
- Time to finish the lab  
- Use **only** the provided student account (your own account may incur charges)

---

## 🚀 Start Your Lab & Sign In
1. Click **Start Lab**  
2. Click **Open Google Cloud Console**  
3. If prompted, choose **Use another account**  
4. Copy/paste the **Username** ➜ click **Next**  
5. Copy/paste the **Password** ➜ click **Next**  

⚠️ Do NOT add recovery info, 2FA, or use free trial.

---

## 💻 Activate Cloud Shell
Cloud Shell provides:
- A VM with dev tools  
- Preinstalled gcloud CLI  
- Persistent 5GB home directory  

### ▶️ Steps
1. Click **Activate Cloud Shell** 💬  
2. Continue through setup dialogs  
3. Authorize access  

Check active account:
```bash
gcloud auth list
```

Check project ID:
```bash
gcloud config list project
```

---

## 🧪 Task 1 — Create a Virtual Environment 🐍

Install virtualenv:
```bash
sudo apt-get install -y virtualenv
```

Create environment:
```bash
python3 -m venv venv
```

Activate it:
```bash
source venv/bin/activate
```

---

## 🧪 Task 2 — Install the Client Library 📦

Install Google Pub/Sub client:
```bash
pip install --upgrade google-cloud-pubsub
```

Clone samples:
```bash
git clone https://github.com/googleapis/python-pubsub.git
```

Go to snippets:
```bash
cd python-pubsub/samples/snippets
```

---

## 🧪 Task 3 — Pub/Sub Basics 📬
What is Pub/Sub?
- 💬 Topics → Message channels
- 📤 Publishers → Send messages
- 📥 Subscribers → Pull messages
- ⏰ Messages must be acknowledged

Pub/Sub is already installed in Cloud Shell.

Confirm Project ID:
```bash
echo $GOOGLE_CLOUD_PROJECT
```

View publisher script:
```bash
cat publisher.py
```

Help:
```
python publisher.py -h
```

---

## 🧪 Task 4 — Create a Topic 🧵

Create topic:
```bash
python publisher.py $GOOGLE_CLOUD_PROJECT create MyTopic
```

List topics:
```bash
python publisher.py $GOOGLE_CLOUD_PROJECT list
```

🔎 You can view topics in:
Navigation menu → Pub/Sub → Topics

---

## 🧪 Task 5 — Create a Subscription 📬

Create subscription:
```bash
python subscriber.py $GOOGLE_CLOUD_PROJECT create MyTopic MySub
```

List subscriptions:
```bash
python subscriber.py $GOOGLE_CLOUD_PROJECT list-in-project
```

Check subscription in Google Cloud Console:
Navigation menu → Pub/Sub → Subscriptions

Help:
```bash
python subscriber.py -h
```

---

## 🧪 Task 6 — Publish Messages ✉️

Publish "Hello":
```bash
gcloud pubsub topics publish MyTopic --message "Hello"
```

Publish more messages:
```bash
gcloud pubsub topics publish MyTopic --message "Publisher's name is <YOUR NAME>"
gcloud pubsub topics publish MyTopic --message "Publisher likes to eat <FOOD>"
gcloud pubsub topics publish MyTopic --message "Publisher thinks Pub/Sub is awesome"
```

---

## 🧪 Task 7 — View Messages 👀

Pull messages using MySub:
```bash
python subscriber.py $GOOGLE_CLOUD_PROJECT receive MySub
```

Example output:
```kotlin
Listening for messages on projects/.../subscriptions/MySub  
Received message: data: 'Publisher thinks Pub/Sub is awesome'  
Received message: data: 'Hello'  
Received message: data: "Publisher's name is Harry"  
Received message: data: 'Publisher likes to eat cheese'
```

Press Ctrl + C to stop.

---

## 🎉 Task Completed
- Created a Pub/Sub topic
- Published message
- Created a subscription
- Pulled and viewed messages using Python
