![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 67: EC2 Tenancy Models — Shared vs Dedicated Instances vs Dedicated Hosts

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept Lecture (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ What is Tenancy?
2️⃣ Default: Shared Tenancy
3️⃣ Dedicated Tenancy: Two Options
4️⃣ Dedicated Instances Explained
5️⃣ Dedicated Hosts Explained
6️⃣ Key Differences: Dedicated Instances vs Dedicated Hosts
7️⃣ When to Choose Which
```

---

## 1️⃣ What is Tenancy? 🏢

> When enterprises move to the cloud, they must decide: **do we share the underlying hardware with other customers, or do we need it dedicated to us?**

> 🎯 This decision is called the **tenancy model**.

---

## 2️⃣ Default: Shared Tenancy 🤝

| Fact | Detail |
|---|---|
| 🏷️ **Default in AWS** | **Shared Tenancy** |
| 🖥️ **What It Means** | EC2 instances can run on **hosts shared with other AWS customers** |

### Example

```
Physical Host Machine
   ├── Instance 1 → Customer A
   ├── Instance 2 → Customer B
   └── Instance 3 → Customer C
```

> 💡 **Why This Is Fine for Most Use Cases:** AWS uses strong **virtualization and isolation** — customers cannot access each other's instances or data, even on shared hardware.

---

## 3️⃣ Dedicated Tenancy: Two Options 🔒

> Some organizations have **regulatory or compliance requirements** that prevent sharing infrastructure with other customers entirely. For these cases, AWS offers **two** dedicated tenancy options:

| Option | Description |
|---|---|
| 1️⃣ 🖥️ **Dedicated Instances** | Virtualized instances on hardware dedicated to **one customer** |
| 2️⃣ 🖥️ **Dedicated Hosts** | An entire **physical server** dedicated to **one customer** |

---

## 4️⃣ Dedicated Instances Explained 🖥️

| Fact | Detail |
|---|---|
| 🔒 **Hardware Dedication** | The underlying hardware is dedicated to **just you** |
| 👁️ **Visibility** | ❌ **No visibility** into the underlying host hardware |
| 🚫 **Access Restrictions** | Cannot access **sockets** or **physical cores** directly |
| 📍 **Placement Control** | ❌ AWS decides where your instance is placed (you don't choose the specific host) |

> 💡 **Simplified Mental Model:** *"I want dedicated hardware, but I don't care about the details of which specific host or how it's organized — just make sure no one else is on it."*

---

## 5️⃣ Dedicated Hosts Explained 🖥️

| Fact | Detail |
|---|---|
| 🔒 **Hardware Dedication** | The **entire physical server** is dedicated to you |
| 👁️ **Visibility** | ✅ **Full visibility** into the underlying host hardware |
| ✅ **Access** | Can access **sockets**, **physical cores**, etc. directly |
| 📍 **Placement Control** | ✅ **You choose** which specific host an instance runs on |

> 💡 **Simplified Mental Model:** *"I want dedicated hardware AND I need to control/see exactly what's happening at the hardware level."*

### 🎯 Key Use Cases for Dedicated Hosts

| Use Case | Why Dedicated Hosts? |
|---|---|
| 📜 **Regulatory Requirements** | Some compliance frameworks require full hardware isolation and visibility |
| 💿 **Server-Bound Software Licensing** | Licensing based on **physical server count**, not instance count |

### 🎓 What Is "Server-Bound Software"?

> 💡 Some software licenses are priced **per physical server**, not per instance. If you run **multiple EC2 instances on a single dedicated host**, you might only need **ONE software license** for that entire host — regardless of how many instances are running on it.

---

## 6️⃣ Key Differences: Dedicated Instances vs Dedicated Hosts 📋

| Feature | 🖥️ Dedicated Instances | 🖥️ Dedicated Hosts |
|---|---|---|
| **Billing Model** | Per **instance** | Per **host** (billed whether or not instances are running) |
| **Targeted Instance Placement** | ❌ Not possible — AWS decides placement | ✅ You choose exactly which host |
| **Access to Underlying Hardware** | ❌ No (no socket/core access) | ✅ Yes (socket/core access) |

### 💰 Billing Deep Dive

| Model | You're Billed For |
|---|---|
| **Dedicated Instances** | Only the **instances you've actually provisioned** |
| **Dedicated Hosts** | The **entire host**, regardless of how many (or how few) instances are running on it |

> ⚠️ **Cost Implication:** Dedicated Hosts can be **more expensive** if you're not fully utilizing the host's capacity, since you pay for the whole server either way.

---

## 7️⃣ When to Choose Which 🎯

### Decision Tree

```
Do you have regulatory/security needs requiring hardware isolation?
   │
   NO → Use SHARED TENANCY (default)
   │
   YES → Do you need:
          - Server-bound software licensing, OR
          - Access to underlying host hardware, OR
          - Targeted/specific instance placement?
          │
          YES → Use DEDICATED HOST
          │
          NO  → Use DEDICATED INSTANCE
                (just need hardware isolation, nothing more)
```

### 📋 Summary Decision Table

| Requirement | Recommended Tenancy |
|---|---|
| No special compliance needs | ✅ **Shared** (default) |
| Need hardware isolation only | ✅ **Dedicated Instances** |
| Need server-bound licensing | ✅ **Dedicated Hosts** |
| Need hardware-level access (sockets/cores) | ✅ **Dedicated Hosts** |
| Need to control exact host placement | ✅ **Dedicated Hosts** |

---

## 📋 Master Comparison Table

| Feature | Shared Tenancy | Dedicated Instances | Dedicated Hosts |
|---|---|---|---|
| **Default?** | ✅ Yes | ❌ No | ❌ No |
| **Hardware Shared with Others?** | ✅ Yes | ❌ No | ❌ No |
| **Billing** | Per instance | Per instance | Per host |
| **Hardware Visibility** | ❌ No | ❌ No | ✅ Yes |
| **Targeted Placement** | ❌ No | ❌ No | ✅ Yes |
| **Best For** | Most general workloads | Basic hardware isolation needs | Licensing + hardware-level control needs |

---

## ✅ Final Takeaways

```
🤝 SHARED TENANCY        → AWS default; hardware may be shared with other customers
🔒 DEDICATED TENANCY      → Two flavors: Dedicated Instances and Dedicated Hosts
🖥️ DEDICATED INSTANCES    → Hardware isolated, but NO visibility/access to host hardware; billed per instance
🖥️ DEDICATED HOSTS        → Full hardware visibility + access + placement control; billed per HOST
💿 SERVER-BOUND LICENSING → A key driver for choosing Dedicated Hosts specifically
📜 REGULATORY NEEDS       → The primary reason to move away from shared tenancy at all
```

> 🎯 **Golden Rule:** If an exam question mentions **software licensed per physical server** or the need to **see/access underlying hardware (sockets/cores)** — the answer is **Dedicated Host**. If it just says **"needs dedicated hardware for compliance"** without those specifics, **Dedicated Instance** is usually sufficient and more cost-effective.

> ➡️ **Next Up:** More architectural considerations for EC2 and ELB!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
