![AWS Logo](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

# 🔐 Lecture 11: IAM — Identity and Access Management

> 📚 **Course:** AWS Certified Solutions Architect – Associate
> 🧩 **Section:** Introduction to AWS
> ⏱️ **Type:** Concept + Hands-On Tutorial

---

## 🎯 What This Lecture Is About

Now that we have an AWS account, it's time to talk **security basics**! 🛡️

In this lecture, we cover:
- 🔑 What **IAM** is and why it matters
- ⚠️ Why you should **never use the root user** for daily work
- 👥 How to create an **IAM Group** and **IAM User**
- 🖇️ How to log in as your new IAM user

---

## 🔑 What Is IAM?

> 📖 **IAM = Identity and Access Management**

IAM is responsible for two things:

| Concept | Emoji | Question It Answers |
|---|---|---|
| **Authentication** | 🪪 | *Are you who you say you are?* (Is this the right user?) |
| **Authorization** | ✅❌ | *What are you allowed to do?* (Does this user have the right access?) |

> 💡 Since AWS lets you create **powerful resources**, you need tight control over **who** can access them and **what** they can do.

---

## ⚠️ The Root User: Handle With Care!

When you created your AWS account, you got a **root user**:

```
📧 Root User = Email address used to create the account
🔑 Root User Password = Password set during account creation
```

> 🚨 **CRITICAL WARNING:** The root user has **ABSOLUTE POWER** over your account. Anyone with root credentials can do **anything** — including deleting everything or racking up huge bills.

> ✅ **Best Practice:** **Never use the root user for daily activities.** Instead, create a separate **IAM user** for everyday work.

---

## 👥 Why Use IAM Groups?

> 🤔 **Problem:** What if you have **thousands of IAM users**? Managing permissions individually for each one is a nightmare.

> ✅ **Solution:** Create an **IAM Group** — assign permissions to the **group**, then add users to it.

```
🗂️ IAM Group "developers" → has Administrator Access
        ⬇️
👤 IAM User "in28minutes_dev" → added to "developers" group
        ⬇️
✅ User inherits the group's permissions
```

---

## 🪜 Hands-On: Creating an IAM Group and User

### 1️⃣ Log In to the Management Console (as Root)

- 🔍 Search **"AWS Management Console login"**
- 🖱️ Select **Root User**, enter your **email**, click **Next**
- 🔑 Enter your **password**, click **Sign In**

---

### 2️⃣ Navigate to IAM

- 🔍 In the console search bar, type **"IAM"**
- 🖱️ Click on **IAM (Identity and Access Management)**

> 🧭 IAM lets you manage **Users**, **Groups**, and **Permissions**.

---

### 3️⃣ Create the "developers" Group

- 🖱️ Go to **User Groups** → **Create Group**
- ✍️ Name it: `developers`
- 🔍 In permissions, search: `AdministratorAccess`
- ☑️ Check the box next to **AdministratorAccess**
- 🖱️ Click **Create Group**

> ⚠️ **Note from the instructor:** In real-world scenarios, you'd normally **restrict** permissions as much as possible. Administrator access is used here **only for learning purposes** throughout this course.

---

### 4️⃣ Create the IAM User

- 🖱️ Go to **Users** → **Add Users**
- ✍️ Username: `in28minutes_dev`

#### 🔐 Choose Access Type

| Access Type | Emoji | Use Case |
|---|---|---|
| **Management Console Access** | 🖥️ | Username + password login (what we're using) |
| **Programmatic Access** | 💻 | Access keys for code/scripts (not covered yet) |

- ☑️ Select **"Provide user access to the Management Console"**
- 🔑 Choose **"Custom password"** → enter your password
- ⬜ **Uncheck** "User must create a new password at next sign-in"
- 🖱️ Click **Next**

---

### 5️⃣ Assign Permissions

There are 3 ways to assign permissions:

| Method | Recommended? |
|---|---|
| **Add user to a group** | ✅ Yes — best practice! |
| Copy permissions from existing user | ❌ Not recommended |
| Attach permissions directly | ❌ Not recommended |

- ☑️ Check **"developers"** group (which has Administrator Access)
- 🖱️ Click **Next** → Review details → **Create User**

---

### 6️⃣ Review & Confirm

| Field | Value |
|---|---|
| Username | `in28minutes_dev` |
| Access Type | Management Console |
| Password | Custom (set by you) |
| Group | `developers` |

- 🖱️ Click **Create User** 🎉

---

## 🔓 Logging In With Your New IAM User

> ⚠️ **Important:** The login URL for IAM users is **different** from the root user login URL!

- 📋 Copy the **IAM sign-in URL** shown on the console (it's specific to your account)
- 🔖 **Bookmark it** — you'll use it frequently from now on!
- 🆕 Open a new browser tab → paste the IAM URL
- ✍️ Enter username: `in28minutes_dev`
- 🔑 Enter your password
- ☑️ Check "Remember this account" (optional, for convenience)
- 🖱️ Click **Sign In**

> ✅ **Success check:** Top-right corner of the console should now show `in28minutes_dev` instead of the root user email.

---

## ✅ Summary

| Concept | Emoji | Key Point |
|---|---|---|
| **IAM** | 🔐 | Manages authentication + authorization for AWS resources |
| **Root User** | 👑 | Has absolute power — avoid using it day-to-day |
| **IAM Group** | 🗂️ | Assign permissions once, apply to many users |
| **IAM User** | 👤 | Created for daily activities (e.g., `in28minutes_dev`) |
| **Login URL** | 🔗 | IAM users log in via a **different, bookmarked URL** |

```
👑 Root User → used ONLY for account setup / emergencies
        ⬇️
👤 IAM User (in a group with right permissions) → used for DAY-TO-DAY work
```

> ➡️ **Next Up:** Let's continue exploring the AWS Management Console!

---
*🖊️ Notes based on the AWS Certified Solutions Architect Associate course lecture series.*
