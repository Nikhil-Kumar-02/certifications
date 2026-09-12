![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 35: Key Pairs Review & SSH/RDP Troubleshooting

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Concept Review (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Recap: Public Key Cryptography & Key Pairs
2️⃣ Creating Key Pairs: Two Options
3️⃣ Troubleshooting Checklist for SSH/RDP
4️⃣ Special Case: Windows Instances Need an Admin Password
5️⃣ Security Group Port Requirements
6️⃣ Public DNS vs Public IP
```

---

## 1️⃣ Recap: Public Key Cryptography & Key Pairs 🔐

| Concept | Explanation |
|---|---|
| 🧮 **Algorithm** | EC2 uses **RSA** — the most popular public key cryptography algorithm |
| 🔑 **Key Pair** | Consists of a **public key** + a **private key** |
| ☁️ **Public Key** | Stored **on the EC2 instance** |
| 💻 **Private Key** | Stored **by the customer** (downloaded as a `.pem` file) |

---

## 2️⃣ Creating Key Pairs: Two Options 🛠️

| Option | Description |
|---|---|
| 1️⃣ **Self-Created** | Create your own key pair locally and **upload the public key** to AWS |
| 2️⃣ **AWS-Generated** | Let **EC2 generate** the key pair for you (the method used throughout this course) |

---

## 3️⃣ Troubleshooting Checklist for SSH/RDP 🛠️

### ✅ Checklist

| # | Check | Detail |
|---|---|---|
| 1 | 🔑 Do you have the **private key**? | Required to connect |
| 2 | 🔒 Are the **permissions** set to `0400`? | ⭐ Frequently tested! |
| 3 | 🛡️ Does the **security group** allow inbound access? | SSH (22) or RDP (3389) |
| 4 | 🌍 Are you using the correct **Public DNS / Public IP**? | Found on the Connect screen or instance details |

---

### 🔒 The `chmod 400` Fix

> ⭐ **Classic Exam Question:** *"I get a permission error connecting via SSH — what's wrong?"*

| Cause | Fix |
|---|---|
| Default key permissions may be too open (e.g., `0777`) | Run `chmod 400 <keyfile>.pem` |

```bash
chmod 400 ec2-default.pem
```

> 🚫 AWS **will not allow** SSH connections using a key with overly permissive settings like `0777`.

---

## 4️⃣ Special Case: Windows Instances Need an Admin Password 🪟

| Instance OS | Requirements to Connect |
|---|---|
| 🐧 **Linux** | Private key only (via SSH) |
| 🪟 **Windows** | Private key **+ Admin password** (via RDP) |

### 🔍 How the Windows Password Works

```
1. EC2 generates a random admin password
2. EC2 encrypts the password using the PUBLIC key
3. You receive: the private key + the encrypted password
4. You decrypt the password using YOUR private key
5. You use the decrypted password to log in via RDP
```

> 💡 **Key Takeaway:** Connecting to a **Windows** EC2 instance requires an **extra decryption step** to obtain the admin password — Linux instances don't need this.

---

## 5️⃣ Security Group Port Requirements 🔌

| Connection Type | Protocol | Port |
|---|---|---|
| 🐧 SSH (Linux) | TCP | **22** |
| 🪟 RDP (Windows) | TCP | **3389** |

> ✅ Verify these ports are allowed via **View inbound rules** on the instance's security group.

---

## 6️⃣ Public DNS vs Public IP 🌍

| Option | Where to Find It |
|---|---|
| 🌐 **Public DNS** | Connect screen, or Instance Details panel |
| 🌍 **Public IP** | Same locations — either can be used to connect |

> 💡 Both work equally well for SSH/RDP connections.

---

## 📋 Quick Reference: Top 3 Things to Remember

| # | Rule |
|---|---|
| 1 | 🔒 Private key permissions must be **`0400`** before connecting via SSH |
| 2 | 🪟 **Windows** instances require an **admin password** in addition to the private key |
| 3 | ⏳ A **timeout** when connecting almost always means a **security group** misconfiguration |

---

## ✅ Final Takeaways

```
🔑 KEY PAIR       → RSA-based, public key on AWS, private key with you
🔒 CHMOD 400      → Required permission level for .pem files
🐧 SSH → Port 22   🪟 RDP → Port 3389
🪟 WINDOWS EXTRA STEP → Decrypt admin password using the private key
⏳ TIMEOUT         → Usually a security group issue, not a key issue
```

> 🎯 **Golden Rule:** Permission errors → check `chmod 400`. Timeout errors → check the **security group**.

> ➡️ **Next Up:** Reviewing important EC2 scenarios and exam trivia!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
