![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 24: Customizing the Web Server with Dynamic Instance Data

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Reconnecting to the Instance
2️⃣ Writing a Custom HTML Page
3️⃣ Injecting Command Output into the Page
4️⃣ Displaying Dynamic Data on the Web Page
5️⃣ Why This Matters for Load Balancing (Preview)
6️⃣ Running curl in Silent Mode
```

---

## 1️⃣ Reconnecting to the Instance 🔗

> If **EC2 Instance Connect** shows an error, simply refresh the AWS Management Console session and reconnect.

```bash
sudo su
```

> ✅ Always become **root** first to avoid file permission issues.

---

## 2️⃣ Writing a Custom HTML Page 📝

By default, Apache shows a generic **test page**. Let's replace it with custom content.

### 📁 Key File Path

```
/var/www/html/index.html
```

### ✏️ Basic Custom Text

```bash
echo "Getting started with AWS with in28minutes" > /var/www/html/index.html
```

> 🔄 Refresh the browser (using the instance's **public IP**) → you'll now see the custom message instead of the default Apache page!

---

## 3️⃣ Injecting Command Output into the Page 💻

Instead of static text, you can **embed the output of shell commands** directly into the HTML.

### 🔧 Syntax: Command Substitution

```bash
echo "Getting started with AWS with $(whoami)" > /var/www/html/index.html
```

> ⚠️ **Important:** Use **parentheses** `$( )`, **not curly braces** `${ }`, for command substitution.

| Command Used | Output Shown on Page |
|---|---|
| `$(whoami)` | `root` |
| `$(hostname)` | The instance's hostname |

```bash
echo "Getting started with AWS with $(hostname)" > /var/www/html/index.html
```

---

## 4️⃣ Displaying Dynamic Data on the Web Page 🌐

Now let's go further — display the **full Dynamic Data Service** output on the web page.

```bash
curl http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

> 🔄 Refreshing the public URL now shows:
> - Account ID
> - Architecture
> - Private IP
> - Instance ID
> - AMI ID
> - Region
> - ...and more!

### ⚠️ Security Consideration

> 🚨 **In real-world applications, you typically would NOT expose this kind of internal data publicly!**

---

## 5️⃣ Why This Matters: Load Balancing Preview 🔮

| Future Use Case | Why This Data Helps |
|---|---|
| ⚖️ **Load Balancers** | With multiple EC2 instances behind a load balancer, this data helps identify **which specific instance** served a given response |
| 🔍 **Debugging** | Instance ID / Private IP / AMI info makes it easy to trace requests back to their source instance |

> 💡 This is a **preview** of a technique used later in the course when working with **Load Balancers** and **multiple EC2 instances**.

---

## 6️⃣ Running `curl` in Silent Mode 🤫

By default, `curl` prints extra progress/status info (bytes received, etc.) to the terminal.

### 🔇 Suppress Output with `-s`

```bash
curl -s http://169.254.169.254/latest/dynamic/instance-identity/document > /var/www/html/index.html
```

| Flag | Effect |
|---|---|
| `-s` | **Silent mode** — suppresses progress meter and error messages in the terminal |

> 📌 The **web page content doesn't change** — only the **terminal output** during the command becomes cleaner.

---

## 📋 Full Command Recap

| Step | Command | Result |
|---|---|---|
| 1 | `sudo su` | Become root |
| 2 | `echo "text" > /var/www/html/index.html` | Static custom page |
| 3 | `echo "text $(whoami)" > /var/www/html/index.html` | Inject command output |
| 4 | `curl <dynamic-data-url> > /var/www/html/index.html` | Show live instance data on the page |
| 5 | `curl -s <dynamic-data-url> > /var/www/html/index.html` | Same result, cleaner terminal output |

---

## ✅ Final Takeaways

```
📁 HTML PATH      → /var/www/html/index.html
🔧 CMD SUBSTITUTION → $(command) — use parentheses, not braces
🌐 DYNAMIC DATA    → curl the instance-identity/document URL into index.html
⚖️ LOAD BALANCER PREVIEW → Instance-specific data helps trace which instance responded
🤫 SILENT CURL     → curl -s suppresses terminal progress output
```

> 🎯 **Golden Rule:** Embedding instance identity data into a web page is a simple but powerful trick for **debugging load-balanced environments** — you'll use this again soon!

> ➡️ **Next Up:** Diving deeper into EC2 Security Groups!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
