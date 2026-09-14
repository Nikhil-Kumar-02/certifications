![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 85: ALB vs NLB vs CLB — Complete Feature Comparison

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing — Advanced Topics
> ⏱️ **Type:** Concept Lecture (⭐ THE definitive comparison — heavily tested in the exam!)

---

## 🎯 What This Lecture Is About

This lecture pulls together **every feature comparison** between the three load balancer types into **one comprehensive reference**. If you only study one load balancing lecture before the exam, make it this one.

```
1️⃣ Quick Recap: Generation & Use Cases
2️⃣ Protocols Supported
3️⃣ Connection Draining
4️⃣ Dynamic Host Port Mapping (Containers/ECS)
5️⃣ Cross-Zone Load Balancing
6️⃣ Sticky Sessions
7️⃣ Server Name Indication (SNI)
8️⃣ Static/Elastic IP
9️⃣ Source IP Preservation
🔟 WebSockets
1️⃣1️⃣ Routing Algorithms (ALB's Big Advantage)
1️⃣2️⃣ Multiple Target Groups
1️⃣3️⃣ The Master Comparison Table
```

---

## 1️⃣ Quick Recap: Generation & Use Cases 🏷️

| Load Balancer | Generation | Best For |
|---|---|---|
| 🕰️ **Classic (CLB)** | Older — **avoid**, not recommended by AWS | Legacy only |
| ⚖️ **Application (ALB)** | Newer | Web applications, microservices, containers |
| 🌐 **Network (NLB)** | Newer | Extreme performance — millions of requests, very low latency |

---

## 2️⃣ Protocols Supported 📡

| Load Balancer | Layer | Protocols |
|---|---|---|
| ⚖️ **ALB** | 7 | HTTP, HTTPS |
| 🌐 **NLB** | 4 | TCP, UDP, TLS |
| 🕰️ **CLB** | 4 & 7 | Both sets |

---

## 3️⃣ Connection Draining 🚰

> 🎯 **Purpose:** Give in-flight requests time to complete before a target is fully removed (during scale-in or health-check failure).

| Load Balancer | Supports Connection Draining? |
|---|---|
| ⚖️ **ALB** | ✅ Yes |
| 🕰️ **CLB** | ✅ Yes |
| 🌐 **NLB** | *(Uses deregistration delay at the target group level, conceptually similar)* |

> 📌 **Note:** In the course's framing, Connection Draining is specifically called out as an **ALB and CLB** feature.

---

## 4️⃣ Dynamic Host Port Mapping (Containers/ECS) 📦

| Concept | Explanation |
|---|---|
| 📦 **Dynamic Host Port Mapping** | Allows load balancing when **multiple instances of the same microservice/container** run on the **same EC2/container instance** (common with Amazon ECS) |

| Load Balancer | Supports This? |
|---|---|
| ⚖️ **ALB** | ✅ Yes |
| 🌐 **NLB** | ✅ Yes |
| 🕰️ **CLB** | ❌ No |

> 🎯 **Key Takeaway:** For load balancing **containerized applications** (ECS), use **ALB or NLB** — not CLB.

---

## 5️⃣ Cross-Zone Load Balancing 🗺️

| Load Balancer | Supported? | Default State |
|---|---|---|
| ⚖️ **ALB** | ✅ Yes | ✅ **Always enabled** (cannot disable) |
| 🌐 **NLB** | ✅ Yes | ❌ **Disabled by default** (must enable) |
| 🕰️ **CLB** | ✅ Yes | ❌ **Disabled by default** (must enable) |

> ⭐ **Exam Tip:** ALB is the **only** one where cross-zone load balancing is **always on** — the other two require manual enablement.

---

## 6️⃣ Sticky Sessions 🍪

> 🎯 **Purpose:** Route all requests from the same user to the same target (useful for session state).

| Load Balancer | Supports Sticky Sessions? |
|---|---|
| ⚖️ **ALB** | ✅ Yes |
| 🕰️ **CLB** | ✅ Yes |
| 🌐 **NLB** | ❌ No |

> 💡 **Why?** Sticky sessions rely on **HTTP/HTTPS cookies** — since NLB doesn't operate at the HTTP layer, it can't implement cookie-based stickiness.

---

## 7️⃣ Server Name Indication (SNI) 📡

> 🎯 **Purpose:** Support **multiple SSL/TLS certificates** for different websites on the **same listener**.

| Load Balancer | Supports SNI? |
|---|---|
| ⚖️ **ALB** | ✅ Yes |
| 🌐 **NLB** | ✅ Yes |
| 🕰️ **CLB** | ❌ Not mentioned/supported |

---

## 8️⃣ Static/Elastic IP 📌

| Load Balancer | Supports Static/Elastic IP? |
|---|---|
| 🌐 **NLB** | ✅ **YES** — the **only** one |
| ⚖️ **ALB** | ❌ No |
| 🕰️ **CLB** | ❌ No |

> ⭐ **This remains one of THE most distinctive, frequently tested NLB facts.**

---

## 9️⃣ Source IP Preservation 🌍

| Load Balancer | Does the EC2 Instance See the Real Client IP? |
|---|---|
| 🌐 **NLB** | ✅ **YES** — preserves and passes the source IP **directly** |
| ⚖️ **ALB** | ❌ **NO** — must check the `X-Forwarded-For` **header** instead |

> 🔗 **Recall from the previous lecture:** This is the same NLB vs ALB distinction covered in the monitoring/logging discussion.

---

## 🔟 WebSockets 🔌

| Load Balancer | Supports WebSockets? |
|---|---|
| ⚖️ **ALB** | ✅ Yes |
| 🌐 **NLB** | ✅ Yes |
| 🕰️ **CLB** | ❌ Not mentioned/supported |

> 💡 **What Are WebSockets?** A protocol enabling **duplex (two-way) communication** — client-to-server AND server-to-client — over a **persistent connection**, without needing to re-establish a connection for each message.

---

## 1️⃣1️⃣ Routing Algorithms (ALB's Big Advantage) 🧠

> 🏆 **This is where Application Load Balancer truly shines** — it supports FAR more sophisticated routing options than NLB or CLB.

| Routing Type | ALB | NLB | CLB |
|---|---|---|---|
| 🌍 **IP Address as Target** | ✅ Yes | ✅ Yes | ❌ No |
| 📐 **CIDR-Based Routing** (IP range) | ✅ Yes | ❌ No | ❌ No |
| 🛣️ **Path-Based Routing** | ✅ Yes | ❌ No | ❌ No |
| 🌐 **Host-Based Routing** | ✅ Yes | ❌ No | ❌ No |
| 📤 **Fixed Response** | ✅ Yes | ❌ No | ❌ No |
| ⚡ **Lambda Functions as Targets** | ✅ Yes | ❌ No | ❌ No |
| 📋 **HTTP Header-Based Routing** | ✅ Yes | ❌ No | ❌ No |
| 📮 **HTTP Method-Based Routing** | ✅ Yes | ❌ No | ❌ No |

> 🎯 **Summary:** **Almost ALL advanced routing algorithms are ALB-exclusive.** NLB and CLB simply don't have the Layer 7 visibility needed to route based on paths, hosts, headers, or methods.

---

## 1️⃣2️⃣ Multiple Target Groups 🎯

| Load Balancer | Supports Multiple Target Groups? |
|---|---|
| ⚖️ **ALB** | ✅ Yes |
| 🌐 **NLB** | ✅ Yes |
| 🕰️ **CLB** | ❌ **No — only ONE target/application per load balancer** |

> 🎯 **Critical Implication:** With CLB, if you need to load balance **multiple separate web applications**, you need a **separate CLB instance for each one**. With ALB or NLB, **one load balancer** can serve **many** applications/microservices via multiple target groups.

---

## 1️⃣3️⃣ The Master Comparison Table 📋

| Feature | ⚖️ ALB | 🌐 NLB | 🕰️ CLB |
|---|---|---|---|
| **Layer** | 7 | 4 | 4 & 7 |
| **Protocols** | HTTP, HTTPS | TCP, UDP, TLS | Both |
| **Connection Draining** | ✅ | — | ✅ |
| **Dynamic Host Port Mapping (ECS)** | ✅ | ✅ | ❌ |
| **Cross-Zone LB Default** | ✅ Always ON | ❌ Off | ❌ Off |
| **Sticky Sessions** | ✅ | ❌ | ✅ |
| **Server Name Indication (SNI)** | ✅ | ✅ | ❌ |
| **Static/Elastic IP** | ❌ | ✅ **Only NLB** | ❌ |
| **Source IP Preservation** | ❌ (use header) | ✅ Direct | — |
| **WebSockets** | ✅ | ✅ | ❌ |
| **IP as Target** | ✅ | ✅ | ❌ |
| **CIDR-Based Routing** | ✅ **Only ALB** | ❌ | ❌ |
| **Path-Based Routing** | ✅ **Only ALB** | ❌ | ❌ |
| **Host-Based Routing** | ✅ **Only ALB** | ❌ | ❌ |
| **Fixed Response** | ✅ **Only ALB** | ❌ | ❌ |
| **Lambda as Target** | ✅ **Only ALB** | ❌ | ❌ |
| **HTTP Header Routing** | ✅ **Only ALB** | ❌ | ❌ |
| **HTTP Method Routing** | ✅ **Only ALB** | ❌ | ❌ |
| **Multiple Target Groups** | ✅ | ✅ | ❌ **Only 1 target** |

---

## 📋 Exam Quick-Reference: "Only X Supports This"

| Feature | Unique To |
|---|---|
| Static/Elastic IP | 🌐 **NLB only** |
| Direct source IP preservation | 🌐 **NLB only** |
| CIDR/Path/Host/Header/Method-based routing | ⚖️ **ALB only** |
| Lambda functions as targets | ⚖️ **ALB only** |
| Fixed response | ⚖️ **ALB only** |
| Single target only (no multiple target groups) | 🕰️ **CLB only** (as a limitation) |

---

## ✅ Final Takeaways

```
⚖️ ALB  → Layer 7 champion: richest routing, sticky sessions, Lambda targets, SNI, WebSockets
🌐 NLB  → Layer 4 champion: raw performance, UDP, Elastic IP, direct source IP preservation, SNI
🕰️ CLB  → Legacy: single target only, limited feature set, avoid for new architectures
🎯 SHARED FEATURES → Dynamic host port mapping (ALB+NLB), WebSockets (ALB+NLB), SNI (ALB+NLB)
⭐ UNIQUE TO NLB    → Elastic IP + direct source IP preservation
⭐ UNIQUE TO ALB    → Virtually ALL advanced Layer-7 routing algorithms + Lambda targets
```

> 🎯 **Golden Rule:** For the exam, memorize this pattern: **"Needs to SEE/ROUTE based on HTTP content" → ALB. "Needs raw speed, UDP, or a fixed IP" → NLB. "It's old/legacy/single-target" → CLB (usually the wrong answer for new designs).** This single table covers the vast majority of load-balancer-comparison exam questions.

> ➡️ **Next Up:** Moving into the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
