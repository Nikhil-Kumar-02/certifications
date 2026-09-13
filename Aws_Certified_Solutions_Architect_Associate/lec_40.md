# 🎯 Lecture 40: Setting Up Billing Alerts (CloudWatch & AWS Budgets)

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Billing & Cost Management
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Step 1: Enable Billing Preferences
2️⃣ What is CloudWatch?
3️⃣ Hands-On: Creating a Billing Alarm with CloudWatch
4️⃣ Hands-On: Creating a Budget with AWS Budgets
5️⃣ CloudWatch Alarm vs Budgets — Comparison
```

---

## 1️⃣ Step 1: Enable Billing Preferences ⚙️

> ✅ Best Practice: Use your **IAM account** (not root) for this — as with most day-to-day AWS tasks.

```
Billing Dashboard → Billing Preferences
```

### Preferences to Enable

| Preference | Purpose |
|---|---|
| 📧 **Receive PDF Invoice by Email** | Get a monthly invoice emailed to you |
| 🆓 **Receive Free Tier Usage Alerts** | Get notified when approaching free tier limits |
| 🔔 **Receive Billing Alerts** | **Required** to enable billing alarms via CloudWatch |

```
Click "Save preferences"
```

> 📌 Saving these preferences **enables** the billing alert feature — but you still need to **configure the actual alert** separately (next steps).

---

## 2️⃣ What is CloudWatch? 👁️

| Concept | Explanation |
|---|---|
| 📡 **CloudWatch** | AWS's **monitoring service** — tracks metrics and triggers **alarms** when conditions are met |
| 📊 **Use Cases** | CPU usage thresholds, billing thresholds, and much more (covered later in the course) |

> 🌍 **Important:** For **billing alarms specifically**, you must select the **US East (N. Virginia)** region — billing metrics are only available there.

---

## 3️⃣ Hands-On: Creating a Billing Alarm with CloudWatch 🔔

### Step 1: Navigate to CloudWatch

```
Services → CloudWatch
⚠️ Make sure region = US East (N. Virginia)
```

> 🆓 **Free Tier:** 10 free alarms + 1,000 free email notifications per month.

### Step 2: Create the Alarm

```
Billing → Create alarm
```

| Setting | Value |
|---|---|
| Metric | Estimated charges over 6 hours |
| Condition | Greater than **$0 USD** |

### Step 3: Configure Notification (via SNS)

> 📬 **SNS (Simple Notification Service)** sends the actual email when the alarm triggers.

| Setting | Value |
|---|---|
| Topic Name | `Billing_CloudWatch_Alarms_Topic` |
| Email | Your notification email address |

```
Create topic → (check email for confirmation link, if received)
```

### Step 4: Name & Create the Alarm

| Setting | Value |
|---|---|
| Alarm Name | `Billing Alert` |

```
Review settings → Create alarm
```

> ✅ **Result:** Whenever estimated charges exceed **$0**, you'll get an **email notification**.

---

## 4️⃣ Hands-On: Creating a Budget with AWS Budgets 📈

> 💡 **AWS Budgets** is a newer, alternative (or complementary) way to monitor spending.

### Step 1: Create a Budget

```
Services → Budgets → Create a budget
```

| Budget Type | Chosen |
|---|---|
| Cost Budget | ✅ Selected |

### Step 2: Configure Budget Details

| Setting | Value |
|---|---|
| Budget Name | Monthly Cost Budget |
| Budgeted Amount | **$1** (fixed amount) — ⚠️ cannot use $0 |

### Step 3: Configure the Alert

| Setting | Value |
|---|---|
| Alert Trigger | Actual costs > **10%** of budgeted amount |
| Effective Threshold | > **$0.10** |
| Notification Email | Your email address |

```
Confirm budget → Review → Create
```

> ✅ **Result:** You'll get an email as soon as your actual spending crosses **$0.10**.

---

## 5️⃣ CloudWatch Alarm vs Budgets — Comparison 📊

| Feature | CloudWatch Billing Alarm | AWS Budgets |
|---|---|---|
| 🌍 Region Requirement | Must use US East (N. Virginia) | No specific region requirement |
| 🎯 Trigger Type | Estimated charges over 6 hours | Actual/forecasted costs vs a defined budget |
| 📧 Notification Method | Via SNS topic | Built-in email alerts |
| 🆓 Free Tier | 10 alarms + 1,000 notifications/month | Free for basic use |
| 🧩 Flexibility | Also used for general resource monitoring (CPU, etc.) | Purpose-built for cost/budget tracking |

> 💡 **Recommendation:** Setting up **both** gives you overlapping protection — different trigger mechanisms catching unexpected spend from different angles.

---

## 📋 Quick Setup Recap

| Step | Action |
|---|---|
| 1 | Enable billing preferences (PDF invoice, free tier alerts, billing alerts) |
| 2 | Go to CloudWatch (region = US East N. Virginia) |
| 3 | Create a billing alarm: charges > $0 → SNS email topic |
| 4 | Go to AWS Budgets |
| 5 | Create a Cost Budget ($1) with alert at 10% (~$0.10) |

---

## ✅ Final Takeaways

```
⚙️ BILLING PREFERENCES → Must enable first, before configuring alarms
👁️ CLOUDWATCH          → Monitoring service; billing alarms require US East N. Virginia
📬 SNS                 → Delivers the actual email notification for CloudWatch alarms
📈 AWS BUDGETS          → Newer alternative — set a $ threshold and alert %
🔔 BOTH METHODS         → Complementary — use both for maximum safety
```

> 🎯 **Golden Rule:** Set up billing alerts (via CloudWatch and/or Budgets) as one of the **very first things** you do in a new AWS account — before you start experimenting with services.

> ➡️ **Next Up:** Moving on to the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
