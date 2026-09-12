![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 33: Understanding Key Pairs & Connecting via SSH

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept + Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Why Look Beyond EC2 Instance Connect?
2️⃣ What is a Key Pair?
3️⃣ EC2 Instance Connect vs Standalone SSH Client
4️⃣ Hands-On: Connecting via SSH (Mac/Linux)
5️⃣ Fixing Permission Issues on the .pem File
6️⃣ Hands-On: Connecting via PuTTY (Windows)
```

---

## 1️⃣ Why Look Beyond EC2 Instance Connect? 🤔

> **EC2 Instance Connect** has been convenient — but it **hides** how the underlying SSH connection actually works.

🎯 **Goal:** Understand key pairs deeply enough to connect using a **standalone SSH client** from your own terminal.

---

## 2️⃣ What is a Key Pair? 🔑

| Component | Where It Lives | Purpose |
|---|---|---|
| 🌍 **Public Key** | Stored by **AWS** | Placed on the EC2 instance to verify identity |
| 🔐 **Private Key** | Stored by **you** (downloaded as a `.pem` file) | Used to **prove your identity** when connecting |

> 📌 When you created your first instance, you downloaded a file like `ec2-default.pem` — this is your **private key**.

> ⚠️ **Lost your key file?** You can't recover it. Instead, create a **new key pair**, download it, and launch a **new** EC2 instance with it.

---

## 3️⃣ EC2 Instance Connect vs Standalone SSH Client 🔀

### Connection Options in the AWS Console

```
Select Instance → Connect
```

| Option | How It Works |
|---|---|
| ⚡ **EC2 Instance Connect** | AWS handles the private key **automatically** — no manual key management needed |
| 🖥️ **Standalone SSH Client** | You connect from **your own terminal**, using the downloaded `.pem` private key file |
| 🛰️ **Session Manager** | (Not covered in this step) |

> 💡 **Key Insight:** With EC2 Instance Connect, **AWS itself provides the private key** to the instance behind the scenes — that's why no manual setup is needed.

---

## 4️⃣ Hands-On: Connecting via SSH (Mac/Linux) 🍎🐧

> ✅ AWS provides **exact connection instructions** directly in the console under **Connect → Standalone SSH client**.

### Step 1: Locate the `.pem` File

```bash
cd ~/Downloads/ec2-private-key
ls
# ec2-default.pem
```

### Step 2: Fix File Permissions ⚠️

> 🚨 **Common Issue:** By default, `.pem` file permissions may be too open (e.g., `777`), which **SSH will reject**.

```bash
chmod 400 ec2-default.pem
```

> ⭐ **Exam Tip:** *"I'm getting permission errors connecting via SSH"* → Check the **`.pem` file permissions**!

### Step 3: Connect via SSH

```bash
ssh -i "ec2-default.pem" ec2-user@<public-dns-name>
```

| Part | Meaning |
|---|---|
| `-i "ec2-default.pem"` | Specifies the **private key file** to use |
| `ec2-user` | The **default username** for Amazon Linux AMIs (⚠️ AWS's console instructions often show `root` — change it to `ec2-user`) |
| `<public-dns-name>` | The instance's **public DNS name** |

```
Are you sure you want to continue connecting? → yes
```

> ✅ You're now logged into your **Linux 2 AMI** instance directly from your local terminal!

---

## 5️⃣ Key Reminders About the SSH Process 📌

| Step | Why It Matters |
|---|---|
| 🔒 `chmod 400` | Restricts the key file so **only you** can read it — required by SSH |
| 👤 `ec2-user` | Default username for **Amazon Linux** — other AMIs may use different defaults (e.g., `ubuntu` for Ubuntu) |
| 🌍 Public DNS | Needed to route to the correct instance |

---

## 6️⃣ Hands-On: Connecting via PuTTY (Windows) 🪟

> 💡 If you're **not on Windows**, this section can be skipped — Mac/Linux users are all set with the standalone SSH client above.

### Step 1: Install PuTTY

> Download and install PuTTY from its official download page (linked directly in the AWS "Connect using PuTTY" instructions).

### Step 2: Convert `.pem` → `.ppk`

> ⚠️ **PuTTY doesn't support the `.pem` format** — it requires **`.ppk`**.

```
Open PuTTYgen (installed alongside PuTTY)
→ Select "RSA" and key size 2048
→ Load → "All Files" → select ec2-default.pem
→ Save private key → produces a .ppk file
```

### Step 3: Configure PuTTY Session

| Setting | Value |
|---|---|
| **Host Name** | `ec2-user@<public-dns-name>` |
| **Port** | `22` |
| **Connection → SSH → Auth → Private key file** | Browse and select the `.ppk` file |

```
Click "Open" to connect
```

### 📋 PuTTY Setup Quick Reference

| Step | Action |
|---|---|
| 1 | Install PuTTY |
| 2 | Convert `.pem` → `.ppk` using PuTTYgen |
| 3 | Set Host Name: `ec2-user@<public-dns>`, Port: `22` |
| 4 | Browse to the `.ppk` file under SSH → Auth |
| 5 | Click **Open** to connect |

> 🛠️ If connection issues occur, AWS provides a **troubleshooting guide** linked directly from the "Connect using PuTTY" screen.

---

## ✅ Final Takeaways

```
🔑 KEY PAIR       → Public key (AWS) + Private key (you, as a .pem file)
⚡ INSTANCE CONNECT → AWS auto-manages the private key for you
🖥️ STANDALONE SSH  → You manually use the .pem file with `ssh -i`
🔒 CHMOD 400       → Fixes "too open" permission errors on .pem files
👤 EC2-USER        → Default username for Amazon Linux AMIs
🪟 PUTTY (WINDOWS) → Requires converting .pem → .ppk via PuTTYgen first
```

> 🎯 **Golden Rule:** If SSH connection fails with a permissions-related error, the first thing to check is the **`.pem` file's file permissions** (`chmod 400`).

> ➡️ **Next Up:** More EC2 topics — wrapping up this section!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
