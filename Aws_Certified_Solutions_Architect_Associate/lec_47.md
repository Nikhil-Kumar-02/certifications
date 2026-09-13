![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 47: Verifying ALB & Locking Down EC2 Instances (Security Best Practice)

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Confirming the ALB Is Active
2️⃣ Testing Load Distribution
3️⃣ The Security Problem: Direct EC2 Access
4️⃣ The Fix: Security-Group-to-Security-Group Rules
5️⃣ Verifying the Fix
6️⃣ Best Practice: Multi-Tier Architecture Security
```

---

## 1️⃣ Confirming the ALB Is Active ✅

> After roughly **10 minutes**, the Application Load Balancer finishes provisioning.

```
Load Balancers → Select ALB → Description tab
```

| Field | Value |
|---|---|
| State | **Active** |
| Type | Application |
| Scheme | Internet-facing |
| IP Type | IPv4 |
| Availability Zones | Multiple |
| Security Group | `application-load-balancer-sg` |

---

## 2️⃣ Testing Load Distribution 🌐

```
Copy the ALB's DNS Name → Open in browser → Refresh multiple times
```

| Request | Response From (Private IP) |
|---|---|
| 1st | `...41.189` |
| 2nd (refresh) | `...35.39` |

> ✅ **Confirmed:** The ALB is **successfully load balancing** between the two registered EC2 instances.

---

## 3️⃣ The Security Problem: Direct EC2 Access ⚠️

> 🔍 **Test:** Copy an EC2 instance's **public IP** directly and access it in the browser.

```
Result: ✅ Works! You can talk directly to the EC2 instance, bypassing the load balancer entirely.
```

> 🚨 **This is a security problem!**

### ❓ Why Is This Bad?

| Risk | Explanation |
|---|---|
| 🔓 **Bypasses Load Balancer Logic** | Health checks, routing rules, and traffic distribution are all skipped |
| 🕵️ **Exposes Internal Infrastructure** | Attackers could target individual instances directly |
| 📊 **Breaks Monitoring Assumptions** | Traffic patterns/metrics become unreliable if some traffic skips the LB |

> 🎯 **Golden Principle:** All traffic to backend EC2 instances should flow **only through the load balancer** — direct access should be **blocked**.

---

## 4️⃣ The Fix: Security-Group-to-Security-Group Rules 🔧

> 💡 **The Solution:** Configure the EC2 instance's security group to only allow traffic **from the load balancer's security group** — not from "everywhere."

### Step-by-Step Fix

```
1. Go to EC2 → Select instance → Security Group → Edit inbound rules
2. REMOVE the existing rule: HTTP (80) from 0.0.0.0/0 (everywhere - IPv4)
3. REMOVE the existing rule: HTTP (80) from ::/0 (everywhere - IPv6)
4. ADD a new rule: HTTP (80) — Source: the ALB's security group (application-load-balancer-sg)
5. Save rules
```

### 📋 Before vs After

| Rule | Before | After |
|---|---|---|
| Source | `0.0.0.0/0` (anywhere, IPv4) | `application-load-balancer-sg` (ALB's security group) |
| Source (IPv6) | `::/0` (anywhere, IPv6) | ❌ **Removed** |

> 🔑 **Key Insight:** AWS security groups can reference **other security groups** as the traffic source — not just IP ranges! This creates a **dynamic** rule: any resource using that security group (like the ALB) is automatically allowed, without needing to know its specific IP.

---

## 5️⃣ Verifying the Fix ✅

| Test | Result |
|---|---|
| 🌐 Access via **ALB DNS name** | ✅ **Still works** — traffic flows through the load balancer |
| 🌍 Access via **EC2 public IP directly** | ❌ **Blocked** — direct access is now denied |

> 🎉 **Success!** Traffic now **must** pass through the load balancer to reach the EC2 instances.

---

## 6️⃣ Best Practice: Multi-Tier Architecture Security 🏗️

> ⭐ **This pattern extends to any multi-tier architecture**, not just load balancer → EC2.

### Example: 3-Tier Architecture

```
[Load Balancer] → [Web Application (EC2)] → [Database]
```

| Tier | Security Group Should Only Allow Traffic From |
|---|---|
| 🖥️ **Web Application** | The **Load Balancer's** security group |
| 🗄️ **Database** | The **Web Application's** security group |

> 🎯 **Principle:** Each tier should only accept traffic from the **tier directly in front of it** — never expose internal tiers to "everywhere" (`0.0.0.0/0`).

### 📋 Why This Matters for the Exam

| ⭐ Exam Tip |
|---|
| Questions about **securing multi-tier architectures** frequently test this **security-group-referencing-security-group** pattern. Know that a security group's **source** can be another **security group**, not just a CIDR range! |

---

## 📋 Quick Reference

| Concept | Key Fact |
|---|---|
| Security Group Source | Can be a **CIDR range** OR **another security group** |
| Best Practice | Restrict EC2 access to **only** the load balancer's security group |
| Multi-Tier Rule | Each tier only trusts the tier **immediately upstream** of it |
| Verification | Direct EC2 access should **fail**; LB-routed access should **succeed** |

---

## ✅ Final Takeaways

```
🌐 LOAD BALANCING CONFIRMED → Refreshing via ALB DNS hits different backend instances
⚠️ DIRECT ACCESS RISK       → EC2 public IPs were reachable directly — a security gap
🔧 THE FIX                  → EC2 security group allows traffic ONLY from ALB's security group
🔑 KEY MECHANISM            → Security groups can reference OTHER security groups as a source
🏗️ MULTI-TIER PRINCIPLE     → Each layer trusts only the layer directly in front of it
```

> 🎯 **Golden Rule:** In production architectures, **never** allow direct traffic to backend instances from `0.0.0.0/0` — always restrict inbound access to the **load balancer's security group** (or the tier immediately upstream).

> ➡️ **Next Up:** Deep dive into Listeners — how load balancers route incoming connections!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
