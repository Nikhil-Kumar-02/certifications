![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 55: Auto Scaling Group Components & Use Cases Review

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing & Auto Scaling
> ⏱️ **Type:** Concept Review (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ The 4 Core Components of an Auto Scaling Group
2️⃣ Min/Max Rules Always Win
3️⃣ Use Case 1: Maintain a Constant Instance Count
4️⃣ Use Case 2: Full Manual Control
5️⃣ Use Case 3: Scheduled Scaling
6️⃣ Use Case 4: Dynamic (Demand-Based) Scaling
7️⃣ Three Types of Dynamic Scaling Policies
8️⃣ Recommendation: Keep It Simple
```

---

## 1️⃣ The 4 Core Components of an Auto Scaling Group 🧩

| # | Component | Purpose |
|---|---|---|
| 1️⃣ | 📄 **Launch Template/Configuration** | Defines instance **hardware, software, and config** (AMI, instance type, security groups, etc.) |
| 2️⃣ | 📊 **Auto Scaling Group** | References the launch template; defines **Min/Max/Desired** capacity |
| 3️⃣ | 🏥 **Health Checks** | EC2 health checks (basic) or ELB health checks (checks a specific URL) |
| 4️⃣ | 📈 **Auto Scaling Policy** | Defines **when and how** to scale |

---

## 2️⃣ Min/Max Rules Always Win ⚠️

> 🔒 **Critical Rule:** No matter what your scaling policy says, the ASG will **never** violate the configured **Minimum** or **Maximum** size.

### Example

```
Configuration: Min = 1, Max = 6
Policy: Scale out when CPU > 70%

Scenario: CPU hits 80%, but the ASG is ALREADY at 6 instances (the max)
Result: ❌ NO further scale-out happens — the maximum limit is a hard ceiling
```

> 🎯 **Exam Tip:** Scaling policies operate **within** the Min/Max bounds — they never override them.

---

## 3️⃣ Use Case 1: Maintain a Constant Instance Count 🔒

> 🎯 **Goal:** Always have exactly **N** healthy instances running — no scaling up or down.

### How to Configure

```
Min = Max = Desired = 5 (for example)
```

> ✅ **Result:** The ASG ensures **5 healthy instances** are **always** running. If one fails or is terminated, a **replacement** is automatically launched — but the count never goes above or below 5.

> 💡 **Best For:** Applications with **predictable, constant load**.

---

## 4️⃣ Use Case 2: Full Manual Control ✋

> 🎯 **Goal:** You want to scale up/down **manually**, on your own schedule/judgment.

### How to Configure

```
Set Min and Max to define a range
Manually adjust "Desired Capacity" whenever you want to scale
```

> 📌 **Constraint:** Desired Capacity must always be **between Min and Max**.

> 💡 **Best For:** Scenarios needing **human judgment** in scaling decisions, or as a stepping stone before adopting automated policies.

---

## 5️⃣ Use Case 3: Scheduled Scaling 🗓️

> 🎯 **Goal:** Scale based on a **known, predictable schedule** — e.g., a batch job that runs every night, or higher expected traffic during business hours.

### How to Configure

```
Auto Scaling Group → Automatic Scaling → Scheduled Actions → Create scheduled action
```

| Setting | Example |
|---|---|
| Desired Capacity | 10 |
| Min | 5 |
| Max | 15 |
| Schedule | "Every day at 9 AM" |

> 💡 **Best For:** Known traffic patterns — e.g., predictable business hours, regular batch processing windows.

---

## 6️⃣ Use Case 4: Dynamic (Demand-Based) Scaling 📊

> 🎯 **Goal:** Scale automatically based on **real-time metrics** — this is also called **Dynamic Scaling** or **Automatic Scaling**.

### How to Configure

```
Create a Scaling Policy → Define what to monitor (e.g., CPU Utilization) → Define the action to take
```

> 💡 **Best For:** **Unpredictable** load patterns — you don't know in advance when traffic will spike or drop, so the system reacts to actual demand.

---

## 7️⃣ Three Types of Dynamic Scaling Policies 📈

### 🎯 Type 1: Target Tracking Scaling (Simplest)

| Fact | Detail |
|---|---|
| 🎯 **How It Works** | You specify a **target metric value** (e.g., "maintain 70% average CPU utilization") |
| 🤖 **Who Decides Actions** | AWS **automatically** figures out how many instances to add/remove to hit that target |
| ✅ **Recommended** | This is the **simplest and most commonly recommended** approach |

```
Example: "Maintain average CPU utilization at 70%"
```

---

### ⚙️ Type 2: Simple Scaling

| Fact | Detail |
|---|---|
| 🎯 **How It Works** | You define **specific actions** for specific thresholds |
| 📋 **Example** | "If CPU > 80%, **add 5 instances**." "If CPU < 60%, **remove 3 instances**." |

```
IF CPU_Utilization > 80%  → +5 instances
IF CPU_Utilization < 60%  → -3 instances
```

---

### 🪜 Type 3: Step Scaling (Most Complex)

| Fact | Detail |
|---|---|
| 🎯 **How It Works** | Define **multiple tiers ("steps")** of response based on how far a metric has crossed a threshold |
| 📋 **Example** | Different scaling responses for **different severity levels** |

```
IF CPU_Utilization is 70-80%   → +1 instance
IF CPU_Utilization is 80-100%  → +3 instances
```

> 💡 **Why Step Scaling?** Allows a **proportional response** — a small CPU spike gets a small correction, a large spike gets a bigger correction. (Similar step-based rules can be configured for scale-down too.)

---

## 8️⃣ Recommendation: Keep It Simple ✅

| Policy Type | Complexity | Recommendation |
|---|---|---|
| 🎯 **Target Tracking** | ⭐ Simple | ✅ **Recommended** — use this by default |
| ⚙️ **Simple Scaling** | ⭐⭐ Moderate | ⚠️ Use only if Target Tracking doesn't fit your needs |
| 🪜 **Step Scaling** | ⭐⭐⭐ Complex | ⚠️ Use only for advanced, fine-tuned control — harder to monitor and reason about |

> 🎯 **Course Recommendation:** *"Keep auto scaling simple and always use target tracking scaling — it works really, really well."* The more complex policies (Simple/Step Scaling) become **difficult to monitor and troubleshoot** as your system grows.

---

## 📋 Master Summary Table

| Use Case | Configuration Approach |
|---|---|
| 🔒 Constant instance count | Min = Max = Desired (fixed value) |
| ✋ Manual control | Adjust Desired Capacity manually within Min/Max |
| 🗓️ Scheduled scaling | Configure scheduled actions with specific desired/min/max at specific times |
| 📊 Dynamic scaling | Create a scaling policy (Target Tracking / Simple / Step) |

---

## ✅ Final Takeaways

```
🧩 4 CORE COMPONENTS → Launch Template, ASG (min/max/desired), Health Checks, Scaling Policy
🔒 MIN/MAX ALWAYS WINS → Scaling policies never override the hard min/max bounds
🔒 CONSTANT COUNT      → Set Min = Max = Desired
✋ MANUAL CONTROL        → Adjust Desired Capacity as needed
🗓️ SCHEDULED SCALING    → For predictable, known traffic patterns
📊 DYNAMIC SCALING      → For unpredictable load — Target Tracking, Simple, or Step
✅ BEST PRACTICE         → Default to Target Tracking Scaling for simplicity
```

> 🎯 **Golden Rule:** Start with **Target Tracking Scaling** for almost every use case — only reach for Simple or Step Scaling when you have a **specific, well-understood** need for finer control, since added complexity makes systems harder to reason about and debug.

> ➡️ **Next Up:** Understanding how CloudWatch alarms actually trigger these scaling policies behind the scenes!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
