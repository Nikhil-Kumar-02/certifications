![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 54: Auto Scaling Group in Action — Self-Healing & Manual Scaling

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing & Auto Scaling
> ⏱️ **Type:** Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ Confirming ASG Instances Are Live
2️⃣ Verifying Traffic Distribution
3️⃣ Testing Self-Healing: Terminating an Instance
4️⃣ Reviewing the Activity Tab
5️⃣ Manual Scaling: Adjusting Desired Capacity
6️⃣ Observing Connection Draining During Scale-In
```

---

## 1️⃣ Confirming ASG Instances Are Live ✅

> ⏳ After about **5 minutes**, the new ASG-launched instances show:

```
Instance State: Running
Status Checks: Passed
```

---

## 2️⃣ Verifying Traffic Distribution 🌐

```
Copy Load Balancer's public DNS → Refresh repeatedly
```

### Instance Placement Observed

| Instance | Availability Zone | Private IP (example) |
|---|---|---|
| ASG Instance 1 | `ap-south-1a` | `...253` |
| ASG Instance 2 | `ap-south-1b` | `...135` |

> ✅ **Confirmed:** Both new ASG-launched instances are **receiving traffic** from the load balancer — refreshing shows responses alternating between `...253` and `...135`.

> 🎯 **Key Insight:** We didn't manually register these instances with the target group — the **ASG did it automatically** because we connected it to `my-target-group` during creation!

---

## 3️⃣ Testing Self-Healing: Terminating an Instance 💥

> 🧪 **Experiment:** Manually terminate one of the ASG-launched instances and observe what happens.

```
Select an ASG instance → Actions → Instance State → Terminate
```

### ⏳ What Happened

| Time | Event |
|---|---|
| T+0 min | Instance manually terminated |
| T+5-6 min | 🆕 **A replacement instance automatically launched** |

> 🎉 **This demonstrates the ASG's core self-healing behavior:** Since the ASG is configured to maintain a **desired capacity of 2**, losing one instance triggered the automatic launch of a **replacement**.

> 💡 **Why the delay?** Launching a new instance takes time — booting the OS, running User Data / AMI setup, passing health checks, etc.

---

## 4️⃣ Reviewing the Activity Tab 📜

```
Auto Scaling Group → Activity tab
```

> 📊 This tab is your **audit log** for everything the ASG has done.

### Example Activity Log Entries

| Event | Detail |
|---|---|
| ❌ Launch attempt failed | An initial launch attempt failed (can happen occasionally) |
| ✅ Launch succeeded | A subsequent attempt succeeded — instances became available |
| 🗑️ Instance terminated | The manual termination we triggered |
| ⏳ Waiting for connection draining | ASG waited for in-flight requests to complete before fully removing the instance |
| 🆕 New instance launched | Replacement instance spun up to restore desired capacity |

> 🎯 **Why This Matters:** If you're ever confused about **why** the ASG did (or didn't do) something, the **Activity tab** is your first stop for troubleshooting.

---

## 5️⃣ Manual Scaling: Adjusting Desired Capacity ✋

> 💡 Beyond automatic scaling policies, you can also **manually** adjust the number of running instances.

```
Auto Scaling Group → Details → Edit → Desired Capacity
```

### Test: Reduce Desired Capacity

```
Change Desired Capacity: 2 → 1
→ Update
```

### ⏳ What Happened

```
Activity tab → Refresh
Result: An instance is being TERMINATED (scale-in triggered immediately)
```

> ✅ **Confirmed:** The ASG **immediately reacted** to the manual change, reducing the running instance count from 2 to 1.

---

## 6️⃣ Observing Connection Draining During Scale-In 🚰

```
Target Groups → my-target-group → Targets tab
```

### Target Status After Scale-In

| Instance | Status |
|---|---|
| Instance A | ✅ **Healthy** (still receiving traffic) |
| Instance B | 🔄 **Draining** (being gracefully removed) |

> 💡 **What "Draining" Means:** The instance that's being scaled down is **not immediately killed** — it's given time to **finish any in-flight requests** (connection draining, covered earlier with target groups) before being fully deregistered and terminated.

---

## 📋 Quick Reference: What We Tested

| Test | Result |
|---|---|
| Load balancing across ASG instances | ✅ Confirmed — traffic alternates between instances |
| Terminate an instance manually | ✅ ASG auto-launched a replacement (~5-6 min) |
| Reduce desired capacity | ✅ ASG immediately began scaling in |
| Scale-in behavior | ✅ Instance enters "Draining" state before removal |

---

## ✅ Final Takeaways

```
🌐 AUTO-REGISTRATION → ASG instances automatically join the target group — no manual step needed
💥 SELF-HEALING       → Terminating an instance triggers automatic replacement (~5-6 min)
📜 ACTIVITY TAB       → Your audit log for everything the ASG has done — great for troubleshooting
✋ MANUAL SCALING      → Changing Desired Capacity directly triggers immediate scale in/out
🚰 GRACEFUL SCALE-IN  → Removed instances enter "Draining" status, not an abrupt kill
```

> 🎯 **Golden Rule:** An Auto Scaling Group doesn't just launch instances — it **continuously monitors and corrects** the running instance count to match your configuration, whether that correction comes from a **failure** (self-healing) or a **manual change** (desired capacity adjustment).

> ➡️ **Next Up:** Reviewing all the Auto Scaling Group components and use cases in depth!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
