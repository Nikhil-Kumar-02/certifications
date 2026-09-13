![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 78: Spot Instance Pricing Mechanics & Proper Cleanup

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ ⭐ The First-Hour Free Rule
2️⃣ Billing Examples: Working Through the Math
3️⃣ Who Terminated It Matters
4️⃣ Recap: Terminate vs Stop vs Hibernate on Interruption
5️⃣ Best Practice: Properly Closing a Spot Request
```

---

## 1️⃣ ⭐ The First-Hour Free Rule 🎁

> 🚨 **Critical, Highly Testable Fact:**

| Condition | Charge |
|---|---|
| Spot instance terminated by **AWS/EC2** **within the FIRST instance hour** | 💰 **$0 — completely FREE** |
| Spot instance runs **beyond one hour** | 💰 Billed **by the second**, for actual usage |

> 🎯 **Why This Exists:** This policy protects customers from being charged for **partial, incomplete work** when AWS reclaims a spot instance very early — before you've gotten meaningful value from it.

---

## 2️⃣ Billing Examples: Working Through the Math 🧮

### Example 1: Terminated by AWS at 50 Minutes

```
Spot instance runs for 50 minutes
AWS terminates it (reclaims capacity)
→ Charge: $0.00 (within the first hour, terminated by AWS)
```

### Example 2: YOU Terminate at 50 Minutes

```
Spot instance runs for 50 minutes
YOU choose to terminate it (not AWS)
→ Charge: You pay for 50 minutes of usage
```

> ⚠️ **Key Distinction:** The "free first hour" rule **only** applies when **AWS/EC2** terminates the instance — **not** when you voluntarily terminate it yourself!

### Example 3: Terminated (by either party) at 70 Minutes

```
Spot instance runs for 70 minutes
Hourly price: $0.06/hour (6 cents)
→ Charge: $0.06 (for the first 60 min) + proportional charge for the extra 10 min
→ Total: ~$0.07 (7 cents) for 70 minutes
```

> 💡 **Once you're past the 1-hour mark**, billing is simply **per-second**, regardless of who initiated the termination.

### 📋 Quick Reference Table

| Scenario | Duration | Who Terminated? | Charge |
|---|---|---|---|
| 1 | 50 min | AWS/EC2 | 💰 **$0.00** |
| 2 | 50 min | You | 💰 Pay for 50 min |
| 3 | 70 min | Either | 💰 Pay for 70 min (per-second billing) |

---

## 3️⃣ Who Terminated It Matters 🔑

> ⭐ **Exam-Critical Distinction:**

```
Terminated by AWS + within first hour → FREE
Terminated by YOU + within first hour → You still pay for that time
Beyond first hour (either party) → Pay per-second, as normal
```

---

## 4️⃣ Recap: Terminate vs Stop vs Hibernate on Interruption 🔁

> Applicable specifically to **Persistent Spot Requests**:

| Option | Default? |
|---|---|
| ❌ **Terminate** | ✅ Default behavior |
| ⏸️ **Stop** | Configurable alternative |
| 🧊 **Hibernate** | Configurable alternative (not all instance types support this) |

> 🎯 **Recall:** Hibernating **saves the EC2 instance's state**, allowing for a **quick restart** when a spot instance becomes available to you again — much faster than starting from scratch.

---

## 5️⃣ Best Practice: Properly Closing a Spot Request 🧹

> ⚠️ **Critical Gotcha:** Canceling a spot **request** does **NOT** automatically terminate the **active spot instances** that request already created!

### ✅ Correct Cleanup Order

```
Step 1: CANCEL the spot request
Step 2: TERMINATE all the spot instances created from that request
```

> 🚨 **Why Order Matters:** If you skip Step 2, you could have **orphaned spot instances** still running (and still costing you money) even after the request itself is canceled.

### 📋 Cleanup Checklist

| Step | Action | Why |
|---|---|---|
| 1️⃣ | Cancel the spot request | Stops any **future** instance allocation attempts |
| 2️⃣ | Terminate all associated spot instances | Stops **billing** for any **currently running** instances |

---

## 📋 Master Summary Table

| Concept | Key Fact |
|---|---|
| First Hour Free | Only applies if **AWS** terminates within the first hour |
| Self-Termination | You pay even if within the first hour, if **YOU** terminate |
| Beyond 1 Hour | Billed **per-second**, regardless of who terminates |
| Interruption Default | **Terminate** (change to Stop/Hibernate for faster recovery) |
| Cleanup Order | **Cancel request FIRST, then terminate instances** |

---

## ✅ Final Takeaways

```
🎁 FIRST HOUR FREE ⭐  → Only if AWS/EC2 terminates the instance within hour 1
💰 SELF-TERMINATION    → You pay regardless of timing, even under 1 hour
⏱️ PER-SECOND BILLING  → Applies once past the 1-hour mark
🧊 STOP/HIBERNATE       → Preferred over Terminate for fast recovery on interruption
🧹 CLEANUP ORDER        → Cancel the spot REQUEST first, THEN terminate the INSTANCES
⚠️ ORPHAN RISK           → Canceling a request alone does NOT stop running instances from billing you
```

> 🎯 **Golden Rule:** Remember the **"first hour free, but only if AWS pulls the plug"** rule as one of the most distinctive and testable Spot Instance billing facts — and always **cancel the request before terminating instances** to avoid orphaned, still-billing resources.

> ➡️ **Next Up:** Deep dive into Reserved Instances and Savings Plans!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
