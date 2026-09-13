![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 75: On-Demand Instances — Deep Dive

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept Lecture

---

## 🎯 What This Lecture Is About

```
1️⃣ What is an On-Demand Instance?
2️⃣ Cost vs Flexibility Trade-off
3️⃣ Use Case 1: Spiky/Unpredictable Traffic
4️⃣ Use Case 2: Batch Programs with Unpredictable Runtime
5️⃣ Summary
```

---

## 1️⃣ What is an On-Demand Instance? 💡

| Concept | Explanation |
|---|---|
| 🖥️ **On-Demand** | **On-demand resource provisioning** — request an instance when you need it, terminate it when you don't |

```
Need an instance?  → Provision it NOW
Don't need it anymore? → Terminate it
```

> 📌 **This is exactly what we've been doing throughout this entire course** — every EC2 instance we've launched has been an On-Demand instance.

---

## 2️⃣ Cost vs Flexibility Trade-off ⚖️

| Attribute | Rating |
|---|---|
| 💰 **Cost** | 🔴 **Highest** of all four pricing models |
| 🔓 **Flexibility** | 🟢 **Highest** of all four pricing models |

> 🎯 **The Trade-off:** You pay a **premium** for the ability to get exactly what you need, exactly when you need it, with **zero commitment**.

---

## 3️⃣ Use Case 1: Spiky/Unpredictable Traffic 📈

> ❓ **Scenario:** *"I don't know how much load I'll get."*

| Situation | Why On-Demand Fits |
|---|---|
| 📈 **Spiky Traffic Patterns** | You can't predict traffic in advance, so committing to Reserved Instances doesn't make sense |
| 🎯 **Strategy** | Use On-Demand to **handle the unpredictable "spike" portion** of your traffic |

> 💡 **Real-World Pattern:** Many architectures use a **baseline** of Reserved Instances for predictable steady-state load, plus **On-Demand instances** to absorb unpredictable spikes on top of that baseline. *(This combined strategy will make more sense once we cover Reserved Instances in the next lecture.)*

---

## 4️⃣ Use Case 2: Batch Programs with Unpredictable Runtime ⏱️

> ❓ **Scenario:** *"I don't know how long this batch program will take to run, and I can't afford to interrupt it."*

### Example: Migrating a Batch Job to the Cloud for the First Time

```
Situation: Moving an on-premises batch program to AWS for the FIRST time
Problem: You have NO historical data on how long it takes to run
Solution: Use On-Demand — no risk of interruption, no need to commit to a duration
```

> 🎯 **Why NOT Spot Instances Here?** Spot instances can be **reclaimed** by AWS at any time — if your batch job **cannot tolerate interruption**, Spot is the wrong choice. On-Demand guarantees the instance stays running until **you** decide to terminate it.

---

## 5️⃣ Summary 📝

> ✅ **On-Demand is the simplest EC2 pricing option AWS provides:**

```
Request it → Use it → Terminate it
```

| Characteristic | Detail |
|---|---|
| 🔓 Flexibility | Highest — no commitment required |
| 💰 Cost | Highest — you pay for that flexibility |
| 🎯 Best For | Unpredictable traffic spikes, unpredictable/uninterruptible batch runtimes |

---

## 📋 Quick Reference

| Scenario | Use On-Demand? |
|---|---|
| Traffic pattern is spiky/unknown | ✅ Yes |
| Batch job with unknown runtime that **cannot** be interrupted | ✅ Yes |
| First-time workload migration with no historical data | ✅ Yes |
| Predictable, long-term, steady-state workload | ❌ Consider Reserved Instances instead |
| Non-critical, interruption-tolerant workload | ❌ Consider Spot Instances instead (cheaper) |

---

## ✅ Final Takeaways

```
🖥️ ON-DEMAND       → Pay for what you use, whenever you need it, zero commitment
💰 HIGHEST COST     → The price of maximum flexibility
🔓 HIGHEST FLEXIBILITY → No long-term commitment required
📈 USE CASE 1        → Spiky, unpredictable traffic
⏱️ USE CASE 2        → Batch jobs with unpredictable, uninterruptible runtime
```

> 🎯 **Golden Rule:** Choose On-Demand when you **cannot predict** your usage pattern AND **cannot tolerate interruption** — it's the "safe default" when neither Reserved (needs predictability) nor Spot (tolerates interruption) fits your situation.

> ➡️ **Next Up:** Deep dive into Spot Instances!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
