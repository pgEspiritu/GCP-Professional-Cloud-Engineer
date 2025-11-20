# 🌟 Prompt Engineering Guide — Google Cloud

## 🚀 Introduction
Generative AI and Large Language Models (LLMs) are powerful technologies, but using them effectively requires understanding how they work and how to craft prompts strategically.

This guide explains:
- 🤖 What is Generative AI?
- 🧠 What is an LLM?
- 📝 What is Prompt Engineering?
- 📌 Best practices for writing effective prompts

A scenario featuring **Sasha**, a cloud architect, is included to apply concepts to real-world situations.

---

# 🤖 What Is Generative AI?

Generative AI (Gen AI) creates:
- ✍️ Text  
- 🖼️ Images  
- 🎵 Audio  
- 💻 Code  

It learns from existing data and generates new content based on user prompts.

### 🔑 Key Points
- 📈 Became mainstream around 2021  
- 🕹️ Works like an intelligent conversational tool  
- 🧭 Uses training data to create new outputs  
- 🌍 Used across many industries  

---

# 🧠 What Are Large Language Models (LLMs)?

LLMs are advanced AI models trained on enormous datasets.

### 📏 What “Large” Means
- 🗄️ Massive datasets (up to petabytes)  
- 🔢 Billions–trillions of parameters  

### 🔄 Pre-training and Fine-tuning
- **Pre-training:** Learns general patterns  
- **Fine-tuning:** Customizes for specific tasks  

### ⚙️ How LLMs Work
LLMs predict the most likely answer—similar to a very advanced autocomplete system.

---

# ⚠️ Hallucinations in LLMs

Hallucinations = incorrect, made-up, or misleading outputs.

### 🧩 Causes
- 📉 Not enough training data  
- 🧹 Noisy/dirty data  
- 🕳️ Missing context  
- 🚫 No clear constraints  

Clear prompts help minimize hallucinations.

---

# 🔮 Gemini — Google Cloud's AI Assistant

Google Cloud offers **Gemini**, a built-in generative AI assistant.

### 💡 What Gemini Can Do
- 📚 Access Cloud documentation & samples  
- 🏗️ Suggest architecture designs  
- 💻 Generate gcloud commands  
- 👩‍💻 Help developers, operators, data scientists  

Sasha uses Gemini to plan her VPC network quickly.

---

# 📝 What Is Prompt Engineering?

Prompt Engineering = crafting prompts that guide LLMs to produce accurate, useful responses.

A good prompt = clear + structured + intentional.

---

# 🧩 Types of Prompts

### 🟦 Zero-Shot Prompt
No examples.  
➡️ *“What is the capital of France?”*

### 🟩 One-Shot Prompt
One example.  
➡️ *“Italy → Rome. What is the capital of France?”*

### 🟧 Few-Shot Prompt
Multiple examples.  
➡️ *“Italy → Rome, Japan → Tokyo. What is the capital of France?”*

### 🎭 Role Prompt
Assign a persona.  
➡️ *“Act as a business professor and explain ROI.”*

---

# 🧱 Structure of a Good Prompt

### 🎬 Preamble  
Context or instructions  
➡️ *“You are a cloud architect.”*

### 🎯 Input  
The main request  
➡️ *“Recommend a dual-stack VPC design.”*

Not all prompts require both parts.

---

# 🔧 Improved Prompt Example (Sasha)

Original:
> “How can I create a network that uses IPv4 and IPv6 addresses?”

Improved:
> “Act as a cloud architect. How can I use gcloud to create a network with IPv4 and IPv6 subnets?”

Refined:
> “How can I adjust my gcloud subnet command to ensure the subnet is dual-stack?”

---

# 🌈 Prompt Engineering Best Practices

### 1️⃣ Provide clear, explicit instructions  
🧼 Avoid vague wording.

### 2️⃣ Define boundaries  
📘 Tell the model what *to do*.

### 3️⃣ Use a persona  
🎭 Helps the model stay focused.

### 4️⃣ Keep sentences short  
✂️ Break complex requests into smaller parts.

---

# 🏗️ Applying the Principles (Sasha's Final Prompt)

> “You're a cloud architect. You want to build a centrally managed Google Cloud VPC network. You also need to connect to VPCs in other regions. You want to avoid maintaining many firewall policy sets. What network architecture do you recommend?”

💡 **Gemini suggests a hub-and-spoke architecture**, perfectly fitting Sasha’s needs.

