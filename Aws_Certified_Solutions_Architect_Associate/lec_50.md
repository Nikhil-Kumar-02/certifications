![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 50: Multiple Target Groups & Path-Based Routing for Microservices

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ The Microservices Challenge
2️⃣ One ALB, Multiple Target Groups
3️⃣ CLB vs ALB: A Critical Difference
4️⃣ Listener Rules: Routing Traffic to the Right Target Group
5️⃣ Hands-On: Setting Up Path-Based Routing for Microservice A
6️⃣ Verifying the Routing
```

---

## 1️⃣ The Microservices Challenge 🧩

> 🏢 Imagine a **microservices architecture** with **thousands of microservices** — each with its own URL path, e.g.:

```
/microservice-a
/microservice-b
/microservice-c
...
```

❓ **Question:** Do we need a **separate load balancer** for each microservice?

✅ **Answer: No!** One **Application Load Balancer** can support **multiple microservices**.

---

## 2️⃣ One ALB, Multiple Target Groups 🎯

> 💡 **The Solution:** Create a **separate Target Group** for each microservice, all behind the **same ALB**.

```
Application Load Balancer
   ├── Target Group 1 → microservice-a instances
   ├── Target Group 2 → microservice-b instances
   └── Target Group 3 → microservice-c instances
```

> 🎯 **Key Benefit:** A single ALB can intelligently route to **many different backend services**, each isolated in its own target group.

---

## 3️⃣ CLB vs ALB: A Critical Difference ⚠️

| Load Balancer | Supports Multiple Target Groups? |
|---|---|
| 🕰️ **Classic Load Balancer (CLB)** | ❌ **No** — only supports a single target |
| ⚖️ **Application Load Balancer (ALB)** | ✅ **Yes** — supports **multiple target groups** |

> 🚨 **Why This Matters:** With CLB, supporting a microservices architecture would require **one Classic Load Balancer per microservice** — extremely difficult to maintain at scale (thousands of load balancers!). ALB solves this elegantly with target groups + listener rules.

> ⭐ **Exam Tip:** This CLB vs ALB distinction (single target vs multiple target groups) is a **frequently tested** concept.

---

## 4️⃣ Listener Rules: Routing Traffic to the Right Target Group 🔀

> ❓ **Question:** If we have multiple target groups, how does the load balancer know **which request** goes to **which target group**?

✅ **Answer:** **Listener Rules**

### How Listener Rules Work

```
Listener: HTTP:80
   ├── Rule 1: IF path = /microservice-a/* → Forward to TARGET_GROUP_A
   ├── Rule 2: IF path = /microservice-b/* → Forward to TARGET_GROUP_B
   └── Default Rule: (no match) → Forward to DEFAULT_TARGET_GROUP
```

| Fact | Detail |
|---|---|
| 📋 **Multiple Rules** | You can configure any number of rules on a single listener |
| 🔢 **Execution Order** | Rules are evaluated **in the order they're configured** (1, 2, 3, ...) |
| 🎯 **Default Rule** | Executes **last**, catching anything that didn't match an earlier rule |

---

## 5️⃣ Hands-On: Setting Up Path-Based Routing for Microservice A 🧪

### Step 0: The Problem

```
Request: <ALB-DNS>/a/test.html
Result: ❌ Error — this page doesn't exist on the default target group's instances
```

### Step 1: Launch a New EC2 Instance for Microservice A

```
Launch Instance from Template → MyEc2LaunchTemplate (v2)
```

| Setting | Value |
|---|---|
| Instance Tag | `Microservice A` |

#### Custom User Data for This Instance

```bash
#!/bin/bash
mkdir /var/www/html/a
echo "Microservice A" > /var/www/html/a/test.html
```

> 💡 This creates a dedicated `/a/test.html` page — unique to this microservice instance.

### Step 2: Create a Target Group for Microservice A

```
Target Groups → Create target group
```

| Setting | Value |
|---|---|
| Name | `microservice-a-target-group` |
| Target Type | Instance |
| Protocol | HTTP |
| Port | 80 |

### Step 3: Register the Microservice A Instance

```
microservice-a-target-group → Targets → Edit
→ Select the "Microservice A" instance → Add to registered → Save
```

### Step 4: Add a Listener Rule for Path-Based Routing

```
Load Balancer → Listeners → HTTP:80 → View/Edit rules
→ Add rule → Insert Rule
```

| Condition | Action |
|---|---|
| **IF** Path is `/a/*` | **THEN** Forward to `microservice-a-target-group` |

```
Save
```

> ⏳ Changes take effect within **about a minute**.

---

## 6️⃣ Verifying the Routing ✅

### Test 1: Default Path (Unchanged Behavior)

```
<ALB-DNS>/
```
| Result | Explanation |
|---|---|
| Responses alternate between `...41.189` and `...35.39` | These are the **original two instances** in the **default target group** — the default rule still applies here |

### Test 2: Microservice A Path

```
<ALB-DNS>/a/test.html
```
| Result | Explanation |
|---|---|
| **"Microservice A"** | ✅ Successfully routed to the **new target group**, hitting the dedicated microservice instance |

> 🎉 **Success!** The same load balancer now intelligently routes **different URL paths** to **completely different sets of backend instances**.

---

## 📋 Quick Reference: Path-Based Routing Setup

| Step | Action |
|---|---|
| 1 | Launch a new EC2 instance with microservice-specific User Data |
| 2 | Create a dedicated Target Group for the microservice |
| 3 | Register the instance(s) with that Target Group |
| 4 | Add a Listener Rule: path condition → forward to that Target Group |
| 5 | Verify: default path still works; new path routes correctly |

---

## ✅ Final Takeaways

```
🧩 MICROSERVICES        → One ALB can serve MANY microservices via multiple target groups
🕰️ CLB LIMITATION ⭐     → Classic LB does NOT support multiple target groups — ALB does
🔀 LISTENER RULES       → Route based on path (and other conditions) to different target groups
🔢 RULE ORDER MATTERS   → Rules execute in configured order; default rule runs last
🎯 REAL-WORLD PATTERN   → This is exactly how large-scale microservices architectures use a single ALB efficiently
```

> 🎯 **Golden Rule:** The combination of **multiple target groups + listener rules** is what makes ALB suitable for **microservices architectures** — a capability Classic Load Balancer fundamentally lacks. This is one of the clearest, most testable differences between CLB and ALB.

> 📝 **Practice Exercise (from the course):** Try creating a second microservice (`Microservice B`) with its own target group and listener rule for `/b/*` — great hands-on reinforcement!

> ➡️ **Next Up:** More load balancer concepts and moving toward Auto Scaling Groups!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
