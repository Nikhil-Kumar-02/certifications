![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 82: Secure Communication & SSL/TLS Termination on Load Balancers

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing — Advanced Topics
> ⏱️ **Type:** Concept + Console Walkthrough (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Recap: HTTPS = HTTP + Encryption
2️⃣ SSL vs TLS Terminology
3️⃣ AWS Certificate Manager (ACM)
4️⃣ The Two Communication Hops in a Load Balancer Setup
5️⃣ ⭐ SSL/TLS Termination Explained
6️⃣ Which Load Balancers Support Which Termination
7️⃣ Configuring HTTPS in the Console
```

---

## 1️⃣ Recap: HTTPS = HTTP + Encryption 🔒

> 🔗 **Recall from the networking deep dive:** HTTPS is HTTP secured via **TLS encryption**.

> ❓ **How do you actually enable HTTPS?** You need to install an **SSL or TLS certificate** on the server handling the connection (web server, load balancer, etc.).

---

## 2️⃣ SSL vs TLS Terminology 📛

| Term | Status |
|---|---|
| 🕰️ **SSL (Secure Sockets Layer)** | **Older** version |
| 🆕 **TLS (Transport Layer Security)** | **Newer**, current version |

> 💡 **Common Usage Quirk:** People **still** say "SSL certificate" colloquially, even though modern systems actually use **TLS certificates** under the hood. Both terms are used interchangeably in casual conversation — but technically, TLS is what's actually running.

---

## 3️⃣ AWS Certificate Manager (ACM) 🪪

| Fact | Detail |
|---|---|
| 🛠️ **AWS Certificate Manager (ACM)** | The AWS service where you **manage your SSL/TLS certificates** |

> 📌 This is the **central place** in AWS to request, store, and manage certificates for use across services like Elastic Load Balancer.

---

## 4️⃣ The Two Communication Hops in a Load Balancer Setup 🔀

> When using an Elastic Load Balancer, there are **TWO distinct communication segments**:

```
[Client] ──(Hop 1)──> [Elastic Load Balancer] ──(Hop 2)──> [EC2 Instance]
```

| Hop | Path | Typical Network |
|---|---|---|
| 1️⃣ | Client → ELB | 🌍 **Over the Internet** (if public/internet-facing LB) |
| 2️⃣ | ELB → EC2 Instance | 🔒 **Inside AWS's internal network** (same VPC) |

### 🔒 Hop 1: Client → ELB

> 🚨 **HTTPS is essentially MANDATORY here.** This traffic travels over the **public internet** — you don't want anyone intercepting it. Install an **X.509 certificate** on the Elastic Load Balancer to enable this.

### 🔓 Hop 2: ELB → EC2 Instance

> 💡 **HTTP CAN be acceptable here**, since this communication happens **internally** within AWS's network (same VPC). However, **HTTPS is still suggested** as a best practice for defense-in-depth. If you do use HTTPS for this hop, the **certificate needs to be handled by your EC2 instance** itself.

---

## 5️⃣ ⭐ SSL/TLS Termination Explained 🎯

> 🎯 **Core Concept:** A common, widely-used pattern where:

```
Client → ELB:        HTTPS (encrypted, secure)
ELB → EC2 Instance:   HTTP (unencrypted, but internal/private network)
```

> 💡 **Why This Pattern Is Popular:** The **expensive/critical part** (securing internet traffic) is handled by HTTPS on Hop 1. The **internal** traffic (Hop 2) is considered lower-risk since it stays within AWS's private network — so plain HTTP is often "good enough" there, simplifying certificate management (you don't need certificates on every EC2 instance).

### 🔑 Where Does Termination Happen?

> 📍 **SSL/TLS Termination happens AT THE LOAD BALANCER.** The load balancer **decrypts** the incoming HTTPS traffic and forwards it as **plain HTTP** to the backend EC2 instances.

---

## 6️⃣ Which Load Balancers Support Which Termination 📋

| Load Balancer | Termination Type | Protocols on Client Side |
|---|---|---|
| ⚖️ **Application Load Balancer (ALB)** | **SSL Termination** | HTTPS |
| 🕰️ **Classic Load Balancer (CLB)** | **SSL Termination** | HTTPS |
| 🌐 **Network Load Balancer (NLB)** | **TLS Termination** | TLS |

### 🌐 Network Load Balancer's Version: TLS Termination

> 🔗 **Recall:** NLB operates at **Layer 4** and supports **TCP, TLS, and UDP**.

```
Client → NLB:        TLS
NLB → EC2 Instance:   TCP
```

> 💡 **Same Concept, Different Name:** Just like ALB/CLB terminate SSL at the load balancer, NLB **terminates TLS** at the load balancer — the encrypted connection ends there, and plain TCP continues to the backend.

### 📋 Quick Reference: Termination Naming by Load Balancer

| Load Balancer | Termination Name | Why |
|---|---|---|
| ALB / CLB | **SSL Termination** | Operates at Layer 7 (HTTP/HTTPS) |
| NLB | **TLS Termination** | Operates at Layer 4 (TCP/TLS/UDP) |

> ⭐ **Exam Tip:** This is a **popular certification question** — know that SSL Termination applies to **ALB/CLB**, while **NLB** uses the term **TLS Termination**, and be able to explain **where** termination actually occurs (always **at the load balancer**).

---

## 7️⃣ Configuring HTTPS in the Console 🖥️

```
Load Balancer → Listeners → Add Listener
```

### Adding an HTTPS Listener

| Setting | Detail |
|---|---|
| 📡 **Protocol** | HTTPS |
| 🔒 **Security Policy** | Configurable (controls which TLS versions/ciphers are allowed) |
| 🪪 **SSL Certificate** | Select an existing certificate, or **request a new one from ACM** directly within this screen |

> 💡 **Convenience:** You can request a **brand new certificate from AWS Certificate Manager** without leaving the load balancer configuration screen — a smooth, integrated workflow.

---

## 📋 Master Summary Table

| Concept | Key Fact |
|---|---|
| HTTPS | HTTP + TLS encryption |
| SSL vs TLS | SSL = older; TLS = current (but "SSL" still used colloquially) |
| Certificate Management | AWS Certificate Manager (ACM) |
| Hop 1 (Client → ELB) | HTTPS strongly recommended/mandatory (public internet) |
| Hop 2 (ELB → EC2) | HTTP acceptable (internal network), HTTPS suggested as best practice |
| SSL Termination | ALB & CLB — decrypts HTTPS at the LB, forwards as HTTP |
| TLS Termination | NLB — decrypts TLS at the LB, forwards as TCP |
| Where Termination Happens | Always **at the load balancer** |

---

## ✅ Final Takeaways

```
🔒 HTTPS = HTTP + TLS    → Installed via certificates managed in AWS Certificate Manager (ACM)
🔀 TWO HOPS              → Client→ELB (public, HTTPS critical) and ELB→EC2 (internal, HTTP okay)
🎯 SSL/TLS TERMINATION ⭐ → Encrypted traffic ends AT the load balancer; internal traffic can be plain
⚖️🕰️ ALB/CLB              → Use "SSL Termination" terminology
🌐 NLB                    → Uses "TLS Termination" terminology (Layer 4: TCP/TLS/UDP)
🖥️ CONSOLE CONFIG         → Load Balancer → Listeners → Add HTTPS listener → attach/request ACM certificate
```

> 🎯 **Golden Rule:** SSL/TLS Termination is one of the most frequently tested load balancer concepts — remember the core idea (encryption ends at the load balancer, internal traffic can be plain), and the terminology split (SSL for ALB/CLB, TLS for NLB).

> ➡️ **Next Up:** More advanced Elastic Load Balancer topics!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
