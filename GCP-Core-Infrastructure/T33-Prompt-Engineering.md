# Prompt Engineering Guide — Google Cloud

## Introduction
Generative AI and Large Language Models (LLMs) are powerful technologies, but using them effectively requires understanding how they work and how to craft prompts strategically.

This guide explains:
- What is Generative AI?
- What is an LLM?
- What is Prompt Engineering?
- Best practices for writing effective prompts

A scenario featuring Sasha, a cloud architect, is used throughout the lesson to connect concepts to real-world use.

---

# What Is Generative AI?

Generative AI (Gen AI) is a type of artificial intelligence capable of creating new content such as:
- Text  
- Images  
- Audio  
- Code  

It generates outputs based on patterns learned from training data and responds to user prompts.

### Key Points
- Gen AI became widely popular around 2021, but AI has existed since the 1950s.
- A prompt is an instruction or question given to the AI.
- Gen AI models act like advanced conversational systems.
- They are used in software development, healthcare, finance, entertainment, and more.

---

# What Are Large Language Models (LLMs)?

LLMs are advanced AI models trained on massive datasets of text, images, and code.

### What “Large” Means
- Huge training datasets, sometimes petabytes in size  
- Billions or trillions of parameters  

### Pre-training and Fine-tuning
- **Pre-training:** Learns general language patterns  
- **Fine-tuning:** Adjusts to specific tasks with smaller datasets  

### How LLMs Work
When given a prompt, an LLM predicts the most probable answer based on training.  
This works like an advanced autocomplete.

---

# Hallucinations in LLMs

A hallucination occurs when the model generates incorrect, nonsensical, or misleading information.

### Causes of Hallucinations
- Insufficient training data  
- Noisy or incorrect training data  
- Lack of context  
- Lack of constraints  

Hallucinations are reduced through clear, structured prompts.

---

# Gemini — Google Cloud’s Gen AI Assistant

Google Cloud provides Gemini, an integrated generative AI tool inside the Google Cloud Console.

### Capabilities
- Provides guidance using Google Cloud documentation
- Generates architecture suggestions
- Creates gcloud commands and inserts them into Cloud Shell
- Supports developers, operators, and data scientists

Sasha uses Gemini to design VPC architectures efficiently.

---

# What Is Prompt Engineering?

Prompt Engineering is the practice of structuring prompts to guide LLMs toward accurate, meaningful outputs.

A prompt is the text you give the model.  
Clearer prompts produce better results.

---

# Types of Prompts

### Zero-Shot Prompt
No examples.  
Example: “What is the capital of France?”

### One-Shot Prompt
One example.  
Example: “Italy → Rome. What is the capital of France?”

### Few-Shot Prompt
Multiple examples.  
Example: “Italy → Rome, Japan → Tokyo. What is the capital of France?”

### Role Prompt
Assigns a persona.  
Example: “Act as a business professor. Explain ROI.”

---

# Structure of a Prompt

A well-designed prompt may include:

### Preamble
Context or instructions.  
Example: “You are a cloud architect.”

### Input
The main request.  
Example: “Recommend a network design that supports IPv4 and IPv6.”

Not all components are required.

---

# Improved Prompt Example (Sasha)

Original:
“How can I create a network that uses IPv4 and IPv6 addresses?”

Improved:
“I want you to act as a cloud architect in Google Cloud. How can I use gcloud to create a network with IPv4 and IPv6 subnets?”

Refined:
“How can I adjust the existing gcloud subnet creation command to ensure the subnet is dual-stack?”

---

# Prompt Engineering Best Practices

### 1. Use Clear and Explicit Instructions
Avoid vague prompts.

### 2. Define Boundaries
Tell the model what to do.

### 3. Adopt a Persona
Example: “Act as a Google Cloud network engineer…”

### 4. Keep Sentences Short
Use short, simple instructions.

---

# Applying the Principles (Sasha’s Final Prompt)

“You're a cloud architect. You want to build a Google Cloud VPC network that can be centrally managed. You also connect to VPC networks in other regions. You want to avoid maintaining many different firewall policies. What network architecture would you recommend?”

Gemini proposes a hub-and-spoke architecture, which meets Sasha’s needs.

