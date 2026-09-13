![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 62: Load Balancing & Auto Scaling — Full Section Recap + Exam Practice Questions

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing & Auto Scaling
> ⏱️ **Type:** Full Section Recap + Practice Questions (⭐ Detailed review + exam-style Q&A)

---

## 🎯 What This Lecture Is About

This is the **complete wrap-up** of the Load Balancing & Auto Scaling section. Since this covered a LOT of ground — three load balancer types, listeners, target groups, and auto scaling groups — this recap goes through everything in **detail**, followed by a set of **exam-style practice questions** based on common certification question patterns from this topic area.

```
1️⃣ Elastic Load Balancer — The Big Picture
2️⃣ Classic Load Balancer (CLB) — Detailed Recap
3️⃣ Application Load Balancer (ALB) — Detailed Recap
4️⃣ Network Load Balancer (NLB) — Detailed Recap
5️⃣ Listeners & Target Groups — Detailed Recap
6️⃣ Auto Scaling Groups — Detailed Recap
7️⃣ Master Comparison Table: CLB vs ALB vs NLB
8️⃣ 📝 Exam Practice Questions
```

---

## 1️⃣ Elastic Load Balancer — The Big Picture 🌐

> An **Elastic Load Balancer (ELB)** distributes incoming traffic across **multiple EC2 instances**, potentially spread across **multiple Availability Zones within a single region**.

### Core Characteristics (Apply to All ELB Types)

| Characteristic | Detail |
|---|---|
| 🛠️ **Managed Service** | AWS handles the underlying infrastructure — you don't manage servers for the load balancer itself |
| ✅ **Highly Available** | AWS ensures the load balancer itself is always up |
| 📈 **Auto Scaling** | The load balancer automatically scales to handle any volume of requests — 10, 10,000, or millions |
| 🌍🔐 **Public or Private** | Can be **internet-facing** (public) or **internal** (private, restricted to the VPC) |
| 🏥 **Health-Aware Routing** | Only routes traffic to instances that pass configured **health checks** |

---

## 2️⃣ Classic Load Balancer (CLB) — Detailed Recap 🕰️

| Attribute | Detail |
|---|---|
| 🎯 **Layers Supported** | **Both** Layer 4 (Transport) **and** Layer 7 (Application) |
| 📡 **Protocols** | TCP, TLS, HTTP, HTTPS |
| 📌 **Status** | **Legacy** — AWS does **not recommend** it for new applications |
| 🎯 **When You'd Still See It** | Older accounts using **EC2-Classic** networking, or legacy applications not yet migrated |
| ⚠️ **Major Limitation** | Supports only **ONE target** — cannot support multiple target groups, making microservices architectures very difficult (you'd need a separate CLB per microservice) |

> 💡 **Historical Context:** AWS eventually decided combining Layer 4 and Layer 7 support into one load balancer was **overly complex**, leading to the creation of two **specialized** successors: ALB and NLB.

---

## 3️⃣ Application Load Balancer (ALB) — Detailed Recap ⚖️

| Attribute | Detail |
|---|---|
| 🎯 **Layer** | **Layer 7** (Application) only |
| 📡 **Protocols** | HTTP, HTTPS, **WebSockets** |
| 🧠 **Routing Intelligence** | Can inspect actual **request content** to make routing decisions |

### 🔀 Advanced Routing Conditions Supported

| # | Condition | Example |
|---|---|---|
| 1 | 🛣️ **Path** | `/microservice-a/*` → Target Group A |
| 2 | 🌐 **Host Header** | `a.example.com` → Target Group A |
| 3 | 📋 **HTTP Header** | `Authorization` header value → specific target |
| 4 | 📮 **HTTP Method** | `GET` vs `POST` → different targets |
| 5 | ❓ **Query String** | `?target=a` → Target Group A |
| 6 | 🌍 **Source IP** | Specific IP range → specific target |

### 🎯 Target Types Supported

```
✅ EC2 Instances
✅ Containerized applications (Amazon ECS)
✅ IP addresses
✅ Lambda functions (serverless)
```

### 🔑 Key ALB Facts

| Fact | Detail |
|---|---|
| 🗺️ **Cross-Zone Load Balancing** | **Always enabled** — cannot be turned off |
| 🔢 **Multiple Target Groups** | ✅ Supported — enables microservices architectures behind ONE ALB |
| 🛡️ **Security Group** | ✅ ALB has its own security group |
| 📌 **Elastic IP Support** | ❌ **Not supported** |

---

## 4️⃣ Network Load Balancer (NLB) — Detailed Recap 🌐

| Attribute | Detail |
|---|---|
| 🎯 **Layer** | **Layer 4** (Transport) only |
| 📡 **Protocols** | TCP, TLS, **UDP** |
| ⚡ **Primary Use Case** | **Extremely high performance** — millions of requests per second |

### 🎯 Target Types Supported

```
✅ EC2 Instances
✅ Containerized applications (Amazon ECS)
✅ IP addresses
❌ Lambda functions — NOT supported (this is unique to ALB)
```

### 🔑 Key NLB Facts

| Fact | Detail |
|---|---|
| 📌 **Elastic IP Support** | ✅ **YES** — the ONLY load balancer type that supports this |
| 🛡️ **Security Group** | ❌ **NLB has NO security group of its own** — filtering happens at the target (EC2) level |
| 🗺️ **Cross-Zone Load Balancing** | **Disabled by default** — must be manually enabled (opposite of ALB!) |
| 🔒 **Delete Protection** | Available as an optional safeguard |
| 🔄 **Fail-Safe Routing** | If ALL targets in ALL AZs are unhealthy, NLB routes to them anyway rather than total failure |

---

## 5️⃣ Listeners & Target Groups — Detailed Recap 👂🎯

### 👂 Listeners

| Concept | Explanation |
|---|---|
| 👂 **Listener** | Checks for connection requests using a specific **protocol + port** combination |
| 🔢 **Multiple Listeners** | A single load balancer can have **many listeners** (e.g., HTTP:80, HTTPS:443, HTTP:8080) |
| 🎯 **Listener Rules** | Route matching requests to specific **target groups**, evaluated **in order**, with a **default rule** last |
| 📤 **Fixed Response** | A listener can also respond **directly** without forwarding to any target |

### 🎯 Target Groups

| Concept | Explanation |
|---|---|
| 🎯 **Target Group** | Groups the resources (EC2, IP, Lambda) that receive traffic |
| ➕➖ **Dynamic Membership** | Instances can be added/removed at any time |
| 🔢 **Multiple Target Groups** | Supported by **ALB and NLB**, NOT by CLB — this enables microservices |
| 🔑 **Shared Membership** | A single instance **can belong to multiple target groups** simultaneously |

### 4 Key Target Group Attributes

| Attribute | Purpose | Range/Default |
|---|---|---|
| ⏳ **Deregistration Delay** | Let in-flight requests finish before removing a target | 0–3600s (default 300s) |
| 🐢 **Slow Start Duration** | Gradually ramp up traffic to newly added targets | 30–900s |
| ⚖️ **Load Balancing Algorithm** | Round Robin (default) or Least Outstanding Requests | — |
| 🍪 **Stickiness** | Cookie-based; routes a user's requests consistently to the same target | Enabled/Disabled |

---

## 6️⃣ Auto Scaling Groups — Detailed Recap 📊

### Two Core Responsibilities

| # | Responsibility | Explanation |
|---|---|---|
| 1️⃣ | 🔄 **Maintain Desired Capacity (Self-Healing)** | Continuously health-checks instances; automatically **replaces** any that go down |
| 2️⃣ | 📈📉 **Adjust to Load (Scaling)** | Scales **out** (add instances) or **in** (remove instances) based on configured policies, within Min/Max bounds |

### The 4 Core ASG Components

| Component | Purpose |
|---|---|
| 📄 **Launch Template/Configuration** | Defines instance hardware/software config |
| 📊 **Auto Scaling Group** | References the template; defines Min/Max/Desired |
| 🏥 **Health Checks** | EC2 health checks or ELB health checks |
| 📈 **Scaling Policy** | Defines when/how to scale |

### 3 Dynamic Scaling Policy Types

| Type | Complexity | How It Works |
|---|---|---|
| 🎯 **Target Tracking** | ⭐ Simple | "Maintain 70% average CPU utilization" — AWS figures out the rest; **CloudWatch alarms auto-created** |
| ⚙️ **Simple Scaling** | ⭐⭐ Moderate | "If CPU > 80%, add 5 instances" — manually configured alarm + action |
| 🪜 **Step Scaling** | ⭐⭐⭐ Complex | Tiered responses: "70-80% → +1 instance, 80-100% → +3 instances" |

> ✅ **Recommendation:** Use **Target Tracking Scaling** by default — it's simplest and AWS handles the CloudWatch alarm creation automatically.

### How Scaling Actually Triggers (CloudWatch)

```
CloudWatch Alarm (monitors a metric, e.g., CPU%) → Threshold crossed → Triggers Scaling Action → ASG adds/removes instances
```

> 🔑 **Every scaling policy = a CloudWatch Alarm (what to watch) + a Scaling Action (what to do).**

### Additional ASG Concepts Covered

| Concept | Purpose |
|---|---|
| 🪝 **Lifecycle Hooks** | Run custom actions before an instance launches or terminates |
| 🎯 **Termination Policy** | Decides WHICH instance is removed during scale-in (default: balance AZs, then oldest) |
| 🕐 **Cooldown Period** | Prevents rapid, repeated scaling actions (default 300s) |
| 🛡️ **Instance Scale-In Protection** | Shields specific instances from being terminated during scale-in |
| ⚠️ **Cannot Edit Templates** | Must create a **new version** and gradually replace instances in small batches for any config change |

---

## 7️⃣ Master Comparison Table: CLB vs ALB vs NLB 📋

| Feature | 🕰️ CLB | ⚖️ ALB | 🌐 NLB |
|---|---|---|---|
| **OSI Layer** | 4 & 7 | 7 | 4 |
| **Protocols** | TCP, TLS, HTTP, HTTPS | HTTP, HTTPS, WebSockets | TCP, TLS, UDP |
| **Content-Based Routing** | ❌ Limited | ✅ Yes (path/host/header/method/query/IP) | ❌ No |
| **Multiple Target Groups** | ❌ No | ✅ Yes | ✅ Yes |
| **Lambda as Target** | ❌ No | ✅ Yes | ❌ No |
| **Elastic/Static IP** | ❌ No | ❌ No | ✅ **Yes** |
| **Own Security Group** | ✅ Yes | ✅ Yes | ❌ **No** |
| **Cross-Zone LB Default** | Configurable | ✅ Always ON | ❌ Off by default |
| **Performance** | Moderate | Good | ⚡ Extremely High |
| **AWS Recommendation** | ⚠️ Avoid (legacy) | ✅ Recommended (web apps) | ✅ Recommended (high-perf/UDP) |

---

## 8️⃣ 📝 Exam Practice Questions

> The following are **practice-style questions** modeled on the kinds of scenarios frequently seen in the AWS Certified Solutions Architect – Associate exam for this topic area. Try answering before revealing!

<details>
<summary>❓ Q1: Which load balancer type should you use if you need to assign a static/Elastic IP address to your load balancer?</summary>

✅ **Answer: Network Load Balancer (NLB)**

NLB is the **only** AWS load balancer type that supports assigning a static or Elastic IP address. Neither ALB nor CLB support this.
</details>

<details>
<summary>❓ Q2: Your application needs to route incoming requests to different backend services based on the URL path (e.g., /orders/* vs /users/*). Which load balancer should you use?</summary>

✅ **Answer: Application Load Balancer (ALB)**

ALB operates at Layer 7 and can inspect HTTP request content — including the **path** — to route to different target groups. NLB (Layer 4) cannot inspect HTTP paths.
</details>

<details>
<summary>❓ Q3: You are building a microservices architecture with 50 different services, each needing to be independently load balanced. What is the MOST cost-effective and manageable approach?</summary>

✅ **Answer: Use a single Application Load Balancer with multiple Target Groups (one per microservice), routed via Listener Rules.**

Classic Load Balancer would require 50 separate CLB instances (one per microservice) since it doesn't support multiple target groups — very difficult to maintain. ALB's support for multiple target groups solves this elegantly with ONE load balancer.
</details>

<details>
<summary>❓ Q4: What happens to in-flight requests on an EC2 instance when it fails a health check and needs to be removed from a target group?</summary>

✅ **Answer: The Deregistration Delay (also called Connection Draining) setting controls this.**

The load balancer stops sending NEW requests to the failing target immediately, but waits up to the configured delay (default 300 seconds, max 3600 seconds) for existing in-flight requests to complete before fully removing the target.
</details>

<details>
<summary>❓ Q5: You need to load balance UDP traffic for a real-time gaming application requiring extremely high throughput. Which load balancer should you use?</summary>

✅ **Answer: Network Load Balancer (NLB)**

NLB is the only load balancer type supporting UDP. It's also purpose-built for extremely high-performance scenarios (millions of requests per second).
</details>

<details>
<summary>❓ Q6: You want your Auto Scaling Group to always maintain exactly 5 running instances, with no scaling up or down. How do you configure this?</summary>

✅ **Answer: Set Minimum = Maximum = Desired Capacity = 5**

This locks the ASG to a constant instance count. If an instance fails, the ASG will still replace it (self-healing) — but the total will never go above or below 5.
</details>

<details>
<summary>❓ Q7: You've updated your Launch Template with a new AMI containing security patches. How do you get your existing Auto Scaling Group instances updated?</summary>

✅ **Answer: You cannot edit an existing launch template/configuration in place.** Create a NEW VERSION of the launch template, update the ASG to reference it, then terminate old instances in SMALL GROUPS (not all at once) — the ASG will automatically replace them using the new template version, preserving availability throughout.
</details>

<details>
<summary>❓ Q8: Your Network Load Balancer's target health checks are all failing, even though the instances are running fine and were previously healthy behind an Application Load Balancer using the same instances. What is the MOST LIKELY cause?</summary>

✅ **Answer: The EC2 instances' Security Group doesn't allow traffic from the source the NLB uses.**

Since NLB has NO security group of its own, its traffic must be explicitly allowed at the EC2 instance's security group level. If the security group was previously configured to only allow traffic from the ALB's security group, NLB traffic would be blocked. Fix: add a rule allowing the required port from the appropriate source (or from anywhere, depending on your architecture).
</details>

<details>
<summary>❓ Q9: What is the difference between Target Tracking Scaling and Simple/Step Scaling in terms of CloudWatch Alarm configuration?</summary>

✅ **Answer:** With **Target Tracking Scaling**, AWS **automatically creates** the underlying CloudWatch alarms (a High alarm and a Low alarm) for you — you just specify a target metric value (e.g., 70% CPU). With **Simple Scaling** or **Step Scaling**, you must **manually configure** the CloudWatch alarms and the exact scaling actions to take.
</details>

<details>
<summary>❓ Q10: You want all requests from a specific logged-in user to always be routed to the same backend EC2 instance to preserve session state. What feature should you enable?</summary>

✅ **Answer: Sticky Sessions (Stickiness), configured on the Target Group.**

This is implemented via a cookie and is supported by both Application Load Balancer and Classic Load Balancer.
</details>

<details>
<summary>❓ Q11: Which load balancer type CANNOT route traffic to AWS Lambda functions?</summary>

✅ **Answer: Network Load Balancer (NLB)**

Only Application Load Balancer supports Lambda functions as a target type. NLB's target types are limited to EC2 instances and IP addresses (no Lambda option).
</details>

<details>
<summary>❓ Q12: Your Auto Scaling Group's load fluctuates rapidly, causing it to scale out and scale in every couple of minutes ("flapping"). How do you reduce this behavior?</summary>

✅ **Answer: Increase the Cooldown Period.**

The default cooldown period is 300 seconds. Increasing it makes the ASG wait longer between scaling actions, reducing rapid oscillation. Best practice: align your CloudWatch monitoring interval with your cooldown period (e.g., don't pay for 1-minute Detailed Monitoring if your cooldown is already 5 minutes).
</details>

---

## ✅ Final Takeaways

```
🌐 ELB CORE            → Managed, highly available, auto-scaling, public/private, health-check-aware
🕰️ CLB                 → Legacy; Layer 4+7; single target only; avoid for new apps
⚖️ ALB                 → Layer 7; 6 routing conditions; multiple target groups; supports Lambda
🌐 NLB                 → Layer 4; TCP/TLS/UDP; Elastic IP support; NO security group; high performance
👂 LISTENERS           → Protocol+port combos; support multiple rules and fixed responses
🎯 TARGET GROUPS       → Deregistration delay, slow start, algorithm choice, stickiness
📊 ASG                 → Self-healing + dynamic scaling; Target Tracking recommended; CloudWatch-powered
```

> 🎯 **Golden Rule for the Exam:** Most questions in this section boil down to **matching a requirement to the one load balancer type that uniquely satisfies it** — Elastic IP → NLB, Lambda targets → ALB, path/host/header routing → ALB, UDP/high-performance → NLB, legacy/single-target → CLB (and usually the "wrong" answer). Memorize the **differentiators**, not just the definitions.

> ➡️ **Next Up:** Moving into the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series, expanded with additional detail and exam-style practice questions for deeper reinforcement.*
