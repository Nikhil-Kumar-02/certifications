![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 83: Server Name Indication (SNI) on Load Balancers

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing — Advanced Topics
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ The Scenario: Multiple Websites, One Load Balancer
2️⃣ The Problem: Multiple Certificates on One Listener
3️⃣ What is Server Name Indication (SNI)?
4️⃣ How to Enable SNI in AWS
5️⃣ How SNI Actually Works (Conceptually)
```

---

## 1️⃣ The Scenario: Multiple Websites, One Load Balancer 🌐

> 🎯 **Setup:** An **Application Load Balancer** is load balancing between **multiple target groups**, each serving a **different website**.

```
ALB (single load balancer)
   ├── Target Group 1 → www.app1.com (EC2 instances)
   └── Target Group 2 → www.app2.com (EC2 instances)
```

> 🔒 Both sites use **HTTPS**, meaning **each website has its own SSL/TLS certificate**.

---

## 2️⃣ The Problem: Multiple Certificates on One Listener 🤔

> ❓ **Core Question:** How does a **single load balancer**, with a **single HTTPS listener**, correctly present the **right certificate** for each of these different websites?

> 🎯 This is **exactly** the problem that **Server Name Indication (SNI)** solves.

---

## 3️⃣ What is Server Name Indication (SNI)? 💡

| Concept | Explanation |
|---|---|
| 📡 **SNI** | An **extension to the TLS protocol** |
| 🎯 **What It Does** | Allows the **client** to indicate **which hostname** it's trying to reach, **at the very start** of the TLS handshake/interaction |

### How It Solves the Problem

```
1. Client initiates a secure connection to the load balancer
2. Client says (via SNI): "I want to talk to www.app1.com"
3. Load balancer sees this hostname BEFORE the handshake completes
4. Load balancer selects the CORRECT certificate (for www.app1.com)
5. Secure connection proceeds using the correct certificate
```

> 🎯 **Key Insight:** Without SNI, a server would have to guess (or use only ONE certificate) since it traditionally didn't know which hostname the client wanted until **after** establishing the encrypted connection. SNI solves this by letting the client **announce the hostname upfront**, in plain text, before encryption is fully established.

---

## 4️⃣ How to Enable SNI in AWS ⚙️

> ✅ **The Good News: You don't need to configure SNI directly at all!**

### The Simple Process

```
1. Go to your Elastic Load Balancer's LISTENER (the one on the secure port, e.g., HTTPS:443)
2. Associate MULTIPLE SSL certificates with that SAME listener
   (e.g., certificate for www.app1.com AND certificate for www.app2.com)
3. That's it! SNI is AUTOMATICALLY enabled by the load balancer
```

> 🎯 **Key Takeaway:** SNI isn't a separate feature you toggle — it's an **automatic behavior** that kicks in the moment you associate **more than one certificate** with a single listener.

---

## 5️⃣ How SNI Actually Works (Conceptually) 🔍

> 📌 **What You Need to Know for the Exam:** You don't need the deep implementation details — just recognize the **scenario** where SNI is the answer.

| Component | Role |
|---|---|
| 🖥️ **Client** | Indicates the desired hostname **at the start** of the TLS interaction |
| ⚖️ **Elastic Load Balancer** | Uses that hostname to **select the matching certificate** automatically |
| 🔒 **Result** | Secure (HTTPS/TLS) communication proceeds with the **correct** certificate for the requested host |

---

## 📋 Quick Reference

| Question | Answer |
|---|---|
| What problem does SNI solve? | Multiple websites (with different certificates) behind **one** load balancer listener |
| What protocol is SNI part of? | An **extension to TLS** |
| How do you enable it in AWS? | Simply **associate multiple SSL certificates** with a single listener |
| Do you configure SNI directly? | ❌ No — it's **automatic** once multiple certificates are attached |

---

## ✅ Final Takeaways

```
🌐 THE SCENARIO ⭐    → Multiple websites/certificates behind ONE load balancer listener
📡 SNI               → A TLS extension letting the client announce the hostname upfront
⚙️ HOW TO ENABLE      → Just associate MULTIPLE certificates with the same listener — SNI activates automatically
🔍 NO DEEP DETAILS NEEDED → For the exam, recognize the SCENARIO, not the low-level protocol mechanics
```

> 🎯 **Golden Rule:** Whenever an exam scenario describes **hosting multiple HTTPS websites behind a single load balancer**, the answer involves **Server Name Indication (SNI)** — and the "how" is simply **attaching multiple certificates to one listener**, nothing more complex than that.

> ➡️ **Next Up:** More advanced Elastic Load Balancer topics!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
