![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 76: Spot Instances Deep Dive — Pricing, Spot Block & Spot Fleet

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept + Console Walkthrough (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Old Model vs New Model: How Spot Pricing Works
2️⃣ Exploring Spot Price History in the Console
3️⃣ How Much Do You Actually Save?
4️⃣ ⭐ The 2-Minute Interruption Notice
5️⃣ Ideal Workloads for Spot Instances
6️⃣ Best Practice: Stop/Hibernate vs Terminate
7️⃣ Spot Block
8️⃣ Spot Fleet
```

---

## 1️⃣ Old Model vs New Model: How Spot Pricing Works 💰

| Model | How It Worked |
|---|---|
| 🕰️ **Old Model (Bidding)** | You **bid** a price; the **highest bidder** got the spot instance |
| 🆕 **New Model (Current)** | You specify your **maximum price**; if the **current spot price** is below your max, you get the instance — **no competitive bidding** |

> 💡 **Key Shift:** The current model is simpler — it's not about "winning" against other bidders, just about whether the **market price** happens to be under your ceiling.

> 📌 **How Prices Are Set:** Spot prices are determined by **long-term supply and demand trends** for unused capacity.

---

## 2️⃣ Exploring Spot Price History in the Console 🔍

```
EC2 → Spot Requests → Pricing History
```

### Example Observations (t2.medium, Mumbai region, 3-month view)

| Price Type | Behavior |
|---|---|
| 🖥️ **On-Demand Price** | **Constant** — e.g., $0.05/hour |
| 💸 **Spot Price** | **Fluctuates** — e.g., hovering between $0.01 and $0.02/hour |

> 💡 **Additional Insight:** Spot prices **vary by Availability Zone** too! At the same moment, `ap-south-1a` might have a different spot price than `ap-south-1b`.

### 🎯 What You're Actually Quoting

> When you request a spot instance, you're specifying your **maximum willing price**.

| Your Max Price | Outcome |
|---|---|
| $0.01 (at/near the low end) | ❌ **Almost never** get the instance |
| $0.02 (comfortably above typical range) | ✅ **Almost always** get the instance |

---

## 3️⃣ How Much Do You Actually Save? 💵

> 💰 **Savings:** Spot instances can be up to **90% cheaper** than On-Demand pricing.

---

## 4️⃣ ⭐ The 2-Minute Interruption Notice ⏰

> 🚨 **THE most important fact about Spot Instances:**

| Instance Type | Guarantee |
|---|---|
| 🖥️ **On-Demand** | You keep it until **YOU** terminate it |
| 💸 **Spot** | AWS can **reclaim it at any time**, with only a **2-minute notice** |

> 🎯 **Why AWS Reclaims Spot Instances:** Typically because the **current spot price has risen above your maximum price** (or AWS needs the capacity back for On-Demand customers).

---

## 5️⃣ Ideal Workloads for Spot Instances 🎯

| Requirement | Detail |
|---|---|
| ⏳ **Non Time-Critical** | Workload can run **whenever** — e.g., "can finish within the next week/month" |
| 🛡️ **Fault-Tolerant** | Must be able to **tolerate sudden interruption** |

### ✅ Ideal Example

```
A batch program that:
- Has NO strict deadline
- Can be STOPPED at short notice
- Can RESTART and CONTINUE from where it left off (not from zero)
```

---

## 6️⃣ Best Practice: Stop/Hibernate vs Terminate 🛑

> ⭐ **Critical Best Practice:** When you receive an interruption notice, choose to **STOP or HIBERNATE** the instance — **NOT terminate** it.

| Action | Recovery Behavior |
|---|---|
| ⏸️ **Stop / Hibernate** | ✅ Can **quickly resume** from where you left off once a spot instance becomes available again |
| ❌ **Terminate** | 🐢 Must **start completely from scratch** — new instance, full boot process, no state preserved |

> 🎯 **Why This Matters:** Stopping/hibernating preserves your instance's **state**, dramatically reducing the recovery time and lost work when interrupted.

---

## 7️⃣ Spot Block 🧱

| Fact | Detail |
|---|---|
| 🧱 **Spot Block** | Reserve a spot instance for a **SPECIFIC, FIXED duration** |
| ⏱️ **Duration Options** | 1 to 6 hours |
| 🎯 **Best For** | Jobs where you **know exactly how long** they'll take to complete |

> 💡 **Key Guarantee:** Unlike regular spot instances, a Spot Block **won't be reclaimed** during your reserved time window (with rare exceptions) — giving you predictability for finite-duration jobs.

---

## 8️⃣ Spot Fleet 🚢

| Fact | Detail |
|---|---|
| 🚢 **Spot Fleet** | Request spot instances across a **RANGE of instance types** |
| 🎯 **Why Use It** | Increases the **probability** of getting a spot instance allocated |

### Example

```
Instead of: "I need a t2.medium spot instance" (limited availability)
Do this:    "I need a spot instance — t2.micro, t2.small, t2.medium, or t2.large, whichever is available"
```

> 💡 **Why This Increases Success Rate:** By being flexible about **which instance type** you'll accept, you're no longer dependent on the spot availability of just **one** specific type — any of several types satisfying your request increases your odds.

---

## 📋 Quick Reference: Spot Instance Variations

| Variation | Purpose |
|---|---|
| 💸 **Regular Spot Instance** | Basic bid-below-max-price model; can be reclaimed anytime (2-min notice) |
| 🧱 **Spot Block** | Reserve for a fixed 1-6 hour duration; predictable, no early reclaim |
| 🚢 **Spot Fleet** | Request across multiple instance types; higher chance of allocation |

---

## ✅ Final Takeaways

```
💰 NEW PRICING MODEL → Specify max price; get instance if current spot price is below it (no bidding war)
💵 SAVINGS            → Up to 90% off On-Demand
⏰ 2-MINUTE NOTICE ⭐   → AWS can reclaim a spot instance anytime, with only 2 minutes warning
🎯 IDEAL WORKLOADS     → Non time-critical + fault-tolerant (e.g., flexible batch jobs)
🛑 BEST PRACTICE       → STOP or HIBERNATE on interruption — never terminate — to preserve state
🧱 SPOT BLOCK          → Fixed 1-6 hour reservation for known-duration jobs
🚢 SPOT FLEET          → Range of instance types → higher chance of getting an instance
```

> 🎯 **Golden Rule:** The single most tested Spot Instance fact is the **2-minute interruption notice** combined with the **stop/hibernate over terminate** best practice — memorize both together, as they're closely related exam themes.

> ➡️ **Next Up:** Hands-on — launching Spot Instances, Spot Blocks, and Spot Fleets!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
