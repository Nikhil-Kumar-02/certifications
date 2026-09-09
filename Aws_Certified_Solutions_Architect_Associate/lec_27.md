![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 27: Elastic IP Addresses

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ The Problem: Changing Public IPs
2️⃣ What is an Elastic IP?
3️⃣ Reboot vs Stop/Start Behavior
4️⃣ Hands-On: Creating & Associating an Elastic IP
5️⃣ Key Rules About Elastic IPs
6️⃣ Billing: When You're Charged
```

---

## 1️⃣ The Problem: Changing Public IPs 🔄

> As we saw earlier, **stopping and starting** an EC2 instance gives it a **new public IP** — breaking any shared URLs.

**Solution?** → **Elastic IP Address** 🎯

---

## 2️⃣ What is an Elastic IP? 💡

| Concept | Explanation |
|---|---|
| 🌐 **Elastic IP** | A **static, unchanging public IP address** you can allocate and attach to an EC2 instance |
| ⚡ **"Quick & Dirty" Fix** | Great for a **single EC2 instance** setup |
| ⚖️ **Not for Multiple Instances** | For multiple instances, use a **Load Balancer** instead |

---

## 3️⃣ Reboot vs Stop/Start Behavior 🔁

| Action | Public IP Behavior |
|---|---|
| 🔄 **Reboot** | Public IP **stays the same** |
| ⏸️▶️ **Stop → Start** | Public IP **changes** (unless using an Elastic IP) |

---

## 4️⃣ Hands-On: Creating & Associating an Elastic IP 🧪

### Step 1: Allocate an Elastic IP

```
EC2 Console → Network & Security → Elastic IPs → Allocate Elastic IP address
```

### Step 2: Associate It with an Instance

```
Select the Elastic IP → Actions → Associate Elastic IP address
→ Choose Instance: "First EC2 Instance"
→ Associate
```

> ✅ The Elastic IP is now **linked** to the instance.

### Step 3: Verify

> Going back to **Instances**, the **Public IPv4 address** field now shows as a **clickable link** — pointing to the associated Elastic IP.

### Step 4: Test Stop/Start Behavior

```
Actions → Instance State → Stop
```
> ✅ Even while **stopped**, the Elastic IP **remains associated** with the instance.

```
Actions → Instance State → Start
```
> ✅ The IP address **stays exactly the same** — problem solved!

---

## 5️⃣ Key Rules About Elastic IPs 📌

| Rule | Detail |
|---|---|
| 🔄 **Reassignable** | Can be **moved/switched** from one EC2 instance to another **within the same region** |
| 🔓 **Manual Disassociation Required** | Elastic IP stays attached **even if the instance is stopped** — must be **manually detached** |
| ⭐ **Exam Tip** | The fact that Elastic IPs **stay attached through stop/start** and require **manual disassociation** is frequently tested |

### 🔄 Switching an Elastic IP to Another Instance

```
Actions → Networking → Disassociate Elastic IP Address
```
> Once disassociated, another EC2 instance can then associate with the same Elastic IP.

---

## 6️⃣ Billing: When You're Charged 💰

> 🚨 **Important & Frequently Misunderstood Rule:**

| Scenario | Billed? |
|---|---|
| ✅ Elastic IP **associated** with a **running** EC2 instance | ❌ **Not charged** |
| ⚠️ Elastic IP **allocated but NOT associated** with any instance | ✅ **Charged** |
| ⚠️ Elastic IP associated with a **stopped** EC2 instance | ✅ **Charged** |

> 💡 **Why?** AWS wants to discourage people from **hoarding unused public IP addresses** — a limited resource.

### 🧹 Best Practice: Clean Up Unused Elastic IPs

```
Actions → Release Elastic IP address
```
> ✅ Always **release** an Elastic IP if you're not actively using it, to avoid unnecessary charges.

---

## 📋 Quick Reference Table

| Situation | Billed? |
|---|---|
| Associated + instance running | ❌ No |
| Associated + instance stopped | ✅ Yes |
| Allocated, not associated | ✅ Yes |
| Released | ❌ No (no longer exists) |

---

## ✅ Final Takeaways

```
🌐 ELASTIC IP    → Static public IP for a single EC2 instance
🔄 SURVIVES      → Stop/Start (stays associated even when stopped)
🔀 REASSIGNABLE  → Can move between instances within the same region
🔓 MANUAL DETACH → Only way to disassociate — doesn't happen automatically
💰 BILLING RULE  → Charged when NOT actively used by a running instance
⚖️ SCALE LIMIT   → For multiple instances, use a Load Balancer instead
```

> 🎯 **Golden Rule:** An Elastic IP is a **rented resource** — AWS charges you for **holding it idle**, not for using it. Release what you don't need!

> ➡️ **Next Up:** Simplifying EC2 setup with User Data, Launch Templates, and AMIs!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
