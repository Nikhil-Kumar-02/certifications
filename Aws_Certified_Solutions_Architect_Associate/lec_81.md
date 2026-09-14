![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 81: Savings Plans Deep Dive — Compute vs EC2 Instance

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Recap: What Are Savings Plans?
2️⃣ Compute Savings Plans — Deep Dive
3️⃣ EC2 Instance Savings Plans — Deep Dive
4️⃣ Compute vs EC2 Instance: Side-by-Side
5️⃣ Why This Matters: Flexibility vs Discount Trade-off
```

---

## 1️⃣ Recap: What Are Savings Plans? 💡

> 🔗 **Recall from the pricing overview lecture:** Savings Plans are a **newer, more flexible** alternative to Reserved Instances — you commit to a **dollar amount** of spend rather than specific instance reservations.

> 🎯 There are **TWO variations** of Savings Plans, covered in depth here.

---

## 2️⃣ Compute Savings Plans — Deep Dive 💻

### The Commitment You Make

```
"I will spend $X on AWS compute resources over [1 or 3] years."
```

**Example:** *"I will spend $10,000 on AWS compute resources over 1 year."*

### 🎯 What Counts as "Compute Resources"?

| Service | Included? |
|---|---|
| 🖥️ **EC2 Instances** | ✅ Yes |
| 📦 **AWS Fargate** | ✅ Yes |
| ⚡ **AWS Lambda** | ✅ Yes |

> 💡 **Key Freedom:** You **don't** need to decide upfront whether you'll use EC2, Fargate, or Lambda — you're simply committing to a **spend level**, and AWS applies the discount across **whichever of these three services** you actually use.

### 💰 Discount & Flexibility

| Aspect | Detail |
|---|---|
| 💰 **Max Discount** | Up to **66% off** On-Demand pricing |
| 🔓 **Flexibility** | **Complete** — can change instance family, size, OS, tenancy, **and even AWS region** |
| 🔀 **Cross-Service Switching** | ✅ Can freely move between EC2, Fargate, and Lambda |

### 🎯 Real-World Example

```
Scenario: You're currently running an app on EC2 instances.
Later: You re-architect the app to be containerized, moving it to AWS Fargate.
Result: Your Compute Savings Plan STILL applies — no need to buy a new plan!
```

> 💡 **This is the standout feature of Compute Savings Plans** — your **architecture can evolve** (EC2 → containers → serverless) and your discount commitment **follows you**, rather than being tied to a specific resource type.

---

## 3️⃣ EC2 Instance Savings Plans — Deep Dive 🖥️

### The Commitment You Make

```
"I will spend $X on EC2 instances of a SPECIFIC instance family, in a SPECIFIC region, over [1 or 3] years."
```

**Example:** *"I will spend $10,000 on `m5` family EC2 instances in `us-east-1` over 1 year."*

### 🎯 Restrictions

| Restriction | Detail |
|---|---|
| 🏷️ **Instance Family** | Locked to a **specific family** (e.g., `m5`, `c5`) |
| 🌍 **Region** | Locked to a **specific region** |
| 🚫 **Service Type** | Only **EC2** — no Fargate/Lambda flexibility |

### ✅ What You CAN Still Change

| Flexibility | Detail |
|---|---|
| 🔄 **Operating System** | ✅ Can switch (e.g., Windows → Linux) |
| 🔄 **Instance Size** | ✅ Can switch within the committed family |
| 🔄 **Tenancy** | ✅ Can switch |

### 💰 Discount & Flexibility

| Aspect | Detail |
|---|---|
| 💰 **Max Discount** | **Higher** than Compute Savings Plans (more restrictive = bigger discount) |
| 🔓 **Flexibility** | **Lower** — locked to a specific instance family + region |

> 🎯 **Why the Higher Discount?** Because you're accepting **more restrictions** (specific family, specific region, EC2-only) — AWS rewards that reduced flexibility with a **better discount rate**, following the same pattern we saw with Standard vs Convertible Reserved Instances.

---

## 4️⃣ Compute vs EC2 Instance: Side-by-Side 📋

| Feature | 💻 Compute Savings Plan | 🖥️ EC2 Instance Savings Plan |
|---|---|---|
| **Commitment Basis** | Dollar amount, any compute service | Dollar amount, EC2 only, specific family + region |
| **Services Covered** | EC2, Fargate, **and** Lambda | EC2 only |
| **Instance Family Flexibility** | ✅ Full flexibility | ❌ Locked to one family |
| **Region Flexibility** | ✅ Full flexibility | ❌ Locked to one region |
| **OS Flexibility** | ✅ Yes | ✅ Yes |
| **Tenancy Flexibility** | ✅ Yes | ✅ Yes |
| **Max Discount** | Up to 66% | **Higher** than Compute Savings Plans |
| **Best For** | Evolving architectures (EC2 → Fargate → Lambda) | Stable, EC2-committed, single-family workloads |

---

## 5️⃣ Why This Matters: Flexibility vs Discount Trade-off ⚖️

> 🎯 **This is the same fundamental pattern seen throughout EC2 pricing models:**

```
MORE FLEXIBILITY → LOWER discount
LESS FLEXIBILITY (more restrictions) → HIGHER discount
```

| Pricing Model | Flexibility | Discount |
|---|---|---|
| On-Demand | Highest | 0% (baseline) |
| Compute Savings Plan | High (cross-service, cross-region) | Up to 66% |
| EC2 Instance Savings Plan | Medium (locked family+region) | Higher than Compute |
| Standard Reserved Instance | Low (locked type, but flexible AZ) | Up to 75% |

> 💡 **AWS's Consistent Philosophy:** The more **certainty** you give AWS about your future usage (by restricting your own flexibility), the **bigger discount** they're willing to offer in return.

---

## 📋 Master Summary Table

| Concept | Key Fact |
|---|---|
| Savings Plans Type 1 | **Compute Savings Plan** — flexible across EC2/Fargate/Lambda, any family/region |
| Savings Plans Type 2 | **EC2 Instance Savings Plan** — EC2-only, locked family + region, higher discount |
| Common Term Length | 1 or 3 years (same as Reserved Instances) |
| Newness | Savings Plans are **relatively new** — expect continued evolution |

---

## ✅ Final Takeaways

```
💻 COMPUTE SAVINGS PLAN     → $ commitment across EC2, Fargate, AND Lambda; up to 66% off; max flexibility
🖥️ EC2 INSTANCE SAVINGS PLAN → $ commitment to EC2 only, specific family+region; HIGHER discount than Compute
🔀 ARCHITECTURE EVOLUTION   → Compute Savings Plans follow you even if you migrate EC2 → Fargate → Lambda
⚖️ UNIVERSAL TRADE-OFF       → Less flexibility = more discount (consistent theme across ALL EC2 pricing models)
🆕 RECENT FEATURE            → Savings Plans are newer than Reserved Instances; expect ongoing evolution
```

> 🎯 **Golden Rule:** If an exam scenario describes an organization that's **actively modernizing/re-architecting** (e.g., moving from EC2 to containers or serverless) but wants to **preserve their savings commitment**, the answer is **Compute Savings Plan** — its cross-service flexibility is precisely designed for this situation.

> ➡️ **Next Up:** Wrapping up the EC2 pricing models section with a full comparison and review!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
