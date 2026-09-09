![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 26: Public & Private IP Addresses on EC2

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Public vs Private IP Addresses
2️⃣ Key Rules About IP Assignment
3️⃣ Hands-On: Testing with Ping (ICMP)
4️⃣ Stop/Start Behavior: Public IP Changes
5️⃣ Why a Changing Public IP Is a Problem
```

---

## 1️⃣ Public vs Private IP Addresses 🌍🔐

| Type | Scope | Description |
|---|---|---|
| 🌍 **Public IP** | Internet-addressable | Can be accessed **from anywhere on the internet** |
| 🔐 **Private IP** | Internal only | Only meaningful **within the corporate/internal network** |

### 📋 Comparison Table

| Aspect | Public IP | Private IP |
|---|---|---|
| Accessible from Internet? | ✅ Yes | ❌ No |
| Uniqueness | Must be **globally unique** | Can be **duplicated** across different private networks |
| Assigned to every instance? | ❌ Optional | ✅ Always assigned |

---

## 2️⃣ Key Rules About IP Assignment 📌

| Rule | Detail |
|---|---|
| 🔐 **Private IP** | Every EC2 instance **always** gets one |
| 🌍 **Public IP** | **Optional** — configurable in "Additional Settings" during instance launch |
| 🔄 **Stop/Start** | Instance **loses its public IP** when stopped; gets a **new one** when started |
| 🔒 **Private IP Stability** | Private IP **remains the same** even after stop/start |

---

## 3️⃣ Hands-On: Testing with Ping (ICMP) 🏓

### ❌ First Attempt: Ping the Public IP

```bash
ping 15.206.167.131
```
> ⏳ **Times out** — even though `ping google.com` works fine!

### 🕵️ Why It Fails

| Cause | Explanation |
|---|---|
| 🛡️ Security Group | Only **HTTP (port 80)** is allowed — **ICMP (ping)** is not |

### ✅ Fix: Allow ICMP in the Security Group

```
Edit inbound rules → Add rule:
   Type: All ICMP - IPv4
   Source: Anywhere
→ Save rules
```

```bash
ping 15.206.167.131
```
> ✅ Now returns responses successfully!

---

### 🔐 Testing the Private IP

| Test | Result |
|---|---|
| Ping private IP from **local machine** | ❌ Times out — private IP isn't reachable from outside the network |
| Ping private IP from **inside the EC2 instance** (via Instance Connect) | ✅ Works — same internal network |

> 💡 **Ideal Test:** Ping one EC2 instance's private IP **from another EC2 instance** — this would also succeed, since they're in the same internal network.

---

## 4️⃣ Stop/Start Behavior: Public IP Changes 🔄

### Demo Steps

| Step | Public IP | Private IP |
|---|---|---|
| Before Stop | `167.131` (example) | `37.0` |
| **Actions → Instance State → Stop** | Public IP **disappears** | Private IP **remains** |
| **Actions → Instance State → Start** | **New** Public IP assigned (e.g., `35.157.121.156`) | Private IP **unchanged** |

> ⚠️ Using the **old public IP** after restart → **no longer works**. You must use the **new** IP.

---

## 5️⃣ Why a Changing Public IP Is a Problem ⚠️

> 🧑‍💻 Imagine sharing your EC2 instance's public IP/URL with a friend or user.

| Scenario | Consequence |
|---|---|
| Instance stopped & started (even accidentally) | 🔴 Public IP **changes** |
| Shared URL uses old public IP | 🔴 **Stops working** for all users |
| Need to redistribute the new IP | 🔴 Poor user experience, not scalable |

> ❓ **Is this acceptable for production use?** ❌ **No!**

---

## 📋 Quick Reference Table

| Action | Public IP Behavior | Private IP Behavior |
|---|---|---|
| Reboot | ✅ Stays the same | ✅ Stays the same |
| Stop → Start | 🔄 **Changes** | ✅ Stays the same |
| Terminate | ❌ Released permanently | ❌ Released permanently |

---

## ✅ Final Takeaways

```
🌍 PUBLIC IP    → Internet-facing, optional, changes on stop/start
🔐 PRIVATE IP   → Internal only, always assigned, stays constant
🏓 ICMP/PING    → Needs its own security group rule to be allowed
🔄 STOP/START   → Public IP changes; Private IP does not
⚠️ PROBLEM      → Changing public IP breaks shared URLs for end users
```

> 🎯 **Golden Rule:** If you need a **constant public IP**, don't rely on the default assignment — you'll need a solution like an **Elastic IP**.

> ➡️ **Next Up:** Solving the changing-IP problem with Elastic IP addresses!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
