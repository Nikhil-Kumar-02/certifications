![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 39: Five Recommendations to Keep Your AWS Costs Low

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Billing & Cost Management
> ⏱️ **Type:** Concept + Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ "With Great Power Comes Great Responsibility" 💸
2️⃣ Recommendation 1: Set Billing Alerts
3️⃣ Recommendation 2: Monitor the Billing Dashboard Daily
4️⃣ Recommendation 3: Stop Resources When Not in Use
5️⃣ Recommendation 4: Understand the Free Tier
6️⃣ Recommendation 5: Understand AWS Pricing
```

---

## 1️⃣ "With Great Power Comes Great Responsibility" 💸

> ☁️ The cloud gives you **enormous power** — provision powerful resources with a single click. But that power comes with a **cost responsibility**.

🎯 **Goal of this lecture:** 5 practical recommendations to keep AWS costs as low as possible.

---

## 2️⃣ Recommendation 1: Set Billing Alerts 🔔

> 📌 Configure **billing alerts** so you're notified automatically if charges start accumulating.

> 🔗 A dedicated video/step (covered next) walks through exactly how to set these up using **CloudWatch** and **AWS Budgets**.

---

## 3️⃣ Recommendation 2: Monitor the Billing Dashboard Daily 📊

> 🗓️ **Especially in your first week** of using AWS — check the billing dashboard **every single day**.

### 🔐 Accessing the Billing Dashboard

| Account Type | Can Access Billing by Default? |
|---|---|
| 👤 **IAM User** | ❌ **No** (unless explicitly enabled) |
| 🔑 **Root Account** | ✅ Yes, always |

### Enabling IAM Access to Billing Information

```
1. Log in with ROOT account
2. Click your account name (top right) → My Account
3. Search for "IAM" → find "IAM User and Role Access to Billing Information"
4. Click Edit → Activate IAM Access → Update
```

> ✅ Once activated, your **IAM user** can access the billing dashboard directly — no need to log in as root each time.

### What to Look For in the Dashboard

| Metric | Why It Matters |
|---|---|
| 📈 **Free Tier Usage %** | Shows how close you are to exceeding free limits for each service |
| 💰 **Current Charges** | Confirms whether you've actually been billed yet |

> 💡 **Example from the course:** S3 usage hit 100% of the free tier limit in week one — but no charges yet. This is exactly the kind of thing daily monitoring catches early!

---

## 4️⃣ Recommendation 3: Stop Resources When Not in Use 🛑

> ⚠️ **Mindset Shift Required:** In traditional on-premises environments, dev/QA/staging servers often run **24/7** without a second thought. **Cloud doesn't work that way.**

| Traditional On-Prem | Cloud |
|---|---|
| Servers procured once, run indefinitely | **Pay-as-you-go** — billed for every hour running |
| No cost incentive to stop servers | **Strong incentive** to stop/terminate unused resources |

### 🎯 Resources to Watch

```
✅ EC2 instances
✅ Relational databases (RDS)
✅ Load balancers
✅ Any other "always-on" expensive resources
```

> 📌 **Rule of Thumb:** If you're not actively using a resource — **stop it** (or terminate it, per the EC2 best practice covered earlier).

> 🎓 **Course-Specific Advice:** If you practice for an hour, **terminate all your environments** at the end of that hour. Don't leave things running "just in case."

---

## 5️⃣ Recommendation 4: Understand the Free Tier 🆓

> 🔍 Search: **"AWS Free Tier"**

### Key Things to Understand

| Category | Detail |
|---|---|
| ⏳ **12-Month Limited** | Many free tier benefits only last **12 months** from account creation |
| ♾️ **Always Free** | Some services have **permanently free** tiers/limits |
| 🔄 **Rules Can Change** | Free tier terms **can change** — periodically re-check the official page |

---

## 6️⃣ Recommendation 5: Understand AWS Pricing 💰

> 🔍 Search: **"AWS Pricing"**

### It's Complex — Aim for a High-Level Understanding

| Service Type | Billing Model Example |
|---|---|
| 🖥️ **EC2** | Billed based on **how long the instance runs** |
| 🗄️ **S3 (Storage)** | Billed based on **storage amount** + number of **GET/PUT requests** |

> 💡 **Takeaway:** Different services are billed in **fundamentally different ways** — don't assume one pricing model applies everywhere. Spend time getting a general sense of how each service you use is billed.

---

## 📋 Quick Reference: The 5 Recommendations

| # | Recommendation | Difficulty |
|---|---|---|
| 1 | 🔔 Set billing alerts | ✅ Easy |
| 2 | 📊 Monitor billing dashboard daily | ✅ Easy |
| 3 | 🛑 Stop/terminate unused resources | ✅ Easy |
| 4 | 🆓 Understand the Free Tier | ⚠️ Takes time |
| 5 | 💰 Understand AWS Pricing | ⚠️ Takes time |

> 🎯 **Priority Advice:** Focus on mastering the **first three** (easy) recommendations right away. The last two (free tier & pricing understanding) will come naturally **with experience**.

---

## ✅ Final Takeaways

```
🔔 SET ALERTS      → Get notified before costs surprise you
📊 DAILY MONITORING → Check the billing dashboard every day, especially early on
🛑 STOP WHEN IDLE   → Don't run cloud resources 24/7 out of old on-prem habits
🆓 FREE TIER        → Know what's free for 12 months vs always free
💰 PRICING          → Different services bill differently — learn the basics
```

> 🎯 **Golden Rule:** Billing is one of the hardest things to get comfortable with early on — but consistently applying the first three habits (alerts, monitoring, stopping resources) will keep you safe while you learn the rest.

> ➡️ **Next Up:** Hands-on — setting up billing alerts using CloudWatch and AWS Budgets!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
