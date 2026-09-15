![Cloud Computing](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 88: IaaS vs PaaS — Setting Up an App & a Database

> 📚 **Course:** Cloud Computing Fundamentals
> 🧩 **Section:** Cloud Service Models
> ⏱️ **Type:** Concept + Examples

---

## 🎯 What This Lecture Is About

> 🎬 In this step, we focus on the first two service models — **Infrastructure as a Service (IaaS)** and **Platform as a Service (PaaS)** — and compare them using two real-world examples:

```
1️⃣ Setting up an APPLICATION
2️⃣ Setting up a DATABASE
```

> 🔗 The goal: see exactly how responsibility shifts between **you** and the **cloud provider** depending on the model you pick.

---

## 1️⃣ Setting Up an Application

### 🏗️ The IaaS Way (You Do (Almost) Everything)

| Step | What You Handle |
|---|---|
| 1️⃣ | Provision a **virtual machine** |
| 2️⃣ | Install the **operating system** |
| 3️⃣ | Install the **runtime** (Python, Node.js, Java, etc.) |
| 4️⃣ | Upload code & **configure manually** |
| 5️⃣ | Set up **multiple instances**, auto scaling & load balancing |
| 6️⃣ | Handle **ongoing maintenance** — OS patches, runtime upgrades, health monitoring |

> 💡 **Core Idea:** The cloud gives you hardware + virtualization. That's it. Everything else — OS, runtime, scaling, code, config — is **your job**.

### ⚡ The PaaS Way (Platform Does the Heavy Lifting)

> 🛠️ Examples: **AWS Elastic Beanstalk**, **Azure App Service**, **Google App Engine**

```
1️⃣ Pick a platform
2️⃣ Upload your code artifact
3️⃣ Platform provisions VMs, installs OS, deploys & starts the app
4️⃣ Auto scaling + load balancing? Just a config toggle away
5️⃣ Maintenance (patching, server management) → NOT your problem
```

> 🎯 **Key Insight:** Hardware, virtualization, OS, runtime, and scaling all become the **provider's** responsibility. You're left with just **application code + configuration**.

---

## 2️⃣ Setting Up a SQL Database

### 🏗️ The IaaS Way

| Step | What You Handle |
|---|---|
| 1️⃣ | Create a **VM** to host the database |
| 2️⃣ | **Install** the database software |
| 3️⃣ | **Configure storage** (add disks as needed) |
| 4️⃣ | Set up **backups & replication** for durability |
| 5️⃣ | Handle **maintenance** — storage growth, patches, performance monitoring |

### ⚡ The PaaS Way

> 🛠️ Examples: **AWS RDS**, **Azure SQL Database**, **Google Cloud SQL**

```
1️⃣ Pick the service
2️⃣ Choose the database type (MySQL / PostgreSQL / SQL Server)
3️⃣ Choose configuration (CPU, memory, storage)
4️⃣ Configure backups + high availability (standby or not)
```

> 💡 **Core Idea:** Everything else is handled by the platform. You get to focus purely on **data and queries** — not servers.

---

## 📋 Master Summary Table

| Pillar | IaaS | PaaS |
|---|---|---|
| 🏗️ **Infrastructure** | Cloud provider | Cloud provider |
| 💻 **OS & Runtime** | 🙋 You | ☁️ Cloud provider |
| 📈 **Scaling & Load Balancing** | 🙋 You (manual setup) | ☁️ Cloud provider (easy config) |
| 🧰 **Application Code & Config** | 🙋 You | 🙋 You |
| 🗄️ **Database Software & Durability** | 🙋 You | ☁️ Cloud provider |
| 📊 **Data & Queries** | 🙋 You | 🙋 You |

---

## ❓ Extra Important Question: "If PaaS handles so much, why would anyone still choose IaaS?"

> 💡 **Answer:** Control and flexibility. With IaaS, you can customize the OS, install any software stack you want, tune the kernel, or meet strict compliance/regulatory requirements that demand full control over the environment. PaaS trades that control away for convenience — great when you want speed and low ops overhead, but limiting if your workload needs deep customization or you're locked into a specific compliance posture that a managed platform can't satisfy.

---

## ✅ Final Takeaways

```
🏗️ IaaS  → Cloud gives HARDWARE + VIRTUALIZATION only. You manage EVERYTHING else.
⚡ PaaS  → Cloud manages INFRASTRUCTURE + OS + RUNTIME + SCALING + DURABILITY.
           You focus on APPLICATION CODE / DATA + CONFIGURATION.
🎯 TRADE-OFF → IaaS = more control, more responsibility.
               PaaS = less control, less responsibility, faster to ship.
```

> ➡️ **Next Up:** Digging deeper into IaaS vs PaaS responsibilities — and expanding beyond apps/databases into other managed services.

---
*🖊️ Notes based on a Cloud Computing Fundamentals course lecture series.*
