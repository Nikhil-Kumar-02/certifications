![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 60: Hands-On — Creating a Network Load Balancer

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ ⚠️ Important Cost Warning
2️⃣ Starting the NLB Creation Wizard
3️⃣ Choosing the Protocol & Listener
4️⃣ Availability Zones & Subnets
5️⃣ Security Settings (Skipped)
6️⃣ Creating the Target Group
7️⃣ Registering Targets & Final Review
```

---

## 1️⃣ ⚠️ Important Cost Warning 💰

> 🚨 **Critical Note:** Unlike some other services we've used, **creating a Network Load Balancer does NOT fall under the AWS Free Tier**.

> 💡 **Recommendation:** If you don't want to incur charges, you can **follow along conceptually** without creating your own NLB — or make sure to **delete it promptly** after experimenting (as we'll do in an upcoming cleanup step).

---

## 2️⃣ Starting the NLB Creation Wizard 🚀

```
EC2 Console → Load Balancers → Create Load Balancer → Network Load Balancer
```

### Basic Configuration

| Setting | Value |
|---|---|
| Name | `my-network-load-balancer` |
| Scheme | **Internet-facing** (public) |

> 💡 As with other load balancer types, you can also choose to create an **internal** (private) NLB instead.

---

## 3️⃣ Choosing the Protocol & Listener 📡

### Supported Protocols

| Protocol | Description |
|---|---|
| 🔵 **TCP** | Chosen for this demo |
| 🔒 **TLS** | Secure version of TCP |
| 🟡 **UDP** | High-performance, best-effort delivery |

### Listener Configuration

| Setting | Value |
|---|---|
| Protocol | **TCP** |
| Port | **80** |

> ⚠️ **Security Warning in Console:** AWS recommends using **TLS** (the secure protocol) instead of plain TCP for production use cases. For this demo, we **proceed with TCP** anyway (unsecured) for simplicity.

---

## 4️⃣ Availability Zones & Subnets 🗺️

| Setting | Value |
|---|---|
| VPC | Default VPC |
| Availability Zones | **All available AZs selected** |

> 📌 Tags were **skipped** for this demo but are available if needed.

---

## 5️⃣ Security Settings (Skipped) 🔓

```
"Configure Security Settings" step → Warning shown → Proceeded anyway
```

> 📌 The warning here is the same one mentioned above — recommending **TLS over TCP** for secure communication. Since this is a learning demo, we skip past it.

---

## 6️⃣ Creating the Target Group 🎯

```
Configure Routing → Create a new target group
```

| Setting | Value |
|---|---|
| Name | `network-load-balancer-target-group` |
| Target Type | **Instance** |
| Protocol | **TCP** |
| Port | 80 |
| Health Check | Default settings |

### ⭐ Important Limitation: No Lambda Support

> 🚨 **Key Difference from ALB:** When creating an NLB target group, you'll notice **there is no "Lambda function" option** for the target type.

| Target Type | Supported by ALB? | Supported by NLB? |
|---|---|---|
| EC2 Instances | ✅ Yes | ✅ Yes |
| IP Addresses | ✅ Yes | ✅ Yes |
| **Lambda Functions** | ✅ **Yes** | ❌ **No** |

> ⭐ **Exam Tip:** If a question asks about **load balancing to Lambda functions**, the answer is always **Application Load Balancer** — **Network Load Balancer does not support this**.

---

## 7️⃣ Registering Targets & Final Review ✅

```
Register Targets → Select all running EC2 instances → Add to registered
```

### 📋 Final Configuration Summary

| Setting | Value |
|---|---|
| Load Balancer Name | `my-network-load-balancer` |
| Protocol / Port | TCP / 80 |
| Subnets | All available AZs |
| Target Group | `network-load-balancer-target-group` |
| Health Check | Default |
| Registered Targets | All running EC2 instances |

```
Click "Create"
```

> ⏳ **Status:** The NLB enters a **"Provisioning"** state — it will take **a little while** to become fully active.

---

## 📋 Quick Reference: NLB Setup Flow

| Step | Action |
|---|---|
| 1 | ⚠️ Note: NLB is **not free-tier eligible** |
| 2 | Choose "Network Load Balancer" |
| 3 | Configure name, scheme, listener (TCP, port 80) |
| 4 | Select VPC and all Availability Zones |
| 5 | (Skip security warning about TLS vs TCP) |
| 6 | Create a Target Group — note: **no Lambda option** |
| 7 | Register EC2 instances as targets |
| 8 | Review and create (provisioning takes time) |

---

## ✅ Final Takeaways

```
💰 NOT FREE TIER    → NLB creation incurs cost — be mindful and clean up afterward
📡 PROTOCOL CHOICE   → TCP chosen for this demo (TLS recommended for production security)
🎯 TARGET GROUP      → Similar setup to ALB, but NO Lambda function target option
⭐ EXAM DIFFERENTIATOR → "Load balance to Lambda" → ALB; NLB cannot do this
⏳ PROVISIONING TIME → NLB takes a while to become fully active after creation
```

> 🎯 **Golden Rule:** When choosing between ALB and NLB for an architecture involving **Lambda functions**, remember: **only ALB supports Lambda as a target** — NLB's target types are limited to EC2 instances and IP addresses.

> ➡️ **Next Up:** Verifying the Network Load Balancer and testing its performance characteristics!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
