![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 19: EC2 Fundamentals — What is EC2?

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept Lecture

---

## 🎯 What This Lecture Is About

```
1️⃣ What is EC2?
2️⃣ Physical Servers vs Virtual Servers
3️⃣ Key Features of the EC2 Service
4️⃣ What's Coming Up: Hands-On Demo
```

---

## 1️⃣ What is EC2? 💡

> **EC2 = Elastic Compute Cloud**

EC2 is the AWS **service** used to provision **virtual servers** in the cloud.

| Term | Meaning |
|---|---|
| 🛠️ **EC2 (the service)** | The AWS service used to create/manage virtual servers |
| 🖥️ **EC2 Instance** | The actual **virtual server** created using the EC2 service |

📌 **Important distinction:** People often say "EC2" to refer to *both* the service and the instance — but technically, EC2 is the **service**, and the virtual servers themselves are called **EC2 instances**.

---

## 2️⃣ Physical Servers vs Virtual Servers 🏢☁️

| Environment | Where Apps Are Deployed |
|---|---|
| 🏢 **Corporate Data Center** | Applications deployed to **physical servers** |
| ☁️ **AWS Cloud** | Applications deployed to **virtual servers** (EC2 instances) — rented/provisioned on demand |

> 💰 **Billing Model:** EC2 instances are **billed by the second** — you only pay for what you use, when you use it.

---

## 3️⃣ Key Features of the EC2 Service 🔑

| Feature | Emoji | Description |
|---|---|---|
| **Instance Lifecycle Management** | 🔄 | Create, start, stop, terminate, and manage EC2 instances |
| **Load Balancing** | ⚖️ | Distribute traffic across multiple EC2 instances |
| **Auto Scaling** | 📈 | Automatically increase/decrease instance count based on load |
| **Storage Attachment** | 💾 | Attach storage (like a virtual hard disk) to boot the OS and store data |
| **Network Connectivity** | 🌐 | Assign public/private IP addresses for internal & internet access |

### 📋 Features Quick Reference Table

| # | Feature | Purpose |
|---|---|---|
| 1 | Lifecycle Management | Create/start/stop/terminate instances |
| 2 | Load Balancing | Spread traffic across multiple instances |
| 3 | Auto Scaling | Dynamically adjust instance count with load |
| 4 | Storage | Attach disks for OS + data |
| 5 | Networking | Public & private IP connectivity |

---

## 4️⃣ Summary 📝

> ✅ EC2 service helps you:
> - Create EC2 instances 🖥️
> - Attach storage 💾
> - Distribute load ⚖️
> - Auto scale 📈
> - Manage network connectivity 🌐

---

## 🚀 What's Coming Up Next

In this section, we will get **hands-on** and:

```
✅ Create multiple EC2 instances configured as HTTP/web servers
✅ Distribute load across them using a Load Balancer
```

---

## ✅ Final Takeaways

```
🖥️ EC2 SERVICE   → Used to provision virtual servers
🖥️ EC2 INSTANCE  → The actual virtual server
💰 BILLING       → Per second
🔑 KEY FEATURES  → Lifecycle mgmt, Load Balancing, Auto Scaling, Storage, Networking
```

> 🎯 **Golden Rule:** EC2 = the *service*. EC2 Instance = the *virtual server* it creates for you.

> ➡️ **Next Up:** Hands-on demo — creating your first EC2 instance!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
