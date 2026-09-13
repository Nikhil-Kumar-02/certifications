![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 48: Load Balancer Listeners Deep Dive

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing
> ⏱️ **Type:** Concept + Hands-On Demo

---

## 🎯 What This Lecture Is About

```
1️⃣ What is a Listener?
2️⃣ Multiple Listeners on One Load Balancer
3️⃣ Example: A Multi-Listener Setup
4️⃣ Hands-On: Adding a New Listener with a Fixed Response
5️⃣ Troubleshooting: Timeout on the New Port
6️⃣ Fixing the Security Group
```

---

## 1️⃣ What is a Listener? 👂

| Concept | Explanation |
|---|---|
| 👂 **Listener** | Checks for **connection requests** from clients, based on a configured **protocol** and **port** |
| 🔀 **Routing Role** | Each listener has a **protocol**, a **port**, and a **set of rules** to route requests to a target group |

### Example

```
Listener: HTTP, Port 80 → Routes to → Target Group A
```

```
EC2 → Load Balancers → Select ALB → Listeners tab
```
> 📌 Here you can see the currently configured listener(s) and which target group(s) they route to.

---

## 2️⃣ Multiple Listeners on One Load Balancer 🔢

> 💡 **Key Feature:** A single load balancer can have **multiple listeners**, each listening on a **different port or protocol**.

### Example Setup

| Listener | Protocol | Port | Action |
|---|---|---|---|
| 1 | HTTP | 80 | Route to a specific Target Group |
| 2 | HTTPS | 443 | Redirect to HTTP port 80 |
| 3 | HTTP | 8080 | Return a **fixed response** |

> 🎯 This flexibility lets a **single load balancer** handle multiple traffic patterns simultaneously.

---

## 3️⃣ Example: A Multi-Listener Setup 🗂️

```
Load Balancer
   ├── Listener: HTTP:80    → Forward to Target Group
   ├── Listener: HTTPS:443  → Redirect to HTTP:80
   └── Listener: HTTP:8080  → Return Fixed Response
```

> 💡 A "fixed response" means the load balancer answers **directly**, without forwarding the request to any backend target at all.

---

## 4️⃣ Hands-On: Adding a New Listener with a Fixed Response 🧪

### Step 1: Add a New Listener

```
Load Balancer → Listeners → Add listener
```

| Setting | Value |
|---|---|
| Protocol | HTTP |
| Port | **8080** |
| Action | **Return fixed response** |

### Step 2: Configure the Fixed Response

| Setting | Value |
|---|---|
| Response Code | 200 |
| Content Type | `text/plain` or `text/html` |
| Response Body | Custom text/HTML, e.g.: |

```html
<html>
<title>Success</title>
<body>Hello from Ranga</body>
</html>
```

```
Save
```

> ✅ Now the load balancer has **two listeners**:
> - HTTP:80 → forwards to a target group
> - HTTP:8080 → returns a fixed response directly

---

## 5️⃣ Troubleshooting: Timeout on the New Port ⏳

### Testing the New Listener

```
<ALB-DNS-Name>:8080
```

> ⚠️ **Result: Timeout!**

> 🎯 **Reminder:** Whenever you see a **timeout**, the first thing to check is the **security group**.

---

## 6️⃣ Fixing the Security Group 🔧

### The Problem

> The ALB's security group (`application-load-balancer-sg`) only allows inbound traffic on **port 80** — **port 8080 was never opened**.

### The Fix

```
EC2 → Security Groups → application-load-balancer-sg → Edit inbound rules
→ Add rule: Allow port 8080 from Anywhere
→ Save rules
```

### ✅ Verification

```
Refresh <ALB-DNS-Name>:8080
→ Result: "Hello from Ranga" 🎉
```

> 🎯 **Key Lesson:** Adding a new **listener** on the load balancer is **not enough** — you must **also** update the security group to allow traffic on that new port!

---

## 📋 Quick Reference

| Concept | Key Fact |
|---|---|
| Listener | Protocol + Port + routing rules |
| Multiple Listeners | ✅ Supported — different ports/protocols on the same LB |
| Fixed Response | LB answers directly, without hitting a backend target |
| ⭐ Common Pitfall | New listener won't work until the **security group** also allows that port |

---

## ✅ Final Takeaways

```
👂 LISTENER        → Protocol + Port combination the LB listens on
🔢 MULTIPLE LISTENERS → One LB can serve many ports/protocols simultaneously
🎯 FIXED RESPONSE   → LB can respond directly without forwarding to a target
⏳ TIMEOUT LESSON   → New listener + missing security group rule = timeout
🔧 TWO-STEP SETUP   → 1) Add the listener  2) Open the port in the security group
```

> 🎯 **Golden Rule:** Whenever you add a new listener on a **new port**, always remember to **update the security group** to allow that port — otherwise you'll hit a timeout, even though the listener itself is configured correctly.

> ➡️ **Next Up:** Deep dive into Target Groups — deregistration delay, slow start, load balancing algorithms, and sticky sessions!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
