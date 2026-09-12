![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 34: Connecting to EC2 from Windows Using PuTTY

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Hands-On Demo (Windows Users Only)

---

## 🎯 What This Lecture Is About

```
1️⃣ Who Needs This Step?
2️⃣ Prerequisites
3️⃣ Step 1: Install PuTTY
4️⃣ Step 2: Convert .pem → .ppk with PuTTYgen
5️⃣ Step 3: Configure & Launch a PuTTY Session
6️⃣ Troubleshooting
```

---

## 1️⃣ Who Needs This Step? 🖥️

> 🐧🍎 **Mac/Linux users:** You're already done — you connected via the **standalone SSH client** in the previous step. **Skip this video.**

> 🪟 **Windows users:** This step is for you — you'll connect to your EC2 Linux instance using **PuTTY**.

---

## 2️⃣ Prerequisites ✅

| Requirement | Status |
|---|---|
| EC2 instance up and running | ✅ Already satisfied |
| General AWS prerequisites | ✅ Already satisfied earlier in the course |

> 💡 The AWS Console's **"Connect using PuTTY"** tab provides **step-by-step instructions** tailored to your specific instance — including direct links for downloads and troubleshooting.

---

## 3️⃣ Step 1: Install PuTTY 📥

```
Download PuTTY from the official download page (linked in the AWS Console)
→ Install it on your local Windows machine
```

> 📦 Installing PuTTY also installs a companion tool called **PuTTYgen** — you'll need this next.

---

## 4️⃣ Step 2: Convert `.pem` → `.ppk` with PuTTYgen 🔄

> ⚠️ **Why convert?** PuTTY does **not** support the `.pem` format that AWS provides — it requires the **`.ppk`** format.

### Conversion Steps

```
1. Launch PuTTYgen (installed with PuTTY)
2. Select key type: RSA, 2048 bits
3. Click "Load"
4. Change file filter to "All Files"
5. Select your downloaded ec2-default.pem file
6. Click "Save private key" → produces a .ppk file
```

| Setting | Value |
|---|---|
| Key Type | RSA |
| Key Size | 2048 |
| Source File | `ec2-default.pem` |
| Output File | `ec2-default.ppk` |

---

## 5️⃣ Step 3: Configure & Launch a PuTTY Session 🔌

### Session Settings

```
Open PuTTY → Session
Host Name: ec2-user@<public-dns-name>
Port: 22
```

### SSH Authentication Settings

```
Category → Connection → SSH → Auth
→ Browse → select the .ppk file you created
```

### Connect

```
Click "Open"
```

### 📋 PuTTY Configuration Quick Reference

| Section | Setting | Value |
|---|---|---|
| Session | Host Name | `ec2-user@<public-dns-name>` |
| Session | Port | `22` |
| Connection → SSH → Auth | Private key file | Your `.ppk` file |

> ✅ Clicking **Open** launches the terminal session and connects you to your **Linux EC2 instance** directly from Windows.

---

## 6️⃣ Troubleshooting 🛠️

| Resource | Where to Find It |
|---|---|
| 🧭 **AWS Troubleshooting Guide** | Linked directly from the "Connect using PuTTY" screen in the console |
| 🎥 **Course Troubleshooting Video** | Covered in the next lecture |

---

## 📋 Full Process Recap

| Step | Action |
|---|---|
| 1 | Install PuTTY |
| 2 | Open PuTTYgen → Load `.pem` → Save as `.ppk` |
| 3 | Configure PuTTY: Host = `ec2-user@<public-dns>`, Port = `22` |
| 4 | Browse to `.ppk` file under SSH → Auth |
| 5 | Click **Open** to connect |

---

## ✅ Final Takeaways

```
🪟 WINDOWS ONLY   → Mac/Linux users can skip this step entirely
🔄 PEM → PPK      → Required conversion, done via PuTTYgen (RSA, 2048-bit)
🔌 HOST FORMAT    → ec2-user@<public-dns-name>, Port 22
🔑 AUTH           → Browse to .ppk file under Connection → SSH → Auth
🛠️ TROUBLESHOOTING → AWS provides a guide directly in the console if issues arise
```

> 🎯 **Golden Rule:** PuTTY needs a `.ppk` key, not the AWS-provided `.pem` — always convert first using **PuTTYgen**.

> ➡️ **Next Up:** Troubleshooting common EC2 connection issues!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
