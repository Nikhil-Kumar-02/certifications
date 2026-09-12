![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 32: AMI Deep Dive — Sources, Sharing & Regional Scope

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Recap: What is an AMI?
2️⃣ Sources of AMIs
3️⃣ What's Inside an AMI: Root & Non-Root Volumes
4️⃣ Sharing an AMI with Other AWS Accounts
5️⃣ AMIs Are Region-Specific
6️⃣ Copying AMIs Across Regions
7️⃣ Exam Trivia: Cross-Region & Cross-AZ Scenarios
```

---

## 1️⃣ Recap: What is an AMI? 💡

> An **Amazon Machine Image (AMI)** contains the **operating system** and **software** you want pre-installed on your EC2 instance.

---

## 2️⃣ Sources of AMIs 📦

| Source | Description |
|---|---|
| ☁️ **AWS-Provided** | Built and maintained by AWS (e.g., Amazon Linux 2) — used throughout most of this course |
| 🏪 **AWS Marketplace** | AMIs published by **third-party vendors** — e.g., a pre-configured SQL Server image. ⚠️ May have an **hourly charge** for the software |
| 🛠️ **Custom AMI** | Created by **you**, from your own configured EC2 instance |
| 🤝 **Shared by Another Account** | Another AWS account can **share** their custom AMI with you |

### 📋 AMI Sources Quick Table

| # | Source | Cost? |
|---|---|---|
| 1 | AWS-provided | Usually free (pay for instance usage) |
| 2 | AWS Marketplace | Sometimes has additional software charges |
| 3 | Custom (self-created) | Free (pay for instance usage) |
| 4 | Shared by another account | Depends on sharer's terms |

---

## 3️⃣ What's Inside an AMI: Root & Non-Root Volumes 💾

| Concept | Explanation |
|---|---|
| 💿 **Root Volume** | The **block storage** containing the **operating system** — this is what the AMI captures by default |
| 🗄️ **Non-Root Volume** | **Additional** hard disks you can attach separately to an EC2 instance |

> 📌 Block Storage (EBS) will be covered in much more depth later in the course.

---

## 4️⃣ Sharing an AMI with Other AWS Accounts 🤝

### Method 1: Permissions Tab

```
AMIs → Select AMI → Permissions tab → Edit
→ Add the target AWS Account ID
```

> 🔍 **Finding an Account ID:** Visible in the AWS account dropdown/menu in the console.

### Method 2: Actions Menu

```
AMIs → Select AMI → Actions → Modify Image Permissions
```

| Option | Effect |
|---|---|
| 🔒 **Private (default)** | Only your own AWS account can use the AMI |
| 🌍 **Public** | **Anyone** can use the AMI |
| 🤝 **Specific Account(s)** | Only the AWS account IDs you list can use it |

---

## 5️⃣ AMIs Are Region-Specific 🌍

| Fact | Detail |
|---|---|
| 📍 **Regional Scope** | An AMI created in one region can **only** be used to launch instances **in that same region** |
| 🗄️ **Storage Backend** | AMIs are stored in **Amazon S3** (AWS's object storage service) |
| 🔄 **Cross-Region Use** | Requires **copying** the AMI to the target region first |

---

## 6️⃣ Copying AMIs Across Regions 🔁

```
AMIs → Select AMI → Actions → Copy AMI
→ Choose destination region
→ Customize name/description (optional)
→ Copy
```

> 💡 **Example:** Copying an AMI from **Mumbai (ap-south-1)** to another Asia-Pacific region.

### 🛡️ Best Practice: Backup AMIs Across Regions

> 📌 It's a **best practice** to **back up up-to-date AMIs to multiple regions** for **disaster recovery** — if one region becomes unavailable, you can still launch instances from the AMI copy in another region.

---

## 7️⃣ Exam Trivia: Cross-Region & Cross-AZ Scenarios ⭐

### ❓ Scenario 1: Launch in a Different Region

> "I have an AMI in Mumbai and want to launch an EC2 instance with it in **Tokyo**."

✅ **Answer:** You must **first copy the AMI to Tokyo**, then launch the instance from the copied AMI **in Tokyo**.

### ❓ Scenario 2: Launch in a Different AZ, Same Region

> "I have an AMI in Mumbai and want to launch an instance in a specific **Availability Zone** within Mumbai."

✅ **Answer:** **No copying needed!** AMIs are **region-scoped**, not AZ-scoped — you can launch into **any AZ within the same region** (e.g., `ap-south-1a`, `1b`, or `1c`) directly by choosing the subnet during instance configuration.

### 📋 Quick Reference

| Scenario | Action Needed |
|---|---|
| Same region, different AZ | ✅ No copy needed — just choose the subnet/AZ |
| Different region | 🔄 **Copy the AMI** to that region first |

---

## ✅ Final Takeaways

```
💾 AMI            → OS + software + configuration, pre-built image
📦 SOURCES        → AWS-provided, Marketplace, Custom, Shared
💿 ROOT VOLUME     → Where the AMI's OS lives
🗄️ STORAGE BACKEND → AMIs are stored in Amazon S3
🌍 REGION-SPECIFIC → AMI usable only within its own region
🔄 COPY AMI        → Required for cross-region use
🛡️ DR BEST PRACTICE → Back up AMIs across multiple regions
```

> 🎯 **Golden Rule:** AMIs are tied to a **region**, not an Availability Zone — copy across regions, but freely choose any AZ within the same region.

> ➡️ **Next Up:** Understanding Key Pairs — public keys, private keys, and connecting via standalone SSH!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
