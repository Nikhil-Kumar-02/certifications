![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 25: EC2 Security Groups Deep Dive

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Heavily tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Recap: What is a Security Group?
2️⃣ Defense in Depth
3️⃣ Default Deny & Allow-Only Rules
4️⃣ Inbound vs Outbound Rules
5️⃣ Multiple Security Groups per Resource
6️⃣ Stateful Behavior
7️⃣ Hands-On: Breaking & Fixing Traffic
8️⃣ Exam Trivia
```

---

## 1️⃣ Recap: What is a Security Group? 🛡️

> A **Security Group** is a **virtual firewall** that controls **inbound** and **outbound** traffic for AWS resources (EC2 instances, databases, etc.).

---

## 2️⃣ Defense in Depth 🏰

| Concept | Explanation |
|---|---|
| 🧱 **Defense in Depth** | Best practice: use **multiple layers** of security, not just one |
| 🛡️ **Security Group's Role** | The **last layer** of defense protecting your AWS resources |

---

## 3️⃣ Default Deny & Allow-Only Rules 🚫

| Rule | Behavior |
|---|---|
| 🚫 **Default** | If **no rules** are configured, **all traffic is denied** — both in and out |
| ✅ **Allow-Only** | You can **only configure ALLOW rules** — there's no "deny" rule to add |
| 📌 **Implicit Denial** | Anything **not explicitly allowed** is **automatically denied** |

---

## 4️⃣ Inbound vs Outbound Rules ↔️

| Rule Type | Controls |
|---|---|
| ⬇️ **Inbound** | Traffic **coming into** the resource (e.g., from a load balancer) |
| ⬆️ **Outbound** | Traffic **going out** of the resource (e.g., to a database) |

> 💡 **Example:** In a multi-tier architecture, an EC2 instance's **inbound** rule might allow traffic from a load balancer, while its **outbound** rule allows connections to a database.

---

## 5️⃣ Multiple Security Groups per Resource 🔢

| Fact | Detail |
|---|---|
| 📊 **Max Security Groups** | Up to **5** per resource |
| 🔄 **Changes** | Can add/remove security groups **at any time** |
| ⚡ **Effect** | Changes are **immediately effective** — no restart required |

---

## 6️⃣ Stateful Behavior 🔁

> 🔑 **Security Groups are STATEFUL.**

| Scenario | Behavior |
|---|---|
| ✅ Outbound request allowed | The **response** is automatically allowed back in |
| ✅ Inbound request allowed | The **response** is automatically allowed back out |

> 💡 **Example:** When we allowed inbound HTTP (port 80) earlier, we didn't need a separate outbound rule for the response — it was automatically permitted.

---

## 7️⃣ Hands-On: Breaking & Fixing Traffic 🧪

### 🌐 Test 1: Outbound Traffic Works by Default

```bash
sudo su
curl google.com
```
> ✅ Returns an HTML response — outbound traffic (All traffic, default) is allowed.

---

### 🚫 Test 2: Remove Outbound Rule → Traffic Breaks

```
EC2 → Security Group → Edit outbound rules → Delete rule → Save
```

```bash
curl google.com
```
> ⏳ **Request times out.** No outbound rule = no traffic allowed out.

> 🔑 **Key Symptom:** When a security group blocks traffic, you typically get a **timeout**, not an explicit error.

---

### ✅ Test 3: Restore Outbound Rule

```
Edit outbound rules → Add "All traffic" → Destination: Anywhere → Save
```

```bash
curl google.com
```
> ✅ Works again after refreshing the connection.

---

### 🚫 Test 4: Remove Inbound SSH Rule → Instance Connect Breaks

```
Edit inbound rules → Delete SSH rule → Save
```

> ⏳ Refreshing **EC2 Instance Connect** now **times out** — SSH (port 22) is no longer allowed in.

---

### ✅ Test 5: Restore Inbound SSH Rule

```
Edit inbound rules → Add rule → SSH → Source: Anywhere → Save
```

> ✅ After a short wait, Instance Connect works again.

---

## 8️⃣ Exam Trivia ⭐

| Question | Answer |
|---|---|
| ❓ What if no inbound/outbound rules are configured? | 🚫 **No traffic** is allowed in or out — security groups are **default deny** |
| ❓ Can I change security groups at runtime? | ✅ **Yes** — changes take effect **immediately** |
| ❓ Does denied traffic show up in logs? | ❌ **No** — traffic blocked by a security group **never reaches** the instance, so it won't appear in instance-level logs |

---

## 📋 Security Groups Quick Reference

| Property | Value |
|---|---|
| Default Behavior | Deny all |
| Configurable Rules | Allow only |
| Rule Direction | Inbound & Outbound (separate) |
| Max per Resource | 5 |
| Change Effect | Immediate |
| Statefulness | Stateful (responses auto-allowed) |

---

## ✅ Final Takeaways

```
🛡️ SECURITY GROUP → Virtual firewall, last layer of Defense in Depth
🚫 DEFAULT DENY   → No rules = no traffic in/out
✅ ALLOW ONLY     → Can't configure explicit "deny" rules
🔁 STATEFUL       → Allowed request ⇒ response auto-allowed
🔢 UP TO 5        → Security groups per resource
⚡ IMMEDIATE       → Changes apply instantly, no restart needed
⏳ TIMEOUT         → Classic symptom of a security group blocking traffic
```

> 🎯 **Golden Rule:** If a connection **times out** (rather than being explicitly refused), suspect the **security group** first.

> ➡️ **Next Up:** Public vs Private IP addresses on EC2 instances!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
