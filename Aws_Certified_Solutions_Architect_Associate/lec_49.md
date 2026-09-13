![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 49: Target Groups Deep Dive

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Why Do Target Groups Exist?
2️⃣ Types of Targets
3️⃣ Target Group Details in the Console
4️⃣ Deregistration Delay (Connection Draining)
5️⃣ Slow Start Duration
6️⃣ Load Balancing Algorithms
7️⃣ Sticky Sessions
8️⃣ Health Checks (Recap)
9️⃣ Monitoring
```

---

## 1️⃣ Why Do Target Groups Exist? 🎯

> A **Target Group** groups together the resources (instances, IPs, or Lambda functions) that a load balancer should **distribute traffic between**.

| Benefit | Explanation |
|---|---|
| 🗂️ **Grouping** | Organizes which resources receive traffic |
| ➕➖ **Dynamic Membership** | You can **add/remove** instances from a target group at any time — the load balancer automatically adjusts |
| 🔀 **Routing Target** | The load balancer's listener rules point traffic **at a target group**, not directly at individual instances |

---

## 2️⃣ Types of Targets 🎯

| Target Type | Description |
|---|---|
| 🖥️ **EC2 Instances** | Traditional virtual servers |
| 🌍 **IP Addresses** | Direct IP-based targets |
| ⚡ **Lambda Functions** | Serverless targets |

---

## 3️⃣ Target Group Details in the Console 🔍

```
EC2 → Target Groups → Select "my-target-group"
```

| Field | Value |
|---|---|
| Name | `my-target-group` |
| ARN | AWS Resource Name (unique identifier) |
| Protocol / Port | HTTP / 80 |
| Target Type | Instance |
| Associated Load Balancer | `my-application-load-balancer` |

> There are **4 important attributes** on a target group worth understanding deeply: **Deregistration Delay**, **Slow Start Duration**, **Load Balancing Algorithm**, and **Stickiness**.

---

## 4️⃣ Deregistration Delay (Connection Draining) ⏳

### What Problem Does It Solve?

> When a target is being **removed** from a target group (due to a failed health check or manual removal), it might still be **actively processing** in-flight requests.

❓ **What should happen to those in-progress requests?**

✅ **Answer:** Let them **complete successfully** before fully removing the target.

| Setting | Value |
|---|---|
| **Name** | Deregistration Delay (also called Connection Draining) |
| **Default** | 300 seconds (5 minutes) |
| **Range** | 0 – 3600 seconds (up to 1 hour) |

```
Target Group → Edit attributes → Deregistration delay
```

> 💡 **How it works:** The load balancer **stops sending new requests** to the deregistering target immediately, but **waits** up to the configured delay for **existing** requests to finish before fully removing it.

---

## 5️⃣ Slow Start Duration 🐢➡️🚀

### What Problem Does It Solve?

> When a **new target** is added to the group, you don't want it to be **immediately flooded** with a full share of traffic — it needs time to "warm up" (e.g., JIT compilation, cache warming, connection pool initialization).

| Setting | Value |
|---|---|
| **Name** | Slow Start Duration |
| **Range** | 30 – 900 seconds |

> 💡 **How it works:** The number of requests sent to the new target **gradually increases** over the configured duration, rather than jumping straight to a full share immediately.

---

## 6️⃣ Load Balancing Algorithms ⚖️

| Algorithm | How It Works |
|---|---|
| 🔄 **Round Robin** *(default)* | Requests are distributed **sequentially** — 1st request → Target 1, 2nd → Target 2, 3rd → Target 3, then back to Target 1, and so on |
| 📊 **Least Outstanding Requests** | Sends the next request to whichever target currently has the **fewest active/outstanding requests** |

### 📋 Comparison

| Aspect | Round Robin | Least Outstanding Requests |
|---|---|---|
| Complexity | Simple | Slightly smarter |
| Best For | Uniform request processing times | Requests with **varying** processing times |
| Default? | ✅ Yes | ❌ No (opt-in) |

---

## 7️⃣ Sticky Sessions 🍪

### What Problem Does It Solve?

> Some web applications need to **maintain session state**, and want **all requests from the same user** to consistently go to the **same backend instance**.

| Fact | Detail |
|---|---|
| 🎯 **Purpose** | Route all requests from a specific user to the **same target**, every time |
| 🍪 **Implementation** | Achieved via a **cookie** |
| ✅ **Supported By** | Both **Application Load Balancer** and **Classic Load Balancer** |

### Example Use Case

```
User A → Always routed to → Instance 1
User B → Always routed to → Instance 2
```

> 💡 **Why This Matters:** Without stickiness, a user's session data stored **locally** on one instance might not be visible if their next request lands on a **different** instance — causing inconsistent behavior (e.g., being logged out unexpectedly).

> ⚠️ **Trade-off:** Sticky sessions can lead to **uneven load distribution** if some users generate significantly more traffic than others. Many modern architectures prefer **externalizing session state** (e.g., using a shared cache like Redis) instead of relying on stickiness.

---

## 8️⃣ Health Checks (Recap) 🏥

> Configured **on the target group** (for ALB) — same core settings we've seen before:

| Setting | Example Value |
|---|---|
| Protocol | HTTP |
| Path | `/` |
| Port | Traffic port (80) |
| Healthy Threshold | 5 |
| Unhealthy Threshold | 2 |
| Timeout | 5 seconds |
| Interval | 30 seconds |
| Success Code | 200 |

```
Target Group → Edit health check
```

---

## 9️⃣ Monitoring 📊

> Like almost everything in AWS, target groups are monitored via **CloudWatch**.

### Available Metrics

| Metric | What It Shows |
|---|---|
| ✅ Healthy Host Count | Targets currently passing health checks |
| ❌ Unhealthy Host Count | Targets currently failing health checks |
| ⏱️ Response Time | How long requests take |
| 📥 Request Count | Total requests processed |
| ❌ Error Count | Failed requests |

---

## 📋 Quick Reference: The 4 Key Target Group Attributes

| Attribute | Purpose | Range/Default |
|---|---|---|
| 🚪 **Deregistration Delay** | Let in-flight requests finish before removing a target | 0–3600s (default 300s) |
| 🐢 **Slow Start Duration** | Gradually ramp up traffic to newly added targets | 30–900s |
| ⚖️ **Load Balancing Algorithm** | How requests are distributed | Round Robin (default) or Least Outstanding Requests |
| 🍪 **Stickiness** | Route a user's requests consistently to the same target | Enabled/Disabled (cookie-based) |

---

## ✅ Final Takeaways

```
🎯 TARGET GROUP       → Groups resources (EC2/IP/Lambda) that receive LB traffic
⏳ DEREGISTRATION DELAY → Graceful removal; lets in-flight requests finish (default 300s)
🐢 SLOW START          → Gradually ramps up traffic to new targets (30-900s)
⚖️ ALGORITHMS          → Round Robin (default) vs Least Outstanding Requests
🍪 STICKY SESSIONS     → Cookie-based; same user → same target; supported by ALB & CLB
📊 MONITORING          → CloudWatch tracks health, latency, and request metrics
```

> 🎯 **Golden Rule:** Target Groups are where most of the **fine-tuning** of load balancer behavior happens — deregistration delay, slow start, algorithm choice, and stickiness are all configured **here**, not on the load balancer itself.

> ➡️ **Next Up:** Using multiple target groups for microservices architectures with path-based routing!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
