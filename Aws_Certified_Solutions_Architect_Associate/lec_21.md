![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 21: Instance Types Deep Dive & Connecting via EC2 Instance Connect

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Understanding Instance Type Naming (t2.micro)
2️⃣ Instance Sizes & Scaling
3️⃣ Exploring the Running Instance
4️⃣ Connecting via EC2 Instance Connect
5️⃣ Running Commands on the Instance
6️⃣ Troubleshooting Instance Connect
```

---

## 1️⃣ Instance Type Naming Convention 🔤

> 📊 AWS has **40+ instance types** and **270+ variations** of compute, memory, disk, and networking combinations!

Let's break down `t2.micro`:

```
   t2.micro
   │  │  └── Size (micro)
   │  └───── Generation (2nd generation)
   └──────── Instance Family (t = general purpose)
```

| Part | Meaning | Example |
|---|---|---|
| **Family** | The category/type of workload it's optimized for | `t` = general purpose |
| **Generation** | Version number — newer generations bring improvements | `2` = 2nd generation (t1 → t2) |
| **Size** | How much CPU/memory/storage/network it gets | `micro` |

---

## 2️⃣ Instance Sizes 📏

> Sizes scale up in a predictable order:

```
nano → micro → small → medium → large → xlarge → ...
```

| Size Increases | Effect |
|---|---|
| CPU | ⬆️ Increases proportionally |
| Memory | ⬆️ Increases proportionally |
| Network Performance | ⬆️ Increases proportionally |

> 💡 **Key Insight:** As you move up in size (e.g., micro → small → medium), **CPU, memory, and network capability all scale up together**.

---

## 3️⃣ Exploring the Running Instance 🔍

Back in the EC2 console, the running instance shows:

| Detail | Value | Where It Comes From |
|---|---|---|
| **Name** | First EC2 Instance | 🏷️ From the `Name` tag added earlier |
| **Instance ID** | Auto-generated | Assigned by AWS |
| **Instance Type** | t2.micro | Chosen during launch |
| **Availability Zone** | ap-south-1a | One of Mumbai's 3 AZs |
| **Public IP Address** | Assigned | 🌍 For internet access |
| **Private IP Address** | Assigned | 🔐 For internal network access |

> 📌 Clicking the expand icon on the instance reveals **AMI details, IPs, and more** — all in one place.

---

## 4️⃣ Managing Instance Lifecycle ⚙️

> Path: **Actions → Instance State**

| Action | Effect |
|---|---|
| ⏸️ **Stop** | Stops the instance (can be restarted later) |
| 🔄 **Reboot** | Restarts the instance |
| ❌ **Terminate** | **Permanently deletes** the instance — cannot be reused! |

---

## 5️⃣ Connecting to the Instance 🔑

> Path: **Actions → Connect** (or click **Connect** directly)

### Connection Options

| Method | Description |
|---|---|
| 🖥️ **SSH Client** | Traditional SSH using terminal + key pair |
| 🛰️ **Session Manager** | Connect via AWS Systems Manager (no key pair needed) |
| ⚡ **EC2 Instance Connect** | **Easiest option** — browser-based SSH, no extra setup |

> ✅ **Chosen Method:** **EC2 Instance Connect**

| Setting | Value |
|---|---|
| Username | `ec2-user` (default for Amazon Linux) |
| Action | Click **Connect** |

---

## 6️⃣ Running Commands on the Instance 💻

Once connected via the browser-based terminal:

```bash
whoami
# Output: ec2-user

python --version
# Output: shows installed Python version
```

> 🎯 This confirms you're **inside the Linux 2 EC2 instance**, executing real commands!

---

## 7️⃣ Troubleshooting EC2 Instance Connect 🛠️

| Issue | Fix |
|---|---|
| ❌ Instance Connect fails to launch | ✅ Use **Chrome browser** (Safari has known issues) |
| ❌ Connect option unavailable/fails | ✅ Confirm AMI is **Amazon Linux 2** |
| ❌ Connection refused | ✅ Check **Security Group inbound rules** — must allow `TCP port 22` from everywhere (`0.0.0.0/0`) |

### 📋 Required Security Group Rule

| Type | Protocol | Port | Source |
|---|---|---|---|
| SSH | TCP | 22 | 0.0.0.0/0 (everywhere) |

---

## ✅ Final Takeaways

```
🔤 NAMING       → [family].[generation].[size] → e.g., t2.micro
📏 SIZES        → nano < micro < small < medium < large < xlarge
🏷️ NAME TAG     → Powers the display name in the console
🌍 AZ           → Instance lives in a specific Availability Zone
⚡ CONNECT      → EC2 Instance Connect = easiest browser-based SSH
🛠️ TROUBLESHOOT → Use Chrome + check AMI + check Security Group (port 22)
```

> 🎯 **Golden Rule:** Instance size scales CPU, memory, AND network performance together — bigger size = more of everything, proportionally.

> ➡️ **Next Up:** Launching a web server on the EC2 instance and hitting it with a URL!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
