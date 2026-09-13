![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 58: Cleaning Up — Deleting ALB, ASG & Target Groups

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing & Auto Scaling
> ⏱️ **Type:** Hands-On Demo (Cost Management / Cleanup)

---

## 🎯 What This Lecture Is About

```
1️⃣ Why Cleanup Matters
2️⃣ Step 1: Delete the Application Load Balancer
3️⃣ Step 2: Delete the Auto Scaling Group
4️⃣ Step 3: Delete the Target Groups
5️⃣ Recommended Deletion Order (and Why)
```

---

## 1️⃣ Why Cleanup Matters 💰

> 🔗 **Callback to earlier lesson:** Remember our cost management recommendations — **stop/terminate resources you're not using**. Load balancers, target groups, and auto scaling groups (and the EC2 instances they manage) all **incur costs** while active.

> 🎯 **Goal of this step:** Cleanly remove **everything** related to the Application Load Balancer setup we built in the previous lectures.

---

## 2️⃣ Step 1: Delete the Application Load Balancer 🗑️

```
EC2 → Load Balancers → Select "my-application-load-balancer" → Actions → Delete
```

> ✅ **Result:** The Application Load Balancer is deleted **immediately**.

---

## 3️⃣ Step 2: Delete the Auto Scaling Group 🗑️

```
EC2 → Auto Scaling Groups → Select the ASG → Actions → Delete
```

### ⏳ What Happens Behind the Scenes

| Step | Detail |
|---|---|
| 1️⃣ | ASG deletion is requested |
| 2️⃣ | AWS **waits** for the associated **EC2 instances to be terminated first** |
| 3️⃣ | Once instances are fully terminated, the **ASG itself is deleted** |

> 💡 **Why the Wait?** Remember — the ASG's entire job is to **ensure a specific number of instances are always running**. Before AWS can safely delete the ASG, it must first **terminate the instances** it's been maintaining. This took about **a minute** in the demo.

> ⚠️ **Order Matters:** You cannot simply terminate the EC2 instances manually and expect the ASG to disappear — deleting the **ASG itself** is what triggers the proper termination and cleanup sequence.

---

## 4️⃣ Step 3: Delete the Target Groups 🗑️

```
EC2 → Target Groups → Select target group(s) → Delete → Confirm (Yes)
```

> ✅ **Result:** All associated target groups (e.g., `my-target-group`, `microservice-a-target-group`) are removed.

---

## 5️⃣ Recommended Deletion Order (and Why) 📋

| Order | Resource | Why This Order? |
|---|---|---|
| 1️⃣ | 🗑️ **Load Balancer** | Nothing depends on it being deleted first — safe to remove immediately |
| 2️⃣ | 🗑️ **Auto Scaling Group** | Must be deleted **before** target groups, since it may still reference them; also handles instance termination internally |
| 3️⃣ | 🗑️ **Target Groups** | Deleted last, once nothing (load balancer or ASG) references them anymore |

> 💡 **General Principle:** When cleaning up interconnected AWS resources, **delete "downstream" consumers first**, then work your way to the "foundational" resources they depended on.

---

## 📋 Quick Reference: Full Cleanup Checklist

| # | Resource | Action |
|---|---|---|
| 1 | Application Load Balancer | Delete |
| 2 | Auto Scaling Group | Delete (waits for instance termination automatically) |
| 3 | Target Group(s) | Delete |
| 4 | *(Optional)* Security Groups created for these resources | Consider deleting if no longer needed |
| 5 | *(Optional)* Launch Templates | Can be kept for reuse, or deleted if no longer needed |

---

## ✅ Final Takeaways

```
🗑️ DELETE ORDER      → Load Balancer → Auto Scaling Group → Target Groups
⏳ ASG DELETION WAITS → AWS automatically terminates the managed EC2 instances first
💰 COST DISCIPLINE    → Regularly clean up unused ALB/ASG/Target Group resources to avoid ongoing charges
🔗 DEPENDENCY AWARE   → Delete resources that DEPEND on others before deleting the resources they depend on
```

> 🎯 **Golden Rule:** Deleting an Auto Scaling Group isn't instantaneous — it **safely terminates its managed instances first**. Don't be alarmed by the short delay; that's the ASG doing its job correctly, even during teardown.

> ➡️ **Next Up:** Moving to the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
