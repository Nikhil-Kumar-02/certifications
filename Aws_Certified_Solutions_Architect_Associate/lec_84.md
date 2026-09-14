![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 84: Monitoring & Troubleshooting Load Balancers — Access Logs & Request Headers

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS Load Balancing — Advanced Topics
> ⏱️ **Type:** Concept + Console Walkthrough (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Access Logs: What They Capture
2️⃣ Enabling Access Logs (ALB & NLB)
3️⃣ ⭐ Key Difference: How Each Load Balancer Passes Client IP
4️⃣ ALB's X-Forwarded-* Headers Explained
5️⃣ CloudWatch Monitoring for Load Balancers
```

---

## 1️⃣ Access Logs: What They Capture 📜

> 🎯 **Access Logs** let you enable detailed **tracing** of every request that flows through your Elastic Load Balancer.

### What Gets Captured

| Data Point | Description |
|---|---|
| ⏱️ **Timestamp** | When the request was received |
| 🌍 **Client IP Address** | Who sent the request |
| ⏳ **Latency** | How long the request took to execute |
| 🛣️ **Request Path** | The URL path that was requested |
| 📤 **Response** | What the server sent back |

---

## 2️⃣ Enabling Access Logs (ALB & NLB) ⚙️

### For Application Load Balancer

```
ALB → Actions → Edit attributes → Enable "Access Logs"
→ Configure S3 bucket destination
```

### For Network Load Balancer

```
NLB → Actions → Edit attributes → Enable "Access Logs"
→ Configure S3 bucket destination
```

> 💾 **Storage:** Both ALB and NLB store their access logs in **Amazon S3** (AWS's object storage service).

> ✅ **Result:** Once enabled, you can trace **every single request** flowing in and out of your load balancer — invaluable for debugging and auditing.

---

## 3️⃣ ⭐ Key Difference: How Each Load Balancer Passes Client IP 🌍

> 🚨 **This is a critical, frequently-tested distinction between NLB and ALB!**

| Load Balancer | How Client IP Reaches the EC2 Instance |
|---|---|
| 🌐 **Network Load Balancer (NLB)** | ✅ **Passes the client IP address DIRECTLY** — the EC2 instance sees the real client IP as the source IP of the connection |
| ⚖️ **Application Load Balancer (ALB)** | ❌ **Does NOT** pass the client IP directly — you must look inside a specific **request header** instead |

> 💡 **Why the Difference?** NLB operates at **Layer 4** (Transport), preserving the original TCP connection characteristics more directly. ALB operates at **Layer 7** (Application) and works as more of a true **proxy**, which means the direct connection to the EC2 instance actually originates **from the ALB itself** — requiring header-based workarounds to convey the original client's info.

---

## 4️⃣ ALB's X-Forwarded-* Headers Explained 📋

> Since ALB doesn't pass the client IP directly, it includes this (and other) information in **special HTTP request headers** sent to the backend EC2 instance.

| Header | Contains |
|---|---|
| 🌍 **X-Forwarded-For** | The **original client's IP address** |
| 🔒 **X-Forwarded-Proto** | The **protocol** used by the client (HTTP or HTTPS) |
| 🚪 **X-Forwarded-Port** | The **originating port** of the client's request |

### 📋 Quick Reference

```
X-Forwarded-For    → "Who sent this?" (client IP)
X-Forwarded-Proto  → "How did they connect?" (HTTP/HTTPS)
X-Forwarded-Port    → "Which port did they use?"
```

> ⭐ **Exam Tip:** If a question asks *"how do I get the original client's IP address when using an Application Load Balancer?"* → the answer is: **look at the `X-Forwarded-For` header** — it's NOT available as the direct connection source IP the way it is with NLB.

---

## 5️⃣ CloudWatch Monitoring for Load Balancers 📊

> 🔗 **Recall:** Just like every other AWS resource we've covered, Elastic Load Balancers are monitored via **CloudWatch**.

```
Load Balancer → Monitoring tab
```

### Metrics Available

| Metric | What It Shows |
|---|---|
| ⏱️ **Response Times** | How long requests are taking |
| 📥 **Request Count** | Total incoming requests |
| ❌ **Error Count** | Requests that resulted in errors |
| ✅ **Successful Request Count** | Requests completed successfully |
| 🔗 **Active Connections** | Number of connections currently maintained by the load balancer |

---

## 📋 Master Summary Table

| Concept | Key Fact |
|---|---|
| Access Logs | Capture timestamp, client IP, latency, path, response — enabled via "Edit attributes" |
| Log Storage | Amazon S3 |
| NLB Client IP | Passed **directly** to the EC2 instance |
| ALB Client IP | **NOT** passed directly — found in the `X-Forwarded-For` header |
| ALB Extra Headers | `X-Forwarded-Proto` (protocol), `X-Forwarded-Port` (port) |
| Monitoring Tool | CloudWatch (response times, request counts, errors, connections) |

---

## ✅ Final Takeaways

```
📜 ACCESS LOGS       → Enable via "Edit attributes"; stored in S3; captures full request details
🌐 NLB CLIENT IP      → Passed directly to the EC2 instance (Layer 4 behavior)
⚖️ ALB CLIENT IP ⭐    → NOT passed directly — check the X-Forwarded-For header instead
📋 X-FORWARDED-* HEADERS → For (IP), Proto (HTTP/HTTPS), Port (originating port)
📊 CLOUDWATCH         → Standard monitoring for response times, requests, errors, connections
```

> 🎯 **Golden Rule:** Remember the NLB vs ALB client IP distinction as a pair: **NLB = direct pass-through** (Layer 4), **ALB = must check `X-Forwarded-For` header** (Layer 7, acts as a true proxy). This is one of the most commonly tested ALB/NLB behavioral differences.

> ➡️ **Next Up:** More advanced Elastic Load Balancer and EC2 topics!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
