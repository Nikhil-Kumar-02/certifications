![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 61: Verifying the Network Load Balancer & Troubleshooting a Timeout

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ The NLB Is Active — But Times Out
2️⃣ ⭐ Key Fact: NLB Has NO Security Group of Its Own
3️⃣ Diagnosing via the Target Group
4️⃣ The Real Culprit: The EC2 Instance's Security Group
5️⃣ The Fix: Allow Traffic on Port 80
6️⃣ Interesting Behavior: Routing Before Instances Are Healthy
7️⃣ NLB Attributes: Delete Protection, Cross-Zone Load Balancing, Access Logs
8️⃣ Monitoring
```

---

## 1️⃣ The NLB Is Active — But Times Out ⏳

> After **~5 minutes**, the Network Load Balancer shows status: **Active**.

```
Copy NLB's DNS name → Execute request in browser
Result: ⏳ TIMEOUT — no response
```

> 🎯 **Reflex Check:** What's the first thing to suspect with a timeout? → **Security Group configuration!**

---

## 2️⃣ ⭐ Key Fact: NLB Has NO Security Group of Its Own 🌟

> 🚨 **Surprising and Important Fact:** Unlike ALB and CLB, the **Network Load Balancer does NOT have its own security group**.

| Load Balancer | Has Its Own Security Group? |
|---|---|
| 🕰️ Classic Load Balancer | ✅ Yes |
| ⚖️ Application Load Balancer | ✅ Yes |
| 🌐 **Network Load Balancer** | ❌ **No** |

> ⭐ **Exam Tip:** NLB is one of the **very few AWS resources** with **no security group configuration at all**. This is a frequently tested, easy-to-miss fact!

> 💡 **Why This Matters:** Since NLB has no security group to configure, **any traffic filtering must happen at the target (EC2 instance) level** instead.

---

## 3️⃣ Diagnosing via the Target Group 🔍

```
Target Groups → network-load-balancer-target-group → Targets tab
```

> 🔴 **Observation:** All registered targets are showing as **Unhealthy**.

> ❓ The port configuration is correct — so why unhealthy?

---

## 4️⃣ The Real Culprit: The EC2 Instance's Security Group 🕵️

```
EC2 → Instances → Select instance → Security Group → View inbound rules
```

### The Problem Found

> 🚨 The EC2 instances' security group **only allows traffic from the Application Load Balancer's security group** (configured back in Lecture 47's security best practice) — **not** from the Network Load Balancer!

> 💡 **Why This Happens:** Since NLB has **no security group of its own**, its health check and routing traffic appears to originate from **its own IP addresses** — not from a recognizable "load balancer security group." The existing rule (allowing only the ALB's SG) doesn't cover this at all.

---

## 5️⃣ The Fix: Allow Traffic on Port 80 🔧

```
EC2 Security Group → Edit inbound rules → Add rule
→ Type: HTTP (TCP, port 80)
→ Source: Anywhere (0.0.0.0/0)
→ Save rules
```

> 📌 **Important:** The existing rule (allowing only the ALB's security group) was **kept, not deleted** — we simply **added** a new rule alongside it to also allow traffic from anywhere (needed for the NLB to reach the instances).

> ⏳ **Wait Time:** It takes **a couple of minutes** for the instances to be re-checked and marked as **healthy**.

---

## 6️⃣ Interesting Behavior: Routing Before Instances Are Healthy 🤔

> 🔍 **Curious Observation:** Even while **all targets were still marked unhealthy**, refreshing the NLB's URL **still returned a response**!

### Why Does This Happen?

| Scenario | NLB Behavior |
|---|---|
| ✅ Some targets healthy | Routes only to **healthy** targets |
| ❌ **ALL** targets unhealthy (no AZ has a healthy target) | 🔄 **Falls back to routing to ALL targets anyway** (essentially ignoring the failed health check as a last resort) |

> 💡 **Key Insight:** This is a **fail-safe behavior** — if the load balancer refused to route to **any** target when all are technically "unhealthy," the application would become **completely unavailable**. Instead, NLB reasons: *"Something's better than nothing — let's still try."*

> ✅ Once the security group fix takes effect and instances start passing health checks, this **fallback behavior naturally resolves** — you'll see all targets marked **Healthy** and normal routing resumes.

---

## 7️⃣ NLB Attributes: Delete Protection, Cross-Zone Load Balancing, Access Logs ⚙️

```
Load Balancer → Actions → Edit attributes
```

### 🔒 Delete Protection

| Setting | Detail |
|---|---|
| Default | Configurable (often off) |
| Effect When Enabled | The NLB **cannot be deleted** until this protection is **manually disabled** first |

> 💡 **Use Case:** Prevents **accidental deletion** of a critical production load balancer.

### 🗺️ Cross-Zone Load Balancing

| Load Balancer | Cross-Zone Load Balancing Default |
|---|---|
| ⚖️ Application Load Balancer | ✅ **Always ON** (cannot be disabled) |
| 🌐 **Network Load Balancer** | ❌ **Disabled by default** (must be manually enabled) |

> ⭐ **Exam Tip:** This is a **key difference** between ALB and NLB — ALB forces cross-zone balancing always on, while NLB requires you to **opt in**.

### 📜 Access Logs

| Feature | Detail |
|---|---|
| 📜 **Access Logs** | Can be enabled to record every request |
| 🗄️ **Storage Destination** | **Amazon S3** (AWS's object storage service) |

> 💡 Both **ALB** and **NLB** support sending access logs to S3 for later analysis.

---

## 8️⃣ Monitoring 📊

```
Load Balancer → Monitoring tab
```

> 📡 Like all AWS load balancers, NLB metrics are tracked via **CloudWatch** — request counts, active connections, healthy/unhealthy host counts, and more.

---

## 📋 Quick Reference: Key NLB Facts from This Lecture

| Fact | Detail |
|---|---|
| ⭐ No Security Group | NLB itself has **no security group** — filtering happens at the target level |
| 🔄 Fail-Safe Routing | If **all** targets are unhealthy, NLB routes to them anyway rather than failing completely |
| 🗺️ Cross-Zone LB | **Disabled by default** (unlike ALB, which is always on) |
| 🔒 Delete Protection | Must be disabled before the NLB can be deleted |
| 📜 Access Logs | Can be sent to **S3** for analysis |

---

## ✅ Final Takeaways

```
⭐ NO SECURITY GROUP  → NLB is one of the few AWS resources without its own SG
🔧 FIX AT TARGET LEVEL → Traffic filtering must happen on the EC2 instance's security group instead
🔄 FAIL-SAFE ROUTING   → All-unhealthy targets still receive traffic as a last resort
🗺️ CROSS-ZONE OFF BY DEFAULT → Must be manually enabled (opposite of ALB)
🔒 DELETE PROTECTION   → Optional safeguard against accidental deletion
📜 ACCESS LOGS → S3   → Both ALB and NLB support this
```

> 🎯 **Golden Rule:** When troubleshooting NLB connectivity issues, remember there's **no NLB security group to check** — go straight to the **target's (EC2 instance's) security group** instead. This distinction is one of the most commonly tested NLB "gotchas" in the certification exam.

> ➡️ **Next Up:** Cleaning up the Network Load Balancer resources!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
