![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 29: Simplifying EC2 Setup with Launch Templates

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ The Problem: Repetitive Configuration
2️⃣ What is a Launch Template?
3️⃣ Launch Templates & Spot Instances (Preview)
4️⃣ Hands-On: Creating a Launch Template from an Existing Instance
5️⃣ Hands-On: Launching a New Instance from the Template
6️⃣ Recap: User Data vs Launch Templates
```

---

## 1️⃣ The Problem: Repetitive Configuration 😓

Every time we launch an EC2 instance, we repeat the same steps:

```
✅ Choose AMI
✅ Choose Instance Type
✅ Configure Network Settings
✅ Choose Security Group
✅ Configure Key Pair
... and more
```

> ❓ **Is there a way to save this configuration and reuse it?** → Yes — **Launch Templates!**

---

## 2️⃣ What is a Launch Template? 💡

| Concept | Explanation |
|---|---|
| 📄 **Launch Template** | A **saved configuration** (AMI, instance type, security group, key pair, user data, etc.) that can be **reused** to launch new EC2 instances |
| 🔁 **Reusability** | Launch as many instances as you want from the **same template** |
| 🗂️ **Versioning** | Launch templates support **multiple versions** (e.g., v1, v2...) |

---

## 3️⃣ Launch Templates & Spot Instances (Preview) ⚡

| Concept | Detail |
|---|---|
| 💰 **Spot Instances** | Much **cheaper** EC2 instances, but can be **reclaimed by AWS** (less reliable) |
| 🚀 **Spot Fleets** | Groups of Spot Instances managed together |
| 🔗 **Connection to Launch Templates** | Launch Templates can be used to launch **Spot Instances and Spot Fleets** too |

> 📌 Spot Instances/Fleets will be covered in more depth later in the course.

---

## 4️⃣ Hands-On: Creating a Launch Template from an Existing Instance 🧪

### Step 0: Clean Up Unused Instances

```
Select "First EC2 Instance" → Actions → Instance State → Stop
```
> 💡 Stopping instances you're not actively using helps avoid unnecessary costs.

### Step 1: Two Ways to Create a Launch Template

| Method | How |
|---|---|
| 🆕 **From Scratch** | EC2 → Launch Templates → Create Launch Template |
| ♻️ **From an Existing Instance** | Select instance → Actions → **Create Template From Instance** |

> ✅ **Chosen Method:** Create from the existing **"EC2 instance using user data"**

### Step 2: Configure the Template

| Setting | Value |
|---|---|
| Template Name | `MyEc2LaunchTemplate` |
| Version Description | `v1` |
| Auto Scaling | Skipped for now (covered later with Load Balancers) |
| AMI | Pre-populated: **Amazon Linux 2** (`amzn2-ami`) |
| Instance Type | Pre-populated: **t2.micro** |
| Key Pair | Pre-populated: **ec2-default** |
| Network | Default VPC / network interfaces |
| User Data | **Automatically pulled** from the source instance (visible under Advanced Details) |

> 💡 **Key Benefit:** All settings — including **User Data** — are automatically copied from the existing instance!

### Step 3: Create the Template

```
Click "Create launch template"
```
> ✅ Template created successfully. Navigate to **Launch Templates** in the EC2 console to view it.

---

## 5️⃣ Hands-On: Launching a New Instance from the Template 🚀

```
Launch Templates → Select "MyEc2LaunchTemplate" → Actions → Launch Instance from Template
```

| Setting | Value |
|---|---|
| Template Version | v1 |
| Additional Configuration | None needed — everything is pre-configured! |

> ⏳ After a couple of minutes, the new instance reaches **running** state.

### ✅ Verification

- Copy the **public IP** of the new instance
- Load it in the browser → the **same web page** (with dynamic instance data) loads successfully
- The **Instance ID** confirms this is a **new, separate instance** created purely from the template

---

## 6️⃣ Recap: User Data vs Launch Templates 📝

| Concept | Purpose |
|---|---|
| 📜 **User Data** | Automates **what happens inside** an instance at launch (bootstrapping — installing software, patches, etc.) |
| 📄 **Launch Template** | Automates **how the instance itself is configured** (AMI, type, security group, key pair, and *includes* the user data) |

> 💡 **Together:** A Launch Template can **package** a User Data script along with all other instance settings — giving you a **one-click way** to launch fully-configured EC2 instances.

---

## 📋 Quick Reference

| Feature | User Data | Launch Template |
|---|---|---|
| Scope | Script executed inside instance | Full instance configuration |
| Reusable? | Only if copied manually | ✅ Yes, natively reusable |
| Versioning | ❌ No | ✅ Yes (v1, v2, ...) |
| Can create Spot Instances/Fleets? | ❌ No | ✅ Yes |

---

## ✅ Final Takeaways

```
📄 LAUNCH TEMPLATE → Reusable, versioned instance configuration
♻️ CREATE FROM INSTANCE → Fastest way to build a template from a working setup
📜 USER DATA INCLUDED  → Templates carry over the bootstrap script automatically
⚡ SPOT INSTANCES/FLEETS → Can be launched using Launch Templates
🚀 ONE-CLICK LAUNCH    → "Launch Instance from Template" recreates the entire setup instantly
```

> 🎯 **Golden Rule:** Launch Templates turn a repeatable, multi-step manual process into a **one-click, versioned, reusable configuration**.

> ➡️ **Next Up:** Creating custom Amazon Machine Images (AMIs)!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
