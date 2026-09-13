![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 68: Hands-On — Configuring Dedicated Hosts & Instance Tenancy

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Console Walkthrough (No resources created — cost awareness demo)

---

## 🎯 What This Lecture Is About

```
1️⃣ Where to Create a Dedicated Host
2️⃣ Dedicated Host Configuration Options
3️⃣ ⚠️ Cost Warning
4️⃣ Where to Choose Tenancy When Launching an Instance
5️⃣ The Three Tenancy Options in the Launch Wizard
```

---

## 1️⃣ Where to Create a Dedicated Host 🧭

```
EC2 Console → Instances → Dedicated Hosts → Allocate Dedicated Host
```

> 📌 This is a **separate resource** from an EC2 instance — you **allocate** a dedicated host first, and **then** launch instances onto it.

---

## 2️⃣ Dedicated Host Configuration Options ⚙️

| Setting | Description |
|---|---|
| 🏷️ **Instance Family** | Choose which instance family this host supports |
| 🔢 **Instance Type Support** | Choose to support **all instance types** within that family, OR restrict to **one specific type** |
| 🗺️ **Availability Zone** | Choose which AZ the dedicated host is located in |
| 🤖 **Instance Auto-Placement** | Choose whether instances can be **automatically placed** onto this host, or must be **explicitly assigned** |

### 🎓 What Is "Instance Auto-Placement"?

| Setting | Behavior |
|---|---|
| ✅ **Enabled** | New instances (without a specifically chosen host) **can automatically land** on this dedicated host if it matches their requirements |
| ❌ **Disabled** | Instances will **only** be placed on this host if **explicitly targeted** |

---

## 3️⃣ ⚠️ Cost Warning 💰

> 🚨 **Important:** Dedicated Hosts are **significantly more expensive** than shared tenancy. The lecture **did not actually create one**, purely to avoid unnecessary charges.

> 💡 **Recall:** Remember — Dedicated Hosts are billed **per host**, regardless of how many (or how few) instances you're actually running on it. This makes them a meaningful cost commitment.

---

## 4️⃣ Where to Choose Tenancy When Launching an Instance 🧭

```
EC2 → Launch Instance → Choose AMI → Choose Instance Type
→ Step 3: Configure Instance Details → Search for "Tenancy"
```

> 📌 The **Tenancy** setting is found in the **"Configure Instance Details"** step of the standard EC2 launch wizard — the same wizard we've used throughout this course.

---

## 5️⃣ The Three Tenancy Options in the Launch Wizard 📋

| Option | Description | Cost |
|---|---|---|
| 🤝 **Shared** | Default — instance may run on hardware shared with other AWS customers | 💰 Standard |
| 🔒 **Dedicated Instance** | Hardware dedicated to you, but no host-level visibility/control | 💰💰 Higher than shared |
| 🖥️ **Dedicated Host** | Runs on a specific dedicated host (must already exist) — full hardware visibility | 💰💰💰 Highest, billed per host |

### 🖥️ Selecting a Specific Dedicated Host

> 💡 If you choose **"Dedicated Host"** as the tenancy option, and you have **previously created** a Dedicated Host (Step 1 above), you can then **select exactly which host** this new instance should launch onto.

```
Tenancy: Dedicated Host
   → Host: [Select from your allocated Dedicated Hosts]
```

> 📌 This is the **targeted instance placement** capability we discussed earlier — only possible with Dedicated Hosts, not Dedicated Instances.

---

## 📋 Quick Reference: Tenancy Selection Workflow

| Step | Action |
|---|---|
| 1 | *(Optional, only for Dedicated Host)* Allocate a Dedicated Host first via EC2 → Dedicated Hosts |
| 2 | Launch Instance → proceed through AMI and Instance Type selection |
| 3 | In "Configure Instance Details," find the **Tenancy** dropdown |
| 4 | Choose: **Shared** (default), **Dedicated Instance**, or **Dedicated Host** |
| 5 | If Dedicated Host chosen → select the **specific host** to launch onto |

---

## ✅ Final Takeaways

```
🖥️ DEDICATED HOSTS → Created SEPARATELY, under EC2 → Instances → Dedicated Hosts
⚙️ HOST CONFIG      → Instance family/type support, AZ, and auto-placement setting
💰 COST WARNING     → Dedicated Hosts are notably more expensive — allocate thoughtfully
🧭 TENANCY SETTING  → Found in "Configure Instance Details" during the normal launch wizard
🔒 3 OPTIONS         → Shared (default) → Dedicated Instance → Dedicated Host (with specific host selection)
```

> 🎯 **Golden Rule:** Tenancy is just **one dropdown** in the familiar EC2 launch wizard — but choosing "Dedicated Host" only becomes meaningful (and gives you **placement control**) if you've **already allocated** a Dedicated Host resource beforehand.

> ➡️ **Next Up:** More architectural EC2 and ELB considerations!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
