# Day-16-Create-IAM-User
#  Day 16: Create IAM User 

## 📘 Lab Concept
AWS IAM (Identity and Access Management) is used to:
* Create users
* Assign permissions
* Control access to AWS services securely


## 📚 IAM User Definition

**An IAM User is an individual identity in AWS that represents a person or application with unique credentials and permissions to access AWS resources.**

---

## 🔐 Step 1 — Login to AWS Console

1. Open browser
2. Go to Console URL:
```
https://062665041731.signin.aws.amazon.com/console?region=us-east-1
```
3. Enter credentials: 
   * Username: `kk_labs_user_760701` 
   * Password: `w@cu856jngWW` 
4. Click Sign In

---

## 🌎 Step 2 — Select Region

Top right corner:
 Select Region: `us-east-1 (N. Virginia)`


---

## 👤 Step 3 — Open IAM Service

1. Click Search Bar at top
2. Type: `IAM`
3. Click **Identity and Access Management (IAM)**

---

## ➕ Step 4 — Create IAM User

1. Click **Users** (left sidebar)
2. Click **Create user** button (top-right)

---

## 🧾 Step 5 — Configure User Details

**User name:**
```
iamuser_yousuf
```


**Access Type:**
Leave default (no console access needed)

Click: **Next**

---

## 🔐 Step 6 — Set Permissions

Lab does NOT ask for permissions.

 Choose: **Add user without permissions**

Click: **Next**

---

## 🏷 Step 7 — Tags (Optional)

Skip tagging.

Click: **Next**

---

##  Step 8 — Review and Create

Verify:
* Username = `iamuser_yousuf`

Click: **✅ Create user**

---

## 🎉 Step 9 — Verify Lab Completion

Go to: **IAM → Users**

You should see: `iamuser_yousuf` 

---
