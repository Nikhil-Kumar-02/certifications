![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 59: Network Load Balancer (NLB) — Concept Deep Dive

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Recap: Load Balancer Layers Comparison
2️⃣ NLB's Protocols: TCP, TLS, UDP
3️⃣ Why NLB Is Built for High Performance
4️⃣ ⭐ NLB's Unique Feature: Static/Elastic IP Support
5️⃣ What NLB Can Load Balance Between
6️⃣ Top 3 Things to Remember
```

---

## 1️⃣ Recap: Load Balancer Layers Comparison 📊

| Load Balancer | Layer(s) | Protocols |
|---|---|---|
| 🕰️ **Classic Load Balancer (CLB)** | **Both** Layer 4 **and** Layer 7 | TCP, TLS, HTTP, HTTPS |
| ⚖️ **Application Load Balancer (ALB)** | **Only** Layer 7 | HTTP, HTTPS |
| 🌐 **Network Load Balancer (NLB)** | **Only** Layer 4 | TCP, TLS, **UDP** |

> 🔑 **Key Distinction:** ALB and NLB are each **specialized** for one layer — CLB (legacy) tried to do both.

---

## 2️⃣ NLB's Protocols: TCP, TLS, UDP 📡

| Protocol | Priority | Reliability vs Speed |
|---|---|---|
| 🔵 **TCP** | Reliability | Ensures packets are sent from source to destination **accurately** |
| 🔒 **TLS** | Reliability + Encryption | TCP's secured version |
| 🟡 **UDP** | Performance | Prioritizes **speed** over guaranteed delivery |

### 🎓 Recall: Why UDP Trades Reliability for Speed

> UDP is used in scenarios where it's **acceptable** if **95% of packets** arrive — as long as they arrive **very quickly**. Perfect for use cases where **speed matters more than perfect accuracy**.

---

## 3️⃣ Why NLB Is Built for High Performance ⚡

| Fact | Detail |
|---|---|
| 🎯 **Primary Use Case** | Scenarios requiring **extremely high performance** |
| 📈 **Scale** | Can handle **millions of requests per second** |

> 💡 **Key Insight:** NLB's ability to support **UDP** (in addition to TCP/TLS) is a major reason it fits high-performance, low-latency scenarios that ALB simply cannot serve.

---

## 4️⃣ ⭐ NLB's Unique Feature: Static/Elastic IP Support 🌟

> 🏆 **This is THE standout, most exam-relevant feature of NLB.**

| Load Balancer | Can Have a Static/Elastic IP? |
|---|---|
| 🕰️ Classic Load Balancer | ❌ No |
| ⚖️ Application Load Balancer | ❌ No |
| 🌐 **Network Load Balancer** | ✅ **YES** |

> ⭐ **Classic Exam Question:** *"Which AWS load balancer type supports assigning an Elastic IP?"* → **Network Load Balancer**

### 🤔 Why Does This Matter?

> 🔗 **Recall from earlier in the course:** We learned that a **static public IP** (via Elastic IP) is useful when you need a **constant, unchanging address** — e.g., for firewall whitelisting by external partners, or DNS configurations that require a fixed IP rather than a changing DNS name.

> 💡 Regular load balancers (ALB, CLB) only expose a **DNS name** that resolves to **changing underlying IPs** — NLB is unique in letting you pin down a **fixed IP address**.

---

## 5️⃣ What NLB Can Load Balance Between 🎯

| Target Type | Description |
|---|---|
| 🖥️ **EC2 Instances** | Traditional virtual servers |
| 📦 **Containerized Applications** | Via **Amazon ECS** (Elastic Container Service) |
| 🌍 **Web Applications** (by IP) | Direct IP-based targets |

> 💡 **Similar to ALB:** NLB also supports multiple target types — but it operates at the **Transport layer**, so it doesn't inspect HTTP-level content the way ALB does.

---

## 6️⃣ Top 3 Things to Remember ⭐

| # | Fact |
|---|---|
| 1️⃣ | 🌐 NLB works at the **Transport Layer (Layer 4)** |
| 2️⃣ | ⚡ NLB is used for **high-performance** use cases (millions of requests/sec) |
| 3️⃣ | 📌 NLB is the **only** load balancer that supports assigning an **Elastic IP** |

---

## 📋 Master Comparison Table (CLB vs ALB vs NLB)

| Feature | CLB | ALB | NLB |
|---|---|---|---|
| Layer | 4 & 7 | 7 | 4 |
| Protocols | TCP, TLS, HTTP, HTTPS | HTTP, HTTPS, WebSockets | TCP, TLS, UDP |
| Content-Based Routing | ❌ Limited | ✅ Yes | ❌ No |
| Multiple Target Groups | ❌ No | ✅ Yes | ✅ Yes |
| Elastic IP Support | ❌ No | ❌ No | ✅ **Yes** |
| Performance | Moderate | Good | ⚡ **Extremely High** |
| Recommendation | ⚠️ Legacy, avoid | ✅ Web apps/microservices | ✅ High-performance/UDP needs |

---

## ✅ Final Takeaways

```
🌐 NLB LAYER        → Transport Layer (Layer 4) only
📡 PROTOCOLS         → TCP, TLS, and UDP
⚡ HIGH PERFORMANCE   → Built for millions of requests per second
📌 ELASTIC IP ⭐      → NLB is the ONLY load balancer type supporting a static/Elastic IP
🎯 TARGETS            → EC2 instances, containerized apps (ECS), IP-based web applications
```

> 🎯 **Golden Rule:** If an exam question asks about assigning a **static/Elastic IP to a load balancer**, the answer is always **Network Load Balancer** — this is one of the most distinctive, testable NLB facts in the entire course.

> ➡️ **Next Up:** Hands-on — creating a Network Load Balancer!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
