![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 31: Using the Custom AMI in a Launch Template

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ The Custom AMI Is Ready
2️⃣ Creating a New Launch Template Version (v2)
3️⃣ Swapping the AMI in the Template
4️⃣ Simplifying User Data (Since Software Is Pre-Installed)
5️⃣ Launching an Instance from v2
6️⃣ Setting v2 as the Default Version
7️⃣ Verifying the New Instance
```

---

## 1️⃣ The Custom AMI Is Ready ✅

> After a few minutes, the custom AMI created in the previous step becomes **available**.

📌 **Tip:** Note down the **AMI ID** (e.g., ending in `1a8`) — you'll need it to select the right image.

Two ways to use this AMI:

| Option | Description |
|---|---|
| 1️⃣ **Traditional Launch** | Go to Instances → Launch new instance → manually select this AMI |
| 2️⃣ **Update Launch Template** ✅ | Create a **new version** of the existing launch template using this AMI |

---

## 2️⃣ Creating a New Launch Template Version (v2) 📄

> 💡 **Key Feature:** Launch templates support **multiple versions** — you don't have to overwrite v1.

```
Launch Templates → Select template → Actions → Modify template (Create new version)
```

| Setting | Value |
|---|---|
| Version Description | `v2-usingcustomami` |

---

## 3️⃣ Swapping the AMI in the Template 🔄

| Step | Detail |
|---|---|
| 🔍 **Find the AMI** | Scroll down through the AMI list, or **type the noted AMI ID** directly to find it faster |
| ✅ **Select** | Choose `MyCustomizedAMI` (ID ending in `1a8`) |
| 🔁 **Everything Else** | Left **unchanged** from v1 |

---

## 4️⃣ Simplifying User Data (Since Software Is Pre-Installed) ✂️

> Since the custom AMI **already has Apache installed and configured**, most of the old user data script is no longer needed.

### Before (v1 User Data)
```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
curl -s http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

### After (v2 User Data) ✅
```bash
#!/bin/bash
curl -s http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

| Removed | Reason |
|---|---|
| `yum update -y` | Already baked into the AMI |
| `yum install httpd -y` | Apache already installed |
| `systemctl start httpd` | Already part of the AMI |
| `systemctl enable httpd` | Already part of the AMI |
| **Kept:** `curl ... > index.html` | ✅ This is **instance-specific** — must run fresh each time to reflect *this* instance's details |

> 💡 **Key Insight:** Generic setup steps belong in the AMI. **Instance-specific** steps (like fetching *this* instance's identity) still belong in User Data.

```
Click "Create template version"
```

---

## 5️⃣ Launching an Instance from v2 🚀

```
Launch Templates → Actions → Launch Instance from template
→ Template Version: v2-usingcustomami
→ Number of instances: 1
→ Launch Instance from template
```

> ⏳ New instance enters **Pending** status, then becomes **Running**.

### 🧹 Cleanup

```
Select all older instances → Actions → Instance State → Stop
```
> 💡 Rename the new instance (e.g., to **"Launch Template v2"**) for clarity.

---

## 6️⃣ Setting v2 as the Default Version ⭐

```
Launch Template → Actions → Set default version → Choose "2" → Set as default version
```

> ✅ From now on, launching an instance from this template **without specifying a version** will use **v2** automatically.

---

## 7️⃣ Verifying the New Instance ✅

```
Instances → Filter: Instance State = Running
```

| Check | Result |
|---|---|
| AMI ID | Ends in `1a8` — confirms it's `MyCustomizedAMI` |
| Public IP → Browser | Web page loads with instance-specific dynamic data |

> ✅ Confirmed: the instance was launched using the **custom AMI**, and the web server responds instantly — no install steps needed at boot!

---

## 📋 Quick Recap: Why This Matters

| Benefit | Explanation |
|---|---|
| ⚡ **Faster Launch** | No software installation needed at boot — it's baked into the AMI |
| ⭐ **Exam Tip** | *"How do you make EC2 launch time faster?"* → **Use a customized AMI** |

---

## ✅ Final Takeaways

```
📄 NEW TEMPLATE VERSION → Swap AMI without losing the old version
✂️ TRIMMED USER DATA    → Keep only instance-specific commands, drop install steps
⭐ SET DEFAULT VERSION  → Makes future launches use v2 automatically
⚡ FASTER LAUNCH        → Custom AMI = quicker boot-to-ready time
```

> 🎯 **Golden Rule:** When you bake software into a custom AMI, trim your User Data down to only the commands that must run **fresh, per-instance** — everything else belongs in the AMI itself.

> ➡️ **Next Up:** A deeper review of AMI concepts — sources, sharing, and regional scope!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
