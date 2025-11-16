# 🐳 Introduction to Containers

In this section, we’ll explore **containers** and understand how they are used.

---

## 💻 From IaaS to Containers

**Infrastructure as a Service (IaaS)** allows you to share compute resources with other developers using **virtual machines (VMs)**.  
- Each developer can deploy their own **operating system (OS)**  
- Access hardware resources like RAM, file systems, networking interfaces  
- Build applications in a self-contained environment  

This works well, but it has some limitations:

- The smallest unit of compute is **a full VM with its guest OS**  
- Guest OS can be large (gigabytes) and slow to boot  
- Scaling requires copying the entire VM, which can be **slow and costly**

---

## 📦 What is a Container?

A **container** is a lightweight, portable unit that packages your **code and its dependencies**.  

Key points:

- Limited access to its own **partition of file system and hardware**  
- Requires only a few system calls to create  
- Starts as quickly as a process  
- Needs only a **host OS kernel** that supports containers and a **container runtime**  

Effectively, the **OS is virtualized**, giving you the **scalability of PaaS** with the **flexibility of IaaS**.  

**Benefits:**

- Ultra-portable: move from **development → staging → production** without rebuilding  
- Works across laptops, on-premises, or cloud environments  

---

## 🚀 Scaling with Containers

**Example:** Scaling a web server

- Containers start in seconds  
- Dozens or hundreds of instances can run on a single host  

For complex applications:

- Use **multiple containers**, each performing a specific function (microservices)  
- Connect containers with **network connections** for modularity  
- Deploy and scale **independently** across hosts  
- Hosts can scale up or down, starting and stopping containers as demand changes or hosts fail  

---

**Summary:** Containers provide a **lightweight, portable, and scalable way** to deploy applications, combining the best aspects of **IaaS and PaaS**.
