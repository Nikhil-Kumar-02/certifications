![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 73: Monitoring EC2 Instances with CloudWatch

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Concept + Console Walkthrough (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ CloudWatch: The Monitoring Tool for EC2
2️⃣ Basic Monitoring vs Detailed Monitoring
3️⃣ Default EC2 Metrics Tracked by CloudWatch
4️⃣ ⭐ Key Gap: What CloudWatch Does NOT Track by Default
5️⃣ How to Get OS-Level Metrics into CloudWatch
6️⃣ Viewing Metrics in the Console
```

---

## 1️⃣ CloudWatch: The Monitoring Tool for EC2 👁️

> ✅ **CloudWatch** is the AWS service used to monitor EC2 instances — and pretty much everything else in AWS.

```
EC2 → Select Instance → Monitoring tab → View CloudWatch metrics directly
```

---

## 2️⃣ Basic Monitoring vs Detailed Monitoring 📊

| Plan | Frequency | Cost | Availability |
|---|---|---|---|
| 📉 **Basic Monitoring** | Every **5 minutes** | ✅ **Free** | All EC2 instance types |
| 📈 **Detailed Monitoring** | Every **1 minute** | 💰 **Paid** | Opt-in, configured at instance creation (or later) |

> 💡 **Recall from Auto Scaling lecture:** We discussed aligning your **monitoring frequency** with your **cooldown period** — if your cooldown is 5 minutes, Basic Monitoring's default 5-minute interval is often **sufficient**, saving you the cost of Detailed Monitoring.

---

## 3️⃣ Default EC2 Metrics Tracked by CloudWatch 📋

> These are the **"EC2 system-level metrics"** — tracked **automatically**, no setup required.

| Category | Metrics |
|---|---|
| 🖥️ **CPU** | CPU Utilization |
| 🌐 **Network** | Network In (bytes received), Network Out (bytes sent) |
| 💽 **Disk** | Disk Reads, Disk Writes |
| 🚀 **Burstable Instances** | CPU Credit Usage, CPU Credit Balance *(for `t` family instances)* |

### 📋 Quick Reference

```
✅ CPU Utilization
✅ Network In / Network Out
✅ Disk Reads / Disk Writes
✅ CPU Credit Usage / CPU Credit Balance (t-family only)
```

---

## 4️⃣ ⭐ Key Gap: What CloudWatch Does NOT Track by Default 🚨

> 🎯 **Critical Exam Fact:** CloudWatch's default EC2 metrics are all **"outside the instance"** metrics (hypervisor-level) — it has **NO visibility into what's happening INSIDE the operating system**.

### ❌ NOT Tracked by Default

```
❌ Memory / RAM utilization
❌ Disk space usage (free vs used)
❌ Any application-level or OS-level metric
```

> ⭐ **Classic Exam Question:** *"Why can't I see memory utilization for my EC2 instance in CloudWatch?"* → **Because memory utilization is an OS-level metric, and CloudWatch does NOT track this by default.**

---

## 5️⃣ How to Get OS-Level Metrics into CloudWatch 🛠️

> Since CloudWatch can't see inside the OS on its own, **you** need to push that data to it.

### Option 1: CloudWatch Agent

| Fact | Detail |
|---|---|
| 🤖 **CloudWatch Agent** | An agent you **install on the EC2 instance** |
| 📤 **What It Does** | Collects OS-level metrics (like memory usage) and **sends them to CloudWatch** |

### Option 2: CloudWatch collectd Plugin

| Fact | Detail |
|---|---|
| 🔌 **collectd Plugin** | An alternative plugin-based approach to get OS metrics into CloudWatch |

### Option 3: Custom Metrics via Your Own Code

| Fact | Detail |
|---|---|
| 💻 **Custom Programs** | You can write your **own application code** on the EC2 instance to push **any custom metric** you want directly to CloudWatch |

### 📋 Summary Table

| Method | Use Case |
|---|---|
| 🤖 CloudWatch Agent | Standard OS metrics (memory, disk space, etc.) |
| 🔌 collectd Plugin | Alternative OS metrics collection method |
| 💻 Custom Code | Application-specific or business-logic metrics |

---

## 6️⃣ Viewing Metrics in the Console 🔍

```
EC2 → Select an Instance → Monitoring tab
```

| What You'll See | Detail |
|---|---|
| 📊 **Default Metrics** | All the CPU/Network/Disk metrics, graphed over time |
| ⚙️ **Monitoring Status** | Shows whether you're on **Basic** or **Detailed** monitoring |
| 🔘 **Enable Detailed Monitoring** | A button/option to switch to 1-minute interval monitoring (paid) |

---

## 📋 Master Summary Table

| Concept | Key Fact |
|---|---|
| Monitoring Service | CloudWatch |
| Basic Monitoring | Free, 5-minute intervals, all instance types |
| Detailed Monitoring | Paid, 1-minute intervals, opt-in |
| Default Metrics | CPU, Network In/Out, Disk Reads/Writes, CPU Credits (t-family) |
| ⭐ NOT Included | Memory/RAM utilization, disk space usage (OS-level metrics) |
| Getting OS Metrics | CloudWatch Agent, collectd plugin, or custom code |

---

## ✅ Final Takeaways

```
👁️ CLOUDWATCH          → The monitoring service for EC2 (and virtually all of AWS)
📉 BASIC MONITORING     → Free, every 5 minutes, all instance types
📈 DETAILED MONITORING  → Paid, every 1 minute, opt-in
📊 DEFAULT METRICS      → CPU, Network In/Out, Disk Reads/Writes, CPU Credits
⭐ MEMORY GAP            → RAM/memory utilization is NOT tracked by default — huge exam gotcha!
🤖 CLOUDWATCH AGENT     → Install this to get OS-level metrics like memory usage
💻 CUSTOM METRICS       → Push any custom application metric via your own code
```

> 🎯 **Golden Rule:** If an exam question asks about monitoring **memory utilization** or **disk space** on an EC2 instance, remember: **CloudWatch does NOT track this by default** — you need the **CloudWatch Agent** (or collectd plugin) installed on the instance to collect and send these OS-level metrics.

> ➡️ **Next Up:** More EC2 architectural topics — status checks and instance recovery!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
