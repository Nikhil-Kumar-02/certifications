![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 79: Reserved Instances Deep Dive — Standard, Convertible & Scheduled

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ What Are Reserved Instances?
2️⃣ Three Types of Reserved Instances (Overview)
3️⃣ Three Payment Models
4️⃣ Standard Reserved Instances — Deep Dive
5️⃣ Convertible Reserved Instances — Deep Dive
6️⃣ Scheduled Reserved Instances — Deep Dive
7️⃣ Discount Summary
8️⃣ Reselling: The Reserved Instance Marketplace
```

---

## 1️⃣ What Are Reserved Instances? 💡

> 🔒 **Reserved Instances (RIs)** let you **reserve EC2 capacity ahead of time**, in exchange for a **significant discount** compared to On-Demand pricing.

---

## 2️⃣ Three Types of Reserved Instances (Overview) 📋

| Type | Flexibility | Key Idea |
|---|---|---|
| 🔹 **Standard** | Lowest | The most basic type — biggest discount, least flexibility |
| 🔸 **Convertible** | Medium | Allows changing instance type, family, OS, or tenancy |
| 🔹 **Scheduled** | Specialized | Reserve for a **specific recurring time block**, not all the time |

---

## 3️⃣ Three Payment Models 💳

| Model | How It Works | Discount Level |
|---|---|---|
| 🔴 **No Upfront** | Pay $0 upfront; pay everything in **monthly installments** | Lowest discount |
| 🟡 **Partial Upfront** | Pay **some** amount upfront; rest in monthly installments | Medium discount |
| 🟢 **All Upfront** | Pay the **full reservation amount** upfront; no monthly bills | **Highest discount** |

> 💰 **Rule of Thumb:** *"The earlier you pay, the more you save."* The difference between payment models can be up to **~5%** in additional discount.

```
All Upfront (cheapest) < Partial Upfront < No Upfront (most expensive)
```

---

## 4️⃣ Standard Reserved Instances — Deep Dive 🔹

### The Commitment You Make

```
"In [Region X], I want to reserve a [Platform] [Instance Type] instance for [1 or 3] years."
```

**Example:** *"In us-east-1, I want to reserve a Linux t2.micro instance for 1 year."*

### ✅ What You CAN Change

| Flexibility | Detail |
|---|---|
| 🔄 **Instance Type** | ✅ Can switch (e.g., t2.micro → t2.large → t2.xlarge) |
| 🗺️ **Availability Zone** | ✅ Can use any AZ in the reserved region |

### ❌ What You CANNOT Change

| Restriction | Detail |
|---|---|
| 🚫 **Instance Family** | Cannot switch (e.g., memory-optimized → storage-optimized) |
| 🚫 **Operating System** | Cannot switch (e.g., Linux → Windows) |
| 🚫 **Tenancy** | Cannot switch (e.g., shared → dedicated) |

> 💰 **Discount:** Up to **75% off** On-Demand pricing.

---

## 5️⃣ Convertible Reserved Instances — Deep Dive 🔸

> 🎯 If you need **more flexibility** than Standard offers, go **Convertible**.

### The Commitment You Make

```
"In [Region X], I want to use an EC2 instance for a term of [1 or 3] years."
```

### ✅ What You CAN Change

| Flexibility | Detail |
|---|---|
| 🔄 **Instance Family** | ✅ Can switch |
| 🔄 **Operating System** | ✅ Can switch |
| 🔄 **Tenancy** | ✅ Can switch |
| 🔄 **Availability Zone** | ✅ Can switch |
| 🔄 **Instance Size** | ✅ Can switch |

> 💰 **Discount:** Up to **54% off** On-Demand pricing.

> 🎯 **Trade-off:** More flexibility = **lower** maximum discount compared to Standard.

---

## 6️⃣ Scheduled Reserved Instances — Deep Dive 🔹

### The Commitment You Make

```
"In [Region X], I want to reserve an EC2 instance for a year,
to be used for X hours, every [day/week/month] at a specific time."
```

### ⚠️ Restrictions

| Restriction | Detail |
|---|---|
| 🌍 **Region Availability** | Only available in **certain regions** |
| 🖥️ **Instance Type Availability** | Only available for **certain instance types** |

### 🎯 Ideal Use Cases

```
✅ Monthly billing batch job (e.g., runs on the 1st of every month)
✅ Daily batch program (runs a few hours every day)
✅ Weekend batch program (runs a few hours every week)
```

> 💰 **Discount:** About **5-10% off** — much smaller than Standard or Convertible, since you're only committing to **partial-time usage**.

---

## 7️⃣ Discount Summary 📊

| Type | Max Discount | Flexibility |
|---|---|---|
| 🔹 **Standard** | Up to **75%** | Lowest (instance type + AZ only) |
| 🔸 **Convertible** | Up to **54%** | Highest (family, OS, tenancy, AZ, size) |
| 🔹 **Scheduled** | ~**5-10%** | Time-based only (specific recurring hours) |

> 🎯 **Key Insight:** There's a clear **inverse relationship** between flexibility and discount — Standard gives you the **biggest discount** but the **least flexibility**; Convertible trades some discount for much more flexibility; Scheduled is a **completely different dimension** (time-based, not type-based).

---

## 8️⃣ Reselling: The Reserved Instance Marketplace 🏪

> 💡 **Important Safety Net:** If you've reserved an instance but **no longer need it**, you're not stuck!

| Fact | Detail |
|---|---|
| 🏪 **Reserved Instance Marketplace** | AWS provides a marketplace where you can **sell** your unused reservation to other AWS customers |

> 🎯 **Why This Matters:** Reduces the risk of committing to a 1 or 3-year reservation — if your needs change, you have an exit option rather than simply wasting the reservation.

---

## 📋 Master Comparison Table

| Feature | 🔹 Standard | 🔸 Convertible | 🔹 Scheduled |
|---|---|---|---|
| **Can Change Instance Type** | ✅ Yes | ✅ Yes | N/A |
| **Can Change Instance Family** | ❌ No | ✅ Yes | N/A |
| **Can Change OS** | ❌ No | ✅ Yes | N/A |
| **Can Change Tenancy** | ❌ No | ✅ Yes | N/A |
| **Can Change AZ** | ✅ Yes | ✅ Yes | N/A |
| **Max Discount** | ~75% | ~54% | ~5-10% |
| **Best For** | Fixed, predictable, long-term workloads | Long-term but evolving requirements | Recurring, time-boxed workloads |

---

## ✅ Final Takeaways

```
🔒 RESERVED INSTANCES → Commit ahead of time for a big discount vs On-Demand
🔹 STANDARD           → Biggest discount (~75%); can change instance type/AZ only
🔸 CONVERTIBLE         → More flexible (family/OS/tenancy/AZ/size); lower discount (~54%)
🔹 SCHEDULED           → For recurring time-boxed workloads; smallest discount (~5-10%)
💳 3 PAYMENT MODELS    → No Upfront < Partial Upfront < All Upfront (in terms of savings)
🏪 RI MARKETPLACE      → Can resell unused reservations if your needs change
```

> 🎯 **Golden Rule:** When an exam scenario mentions needing to **change instance family, OS, or tenancy** during the reservation term, the answer is **Convertible**. When it mentions a **fixed, unchanging** long-term workload, the answer is **Standard**. When it mentions **recurring scheduled usage** (daily/weekly/monthly batch jobs), the answer is **Scheduled**.

> ➡️ **Next Up:** Hands-on — purchasing Reserved Instances and Scheduled Instances in the console!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
