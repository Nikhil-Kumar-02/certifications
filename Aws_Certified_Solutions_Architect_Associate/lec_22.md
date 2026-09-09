![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 22: Installing an Apache HTTP Server on EC2

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Reconnecting to the EC2 Instance
2️⃣ Becoming a Superuser
3️⃣ Updating Software (yum update)
4️⃣ Installing Apache HTTP Server (httpd)
5️⃣ Starting & Enabling the Service
6️⃣ Troubleshooting: Security Group Blocks Access
7️⃣ Fixing It: Allowing HTTP Traffic (Port 80)
```

---

## 1️⃣ Reconnecting to the Instance 🔗

> 💡 Tip: Instead of the small pop-up window for **EC2 Instance Connect**, you can copy the Instance Connect URL and open it in a **full browser tab** for a better experience.

Quick check commands once connected:

```bash
whoami          # confirms current user
ls              # lists files (empty home folder)
pwd             # confirms present working directory (home folder)
```

---

## 2️⃣ Become a Superuser 🔐

To install software, you need **root/superuser** privileges:

```bash
sudo su
```

> ✅ Once elevated, you don't need to prefix every command with `sudo`.

---

## 3️⃣ Update Software: `yum update` 🔄

```bash
yum update
```

| What It Does | Why It Matters |
|---|---|
| Installs all available **software updates** | Ensures **security patches** are applied |

> ⚠️ **Interactive Prompt:** Running this manually will ask *"Is this OK? [y/n]"*.

### 🤖 Automating the Prompt (for Scripts)

```bash
yum update -y
```

> 📌 The `-y` flag **auto-confirms** the prompt — essential when running this command inside a **startup script** (no human available to answer).

---

## 4️⃣ Install Apache HTTP Server: `yum install httpd` 🌐

```bash
yum install httpd
```

| Package | Purpose |
|---|---|
| `httpd` | Apache HTTP Server — turns the EC2 instance into a **web server** |

> 📌 Same as before — add `-y` to skip the confirmation prompt in automated scripts:
> ```bash
> yum install httpd -y
> ```

---

## 5️⃣ Start & Enable the HTTP Server ▶️

Two essential commands:

| Command | Purpose |
|---|---|
| `systemctl start httpd` | **Starts** the Apache server **right now** |
| `systemctl enable httpd` | Ensures Apache **auto-starts** whenever the instance reboots |

```bash
systemctl start httpd
systemctl enable httpd
```

> 💡 **Why both?** `start` handles the current session; `enable` handles all **future restarts**.

---

## 6️⃣ First Attempt: Accessing the Web Server ❌

> Using the instance's **Public IP address** in the browser → **"Not Allowed"** ⛔

### 🕵️ Why It Failed

| Cause | Explanation |
|---|---|
| 🛡️ **Security Group** | Acts as a **firewall** — blocks any traffic not explicitly allowed |
| 🚫 **Missing Rule** | The security group only allowed **SSH (port 22)** — **not HTTP (port 80)** |

---

## 7️⃣ Fixing It: Allow HTTP Traffic (Port 80) 🔧

### Steps to Update the Security Group

```
1. Go to EC2 → Instance → Security Group (ec2-security-group)
2. Click "Edit inbound rules"
3. Add new rule:
     Type: HTTP
     Port: 80
     Source: Anywhere (0.0.0.0/0)
4. Click "Save rules"
```

### 📋 Updated Inbound Rules Table

| Type | Protocol | Port | Source | Purpose |
|---|---|---|---|---|
| SSH | TCP | 22 | 0.0.0.0/0 | Remote login |
| **HTTP** ✅ *(new)* | TCP | **80** | 0.0.0.0/0 | Web traffic |

---

## ✅ Success! 🎉

After saving the new rule, refreshing the browser with the instance's public IP now shows the **Apache HTTP test page** — confirming the web server is **live and accessible**!

---

## 📋 Full Command Recap

| Step | Command | Purpose |
|---|---|---|
| 1 | `sudo su` | Become root user |
| 2 | `yum update -y` | Update software & security patches |
| 3 | `yum install httpd -y` | Install Apache HTTP Server |
| 4 | `systemctl start httpd` | Start the server now |
| 5 | `systemctl enable httpd` | Auto-start server on reboot |
| 6 | *(Console)* Edit Security Group | Allow HTTP (port 80) inbound |

---

## ✅ Final Takeaways

```
🔐 SUDO SU        → Become root to install software
🔄 YUM UPDATE     → Patch software (-y to auto-confirm)
🌐 YUM INSTALL HTTPD → Installs Apache web server
▶️ SYSTEMCTL START/ENABLE → Start now + auto-start on reboot
🛡️ SECURITY GROUP  → Must explicitly allow port 80 for HTTP access
```

> 🎯 **Golden Rule:** Installing and starting a service isn't enough — the **Security Group** must also allow the relevant port, or external traffic will be blocked.

> ➡️ **Next Up:** More EC2 exploration — Instance Metadata & Dynamic Data!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
