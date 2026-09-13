![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 43: Types of AWS Elastic Load Balancers (CLB, ALB, NLB)

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Three Types of Elastic Load Balancers
2️⃣ Classic Load Balancer (CLB) — Legacy
3️⃣ Application Load Balancer (ALB) — Layer 7
4️⃣ Network Load Balancer (NLB) — Layer 4
5️⃣ Why AWS Split CLB into ALB & NLB
6️⃣ Comparison Table
```

---

## 1️⃣ Three Types of Elastic Load Balancers ⚖️

| # | Load Balancer | Layer | Status |
|---|---|---|---|
| 1️⃣ | 🕰️ **Classic Load Balancer (CLB)** | Layer 4 + Layer 7 | ⚠️ **Legacy — not recommended** |
| 2️⃣ | ⚖️ **Application Load Balancer (ALB)** | Layer 7 (Application) | ✅ **Recommended** for HTTP/HTTPS apps |
| 3️⃣ | 🌐 **Network Load Balancer (NLB)** | Layer 4 (Transport) | ✅ **Recommended** for high-performance TCP/UDP |

> 🔗 **Recall from the networking deep dive:** Layer 4 = Transport (TCP/TLS/UDP), Layer 7 = Application (HTTP/HTTPS).

---

## 2️⃣ Classic Load Balancer (CLB) — Legacy 🕰️

| Fact | Detail |
|---|---|
| 🎯 **Layers Supported** | **Both** Layer 4 (TCP, TLS) **and** Layer 7 (HTTP, HTTPS) |
| ⚠️ **Recommendation** | AWS **does not recommend** using CLB for **new applications** |
| 📚 **Exam Relevance** | You can largely **"forget" about CLB** for the certification exam — it's mostly a legacy/historical concept now |

> 💡 **Why is it being phased out?** AWS realized that trying to support **both layers in a single load balancer** was overly complex and less optimized. This led to splitting the functionality into two purpose-built load balancers: **ALB** and **NLB**.

---

## 3️⃣ Application Load Balancer (ALB) — Layer 7 ⚖️

| Fact | Detail |
|---|---|
| 🎯 **Layer** | **Layer 7** — Application layer |
| 📡 **Protocols Supported** | HTTP, HTTPS, **WebSockets** |
| 🧭 **Routing Intelligence** | Can inspect the **actual content** of requests |

### 🔍 Advanced Routing Capabilities

ALB can route traffic based on:

| Routing Criteria | Example |
|---|---|
| 📋 **Request Header** | Route based on a custom header value |
| 🛣️ **Request Path** | `/api/*` → API servers, `/images/*` → image servers |
| 🌐 **Request Hostname** | `blog.example.com` → blog servers, `shop.example.com` → shop servers |

> 🎯 **Key Strength:** Because ALB understands **HTTP/HTTPS content**, it can make **smart, content-aware routing decisions** — something a Layer 4 load balancer simply cannot do (it can't "see" inside the request).

> ✅ **Most Widely Used:** ALB is the **most flexible and popular** load balancer type for typical **web applications and microservices**.

---

## 4️⃣ Network Load Balancer (NLB) — Layer 4 🌐

| Fact | Detail |
|---|---|
| 🎯 **Layer** | **Layer 4** — Transport layer |
| 📡 **Protocols Supported** | TCP, TLS, **UDP** |
| ⚡ **Primary Strength** | **Extremely high performance** |

> 🎯 **Best For:** Use cases requiring **very high throughput**, **low latency**, and **massive scale** — where you don't need to inspect HTTP-level content, just route raw TCP/UDP traffic as fast as possible.

> 💡 **Recall:** UDP is used for things like gaming and streaming (per our networking deep dive) — NLB is the load balancer type capable of handling **UDP traffic**, which ALB **cannot**.

---

## 5️⃣ Why AWS Split CLB into ALB & NLB 🔀

> 🧠 **AWS's Reasoning:** Combining Layer 4 and Layer 7 support into a **single load balancer** (as CLB did) was a **design mistake**.

| Problem with CLB | Solution |
|---|---|
| Trying to be "good at everything" led to being **not optimal** at anything | Split into **two purpose-built** load balancers |
| No advanced content-based routing | ✅ **ALB** — purpose-built for Layer 7 intelligence |
| Not optimized for raw performance | ✅ **NLB** — purpose-built for Layer 4 speed |

> 🎯 **Design Philosophy:** Specialize each load balancer for its layer, rather than compromise with a "jack of all trades" solution.

---

## 6️⃣ Comparison Table 📋

| Feature | 🕰️ Classic (CLB) | ⚖️ Application (ALB) | 🌐 Network (NLB) |
|---|---|---|---|
| **OSI Layer** | 4 & 7 | 7 (Application) | 4 (Transport) |
| **Protocols** | TCP, TLS, HTTP, HTTPS | HTTP, HTTPS, WebSockets | TCP, TLS, UDP |
| **Content-Based Routing** | ❌ Limited | ✅ Yes (path, header, hostname) | ❌ No |
| **Performance** | Moderate | Good | ⚡ **Extremely High** |
| **UDP Support** | ❌ No | ❌ No | ✅ **Yes** |
| **AWS Recommendation** | ⚠️ Avoid for new apps | ✅ Recommended for web apps | ✅ Recommended for high-perf/TCP-UDP |
| **Typical Use Case** | Legacy applications | Web apps, microservices, REST APIs | Gaming, IoT, high-throughput systems |

---

## 📋 Quick Decision Guide

```
Building a typical web app / REST API / microservices architecture?
   → Use Application Load Balancer (ALB)

Need extreme performance, low latency, or UDP support?
   → Use Network Load Balancer (NLB)

Working with a legacy application that already uses CLB?
   → Leave it as-is, but don't build NEW systems on CLB
```

---

## ✅ Final Takeaways

```
🕰️ CLB  → Layer 4 + 7, legacy, NOT recommended for new apps
⚖️ ALB  → Layer 7, HTTP/HTTPS/WebSockets, smart content-based routing
🌐 NLB  → Layer 4, TCP/TLS/UDP, extreme performance & scale
🧠 WHY SPLIT? → Specialization beats a one-size-fits-all approach
📚 EXAM TIP  → Know that ALB = Layer 7 (path/host/header routing), NLB = Layer 4 (raw performance, UDP)
```

> 🎯 **Golden Rule:** If your application needs **smart routing based on HTTP content** → **ALB**. If it needs **raw speed at massive scale, including UDP** → **NLB**. If you see **CLB** in an exam question, it's almost certainly testing whether you know it's **legacy and not recommended**.

> ➡️ **Next Up:** Hands-on — creating an Application Load Balancer!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
