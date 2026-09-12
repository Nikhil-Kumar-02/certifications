![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 38: EC2 Section Summary — Complete Recap

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Full Section Recap (⭐ Detailed review of everything covered)

---

## 🎯 What This Lecture Is About

This is the **wrap-up** of the entire EC2 section. Since a lot of ground was covered, this recap goes through **every major concept in detail** — perfect for refreshing your memory before moving on.

```
1️⃣ Amazon Machine Images (AMIs)
2️⃣ EC2 Instance Types
3️⃣ Security Groups
4️⃣ Key Pairs & SSH/RDP Access
5️⃣ Instance Metadata Service
6️⃣ User Data & Bootstrapping
7️⃣ Public IPs vs Elastic IPs
8️⃣ Launch Templates
9️⃣ One-Page Master Summary
```

---

## 1️⃣ Amazon Machine Images (AMIs) 💾

### What Is It?
> An AMI contains the **operating system**, **software**, and **pre-configured security settings** you want your EC2 instance to launch with.

### Why Use a Custom AMI?

| Benefit | Explanation |
|---|---|
| ⚡ **Reduced Boot Time** | No need to install software/patches at launch — it's already baked in |
| 🛡️ **Improved Security** | Pre-configure security settings once, reuse everywhere |
| 🔧 **Hardening** | The practice of customizing an AMI to meet specific software/security requirements is called **"hardening"** |

### Key Rules to Remember

| Rule | Detail |
|---|---|
| 🌍 **Region-Specific** | An AMI can only be used to launch instances **in the same region** it was created in |
| 🔄 **Cross-Region Use** | You must **copy** the AMI to another region before using it there |
| 🛡️ **Disaster Recovery Best Practice** | **Back up AMIs across multiple regions** so you can recover even if one region goes down |
| 🤝 **Sharing** | AMIs can be **shared with other AWS accounts** (or made public) via image permissions |
| 🗺️ **AZ Flexibility** | Within the **same region**, an AMI can be used to launch instances in **any Availability Zone** — no copying needed |

---

## 2️⃣ EC2 Instance Types ⚙️

### What Is It?
> An **optimized combination** of **compute (CPU)**, **memory**, **disk**, and **networking** — tailored for different types of workloads.

### The Instance Families

| Family | Optimized For | Example Use Case |
|---|---|---|
| 🧩 **General Purpose** | Balanced resources | Web servers, small apps (`t2.micro`) |
| ⚡ **Compute Optimized** | High CPU-to-memory ratio | High-performance computing (HPC) |
| 🧠 **Memory Optimized** | High memory-to-CPU ratio | Memcached, in-memory caches |
| 💽 **Storage Optimized** | High storage capacity | Data warehouses, I/O-intensive apps |
| 🎮 **GPU Optimized** | Graphics processing | 3D rendering, media processing |

### Naming Convention Recap

```
t2.micro
│  │  └── Size (nano, micro, small, medium, large, xlarge...)
│  └───── Generation (2nd generation)
└──────── Family (t = general purpose)
```

> 📌 As size increases, **CPU, memory, and network performance all scale up proportionally**.

---

## 3️⃣ Security Groups 🛡️

### What Is It?
> A **virtual firewall** controlling **incoming (inbound)** and **outgoing (outbound)** traffic to/from AWS resources like EC2 instances.

### Core Rules

| Rule | Detail |
|---|---|
| 🚫 **Default Deny** | No configured rules = **no traffic allowed** in or out |
| ✅ **Allow-Only** | You can only add **ALLOW** rules — there's no explicit "deny" |
| ↔️ **Separate Directions** | Inbound and outbound rules are configured **independently** |
| 🔁 **Stateful** | If an inbound request is allowed, the **response is automatically allowed out** (and vice versa) |
| 🔢 **Up to 5** | You can attach **up to 5 security groups** to a single EC2 instance |
| ⚡ **Immediate Effect** | Changes apply **instantly** — no restart needed |
| 🔍 **No Logging of Blocked Traffic** | Denied traffic **never reaches** the instance, so it won't show in instance-level logs |

### Common Port Reference

| Protocol | Port |
|---|---|
| HTTP | 80 |
| HTTPS | 443 |
| SSH | 22 |
| RDP | 3389 |
| All ICMP (ping) | N/A (protocol-based, not port-based) |

> ⭐ **Exam Tip:** A **timeout** when connecting almost always points to a **security group** misconfiguration.

---

## 4️⃣ Key Pairs & SSH/RDP Access 🔑

### What Is It?
> AWS uses **public key cryptography (RSA)** instead of traditional usernames/passwords.

| Key | Location |
|---|---|
| 🌍 **Public Key** | Stored on the EC2 instance (by AWS) |
| 🔐 **Private Key** | Downloaded by you as a `.pem` file — **keep it safe, it can't be re-downloaded** |

### Connecting to Linux Instances (SSH)

```bash
chmod 400 ec2-default.pem
ssh -i "ec2-default.pem" ec2-user@<public-dns-name>
```

| Requirement | Detail |
|---|---|
| 🔒 Permissions | Must be `0400` — overly open permissions (e.g., `0777`) are **rejected** |
| 🚪 Port | **22** must be allowed in the security group |
| 👤 Default User | `ec2-user` for Amazon Linux |

### Connecting to Windows Instances (RDP)

| Requirement | Detail |
|---|---|
| 🔑 Private Key | Same as Linux — required |
| 🔓 Admin Password | **Extra requirement** — EC2 generates a random password, encrypts it with your **public key**; you decrypt it using your **private key** |
| 🚪 Port | **3389** must be allowed in the security group |

### Windows on Non-Windows Machines

> 🪟 If you're on Windows and need to SSH into a Linux instance, use **PuTTY** — but first convert your `.pem` file to `.ppk` format using **PuTTYgen**.

---

## 5️⃣ Instance Metadata Service 📊

### What Is It?
> A service that lets an EC2 instance retrieve **information about itself**, accessible only **from inside** the instance.

| Service | Base URL |
|---|---|
| 🗂️ **Instance Metadata** | `http://169.254.169.254/latest/meta-data` |
| 📄 **Dynamic Data** | `http://169.254.169.254/latest/dynamic/instance-identity/document` |
| 📜 **User Data** (view configured script) | `http://169.254.169.254/latest/user-data` |

### What You Can Retrieve

```
AMI ID, Instance ID, Instance Type, Hostname,
Public/Private IP addresses, Security Groups,
Availability Zone, Account ID, Region, and more
```

> ⭐ **Exam Tip:** Memorize the `169.254.169.254` IP address — it's commonly tested!

---

## 6️⃣ User Data & Bootstrapping 📜

### What Is It?
> **User Data** is a script executed **automatically at instance launch**. The process of installing OS patches/software at launch time is called **bootstrapping**.

### Example Script

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
curl -s http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

| Note | Detail |
|---|---|
| 🔄 **Modifying User Data** | You must **stop** the instance first, change the user data, then **start** it again |
| 🐢 **Trade-off** | Using User Data to install a lot of software **slows down boot time** — for production/scale, prefer a **custom AMI** instead |

---

## 7️⃣ Public IPs vs Elastic IPs 🌍

### Public vs Private IP Recap

| Type | Internet-Facing? | Changes on Stop/Start? | Always Assigned? |
|---|---|---|---|
| 🌍 **Public IP** | ✅ Yes | ✅ **Changes** | ❌ Optional |
| 🔐 **Private IP** | ❌ No | ❌ Stays the same | ✅ Always |

> 📌 A **reboot** does NOT change the public IP — only a **stop/start** does.

### The Problem & The Fix: Elastic IP

| Problem | Solution |
|---|---|
| Public IP changes on stop/start, breaking shared URLs | ✅ Use an **Elastic IP** — a static public IP address |

### Key Elastic IP Rules

| Rule | Detail |
|---|---|
| 🔄 **Reassignable** | Can move between EC2 instances **within the same region** |
| 🔓 **Manual Detach Required** | Stays attached even when the instance is **stopped** — must be manually disassociated |
| 💰 **Billing Rule** ⭐ | You are **charged** when the Elastic IP is: <br>• **Not associated** with any instance, OR <br>• Associated with a **stopped** instance |
| ✅ **Free When** | Associated with a **running** instance |
| ⚖️ **Scale Limitation** | Good for a **single instance**; for multiple instances, use a **Load Balancer** instead |

---

## 8️⃣ Launch Templates 📄

### What Is It?
> A **reusable, pre-configured template** containing the AMI ID, instance type, security group, key pair, network settings, and User Data — used to simplify and standardize EC2 instance creation.

### Key Features

| Feature | Detail |
|---|---|
| 🔁 **Reusable** | Launch as many instances as needed from one template |
| 🗂️ **Versioned** | Supports multiple versions (v1, v2, ...) — you can set a **default version** |
| ♻️ **Create From Existing Instance** | Fastest way to build a template — copies all settings, including User Data |
| ⚡ **Supports Spot Instances/Fleets** | Can be used to launch cheaper (but less reliable) Spot Instances |

---

## 9️⃣ One-Page Master Summary 📋

| Topic | Key Takeaway |
|---|---|
| 💾 **AMI** | OS + software + settings; region-specific; hardening reduces boot time |
| ⚙️ **Instance Types** | `family.generation.size`; different families for different workloads |
| 🛡️ **Security Groups** | Default deny, allow-only, stateful, up to 5 per instance, immediate effect |
| 🔑 **Key Pairs** | RSA public/private keys; `chmod 400`; Windows needs admin password too |
| 📊 **Instance Metadata** | `169.254.169.254` — get info about the instance from inside it |
| 📜 **User Data** | Script that runs at launch (bootstrapping); slows boot if overused |
| 🌍 **Public IP** | Changes on stop/start; stays the same on reboot |
| 📌 **Elastic IP** | Static public IP; billed when idle or attached to a stopped instance |
| 📄 **Launch Templates** | Reusable, versioned instance configs — simplifies repeated launches |

---

## ✅ Final Takeaways

```
💾 AMI            → Pre-baked image; region-specific; hardens for speed & security
⚙️ INSTANCE TYPE   → Right-size compute/memory/storage/network for your workload
🛡️ SECURITY GROUP  → Default deny firewall; stateful; immediate changes
🔑 KEY PAIRS       → RSA keys; chmod 400; Windows needs an extra password step
📊 METADATA        → 169.254.169.254 — self-info from inside the instance
📜 USER DATA       → Bootstraps software at launch; trade speed for simplicity
🌍 PUBLIC IP       → Changes on stop/start (not reboot) — use Elastic IP to fix
📄 LAUNCH TEMPLATE → Reusable, versioned config for fast, consistent launches
```

> 🎯 **Golden Rule:** EC2 is the foundation of AWS compute — mastering AMIs, security groups, key pairs, and networking here sets you up for everything that follows (Load Balancers, Auto Scaling, and beyond).

> ➡️ **Next Up:** Moving into the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
