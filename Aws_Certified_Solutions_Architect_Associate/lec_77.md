![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 77: Hands-On — Launching Spot Instances, Spot Blocks & Spot Fleets

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Console Walkthrough

---

## 🎯 What This Lecture Is About

```
1️⃣ Requesting a Basic Spot Instance
2️⃣ Setting Your Maximum Price
3️⃣ Persistent Spot Requests
4️⃣ Interruption Behavior: Terminate vs Stop vs Hibernate
5️⃣ Request Valid From/To (Time Windows)
6️⃣ Launching a Spot Block
7️⃣ Launching a Spot Fleet
```

---

## 1️⃣ Requesting a Basic Spot Instance 🚀

```
EC2 → Launch Instance → Select AMI → Configure Instance Details
→ Look for "Request Spot Instances"
```

> 📌 This demo is a **walkthrough only** — no actual spot instance was launched.

### What You'll See

| Element | Detail |
|---|---|
| 💰 **Current Prices** | Shown for **different Availability Zones** |
| 🎯 **Maximum Price Field** | Where you specify your willingness-to-pay ceiling |

---

## 2️⃣ Setting Your Maximum Price 💵

```
Example: Maximum Price = $0.005
```

> 🎯 **Behavior:**

| Condition | Result |
|---|---|
| Current spot price ≤ $0.005 | ✅ You **keep** the spot instance |
| Current spot price > $0.005 | ❌ You **lose** the spot instance |

---

## 3️⃣ Persistent Spot Requests 🔁

> ❓ **What Is a Persistent Request?**

| Fact | Detail |
|---|---|
| 🔁 **Persistent Request** | Keeps your spot request **active indefinitely** |
| 🔄 **Behavior** | Whenever the price drops back **below** your maximum, a **new spot instance is automatically created** for you |

```
☑️ Check the "Persistent Request" checkbox to enable this
```

> 💡 **Why Use This?** Instead of a one-time request that either succeeds or fails, a persistent request keeps **trying** on your behalf over time — reallocating an instance whenever conditions become favorable again.

---

## 4️⃣ Interruption Behavior: Terminate vs Stop vs Hibernate ⚙️

> When making a **persistent request**, you can choose what happens when your spot instance is **interrupted**:

| Option | Availability | Recovery Speed |
|---|---|---|
| ❌ **Terminate** | Always available | 🐢 Slow — full restart from scratch |
| ⏸️ **Stop** | Always available | ⚡ Fast — resumes from stopped state |
| 🧊 **Hibernate** | ⚠️ **Not available for all instance types** | ⚡ Fastest — preserves memory state too |

> ⭐ **Best Practice (recap from previous lecture):** Choose **Stop** or **Hibernate** — **NOT Terminate** — to enable quick recovery when the instance becomes available again.

---

## 5️⃣ Request Valid From/To (Time Windows) 🕐

> 💡 You can also restrict **when** your spot request is considered active.

```
Request Valid From: [start time]
Request Valid To:   [end time]
```

> 🎯 **Use Case:** Only try to get a spot instance during a **specific time window** — outside that window, the request is simply inactive.

---

## 6️⃣ Launching a Spot Block 🧱

```
EC2 → Instances → Spot Requests → Request Spot Instances
```

> 📌 On this screen, you can customize your spot request for different **workload types**: Load Balancing workloads, Flexible workloads, or Big Data workloads.

### Configuring the Spot Block Duration

```
"Launch instances into a Spot Block for [1-6] hours"
```

| Setting | Options |
|---|---|
| ⏱️ **Duration** | 1, 2, 3, 4, 5, or 6 hours |

> 💡 Beyond the duration, you configure a Spot Block **exactly like a normal instance**: AMI, instance type, network, Availability Zone, key pair, etc.

---

## 7️⃣ Launching a Spot Fleet 🚢

```
Same "Request Spot Instances" screen → Configure for Spot Fleet
```

### Instance Type Selection

| Default Behavior | AWS **automatically selects** multiple instance types for you |
|---|---|
| ✏️ **Customization** | You can **remove** the auto-selected types and **manually choose** exactly which instance types should be eligible |

```
Example: Accept ANY of these types for the fleet request:
   t2.micro, t2.small, t2.medium, t2.large
```

> 💡 **Reminder:** You don't need to master every configuration detail here — the **key concept** to walk away with is understanding **how** and **where** to configure a Spot Block vs a Spot Fleet.

---

## 📋 Quick Reference: Spot Configuration Locations

| Feature | Where to Configure |
|---|---|
| Basic Spot Instance | EC2 → Launch Instance → Configure Instance Details → "Request Spot Instances" |
| Maximum Price | Same screen — price field |
| Persistent Request | Checkbox on the same screen |
| Interruption Behavior (Stop/Hibernate/Terminate) | Appears when Persistent Request is checked |
| Request Valid From/To | Same request screen |
| Spot Block | EC2 → Spot Requests → Request Spot Instances → choose duration (1-6 hrs) |
| Spot Fleet | EC2 → Spot Requests → Request Spot Instances → configure instance type range |

---

## ✅ Final Takeaways

```
💵 MAX PRICE FIELD    → Set your ceiling; instance kept as long as spot price stays below it
🔁 PERSISTENT REQUEST → Keeps retrying automatically whenever price drops below your max
🛑 STOP/HIBERNATE      → Preferred over Terminate for fast recovery after interruption
🕐 VALID FROM/TO       → Restrict WHEN your spot request is active
🧱 SPOT BLOCK          → Fixed 1-6 hour duration, configured like a normal instance otherwise
🚢 SPOT FLEET          → Configurable range of instance types for higher allocation success
```

> 🎯 **Golden Rule:** All spot instance variations (basic, block, fleet) are configured from the **same general area** of the EC2 console (Spot Requests) — the differences lie in **duration control** (Block) vs **instance type flexibility** (Fleet).

> ➡️ **Next Up:** Deep dive into Reserved Instances and Savings Plans!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
