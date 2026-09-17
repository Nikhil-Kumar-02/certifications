![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 95: Elastic Beanstalk Demo — Creating a Web Application

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Managed Services
> ⏱️ **Type:** Hands-On Demo, Part 1 (⭐ Builds on Lecture 94!)

---

## 🎯 What This Lecture Is About

> 🎬 Time to get hands-on! In this step, we walk through **creating a web application in AWS Elastic Beanstalk** from the console, step by step.

```
1️⃣ Navigate to Elastic Beanstalk
2️⃣ Create the Application
3️⃣ Choose the Platform
4️⃣ Configure Software, Instances & Capacity
5️⃣ Configure Deployment Policy & Extras
6️⃣ Launch!
```

---

## ⚠️ Important Naming Warning!

> 🚨 **Do NOT abbreviate Elastic Beanstalk as "EBS"!** AWS already has a well-known service called **Elastic Block Store (EBS)** — a totally different storage service (covered later in the course). Calling Elastic Beanstalk "EBS" is a dead giveaway that you're new to AWS. 😅

---

## 1️⃣ Creating the Application 🚀

Navigate: **Services → Elastic Beanstalk → Run and Manage Web Applications → Create app**

```
📛 App Name: my-first-elastic-beanstalk-application
🏷️ Tags: optional
```

---

## 2️⃣ Choosing the Platform 🌐

Elastic Beanstalk supports a wide variety of platforms:

```
.NET | Docker | GlassFish | Go | Java | Node.js | PHP | Python | Ruby | Tomcat
```

> 🎓 **In this demo:** We chose **Python 3.7** (latest available) with **Platform version 3.0.1** (latest, recommended).

For the code itself, we deployed AWS's **sample application** — no need to write our own Python app just to understand how Elastic Beanstalk works.

---

## 3️⃣ Exploring "Configure More Options" 🔧

### 📦 Software

- Can send logs to **X-Ray**, **Amazon S3**, or **CloudWatch Logs**
- *(Skipped for this demo, but good to know these integrations exist!)*

### 💻 Instances

- Configure the **root volume**
- Assign a **Security Group** (we used `ec2-security-group`)

### 📈 Capacity

| Setting | Choice |
|---|---|
| Deployment Type | **Load balanced** (vs. single instance) |
| Min Instances | **1** |
| Max Instances | **3** |
| Pricing Option | On-Demand, or a mix of On-Demand + Spot |
| Also Configurable | Instance type, AMI ID, Availability Zones, auto-scaling metric |

### 🔄 Rolling Updates & Deployments

| Strategy | How It Works |
|---|---|
| **All at once** | Deploy the new version everywhere immediately |
| **Rolling** | Update a batch of instances at a time (e.g., 2 of 10) |
| **Rolling with additional batch** | Spin up new instances with the new version first, then phase out old ones |
| **Immutable** | Create a full duplicate set of instances with the new version, then switch over completely once verified |

> 💡 **Key Insight:** Elastic Beanstalk gives you real control over *how* new versions roll out — from risky-but-fast to safe-but-gradual.

### 🔐 Security, Monitoring & Notifications

- Configure IAM permissions, health checks, **Managed Updates** (auto-apply security patches), notifications, and VPC/private network + database attachment options.

---

## ✅ Final Configuration Used in the Demo

```
🐍 Platform: Python 3.7
📦 Code: Sample application
⚖️ Load Balancer: Enabled (min 1, max 3 instances)
```

Click **Create app** → and the environment takes a few minutes to spin up! ☕

---

## ❓ Extra Important Question: "What's the actual difference between 'rolling' and 'rolling with additional batch' deployments — and why would I pick one over the other?"

> 💡 **Answer:** Both update instances in batches rather than all at once, but the key difference is **capacity during the update**:
> - **Rolling** takes existing instances *out of service* to update them — meaning your total capacity temporarily *drops* during the deployment (e.g., updating 2 of 10 means only 8 are serving traffic at that moment).
> - **Rolling with additional batch** spins up *new* instances with the new version *first*, so your capacity never drops below normal — it briefly goes *above* normal instead.
>
> Pick **rolling** when a temporary capacity dip is acceptable and you want to save cost. Pick **rolling with additional batch** when you can't afford any reduction in capacity during deployment (e.g., high-traffic production systems).

---

## ✅ Final Takeaways

```
🚀 Elastic Beanstalk setup = choose platform → configure options → create app
⚠️ NEVER call it "EBS" — that's Elastic Block Store!
⚖️ Load-balanced deployment = min/max instance scaling built in
🔄 Multiple deployment strategies available: all-at-once, rolling, rolling+batch, immutable
🔧 Deep configurability hides behind a simple-looking UI
```

> ➡️ **Next Up:** Seeing our deployed application live, and exploring what Elastic Beanstalk created behind the scenes!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
