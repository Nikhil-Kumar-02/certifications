![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 20: Hands-On — Launching Your First EC2 Instance

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Goal of This Demo
2️⃣ Step-by-Step: Launching an EC2 Instance
3️⃣ Understanding AMIs
4️⃣ Understanding Instance Types
5️⃣ Tags, Security Groups & Key Pairs
```

---

## 1️⃣ Goal of This Demo 🎯

In this hands-on step, we will:

| Goal | Emoji |
|---|---|
| Create EC2 instances | 🖥️ |
| Explore the instance lifecycle (create, start, stop, terminate) | 🔄 |
| Use **EC2 Instance Connect** to SSH into instances | 🔑 |
| Run commands on the instance | 💻 |

---

## 2️⃣ Step-by-Step: Launching an EC2 Instance 🚀

### 🌍 Step 0: Choose a Region

> The instructor logs in using an **IAM user** and selects the **Mumbai (ap-south-1)** region — chosen simply because it's geographically closest.

📌 **Note:** The region you choose **doesn't matter** for this demo — the steps look the same everywhere.

---

### 🔍 Step 1: Navigate to EC2

```
Services → Search "EC2" → Select "Virtual Servers in the Cloud"
```

---

### 💾 Step 2: Choose an AMI (Amazon Machine Image)

| Concept | Explanation |
|---|---|
| 🖼️ **AMI** | A **pre-baked image** containing an OS, application servers, and applications |
| 📦 **Sources** | AWS-provided AMIs, AWS Marketplace, or Community AMIs |
| ✅ **Chosen AMI** | **Amazon Linux 2** — Free Tier eligible |

---

### ⚙️ Step 3: Choose an Instance Type

An **instance type** defines the compute power: CPU, memory, storage, and network performance.

| Instance Family | Optimized For | Example |
|---|---|---|
| 🧩 **General Purpose** | Balanced CPU/memory/network | `t2.micro` (1 vCPU, ~0.5 GB RAM) |
| ⚡ **Compute Optimized** | High CPU-to-memory ratio | HPC, CPU-intensive workloads |
| 🎮 **GPU Optimized** | Graphics processing | 3D graphics, media processing |
| 🧠 **Memory Optimized** | High memory-to-CPU ratio | Memcached, distributed caches |
| 💽 **Storage Optimized** | High storage capacity | Data warehouses, I/O-intensive apps |

> ✅ **Chosen Instance Type:** `t2.micro` — Free Tier eligible, general purpose.

---

### 💽 Step 4: Configure Storage

| Setting | Value |
|---|---|
| Root Disk Size | 8 GB (default) |
| Disk Type | General Purpose SSD |

> ✅ Defaults are fine for this demo.

---

### 🏷️ Step 5: Add Tags

> 🌟 **Tags are underrated but extremely important in AWS!**

| Tag Key | Tag Value | Purpose |
|---|---|---|
| `Name` | First EC2 Instance | Identify the resource |
| `Environment` | DEV | Identify environment (dev/test/prod) |
| `BusinessUnit` | Business Unit A | Cost allocation / chargeback |

💡 **Why tags matter:** They help identify **which business unit** should be billed for a resource, and help organize resources across large enterprises.

---

### 🔒 Step 6: Configure Security Group

| Concept | Explanation |
|---|---|
| 🛡️ **Security Group** | A **virtual firewall** in front of the EC2 instance |
| ✅ **Rule Configured** | Allow **SSH (TCP, Port 22)** from anywhere |
| 📛 **Name Used** | `ec2-security-group` |

> 📌 For now, only SSH access is allowed — just enough to log in and explore the instance.

---

### 🔑 Step 7: Create a Key Pair

| Concept | Explanation |
|---|---|
| 🔐 **Key Pair** | AWS uses **public/private key pairs** instead of traditional usernames & passwords |
| 📛 **Key Pair Name** | `ec2-default` |
| ⚠️ **Critical Action** | **Download the key pair** immediately — it cannot be re-downloaded later! |

> 🚨 **Security Warning:** Anyone with access to this private key can log into your EC2 instance. Store it in a **secure location**.

---

### ✅ Step 8: Review & Launch

Final configuration reviewed before launch:

| Setting | Value |
|---|---|
| AMI | Amazon Linux 2 |
| Instance Type | t2.micro |
| Security Group | ec2-security-group (SSH only) |
| Key Pair | ec2-default |

> 🚀 Click **Launch Instance** — the instance will take a short while to start up.

---

## 📋 Quick Reference: Launch Wizard Steps

| # | Step | Key Decision |
|---|---|---|
| 1 | Choose AMI | Amazon Linux 2 (Free Tier) |
| 2 | Choose Instance Type | t2.micro (Free Tier) |
| 3 | Configure Storage | 8 GB, General Purpose SSD |
| 4 | Add Tags | Name, Environment, Business Unit |
| 5 | Configure Security Group | Allow SSH (port 22) |
| 6 | Create/Select Key Pair | Download and secure the private key |
| 7 | Review & Launch | Confirm settings, launch instance |

---

## ✅ Final Takeaways

```
🖼️ AMI            → Pre-baked OS + software image
⚙️ INSTANCE TYPE  → Defines CPU/memory/storage/network power
🏷️ TAGS           → Metadata for organization & cost tracking
🛡️ SECURITY GROUP → Virtual firewall controlling traffic
🔑 KEY PAIR       → Public/private keys used instead of passwords
```

> 🎯 **Golden Rule:** Always download and safely store your key pair immediately after creation — AWS won't let you download it again later!

> ➡️ **Next Up:** Exploring the running instance, EC2 Instance Connect, and running commands on it!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
