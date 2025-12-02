# 🌐 Cloud Trace

## 🔍 What is Cloud Trace?
- Cloud Trace is a **distributed tracing system** that collects **latency data** from applications and displays it in the Google Cloud console.
- Helps you track how requests move through your application with **near real-time performance insights**.

## ⚡ Key Features
### 🛑 Issue Detection
- Continuously gathers and analyzes trace data.
- Automatically identifies **recent performance changes**.
- Alerts you if there is a **significant shift** in latency.

### 🚧 Bottleneck Identification
- View detailed latency for a **single request** or aggregate across the entire application.
- Powerful filters help you quickly locate the **root cause** of slow operations.

### 🏗️ Google-Scale Reliability
- Built using the same tracing tools Google uses internally.
- Broad platform support with language-specific SDKs: **Java, Node.js, Ruby, Go**.

## 📦 Core Concepts
- A **trace** shows how long a full operation takes.
- A **span** represents a sub-operation (e.g., an RPC call).
- A trace = a collection of spans.

## 🛠️ How to Send Trace Data
### 1️⃣ Automatic Tracing (Supported Environments)
- App Engine Standard (Java 8, Python 2, PHP 5)
- Cloud Run and Cloud Run functions (HTTP/latency data)

### 2️⃣ Manual Instrumentation
- Use **Google Cloud client libraries** or **OpenTelemetry** (recommended).
- If OpenCensus is available for your language, it simplifies batching and sending data.

## 🔐 IAM Requirements
- External systems, GKE, or Compute Engine (non-default service accounts) need:
  - **Cloud Trace Agent** role
- App Engine, Cloud Run, Cloud Functions, GKE, and GCE default service accounts already have required access.

## 📊 Analysis Reports
- Provide an overview of **latency distributions** for all or specific requests.
- Custom reports can filter by **request URIs** or other qualifiers.
- View formats:
  - 📉 Density distribution
  - 📈 Cumulative distribution
