![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 74: EC2 Pricing Models — Overview

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Why Pricing Models Matter
2️⃣ The 4 EC2 Pricing Models (Overview)
3️⃣ On-Demand Instances
4️⃣ Spot Instances
5️⃣ Reserved Instances
6️⃣ Savings Plans
7️⃣ Quick Comparison Table
```

---

## 1️⃣ Why Pricing Models Matter 💰

> 🎯 In **any** project — regardless of your role (developer, architect, etc.) — **reducing cost** is a critical goal. Understanding AWS's EC2 pricing models is essential for making cost-effective architectural decisions.

---

## 2️⃣ The 4 EC2 Pricing Models (Overview) 📋

| # | Model | Core Idea |
|---|---|---|
| 1️⃣ | 🖥️ **On-Demand** | Pay for what you use, no commitment — most flexible, most expensive |
| 2️⃣ | 💸 **Spot Instances** | Bid on unused AWS capacity — cheapest, but no guarantee |
| 3️⃣ | 📅 **Reserved Instances** | Commit to 1 or 3 years upfront — big discount, less flexible |
| 4️⃣ | 💡 **Savings Plans** | Commit to a $ spend over 1 or 3 years — discount **with** flexibility |

---

## 3️⃣ On-Demand Instances 🖥️

| Fact | Detail |
|---|---|
| 🏷️ **Model** | The **default** AWS pricing model |
| 💰 **Cost** | Most **expensive** of the four models |
| 🔓 **Flexibility** | Most **flexible** — no commitment, request instances whenever you need them |
| 📌 **Usage in This Course** | This is **exactly what we've been using** throughout the entire course so far! |

> 💡 **Simple Mental Model:** *"I want an EC2 instance right now"* → AWS creates it → you pay for exactly the time you use it.

---

## 4️⃣ Spot Instances 💸

| Fact | Detail |
|---|---|
| 🏷️ **How It Works** | You specify: *"I'm willing to pay up to $X"* — if the current spot price is **below** your bid, you get the instance |
| 💰 **Savings** | Up to **90% off** the On-Demand price |
| ⚠️ **No Guarantee** | You might **not** get an instance — and even if you do, it can be **reclaimed** by AWS at any time |
| 🎯 **Why It Exists** | Lets AWS monetize its **unused/spare capacity**, passing savings to customers willing to accept the risk |

### 🎯 Best Use Cases

```
✅ Non-critical batch jobs
✅ Workloads with flexible timing ("this can run sometime in the next month")
✅ Fault-tolerant, interruptible processing (e.g., big data analysis, rendering)
```

### ❌ NOT Suitable For

```
❌ Critical workloads that need to run RIGHT NOW
❌ Anything that can't tolerate sudden instance termination
```

---

## 5️⃣ Reserved Instances 📅

> ❓ **Question:** What if you have **critical workloads** but still want to save money?

✅ **Answer:** **Reserve** your capacity ahead of time.

| Fact | Detail |
|---|---|
| 🏷️ **How It Works** | Commit to using a specific number of EC2 instances for **1 or 3 years** |
| 💰 **Savings** | Up to **75% off** the On-Demand price |
| 🔒 **Flexibility** | **Low** — you commit to specific instance counts/types; hard to shift (e.g., can't easily switch to Lambda) |

> 🎯 **Best For:** Predictable, **steady-state** workloads that you know you'll be running long-term.

---

## 6️⃣ Savings Plans 💡

> 🆕 A **newer**, more flexible alternative to Reserved Instances.

| Fact | Detail |
|---|---|
| 🏷️ **How It Works** | Commit to spending a **specific dollar amount** (e.g., "$10,000") across **EC2, Fargate, and Lambda** — not tied to a specific instance count/type |
| 💰 **Savings** | Up to **66% off** the On-Demand price |
| 🔓 **Flexibility** | **Higher** than Reserved Instances — your commitment is a **spend amount**, not a rigid resource commitment |
| ⏳ **Commitment Term** | Same as Reserved Instances — **1 or 3 years** |

### 🎓 What Are Fargate and Lambda? (Quick Preview)

| Service | What It Is |
|---|---|
| 📦 **Fargate** | AWS's **serverless container** service |
| ⚡ **Lambda** | AWS's **serverless compute** offering |

> 📌 Both will be covered in **much more depth** later in the course — for now, just know Savings Plans can apply savings across **all three** (EC2, Fargate, Lambda).

---

## 7️⃣ Quick Comparison Table 📋

| Feature | 🖥️ On-Demand | 💸 Spot | 📅 Reserved | 💡 Savings Plans |
|---|---|---|---|---|
| **Commitment** | None | None (bid-based) | 1 or 3 years | 1 or 3 years |
| **Max Savings** | 0% (baseline) | Up to **90%** | Up to **75%** | Up to **66%** |
| **Flexibility** | ✅ Highest | ⚠️ Unpredictable availability | ❌ Low (fixed instance commitment) | ✅ Higher (flexible $ commitment across services) |
| **Guarantee** | ✅ Yes | ❌ No (can be reclaimed) | ✅ Yes | ✅ Yes |
| **Best For** | Unpredictable, short-term needs | Non-critical, interruptible workloads | Predictable, long-term, fixed workloads | Predictable, long-term, but across varied services |

---

## 📋 Quick Decision Guide

```
Need it RIGHT NOW, unpredictable duration?
   → On-Demand

Non-critical, flexible timing, want MAXIMUM savings?
   → Spot Instances

Predictable long-term workload, know EXACTLY what instances you need?
   → Reserved Instances

Predictable long-term spend, but want flexibility across EC2/Fargate/Lambda?
   → Savings Plans
```

---

## ✅ Final Takeaways

```
🖥️ ON-DEMAND      → Default, most flexible, most expensive — what we've used all course
💸 SPOT            → Up to 90% off; no guarantee; best for non-critical/interruptible workloads
📅 RESERVED         → Up to 75% off; 1-3 year commitment; low flexibility (fixed instance commitment)
💡 SAVINGS PLANS   → Up to 66% off; 1-3 year commitment; HIGH flexibility ($ spend across EC2/Fargate/Lambda)
🎯 KEY TRADE-OFF   → More savings generally means LESS flexibility (except Savings Plans, which improve on Reserved's flexibility)
```

> 🎯 **Golden Rule:** The core exam pattern here is matching a **workload's characteristics** (critical vs non-critical, predictable vs unpredictable, fixed vs varied services) to the **pricing model** that fits best — it's rarely about which model is "cheapest" in isolation, but which one fits the **use case**.

> ➡️ **Next Up:** Deep dive into each pricing model, starting with On-Demand and Spot Instances!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
