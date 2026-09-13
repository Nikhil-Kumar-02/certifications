![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 46: Application Load Balancer — Concept & Creation

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Most important load balancer type!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Why ALB Is the Most Important Load Balancer
2️⃣ What ALB Supports (Protocols & Targets)
3️⃣ ALB Scaling: What AWS Manages vs What You Manage
4️⃣ Hands-On: Creating an Application Load Balancer
5️⃣ Configuring the Security Group
6️⃣ Configuring the Target Group
7️⃣ Registering Targets & Final Review
```

---

## 1️⃣ Why ALB Is the Most Important Load Balancer ⭐

> 🏆 **Application Load Balancer (ALB)** is the **most popular and frequently used** Elastic Load Balancer in AWS.

| Fact | Detail |
|---|---|
| 🎯 **Layer** | **Layer 7** — Application layer |
| 📡 **Protocols** | HTTP, HTTPS, **WebSockets** |
| 🧠 **Intelligence** | Can inspect request **content** and route based on it |

---

## 2️⃣ What ALB Supports (Protocols & Targets) 🎯

> ALB isn't limited to just EC2 instances — it can load balance across **multiple target types**:

| Target Type | Description |
|---|---|
| 🖥️ **EC2 Instances** | Traditional virtual server targets |
| 📦 **Containerized Applications** | Via **Amazon ECS** (Docker-based apps) |
| 🌍 **IP Addresses** | Load balance to specific IPs directly |
| ⚡ **Lambda Functions** | Supports serverless architectures too |

> 💡 **Note on Lambda:** While ALB *can* load balance Lambda functions, this is **not commonly done** in practice — serverless architectures typically prefer **API Gateway** instead. (Covered later in the course.)

---

## 3️⃣ ALB Scaling: What AWS Manages vs What You Manage ⚖️

| Responsibility | Who Handles It? |
|---|---|
| 📈 **Load Balancer Scaling** | ✅ **AWS** — ALB automatically scales to handle 10, 100, or 10,000+ requests |
| 🖥️ **EC2 Instance Scaling** | ⚠️ **You** — AWS does NOT automatically scale your backend EC2 instances |

> 🎯 **Key Distinction:** ALB itself is a **managed, auto-scaling service** — but the **instances behind it** need a separate mechanism (covered soon: **Auto Scaling Groups**) to scale up/down.

---

## 4️⃣ Hands-On: Creating an Application Load Balancer 🚀

```
EC2 Console → Load Balancers → Create Load Balancer → Application Load Balancer
```

### Basic Configuration

| Setting | Value |
|---|---|
| Name | `my-application-load-balancer` |
| Scheme | **Internet-facing** (public) |
| IP Address Type | **IPv4** |
| Listener | HTTP, Port **80** (default) |
| VPC | Default VPC |
| Availability Zones | **All available AZs selected** |

> 🌟 **Important Rule:** Cross-zone load balancing is **automatically enabled** for ALB — and **cannot be disabled**. (Unlike Classic Load Balancer, where it was optional.)

---

## 5️⃣ Configuring the Security Group 🛡️

> ✅ **Best Practice:** Create a **dedicated** security group for the ALB.

| Setting | Value |
|---|---|
| Name | `application-load-balancer-sg` (or `alb-sg`) |
| Inbound Rule | Allow **HTTP (port 80)** from anywhere |

> 💡 Creating a separate security group gives you **fine-grained control** over what traffic reaches the load balancer — separate from the EC2 instances' own security group.

---

## 6️⃣ Configuring the Target Group 🎯

> A **Target Group** defines the set of resources (EC2 instances, IPs, or Lambda functions) that the ALB routes traffic to.

### Target Group Settings

| Setting | Value |
|---|---|
| Name | `my-target-group` |
| Target Type | **Instance** (EC2) — could also be IP or Lambda |
| Protocol | HTTP |
| Port | 80 |
| Health Check Path | `/` (default root) |
| Health Check Protocol | HTTP |

### 🩺 Advanced Health Check Settings

| Setting | Meaning |
|---|---|
| **Healthy Threshold** | Consecutive successful checks before marking a target **healthy** |
| **Unhealthy Threshold** | Consecutive failed checks before marking a target **unhealthy** |
| **Timeout** | How long to wait for a health check response |
| **Interval** | How often health checks run |
| **Success Code** | The expected HTTP response code (e.g., 200) |

---

## 7️⃣ Registering Targets & Final Review ✅

```
Target Group Wizard → Register Targets
→ Select available EC2 instances → Add to registered
```

### 📋 Final Configuration Summary

| Setting | Value |
|---|---|
| Type | Application Load Balancer |
| Scheme | Internet-facing |
| Listener | HTTP, Port 80 |
| Subnets | Multiple (all AZs) |
| Security Group | `application-load-balancer-sg` (new) |
| Target Group | `my-target-group` |
| Health Check | HTTP, path `/` |
| Registered Targets | 2 EC2 instances |

```
Click "Create"
```

> ⏳ **Note:** ALB creation takes **around 10 minutes** — noticeably longer than a Classic Load Balancer.

> 💡 **No EC2 instances yet?** Use the **Launch Template** created earlier to quickly spin up a couple of instances for testing.

---

## 📋 Quick Reference: ALB Setup Flow

| Step | Action |
|---|---|
| 1 | Choose "Application Load Balancer" |
| 2 | Configure name, scheme (internet-facing), IPv4, listener (port 80) |
| 3 | Select VPC and all Availability Zones |
| 4 | Create a **dedicated** security group |
| 5 | Create a **Target Group** (instance/IP/Lambda) with health check settings |
| 6 | Register EC2 instances as targets |
| 7 | Review and create (~10 min provisioning time) |

---

## ✅ Final Takeaways

```
⭐ ALB              → Most popular, Layer 7, supports HTTP/HTTPS/WebSockets
🎯 MULTIPLE TARGETS → EC2 instances, containers (ECS), IPs, or Lambda functions
📈 SCALING SPLIT    → AWS scales the ALB itself; YOU scale the EC2 instances behind it
🗺️ CROSS-ZONE       → Always ON for ALB, cannot be disabled
🎯 TARGET GROUP     → Defines which resources receive traffic + their health check config
🛡️ DEDICATED SG      → Best practice: separate security group just for the load balancer
```

> 🎯 **Golden Rule:** ALB scaling is automatic and managed by AWS — but **your backend EC2 instances still need their own scaling strategy** (Auto Scaling Groups, covered soon).

> ➡️ **Next Up:** Verifying the Application Load Balancer works, and locking down EC2 instances to only accept traffic from the load balancer!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
