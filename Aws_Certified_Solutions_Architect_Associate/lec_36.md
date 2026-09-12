![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🎯 Lecture 36: EC2 Scenarios & Exam Trivia Review

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** AWS EC2 — Virtual Servers in the Cloud
> ⏱️ **Type:** Scenario-Based Review (⭐ Frequently tested in the exam!)

---

## 🎯 What This Lecture Is About

```
1️⃣ Storage Costs Continue Even When Stopped
2️⃣ Best Practice: Terminate, Don't Just Stop
3️⃣ Scenario: Grouping Instances by Project/Environment
4️⃣ Scenario: Changing Instance Type
5️⃣ Scenario: Termination Protection
6️⃣ Scenario: Updating to a New AMI
7️⃣ Scenario: Migrating On-Premises VMs
8️⃣ Scenario: Changing Security Groups
9️⃣ Scenario: Connection Timeouts
🔟 Scenario: Slow Boot Due to User Data
1️⃣1️⃣ Scenario: Billing for Stopped Instances
```

---

## 1️⃣ Storage Costs Continue Even When Stopped 💾💰

> 🚨 **Key Insight:** Even after **stopping** an EC2 instance, its attached **EBS volumes (hard disks)** remain **in use** and continue to be **billed**.

```
EC2 Console → Elastic Block Store → Volumes
```
> All volumes show as **"in-use"** even for stopped instances.

---

## 2️⃣ Best Practice: Terminate, Don't Just Stop 🧹

| Action | Billed for Compute? | Billed for Storage? |
|---|---|---|
| ⏸️ **Stop** | ❌ No | ✅ **Yes** (storage still attached) |
| ❌ **Terminate** | ❌ No | ❌ No (storage released too) |

> ✅ **Best Practice:** **Terminate** EC2 instances you're no longer using — this is the only way to stop **all** associated billing (compute + storage).

---

## 3️⃣ Scenario: Grouping Instances by Project/Environment 🏷️

> ❓ *"How do I identify all instances belonging to a project, environment, or billing type?"*

✅ **Answer:** Use **Tags**! Add tags like:

| Tag Key | Example Value |
|---|---|
| `Project` | Project A |
| `Environment` | Dev |
| `BusinessUnit` | Business Unit ABC |

> 💡 Tags can be applied to **any AWS resource** — instances, AMIs, volumes, etc.

---

## 4️⃣ Scenario: Changing Instance Type ⚙️

> ❓ *"Can I change the instance type of a running EC2 instance?"*

❌ **Answer: No.**

### ✅ Correct Process

```
1. Stop the instance (Actions → Instance State → Stop)
2. Wait ~30 seconds for it to fully stop
3. Actions → Instance Settings → Change Instance Type
4. Select new type (e.g., t2.micro → t2.medium)
5. Start the instance again
```

> 📌 The **"Change Instance Type"** option is **disabled** while the instance is running.

---

## 5️⃣ Scenario: Termination Protection 🛡️

> ❓ *"How do I prevent an EC2 instance from being accidentally terminated?"*

✅ **Answer:** Enable **Termination Protection**.

### Enable on a Running Instance

```
Select Instance → Actions → Instance Settings → Change Termination Protection → Enable
```

### Enable During Launch

```
Launch Instance → Configure Instance Details → check "Termination Protection"
```

### To Terminate a Protected Instance

```
Actions → Instance Settings → Change Termination Protection → Disable
→ Then: Actions → Instance State → Terminate
```

### ⚠️ Important Limitations

| Termination Protection Does NOT Protect Against |
|---|
| ❌ Auto Scaling Group deciding to terminate the instance |
| ❌ Spot Instance reclamation/termination |
| ❌ OS-level shutdown (e.g., someone SSHs in and runs `shutdown`) |

---

## 6️⃣ Scenario: Updating to a New AMI 💾

> ❓ *"I created a new AMI with updated patches — can I update a running instance to use it?"*

❌ **Answer: No.** You **cannot** change the AMI of a running EC2 instance.

✅ **Correct Process:**
```
1. Terminate the existing instance
2. Launch a NEW instance using the new AMI
```

---

## 7️⃣ Scenario: Migrating On-Premises VMs 🏢➡️☁️

> ❓ *"I have virtual machine images from my on-premises data center — can I use them to create EC2 instances?"*

✅ **Answer: Yes** — using **VM Import/Export**, a tool provided by AWS.

| ⚠️ Important Note |
|---|
| **You are fully responsible for software licenses** (e.g., Oracle, Windows, etc.) on the imported VM image |

---

## 8️⃣ Scenario: Changing Security Groups 🔄

> ❓ *"Can I change the security group on a running EC2 instance?"*

✅ **Answer: Yes** — at **any time**, and changes are **immediately effective**.

```
Select Instance → Actions → Networking → Change Security Groups
```

> 💡 You can attach **multiple security groups** to a single EC2 instance (up to 5).

---

## 9️⃣ Scenario: Connection Timeouts ⏳

> ❓ *"I get a timeout trying to connect to my EC2 instance — why?"*

✅ **Most Likely Cause:** The **Security Group** doesn't allow inbound traffic on the required port.

| Protocol | Port |
|---|---|
| HTTP | 80 |
| HTTPS | 443 |
| SSH | 22 |

> 📌 Check the **inbound rules** and ensure the correct port is open for the protocol you're using.

---

## 🔟 Scenario: Slow Boot Due to User Data 🐢

> ❓ *"My instance takes a long time to launch because User Data installs a lot of software — how do I fix this?"*

✅ **Answer:** Create a **custom AMI** with the software pre-installed, and launch new instances from that AMI instead.

---

## 1️⃣1️⃣ Scenario: Billing for Stopped Instances 💰

> ❓ *"If I stop my EC2 instance, will I be billed for it?"*

| Component | Billed While Stopped? |
|---|---|
| 🖥️ Compute (the instance itself) | ❌ No |
| 💾 Attached Storage (EBS volumes) | ✅ **Yes** |

---

## 📋 Master Scenario Reference Table

| Scenario | Answer |
|---|---|
| Group instances by project/env | Use **Tags** |
| Change instance type | Must **stop** instance first |
| Prevent accidental termination | Enable **Termination Protection** |
| Update to new AMI | **Terminate & relaunch** — no in-place update |
| Import on-prem VM | Use **VM Import/Export**; you own the licensing |
| Change security group | Allowed **anytime**, effective **immediately** |
| Connection timeout | Check **Security Group** inbound rules |
| Slow boot from User Data | Use a **custom AMI** instead |
| Billed while stopped? | Only for **attached storage**, not compute |

---

## ✅ Final Takeaways

```
🧹 TERMINATE > STOP   → Only termination avoids storage billing
🏷️ TAGS               → Group/identify resources by project, env, business unit
⚙️ INSTANCE TYPE       → Must stop instance before changing
🛡️ TERMINATION PROTECTION → Guards against accidental termination, not ASG/Spot/OS shutdown
💾 AMI UPDATES         → No in-place AMI change — terminate & relaunch
🏢 VM IMPORT/EXPORT    → Migrate on-prem VMs; licensing is YOUR responsibility
🔄 SECURITY GROUPS     → Changeable anytime, effective immediately
⏳ TIMEOUTS            → Almost always a security group misconfiguration
💰 STOPPED BILLING     → No compute charge, but storage still billed
```

> 🎯 **Golden Rule:** Many "can I change X on a running instance?" exam questions have the same answer pattern: **stop the instance first**, or **terminate and relaunch**.

> ➡️ **Next Up:** Moving on to the next section of the course!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
