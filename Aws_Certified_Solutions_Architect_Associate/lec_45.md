![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 45: Verifying the Classic Load Balancer & Exploring the Console

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Confirming the Load Balancer Is Active
2️⃣ Testing Load Distribution via DNS Name
3️⃣ Exploring the Load Balancer Console Tabs
4️⃣ Managing Instances & Availability Zones
5️⃣ Monitoring Tab Deep Dive
6️⃣ Other Available Actions
7️⃣ Cleaning Up: Deleting the Load Balancer
```

---

## 1️⃣ Confirming the Load Balancer Is Active ✅

> After creation completes, the Classic Load Balancer shows:

```
Status: 2 of 2 instances in service
```

> ✅ Both EC2 instances have **passed their health checks** and are actively receiving traffic.

---

## 2️⃣ Testing Load Distribution via DNS Name 🌐

> Instead of using an EC2 instance's public IP directly, we now access the app through the **Load Balancer's DNS name**.

### Test: Refresh Multiple Times

```
Request 1 → Response from Instance ending in "d0" (Private IP ending in ...39)
Request 2 → Response from Instance ending in "0f" (Private IP ending in ...41.189)
Request 3 → Response switches back and forth between instances
```

> 🎯 **Confirmed:** The Classic Load Balancer is **successfully distributing traffic** between both EC2 instances — each refresh may hit a **different backend instance**.

> 💡 **How to verify:** Compare the **Instance ID** and **Private IP** shown in each response against the actual EC2 instance details in the console.

---

## 3️⃣ Exploring the Load Balancer Console Tabs 🔍

### 📄 Description Tab

| Field | Value Shown |
|---|---|
| Name | `my-classic-load-balancer` |
| Type | Classic |
| Scheme | Internet-facing (public) |
| Cross-Zone Load Balancing | ✅ Enabled |
| Instances in Service | 2 of 2 |

> 📌 Cross-zone load balancing means this load balancer is capable of distributing traffic across **all Availability Zones** in the region — not just the one(s) currently hosting instances.

---

## 4️⃣ Managing Instances & Availability Zones ⚙️

### Instances Tab

| Action | How |
|---|---|
| ➕ View registered instances | See which EC2 instances are currently targets |
| ➖ Remove an instance | Deregister it directly from this tab — it will stop receiving traffic |

### Availability Zones

| Action | How |
|---|---|
| ➖ Remove an AZ | Stop load balancing to instances in a specific AZ, directly from the console |

> 💡 **Flexibility:** You can fine-tune exactly which instances and which AZs participate in load balancing, without recreating the load balancer.

---

## 5️⃣ Monitoring Tab Deep Dive 📊

> The **Monitoring** tab surfaces key **CloudWatch metrics** about the load balancer's behavior.

### 📋 Key Metrics Available

| Metric | What It Tells You |
|---|---|
| ✅ **Healthy Host Count** | Number of instances currently passing health checks |
| ❌ **Unhealthy Host Count** | Number of instances currently failing health checks |
| ⏱️ **Latency** | How long requests are taking |
| 📥 **Request Count** | Total number of incoming requests |
| ✅ **Successful Requests** | Requests completed without error |
| ❌ **Failed Requests** | Requests that resulted in errors |
| 🔗 **Active Connections** | Number of connections currently maintained by the load balancer |

> 💡 **Real Observation from the Demo:** Initially, both instances showed as **unhealthy** right after creation (health checks hadn't run yet) — after a short wait, they transitioned to **healthy** once checks started passing.

> 🎯 **Why This Matters:** This tab is your **go-to dashboard** for understanding load balancer health and performance in real time — extremely useful for troubleshooting in production.

---

## 6️⃣ Other Available Actions 🛠️

| Tab/Action | Purpose |
|---|---|
| 🏷️ **Tags** | View/edit tags assigned to the load balancer |
| 🔄 **Migration** | Migrate this Classic Load Balancer to an **Application Load Balancer** |
| ✏️ **Edit Health Check** | Modify health check settings after creation |
| ✏️ **Edit Instances** | Add/remove backend EC2 instances |
| ✏️ **Edit Listeners** | Add additional listeners (e.g., add an HTTPS listener alongside HTTP) |
| ✏️ **Edit Security Groups** | Change which security groups are attached |
| 🗑️ **Delete** | Permanently remove the load balancer |

> 💡 **Migration Tab Callout:** Since Classic Load Balancer is **legacy**, AWS provides a **built-in migration path** to move to ALB — reinforcing that CLB is meant to be phased out.

---

## 7️⃣ Cleaning Up: Deleting the Load Balancer 🗑️

> Since we're moving on to other load balancer types next, we **don't need to keep this Classic Load Balancer around**.

```
Select Load Balancer → Actions → Delete → Confirm
```

> ✅ **Result:** The Classic Load Balancer is successfully deleted.

> 💰 **Cost Reminder:** Just like EC2 instances, **load balancers incur costs while running** — deleting resources you're not actively using is good practice (tying back to our earlier cost management lecture!).

---

## 📋 Quick Reference: Console Tabs Summary

| Tab | Purpose |
|---|---|
| Description | Basic config overview (type, scheme, cross-zone setting) |
| Instances | View/add/remove backend EC2 instances |
| Health Check | View/edit health check configuration |
| Listeners | View/edit ports and protocols the LB listens on |
| Monitoring | CloudWatch metrics — health, latency, request counts |
| Tags | Resource tagging |
| Migration | Path to upgrade CLB → ALB |

---

## ✅ Final Takeaways

```
🌐 DNS NAME TEST    → Refreshing via the LB's DNS shows responses from DIFFERENT EC2 instances
📊 MONITORING TAB   → Real-time health, latency, and request metrics via CloudWatch
🔄 MIGRATION TAB    → Built-in path to move from CLB → ALB (reinforcing CLB is legacy)
🛠️ FULL EDITABILITY → Health checks, instances, listeners, and security groups can all be modified post-creation
🗑️ CLEAN UP         → Delete unused load balancers to avoid ongoing charges
```

> 🎯 **Golden Rule:** A working load balancer should show **different backend instance details** on repeated requests — that's your simplest, fastest way to confirm load balancing is actually happening.

> ➡️ **Next Up:** Creating an Application Load Balancer (ALB)!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
