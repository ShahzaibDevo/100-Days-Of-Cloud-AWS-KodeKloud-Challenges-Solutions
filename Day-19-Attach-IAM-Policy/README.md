# Day 19: Attach IAM Policy to IAM User 🔐

## 📋 Overview
Attach an existing IAM policy to an IAM user to grant them specific permissions in AWS.

---

## 🎯 Lab Objective
Attach the IAM policy `iampolicy_kareem` to the IAM user `iamuser_kareem` in `us-east-1` region.

**Prerequisites:**
- ✅ IAM User `iamuser_kareem` exists
- ✅ IAM Policy `iampolicy_kareem` exists

---

## 🚀 AWS Console - 6 Simple Steps

### 1️⃣ Login to AWS
```
URL: https://141563439981.signin.aws.amazon.com/console?region=us-east-1
Username: kk_labs_user_673084
Password: xh@a^ZB5%rr^
```

### 2️⃣ Go to IAM
Search for **"IAM"** → Click **IAM** service

### 3️⃣ Find User
Click **Users** → Click **`iamuser_kareem`**

### 4️⃣ Add Permissions
Scroll to **Permissions** section → Click **"Add permissions"**

### 5️⃣ Attach Policy
- Select **"Attach policies directly"**
- Search for **`iampolicy_kareem`**
- Check the policy ✓
- Click **"Attach policy"**

### 6️⃣ Verify
Policy `iampolicy_kareem` now appears in user's permissions ✅

---

## 💻 AWS CLI - 2 Simple Commands

### Get Credentials
```bash
showcreds
```

### Attach Policy
```bash
aws iam attach-user-policy \
    --user-name iamuser_kareem \
    --policy-arn arn:aws:iam::141563439981:policy/iampolicy_kareem
```

### Verify
```bash
aws iam list-attached-user-policies --user-name iamuser_kareem
```

---

## 📖 Key Definitions

| Term | Definition |
|------|-----------|
| **IAM** | AWS service to manage users, groups, and permissions |
| **IAM User** | An entity representing a person or application |
| **IAM Policy** | A document that defines permissions (Allow/Deny) |
| **Policy Attachment** | Linking a policy to a user to grant permissions |
| **ARN** | Amazon Resource Name - unique identifier for AWS resources |
| **Region** | Geographic location where AWS resources are deployed |

---

