![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 87: EC2 & ELB — Key Architectural Considerations

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** EC2 & ELB for Architects
> ⏱️ **Type:** Section Wrap-Up (⭐ Ties together everything from this and prior sections!)

---

## 🎯 What This Lecture Is About

> 🎬 This is one of the **final** lectures tying together everything we've learned about EC2 and ELB — viewed through **four key architectural lenses**.

```
1️⃣ Security
2️⃣ Performance
3️⃣ Cost Efficiency
4️⃣ Resiliency
```

> 🔗 **Recall Lecture 63:** This is exactly the "architect mindset" framework introduced at the start of this section — now we're pulling it all together.

---

## 1️⃣ Security 🔒

| Consideration | How to Achieve It |
|---|---|
| 🛡️ **Restrict Traffic** | Use **Security Groups** to control inbound/outbound traffic |
| 🔐 **Network-Level Restriction** | Use **NACLs** (Network Access Control Lists) — covered in more depth when we get to VPCs |
| 🏠 **Hide Instances from the Internet** | Place EC2 instances in **Private Subnets** |
| 📜 **Regulatory/Compliance Needs** | Use **Dedicated Hosts** or **Dedicated Instances** when regulations require dedicated hardware |

### 🎓 Quick Preview: Private Subnets

> 💡 **Core Idea:** If an EC2 instance is in a **private subnet**, it's **not accessible from outside the AWS network** — a powerful way to protect backend resources (like databases) that should never be directly internet-facing. *(Full VPC/subnet details covered later in the course.)*

---

## 2️⃣ Performance 🏎️

| Consideration | How to Achieve It |
|---|---|
| 🏷️ **Right Instance Family** | Choose the family (m/t/c/r/i/g/f/inf) that matches your **compute/memory/disk/network** needs |
| 📍 **Right Placement Group** | Cluster (low latency), Spread (high availability), or Partition (hybrid) — matched to your workload |
| 💾 **Custom AMI** | Pre-install software into a custom AMI to ensure **faster instance launch times** |
| ⚖️ **Right Load Balancer** | Choose based on your performance needs: |

```
Need EXTREME performance?  → Network Load Balancer (NLB)
Need HIGH FLEXIBILITY?      → Application Load Balancer (ALB)
```

---

## 3️⃣ Cost Efficiency 💰

| Consideration | How to Achieve It |
|---|---|
| 🔢 **Optimal Instance Count & Type** | Use the **right instance family**, and ensure you're not over/under-provisioned |
| 📈 **Auto Scaling Groups** | Ensure the **right number** of EC2 instances are running at all times — matching actual demand |
| 🧩 **Right Pricing Model Mix** | Combine **Savings Plans, Reserved Instances, On-Demand, and Spot Instances** strategically |

### 🎯 Example: Mixing Pricing Models for a Single Workload

> 💡 **Scenario:** Processing messages from a queue.

```
✅ A FEW Reserved Instances → handle the BASELINE, steady-state load (always running)
✅ ADDITIONAL Spot Instances → launched on-demand when the queue backlog grows beyond baseline capacity
```

> 🎯 **Key Insight:** You don't have to choose **one** pricing model for an entire workload — **mixing models strategically** (reserved for baseline + spot for burst) is a common, cost-effective real-world pattern.

> 📌 **General Principle:** Always evaluate **what mix** of Savings Plans / Reserved / On-Demand / Spot fits YOUR specific use case — there's no single "correct" answer, it depends on your workload's predictability and criticality.

---

## 4️⃣ Resiliency 🚑

> ❓ **What is Resiliency?** *How quickly can you recover from failures?*

| Consideration | How to Achieve It |
|---|---|
| 🏥 **Proper Health Checks** | Configure health checks that actually **detect real problems** |
| 📊 **Auto Scaling Groups** | Automatically **replace failed instances** (self-healing) |
| 👁️ **CloudWatch Monitoring** | Continuously monitor to **detect issues early** |
| 🌍 **Disaster Recovery: Multi-Region AMIs** | Keep **up-to-date AMIs copied across multiple regions** |

### 🎯 Why Multi-Region AMI Backups Matter

> 💡 **Scenario:** An entire AWS **region** becomes unavailable (rare, but possible).

```
If your AMI is ONLY in the affected region → You're stuck, can't launch replacement instances
If your AMI is ALSO copied to another region → You can launch instances there and recover quickly
```

> 🔗 **Recall:** This connects directly back to the AMI lecture, where we discussed **copying AMIs across regions** as a disaster recovery best practice.

---

## 📋 Master Summary Table

| Pillar | Key Tools/Practices |
|---|---|
| 🔒 **Security** | Security Groups, NACLs, Private Subnets, Dedicated Hosts/Instances |
| 🏎️ **Performance** | Right instance family, right placement group, custom AMI, right load balancer type |
| 💰 **Cost Efficiency** | Right instance count/type, Auto Scaling, strategic pricing model mix |
| 🚑 **Resiliency** | Health checks, Auto Scaling (self-healing), CloudWatch, multi-region AMI backups |

---

## ✅ Final Takeaways

```
🔒 SECURITY       → Security Groups + NACLs + Private Subnets + Dedicated Hosts (for compliance)
🏎️ PERFORMANCE     → Right instance family + placement group + custom AMI + right ELB type
💰 COST            → Right-size instances + Auto Scaling + MIX pricing models strategically
🚑 RESILIENCY      → Health checks + Auto Scaling self-healing + CloudWatch + multi-region AMI backups
🎯 ARCHITECT LENS  → Every EC2/ELB decision should be evaluated against these 4 pillars together
```

> 🎯 **Golden Rule:** Great architecture isn't about maximizing just ONE of these four pillars — it's about finding the **right balance** between security, performance, cost, and resiliency for your **specific** workload's requirements. This is the essence of the "architect mindset" this whole section has been building toward.

> ➡️ **Next Up:** Moving into the next major section of the course — likely VPCs, storage, or serverless services!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
