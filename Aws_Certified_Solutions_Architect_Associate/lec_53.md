![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 53: Hands-On — Creating an Auto Scaling Group

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing & Auto Scaling
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Navigating to Auto Scaling Groups
2️⃣ Launch Configuration vs Launch Template
3️⃣ Creating the Launch Template for ASG
4️⃣ Configuring the Auto Scaling Group Basics
5️⃣ Connecting the ASG to the Load Balancer
6️⃣ Health Checks & Grace Period
7️⃣ Scaling Policy Configuration
8️⃣ Notifications & Tags
9️⃣ Final Review & Creation
```

---

## 1️⃣ Navigating to Auto Scaling Groups 🧭

```
EC2 Console → Auto Scaling Groups → Create Auto Scaling Group
```

> 💡 As the console describes it: Auto Scaling helps you **manage EC2 capacity automatically** — maintaining the right number of **healthy** instances and scaling based on **demand**.

---

## 2️⃣ Launch Configuration vs Launch Template ⚖️

| Option | Status |
|---|---|
| 📄 **Launch Configuration** | Older approach |
| 📄 **Launch Template** | ✅ **Newer approach** — recommended and used in this demo |

> 📌 Both serve the same purpose (defining instance hardware/software config), but **Launch Template is the modern, recommended choice**.

---

## 3️⃣ Creating the Launch Template for ASG 📄

```
Create new launch template
```

| Setting | Value |
|---|---|
| Name | `ASGLaunchTemplate` |
| Version Description | `v1` |
| ☑️ **"Provide guidance for Auto Scaling"** | **Checked** — required for ASG compatibility |
| Source Template | `MyEC2LaunchTemplate` (v2) — building on top of our existing custom-AMI template |

### Instance Details (Inherited & Adjusted)

| Setting | Value |
|---|---|
| AMI | Custom AMI (inherited from source template) |
| Instance Type | `t2.micro` |
| Key Pair | `ec2-default` |
| Networking | Same as source |
| Storage | Same volumes as source |
| Instance Tag | Changed to `ASG instances` |

```
Click "Create launch template"
```

---

## 4️⃣ Configuring the Auto Scaling Group Basics ⚙️

```
Name: my-auto-scaling-group
Launch Template: ASGLaunchTemplate (Default version)
```

### Instance Purchase Options

| Option | Chosen |
|---|---|
| Adhere to launch template | ✅ **Selected** (no On-Demand/Spot mix for this demo) |

### Group Size & Network

| Setting | Value |
|---|---|
| Desired Capacity | **2** instances |
| VPC | Default VPC |
| Subnets | **All available subnets/AZs** selected |

---

## 5️⃣ Connecting the ASG to the Load Balancer ⚖️

> 🌟 **This is the key step** that ties Auto Scaling into our existing load balancing setup!

```
Advanced Details → "Receive traffic from one or more load balancers" → Yes
→ Select Target Group: my-target-group
```

> ✅ **Result:** Any instance launched by this ASG will **automatically register** with `my-target-group`, and the ALB will **start routing traffic to it**.

---

## 6️⃣ Health Checks & Grace Period 🏥

| Setting | Value |
|---|---|
| Health Check Type | **EC2** (could also choose **ELB**) |
| Health Check Grace Period | **60 seconds** *(reduced from higher default)* |
| Monitoring | Default (5-minute frequency) |

### 🎓 What Is the Grace Period?

> ⏳ After a new EC2 instance starts up, it's **not immediately ready** to serve requests (OS booting, application starting, etc.). The **grace period** tells the ASG to **wait** before running health checks — avoiding a false "unhealthy" verdict on a perfectly fine, just-starting instance.

---

## 7️⃣ Scaling Policy Configuration 📊

### Group Size Bounds

| Setting | Value |
|---|---|
| Minimum Size | **1** |
| Maximum Size | **3** |
| Policy Name | `Scale Group Size` |

### Target Tracking Configuration

| Setting | Value |
|---|---|
| Metric | **Average CPU Utilization** |
| Target Value | **70%** |
| Instance Warm-Up | **20 seconds** |

> 💡 **What This Means:** The ASG will try to **add or remove instances** to keep the **average CPU utilization around 70%** across the group.

> ⚠️ **Alternative: Disable Scaling** — this only allows the group to **grow**, never shrink automatically. Generally **not recommended**, since you'd have to manually reduce instances.

---

## 8️⃣ Notifications & Tags 🔔🏷️

### Notifications

> Configure **SNS (Simple Notification Service)** to get **emailed** whenever a scaling action occurs (instance added or removed).

### Tags

| Tag Key | Value |
|---|---|
| `Environment` | `Dev` |

---

## 9️⃣ Final Review & Creation ✅

### 📋 Configuration Summary

| Setting | Value |
|---|---|
| Launch Template | ASGLaunchTemplate (default version) |
| Desired / Min / Max | 2 / 1 / 3 |
| Target Group | my-target-group |
| Health Check Type | EC2 |
| Health Check Grace Period | 60 seconds |
| Scaling Policy | Target Tracking — 70% avg CPU utilization |
| Warm-Up Time | 20 seconds |

```
Click "Create Auto Scaling Group"
```

> ⚠️ **Common Hiccup:** ASG creation can sometimes fail on the first attempt. If it does, **review your configuration and try again** — it typically succeeds after a retry.

> ⏳ It takes a little while for the ASG instances to actually launch — a good time for a break!

---

## 📋 Quick Reference: ASG Setup Flow

| Step | Action |
|---|---|
| 1 | Create/select a Launch Template (checked for ASG compatibility) |
| 2 | Set Desired Capacity, VPC, and Subnets (all AZs) |
| 3 | Connect to Load Balancer's Target Group |
| 4 | Configure Health Check type + Grace Period |
| 5 | Set Min/Max bounds + Scaling Policy (Target Tracking recommended) |
| 6 | Configure notifications (optional) and tags |
| 7 | Review and create |

---

## ✅ Final Takeaways

```
📄 LAUNCH TEMPLATE → Preferred over Launch Configuration for ASGs
⚖️ ASG + TARGET GROUP → Instances automatically register with the LB's target group
🏥 GRACE PERIOD     → Gives new instances time to boot before health checks start
📊 TARGET TRACKING  → Simplest scaling policy — e.g., maintain 70% avg CPU utilization
📉📈 MIN/MAX/DESIRED  → 1 / 3 / 2 in this demo — bounds all scaling activity
🔔 SNS NOTIFICATIONS → Optional email alerts on scaling events
```

> 🎯 **Golden Rule:** Connecting an ASG to a load balancer's **target group** during creation is what makes the whole system self-managing — new instances automatically join the load balancing pool, no manual registration needed.

> ➡️ **Next Up:** Watching the Auto Scaling Group in action — testing scale behavior and instance replacement!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
