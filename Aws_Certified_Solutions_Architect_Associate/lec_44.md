![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 44: Hands-On — Creating a Classic Load Balancer

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Preparing EC2 Instances
2️⃣ Starting the Load Balancer Creation Wizard
3️⃣ Basic Configuration
4️⃣ Security Group for Load Balancers
5️⃣ Health Check Configuration (Deep Dive)
6️⃣ Adding EC2 Instances (Targets)
7️⃣ Cross-Zone Load Balancing
8️⃣ Connection Draining
9️⃣ Tags & Final Review
```

---

## 1️⃣ Preparing EC2 Instances 🖥️

> Before creating the load balancer, we need **at least two EC2 instances** to distribute traffic between.

```
EC2 → Instances → Select stopped instance → Actions → Instance State → Start
```

> 💡 **No existing instances?** Use the **Launch Template** created earlier:
> ```
> Launch Templates → EC2LaunchTemplate → Launch instance from template
> → Choose v1 or v2 → Launch 2 instances
> ```

---

## 2️⃣ Starting the Load Balancer Creation Wizard 🚀

```
EC2 Console → Load Balancers → Create Load Balancer
```

### Choosing the Type

| Option | Description |
|---|---|
| 🕰️ **Classic Load Balancer** | Previous generation — supports HTTP, HTTPS, **and** TCP |
| ⚖️ Application Load Balancer | (Not chosen in this demo) |
| 🌐 Network Load Balancer | (Not chosen in this demo) |

> ✅ **Chosen for this demo:** **Classic Load Balancer**

---

## 3️⃣ Basic Configuration ⚙️

| Setting | Value |
|---|---|
| Name | `my-classic-load-balancer` |
| VPC | Default VPC (pre-selected) |
| Internal Load Balancer? | ⬜ **Unchecked** — we want a **public** load balancer |
| Listener Port | **80** (HTTP) |

> 📌 A **listener** defines what port/protocol the load balancer accepts incoming traffic on. Here, we configure it to listen for **HTTP on port 80**.

---

## 4️⃣ Security Group for Load Balancers 🛡️

> ⚠️ **Best Practice:** Don't reuse the **same security group** for both EC2 instances and load balancers — keep them **separate**.

### New Security Group Configuration

| Setting | Value |
|---|---|
| Name | `load-balancers-sg` |
| Rule | Allow **TCP traffic on port 80** from **everywhere** |

> 💡 **Reusability Tip:** This same `load-balancers-sg` security group will be reused for **all future load balancers** created in this course.

> 🔒 **Secure Listener:** The wizard also offers to configure **HTTPS** with certificates — **skipped** for this demo (no certificate configured).

---

## 5️⃣ Health Check Configuration (Deep Dive) 🏥

> Every load balancer performs **regular health checks** on its EC2 instances — only **healthy** instances receive traffic.

### 📋 Health Check Settings

| Setting | Value | Meaning |
|---|---|---|
| **Ping Path** | `/` (root) | The URL path checked (using HTTP GET) |
| **Ping Port** | 80 | Port used for the health check |
| **Response Timeout** | 5 seconds | How long to wait for a response before considering it failed |
| **Health Check Interval** | 20 seconds *(reduced from default 30s)* | How often the health check runs |
| **Unhealthy Threshold** | 2 | Number of **consecutive failed** checks before marking an instance **unhealthy** |
| **Healthy Threshold** | 2 | Number of **consecutive successful** checks before marking an instance **healthy** |

### 🔍 How Each Setting Works

| Concept | Explanation |
|---|---|
| ⏱️ **Interval** | Could be set as low as a few seconds or as high as several minutes (e.g., 180s, 300s) — this demo uses **20 seconds** |
| ❌ **Unhealthy Threshold** | If a previously healthy instance **fails 2 checks in a row**, it's marked unhealthy and **removed** from traffic rotation |
| ✅ **Healthy Threshold** | An instance must **pass 2 checks in a row** before it's trusted again and **added back** to traffic rotation |

---

## 6️⃣ Adding EC2 Instances (Targets) 🎯

```
Select the EC2 instances the load balancer should distribute traffic to
→ Choose both running instances
```

> ✅ These become the **backend targets** the Classic Load Balancer will route requests to.

---

## 7️⃣ Cross-Zone Load Balancing 🗺️

| Concept | Explanation |
|---|---|
| 🌐 **Cross-Zone Load Balancing** | Allows the load balancer to distribute traffic **evenly across instances in different Availability Zones** |
| 📌 **Example** | One instance in `ap-south-1a`, another in `ap-south-1b` — cross-zone load balancing ensures traffic is spread evenly across **both AZs**, not just within one |

> ✅ **Enabled** in this demo.

---

## 8️⃣ Connection Draining 🚰

> 🎯 **Problem It Solves:** When an instance is marked **unhealthy**, it might still be **actively processing** some in-flight requests. Terminating it immediately would **cause those requests to fail**.

| Setting | Value |
|---|---|
| Connection Draining | ✅ Enabled |
| Draining Timeout | **300 seconds** |

### How It Works

```
1. Health check fails → instance marked unhealthy
2. Load balancer STOPS sending NEW requests to this instance
3. Load balancer WAITS UP TO 300 seconds for in-flight requests to complete
4. After draining completes (or timeout expires) → instance is fully removed
```

> 💡 **Key Benefit:** Prevents **dropped requests** during instance removal — a graceful, zero-disruption approach.

---

## 9️⃣ Tags & Final Review 🏷️

| Tag Key | Value |
|---|---|
| `Environment` | `Dev` |

### ✅ Final Configuration Review

| Setting | Value |
|---|---|
| Listener Port | 80 |
| Health Check | HTTP, port 80, path `/` |
| Response Timeout | 5 seconds |
| Health Check Interval | 20 seconds |
| Healthy/Unhealthy Threshold | 2 / 2 |
| Cross-Zone Load Balancing | ✅ Enabled |
| Connection Draining | ✅ Enabled (300 seconds) |

```
Click "Create"
```

> ⏳ Load balancer creation is initiated — it will take **a little while** to fully provision and become active.

---

## 📋 Quick Reference: Classic LB Setup Steps

| Step | Action |
|---|---|
| 1 | Ensure 2+ EC2 instances are running |
| 2 | EC2 → Load Balancers → Create → Classic Load Balancer |
| 3 | Configure listener (port 80, HTTP) |
| 4 | Create a **dedicated** security group for the load balancer |
| 5 | Configure health check (path, timeout, interval, thresholds) |
| 6 | Add EC2 instances as targets |
| 7 | Enable Cross-Zone Load Balancing |
| 8 | Enable Connection Draining |
| 9 | Add tags → Review → Create |

---

## ✅ Final Takeaways

```
🛡️ SEPARATE SECURITY GROUP → Use a dedicated SG for load balancers, not shared with EC2 instances
🏥 HEALTH CHECK            → Path, interval, timeout, healthy/unhealthy thresholds all configurable
🗺️ CROSS-ZONE BALANCING    → Spreads traffic evenly across instances in different AZs
🚰 CONNECTION DRAINING     → Gives in-flight requests time to finish before removing an unhealthy instance
🏷️ TAGS                    → Load balancers support tags too, just like EC2 instances
```

> 🎯 **Golden Rule:** Health checks and connection draining work together to ensure **graceful, zero-disruption** traffic management — critical concepts for the exam and for real production systems.

> ➡️ **Next Up:** Verifying the Classic Load Balancer is working and testing failover behavior!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
