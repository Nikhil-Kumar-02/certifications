![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 96: Elastic Beanstalk Demo — Exploring What Got Created

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Managed Services
> ⏱️ **Type:** Hands-On Demo, Part 2 (⭐ Continues directly from Lecture 95!)

---

## 🎯 What This Lecture Is About

> 🎬 Our application is live! Now let's peek **behind the curtain** to see everything Elastic Beanstalk automatically created for us — and explore the Elastic Beanstalk console UI in depth.

```
1️⃣ Launching the Deployed App
2️⃣ What Got Created Behind the Scenes (EC2, ELB, Target Groups)
3️⃣ Applications vs Environments
4️⃣ Application Versions
5️⃣ Exploring the Environment Dashboard
```

---

## 1️⃣ Launching the Deployed App 🎉

> ⏱️ **Creation time:** ~3 minutes

Once ready, clicking the provided **URL** launches the live Python application — running in its own dedicated environment in the AWS cloud. 🎊

---

## 2️⃣ What Elastic Beanstalk Created Behind the Scenes 🔍

| AWS Resource | What Happened |
|---|---|
| 🖥️ **EC2 Instances** | Created automatically to run the application |
| ⚖️ **Elastic Load Balancer (ELB)** | Created because we requested load balancing — listens on port 80 |
| 🎯 **Target Group** | Contains the EC2 instance(s) that the load balancer routes to |

> 🏷️ **Naming Tip:** Look for the prefix **`AWSEB`** on resources — that's your signal they were created by Elastic Beanstalk.

> 🎯 **Key Insight:** This is the magic of a **managed service** — you didn't create the EC2 instance, the target group, or the load balancer manually. Elastic Beanstalk did it all for you based on your configuration choices.

### ⚠️ A Word of Caution

> 💡 You **can** directly edit these resources (e.g., load balancer attributes) — Elastic Beanstalk gives you that flexibility. **But it's recommended you don't.** Always make changes *through* Elastic Beanstalk so everything stays tracked and consistent at the Beanstalk level.

---

## 3️⃣ Applications vs Environments 🏗️

```
📦 APPLICATION = "my-first-elastic-beanstalk" (the container)
   └── 🌱 ENVIRONMENT #1 (what we just created — e.g., could be "dev")
   └── 🌱 ENVIRONMENT #2 (could add "QA")
   └── 🌱 ENVIRONMENT #3 (could add "staging" / "production")
```

> 💡 **Core Idea:** A single application can have **multiple environments** (dev, QA, stage, prod, etc.), each independently configurable.

---

## 4️⃣ Application Versions 📌

> 💡 **Core Idea:** Elastic Beanstalk tracks every version of code you've deployed.

```
1️⃣ Upload a new code version
2️⃣ Select that version
3️⃣ Actions → Deploy to a specific environment
```

> 🎯 **Key Insight:** You can deploy *any* application version to *any* environment — giving you flexible control over what runs where (e.g., testing v3 in QA while production stays on v2).

---

## 5️⃣ Exploring the Environment Dashboard 📊

| Section | What You'll Find |
|---|---|
| 📰 **Recent Events** | Load balancer created, listener created, app ready, etc. |
| 🩺 **Health** | Current health status of the application |
| ⬆️ **Upload & Deploy** | Push a new version directly from here |
| ⚙️ **Configuration** | Software, instances, capacity, rolling updates, security, monitoring |
| 📜 **Logs** | Request logs (e.g., "Last 100 Lines") directly from the console — **no server login needed!** |
| 📈 **Monitoring** | Environment health, response times, request overview, CPU utilization, network in/out |
| 🏷️ **Tags** | View and add tags |

### 📜 What's in the Logs?

```
🔧 Elastic Beanstalk engine logs
📦 Application logs
🌐 Nginx (web server) logs
❌ Error logs
```

> 🎯 **Key Insight:** You never need to SSH into a server to grab logs — everything's accessible right from the Elastic Beanstalk console.

---

## ❓ Extra Important Question: "Since Elastic Beanstalk lets me edit the load balancer and EC2 instances directly, what actually breaks if I do that anyway?"

> 💡 **Answer:** Nothing breaks immediately — but you risk **configuration drift**. Elastic Beanstalk maintains its own understanding of your environment's desired state based on what *you configured through it*. If you manually change a resource directly (say, editing the load balancer's listener rules), Elastic Beanstalk doesn't know about that change. The next time it performs an update, a health check, or a redeploy, it may **overwrite your manual change** back to what it thinks the configuration should be — silently undoing your fix. This is why the recommendation is to always route changes through Elastic Beanstalk itself, so its internal state and the actual AWS resources stay in sync.

---

## 📋 Master Summary Table

| Concept | Description |
|---|---|
| 📦 **Application** | Top-level container for environments, versions, and config |
| 🌱 **Environment** | A deployed instance of your app (dev, QA, prod, etc.) |
| 📌 **Application Version** | A specific version of your deployable code |
| 🖥️ **Auto-Created Resources** | EC2 instances, Load Balancer, Target Group (prefixed `AWSEB`) |
| 📜 **Logs** | Accessible directly via console — no server login required |

---

## ✅ Final Takeaways

```
🎉 Deployment took ~3 minutes and created a fully working, load-balanced app
🖥️ Elastic Beanstalk auto-created EC2 instances, an ELB, and a target group
⚠️ You CAN edit resources directly, but SHOULDN'T — always go through Beanstalk
📦 One application → many environments (dev/QA/stage/prod)
📌 Application versions can be deployed to any environment independently
📜 Logs, health, and monitoring — all accessible right from the console
```

> ➡️ **Next Up:** A recap of key Elastic Beanstalk concepts — plus exam-relevant details!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
