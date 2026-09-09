![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 28: Simplifying EC2 Setup with User Data (Bootstrapping)

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ The Problem: Too Many Manual Steps
2️⃣ Three Simplification Options (Preview)
3️⃣ What is User Data?
4️⃣ What is Bootstrapping?
5️⃣ Hands-On: Launching an Instance with User Data
6️⃣ Verifying User Data on the Instance
7️⃣ Important Notes About User Data
```

---

## 1️⃣ The Problem: Too Many Manual Steps 😓

So far, setting up a working web server required **many manual steps**:

```
1. Launch EC2 instance
2. Connect via Instance Connect
3. sudo su
4. yum update -y
5. yum install httpd -y
6. systemctl start httpd
7. systemctl enable httpd
8. curl dynamic data → index.html
9. Configure security group for HTTP
```

> ❓ **How do we reduce all this manual work?**

---

## 2️⃣ Three Simplification Options (Preview) 🗺️

| # | Option | What It Does |
|---|---|---|
| 1️⃣ | **User Data** | Run a script automatically **at launch** |
| 2️⃣ | **Launch Templates** | Save a reusable **instance configuration** |
| 3️⃣ | **Custom AMI** | Bake a **pre-configured image** with everything already installed |

> 📌 This lecture covers **User Data** — the next lectures cover Launch Templates and AMIs.

---

## 3️⃣ What is User Data? 💡

| Concept | Explanation |
|---|---|
| 📜 **User Data** | A **script** that's automatically executed **at instance launch** |
| 🎯 **Purpose** | Automates setup tasks that would otherwise be done manually |

> 💡 Instead of manually typing each command after connecting to the instance, you **configure the entire script upfront** — it runs automatically when the instance boots.

---

## 4️⃣ What is Bootstrapping? 🥾

| Concept | Explanation |
|---|---|
| 🥾 **Bootstrapping** | Installing **OS patches** or **software** automatically when an EC2 instance launches |
| 🎯 **Typical Tasks** | Security patches, required software, enterprise security settings |
| 🛠️ **How in EC2** | Achieved via **User Data** |

### 🔍 Viewing User Data from Inside an Instance

```bash
curl http://169.254.169.254/latest/user-data
```

---

## 5️⃣ Hands-On: Launching an Instance with User Data 🧪

### Step 1: Start the Launch Wizard

```
EC2 → Launch Instance → Amazon Linux 2 AMI → t2.micro
```

### Step 2: Add User Data (Step 3: Configure Instance Details)

> Scroll down to the **User Data** field and paste the bootstrap script:

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
curl -s http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

| Script Line | Purpose |
|---|---|
| `#!/bin/bash` | Declares this is a bash script |
| `yum update -y` | Installs all patches automatically (no prompt) |
| `yum install httpd -y` | Installs Apache automatically |
| `systemctl start httpd` | Starts the web server |
| `systemctl enable httpd` | Auto-starts server on reboot |
| `curl ... > index.html` | Populates the web page with instance data |

### Step 3: Configure Storage, Tags & Security Group

| Setting | Value |
|---|---|
| Storage | Default |
| Tag: Name | `EC2 instance using userdata` |
| Security Group | Existing group — `ec2-security-group` |

### Step 4: Review & Launch

> 📌 On the review screen, user data appears — but it's shown **Base64-encoded**, not as plain text. That's expected.

### Step 5: Select Key Pair & Launch

```
Choose existing key pair → ec2-default → Acknowledge → Launch Instance
```

> ⏳ Launch may take **a few minutes** before the instance is fully up and the web server is responding.

---

## 6️⃣ Verifying User Data on the Instance ✅

### Test the Web Server

> Copy the **new instance's public IP** and load it in the browser → the page should show the **dynamic instance data** automatically, with **no manual setup needed**!

### Confirm User Data Was Applied

```
EC2 → Select Instance → Actions → Instance Settings → View/Change User Data
```
> This shows the **exact script** that was configured — useful for troubleshooting if the web server isn't responding as expected.

### Query User Data from Inside the Instance

```bash
curl http://169.254.169.254/latest/user-data
```

| Instance | Result |
|---|---|
| **Old instance** (created manually, no user data) | ❌ `404 Not Found` |
| **New instance** (created with user data) | ✅ Returns the full bootstrap script |

> 💡 **Tip:** You can connect to a specific instance quickly by copying its **Instance ID** into the Instance Connect URL (region-specific).

---

## 7️⃣ Important Notes About User Data ⚠️

| Note | Detail |
|---|---|
| 🔄 **Changing User Data** | To update the user data on an **existing** instance, you must **stop** it first, make the change, then **start** it again |
| 🌍 **Region-Specific Connect URL** | The Instance Connect URL (with instance ID) only works within the **same region** |
| 🔍 **Troubleshooting** | If the web server doesn't work as expected, check the configured user data via **View/Change User Data** |

---

## 📋 Quick Reference

| Concept | Key Fact |
|---|---|
| User Data | Script run automatically at launch |
| Bootstrapping | Installing patches/software at launch time |
| Metadata URL for User Data | `http://169.254.169.254/latest/user-data` |
| Encoding on Review Screen | Base64 |
| Modifying User Data | Requires **stop → edit → start** |

---

## ✅ Final Takeaways

```
📜 USER DATA      → Script that auto-runs at EC2 instance launch
🥾 BOOTSTRAPPING  → Installing patches/software automatically at launch
🔍 VERIFY         → curl http://169.254.169.254/latest/user-data
🔄 MODIFY         → Must stop instance before changing user data
⚙️ AUTOMATION WIN → No more manual yum/systemctl/curl steps after launch!
```

> 🎯 **Golden Rule:** User Data eliminates repetitive manual setup — perfect for consistent, repeatable EC2 provisioning.

> ➡️ **Next Up:** Simplifying setup further with Launch Templates!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
