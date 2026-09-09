![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 23: Instance Metadata Service & Dynamic Data Service

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Hands-On Demo (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ What is Instance Metadata Service?
2️⃣ Exploring Metadata Endpoints
3️⃣ What is Dynamic Data Service?
4️⃣ Exploring Dynamic Data Endpoints
5️⃣ Metadata vs Dynamic Data — Key Exam Notes
```

---

## 1️⃣ What is Instance Metadata Service? 💡

> The **Instance Metadata Service** lets an EC2 instance retrieve **information about itself** — from **inside** the instance.

| Detail | Value |
|---|---|
| 🌐 **Base URL** | `http://169.254.169.254/latest/meta-data` |
| 📍 **Availability** | Only accessible **from inside the EC2 instance** — cannot be called from your local machine |
| ⭐ **Exam Tip** | This exact URL has appeared in exam questions — **memorize it!** |

---

## 2️⃣ Exploring Metadata Endpoints 🔍

### Step 1: Connect & Become Root

```bash
sudo su
```

### Step 2: Query the Base Metadata URL

```bash
curl http://169.254.169.254/latest/meta-data
```

> 📌 This doesn't return actual values — it returns the **list of available paths** you can query further.

### Step 3: Query Specific Metadata Paths

| Path Appended | Returns |
|---|---|
| `/ami-id` | The AMI ID used to launch the instance |
| `/hostname` | The instance's hostname |
| `/instance-id` | The unique Instance ID |
| `/instance-type` | The instance type (e.g., `t2.micro`) |
| `/local-hostname` | Internal hostname |
| `/local-ipv4` | Private IP address |
| `/public-hostname` | Public hostname |
| `/public-ipv4` | Public IP address |
| `/security-groups` | Attached security groups |
| `/placement/availability-zone` | The AZ the instance runs in |

Example:
```bash
curl http://169.254.169.254/latest/meta-data/ami-id
curl http://169.254.169.254/latest/meta-data/hostname
curl http://169.254.169.254/latest/meta-data/instance-id
curl http://169.254.169.254/latest/meta-data/instance-type
```

---

## 3️⃣ What is Dynamic Data Service? 💡

> Similar to Instance Metadata, but returns **dynamic data** about the instance — accessed via a different path.

| Detail | Value |
|---|---|
| 🌐 **Base URL** | `http://169.254.169.254/latest/dynamic/instance-identity` |
| 📄 **Key Endpoint** | `/document` |

---

## 4️⃣ Exploring Dynamic Data Endpoints 🔍

```bash
curl http://169.254.169.254/latest/dynamic/instance-identity/document
```

### 📋 Data Returned by the `document` Endpoint

| Field | Description |
|---|---|
| Account ID | AWS account that owns the instance |
| Architecture | CPU architecture (e.g., x86_64) |
| Availability Zone | Where the instance is running |
| AMI ID | The AMI used |
| Instance ID | Unique instance identifier |
| Instance Type | e.g., t2.micro |
| Private IP | Internal IP address |
| Region | AWS region |

> 📌 Other available sub-services under `/dynamic/instance-identity` include **keys**, **signature**, and **pkcs7** — but the `document` endpoint is the most commonly used.

---

## 5️⃣ Metadata vs Dynamic Data — Key Exam Notes ⭐

| Service | URL Pattern | Purpose |
|---|---|---|
| 🗂️ **Instance Metadata** | `/latest/meta-data` | Info about the instance (IDs, IPs, AMI, security groups, etc.) |
| 📄 **Dynamic Data** | `/latest/dynamic/instance-identity/document` | Similar/overlapping info, packaged as a **signed document** |

> ✅ **Exam Tip:** You will **not** be asked to differentiate deeply between the two services. What matters is knowing:
> - Both services **exist**
> - Both are accessed via `169.254.169.254`
> - Both return **information about the EC2 instance itself**

---

## ✅ Final Takeaways

```
🗂️ METADATA URL   → http://169.254.169.254/latest/meta-data
📄 DYNAMIC DATA URL → http://169.254.169.254/latest/dynamic/instance-identity/document
📍 ACCESS         → Only from INSIDE the EC2 instance
⭐ EXAM RELEVANCE  → Memorize the base URL — commonly tested!
```

> 🎯 **Golden Rule:** Both services return **self-referential data** about the running instance — useful for scripts that need to know "who am I and where am I running?"

> ➡️ **Next Up:** Customizing the web server to display dynamic instance data!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
