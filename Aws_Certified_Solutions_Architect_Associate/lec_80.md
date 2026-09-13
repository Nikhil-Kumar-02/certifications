![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 80: Hands-On — Purchasing Reserved Instances & Scheduled Instances

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Console Walkthrough

---

## 🎯 What This Lecture Is About

```
1️⃣ Region Matters: Where These Options Live
2️⃣ Purchasing a Reserved Instance — Step by Step
3️⃣ How Pricing Actually Compares in the Console
4️⃣ How Reserved Instances Apply Automatically to On-Demand Launches
5️⃣ Purchasing a Scheduled Instance
6️⃣ Switching Back to Your Working Region
```

---

## 1️⃣ Region Matters: Where These Options Live 🌍

```
EC2 Console → (switch region to us-east-1) → Reserved Instances / Scheduled Instances
```

> ⚠️ **Important Regional Difference:**

| Region | Reserved Instances Available? | Scheduled Instances Available? |
|---|---|---|
| 🇺🇸 **us-east-1** (N. Virginia) | ✅ Yes | ✅ Yes |
| 🇮🇳 **ap-south-1** (Mumbai) | ✅ Yes | ❌ **NOT available** |

> 🔑 **Recall from the concept lecture:** Scheduled Instances are only available in **certain regions** and for **certain instance types** — this is a real, hands-on example of that restriction.

---

## 2️⃣ Purchasing a Reserved Instance — Step by Step 🛒

```
EC2 → Reserved Instances → Purchase Reserved Instance
```

### Configuration Options

| Setting | Choices |
|---|---|
| 🖥️ **Platform** | Linux, Windows, etc. |
| 🏢 **Tenancy** | Default (shared) or Dedicated |
| 🏷️ **Class** | Standard or Convertible |
| 📅 **Term** | 1 year or 3 years |
| 💳 **Payment Option** | No Upfront, Partial Upfront, or All Upfront |

```
Leave filters unselected → Click "Search" to browse all available options
```

---

## 3️⃣ How Pricing Actually Compares in the Console 💰

### Observed Example Pricing (illustrative — actual prices vary by region/instance)

| Reservation Type | Hourly Rate |
|---|---|
| 🔹 Standard | $0.008/hour |
| 🔸 Convertible | $0.009/hour |

> ✅ **Confirms the concept lecture:** Standard is consistently **cheaper** than Convertible for the same configuration — the price of extra flexibility.

### Term Length Impact

| Term | Hourly Rate |
|---|---|
| 12 months (1 year) | $0.008/hour |
| 36 months (3 years) | $0.005/hour |

> ✅ **Confirms:** **Longer commitments = lower hourly rates.**

### All Upfront Example

```
t2.micro, All Upfront: Pay $63 total upfront
→ No hourly billing at all for the reservation term
```

### Purchasing

```
Set "Desired Quantity" (e.g., 5 instances) → Add to Cart → Purchase
```

---

## 4️⃣ How Reserved Instances Apply Automatically to On-Demand Launches 🔄

> 🎯 **Key Mechanism:** Reserved Instances aren't a **separate type** of instance you explicitly launch — they work **behind the scenes**.

```
1. You purchase a Reserved Instance matching a specific configuration
   (region, platform, instance type, tenancy)
2. Later, you launch a NORMAL On-Demand instance with a MATCHING configuration
3. AWS automatically detects the match
4. Your On-Demand instance is billed AS IF it were the reserved instance
```

> 💡 **Practical Impact:** If you have an **All Upfront** reservation already paid for, and you later launch a matching On-Demand instance, **you will NOT be billed again** for that instance — the reservation covers it automatically!

---

## 5️⃣ Purchasing a Scheduled Instance 🗓️

```
(Must be in us-east-1) → EC2 → Scheduled Instances → Purchase Scheduled Instances
```

### Example Search Criteria

| Setting | Example Value |
|---|---|
| Start | End of this month |
| Duration | 4 hours |
| Recurrence | Daily |

```
Click Search → Browse available matching schedules
```

> 📌 **Note from the Demo:** Finding an exact matching scheduled instance slot can be **tricky** — availability is limited compared to standard/convertible reservations.

### Configurable Options

| Option | Choices |
|---|---|
| ⏱️ **Hours** | Adjustable |
| 🔁 **Recurring Pattern** | Daily, Weekly, or Monthly |
| 🖥️ **Instance Type** | ⚠️ Limited — only certain types available (not all instance types support Scheduled Instances) |

```
Add to Cart → Purchase
```

---

## 6️⃣ Switching Back to Your Working Region 🔙

> ⚠️ **Important Reminder:** After exploring `us-east-1` for this demo, **switch back** to your course's primary working region (e.g., Mumbai) before continuing.

```
Region Selector → Choose your original region (e.g., ap-south-1 / Mumbai)
```

> 📌 **Possible Message:** You might see a **"Region Unsupported"** message when switching back if you were still on a Reserved/Scheduled Instances screen — this is expected. Simply navigate back to **EC2** and you'll be back in your normal working region.

---

## 📋 Quick Reference

| Task | Where |
|---|---|
| Purchase Standard/Convertible Reserved Instance | Available in **most regions**, including Mumbai |
| Purchase Scheduled Instance | Only in **select regions** (e.g., us-east-1) |
| Reserved Instance auto-applies to matching On-Demand launches | Automatic — no manual linking needed |
| Reselling unused reservations | AWS Reserved Instance Marketplace |

---

## ✅ Final Takeaways

```
🌍 REGION RESTRICTION  → Scheduled Instances not available everywhere (e.g., not in Mumbai)
💰 PRICING CONFIRMED    → Standard < Convertible (hourly rate); longer term = lower rate
🛒 PURCHASE FLOW        → Platform → Tenancy → Class → Term → Payment → Search → Add to Cart
🔄 AUTO-APPLY MAGIC     → Matching On-Demand launches automatically use your reservation — no extra billing
🗓️ SCHEDULED INSTANCES  → Configure hours + recurring pattern (daily/weekly/monthly); limited instance types
```

> 🎯 **Golden Rule:** You never "launch" a Reserved Instance directly the way you launch an On-Demand instance — you **purchase the reservation**, and AWS **automatically matches** it against your regular On-Demand launches, silently applying the discount.

> ➡️ **Next Up:** Deep dive into Savings Plans!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
