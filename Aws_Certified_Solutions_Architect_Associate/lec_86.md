![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 86: Load Balancer Scenarios — Quick Review

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing — Advanced Topics
> ⏱️ **Type:** Scenario-Based Review (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Scenario: Maintaining Sticky Sessions
2️⃣ Scenario: Distributing Load Only to Healthy Instances
3️⃣ Scenario: Distributing Load Across Multiple AZs
4️⃣ Scenario: Letting In-Flight Requests Complete
5️⃣ Scenario: Giving New Instances Warm-Up Time
6️⃣ Scenario: Protecting Against Web Attacks (WAF)
7️⃣ Scenario: Protecting Against DDoS Attacks
```

---

## 1️⃣ Scenario: Maintaining Sticky Sessions 🍪

> ❓ *"I want requests from the same user to always go to the same target."*

✅ **Solution: Enable Stickiness**

| Fact | Detail |
|---|---|
| 🍪 **Mechanism** | Cookie-based |
| 📛 **Typical Cookie Name** | `AWSELB` |

---

## 2️⃣ Scenario: Distributing Load Only to Healthy Instances 🏥

> ❓ *"I only want traffic going to instances that are actually working."*

✅ **Solution: Configure Health Checks on the Target Group**

### Health Check Types

```
✅ Ping
✅ Connection
✅ Web page request (HTTP)
```

### Configurable Health Check Settings

| Setting | Purpose |
|---|---|
| ⏱️ **Interval** | How often the health check runs |
| ⏳ **Timeout** | How long to wait for a response |
| ❌ **Unhealthy Threshold** | How many consecutive failures before marking a target **unhealthy** |
| ✅ **Healthy Threshold** | How many consecutive successes before marking a target **healthy** again |

### Instance States

| State | Meaning |
|---|---|
| ✅ **InService** | Passing health checks — receiving traffic |
| ❌ **OutOfService** | Failing health checks — traffic withheld |

---

## 3️⃣ Scenario: Distributing Load Across Multiple AZs 🗺️

> ❓ *"I want to distribute load between instances in two different Availability Zones in the same region."*

✅ **Solution: Enable Cross-Zone Load Balancing**

> 🔗 **Recall:** Always ON for ALB (can't disable); must be manually enabled for NLB and CLB.

---

## 4️⃣ Scenario: Letting In-Flight Requests Complete 🚰

> ❓ *"How do I ensure requests already in progress to an unhealthy instance are given a chance to finish?"*

✅ **Solution: Enable Connection Draining (Deregistration Delay)**

| Setting | Value |
|---|---|
| ⏳ **Default** | 300 seconds |
| 📏 **Configurable Range** | 1 to 3600 seconds |

---

## 5️⃣ Scenario: Giving New Instances Warm-Up Time ⏰

> ❓ *"I want new EC2 instances to have some time to 'warm up' before receiving full load from the Elastic Load Balancer."*

✅ **Solution: Configure a Health Check Grace Period**

> 🔗 **Recall:** This is set on the **Auto Scaling Group**, ensuring instances aren't marked unhealthy (and thus denied traffic, or worse, recycled) simply because they haven't finished booting up yet.

---

## 6️⃣ Scenario: Protecting Against Web Attacks (WAF) 🛡️

> ❓ *"How do I protect my load balancer from attacks like SQL injection or cross-site scripting (XSS)?"*

✅ **Solution: Integrate with AWS WAF (Web Application Firewall)**

| Fact | Detail |
|---|---|
| 🛡️ **AWS WAF** | Web Application Firewall — protects against common web exploits |
| 🎯 **Example Threats Covered** | SQL Injection, Cross-Site Scripting (XSS) |
| 🔗 **Integration** | Attach WAF to your Elastic Load Balancer (specifically ALB) |

> 📌 **Note:** WAF will be covered in **much more depth** later in the course — for now, just know it's the answer for **application-layer attack protection**.

---

## 7️⃣ Scenario: Protecting Against DDoS Attacks 🛡️

> ❓ *"How do I protect my web application from Distributed Denial of Service (DDoS) attacks?"*

✅ **Solution: Use Application Load Balancer**

| Fact | Detail |
|---|---|
| 🛡️ **Built-In Protection** | ALB provides **default protection** against many common DDoS attack types |
| 🎯 **Example Attack Types Covered** | SYN floods, UDP reflection attacks |

> 💡 **Key Insight:** You don't need to do anything special to get **baseline DDoS protection** — it comes **built into ALB** automatically. (AWS Shield, a dedicated DDoS protection service, provides even more advanced protection and will be covered later in the course.)

---

## 📋 Master Scenario Reference Table

| Scenario | Solution |
|---|---|
| Same user → same target | **Sticky Sessions** (cookie-based, `AWSELB`) |
| Only route to healthy instances | **Health Checks** on target group (interval, timeout, thresholds) |
| Spread load across multiple AZs | **Cross-Zone Load Balancing** |
| Let in-flight requests finish before removal | **Connection Draining** (default 300s, range 1-3600s) |
| Give new instances time to boot | **Health Check Grace Period** (on the ASG) |
| Protect against SQL injection / XSS | **AWS WAF** integration |
| Protect against DDoS attacks | **Application Load Balancer** (built-in protection) |

---

## ✅ Final Takeaways

```
🍪 STICKY SESSIONS      → Cookie-based (AWSELB); same user → same target
🏥 HEALTH CHECKS         → Ping/Connection/HTTP; configurable interval/timeout/thresholds; InService/OutOfService
🗺️ CROSS-ZONE LB         → Spreads traffic across AZs
🚰 CONNECTION DRAINING   → Default 300s, range 1-3600s; lets in-flight requests finish
⏰ GRACE PERIOD           → Gives new instances warm-up time before health checks begin (on ASG)
🛡️ WAF                   → Protects against SQL injection, XSS, and other web application attacks
🛡️ ALB DDoS PROTECTION   → Built-in protection against SYN floods, UDP reflection attacks
```

> 🎯 **Golden Rule:** This lecture is essentially a **rapid-fire scenario-to-solution mapping** — for the exam, practice recognizing the **scenario keywords** ("same user," "in-flight requests," "warm up," "SQL injection," "DDoS") and instantly recalling the matching **feature name**.

> ➡️ **Next Up:** Moving into the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
