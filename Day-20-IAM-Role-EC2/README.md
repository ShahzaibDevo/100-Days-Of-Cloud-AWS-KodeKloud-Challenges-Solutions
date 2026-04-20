# Day 20: Create IAM Role for EC2 with Policy Attachment

## 📋 Objective

Create an IAM role named `iamrole_jim` for EC2 service and attach policy `iampolicy_jim` to grant EC2 instances specific permissions.

---

## 🎯 What is an IAM Role?

An IAM Role is a set of permissions that AWS services (like EC2) can assume without storing permanent credentials.

**Why Roles > Hardcoded Credentials:**
- No AWS keys stored in application code
- Temporary credentials that auto-rotate
- Easier to audit and manage
- Production best practice

**Think of it like:** AWS keeps the lock, EC2 borrows the key temporarily, then returns it.

---

## 🛠️ AWS Console Steps

### Step 1: Log in to AWS Console
- Navigate to provided Console URL
- Enter username and password
- Verify region is `us-east-1`

### Step 2: Open IAM Dashboard
- Click **Services** → Search for **IAM**
- Click **IAM** from results

### Step 3: Navigate to Roles
- Left sidebar → Click **Roles**

### Step 4: Create Role
- Click **Create role** button (top-right)

### Step 5: Select Trusted Entity
**On "Select trusted entity" page:**
- Entity type: `AWS Service`
- Use case: `EC2`
- Click **Next**

**What this means:**
- Only EC2 service can assume this role
- This is the trust relationship

### Step 6: Add Permissions
**On "Add permissions" page:**
- Search for: `iampolicy_jim`
- Check the checkbox next to `iampolicy_jim`
- Click **Next**

### Step 7: Name the Role
**On "Name, review, and create" page:**
- Role name: `iamrole_jim`
- Description (optional): "Role for EC2 to access AWS services"
- Review the settings
- Click **Create role**

---

## ✅ Verification

After creation, verify in AWS Console:

1. Go to **IAM → Roles**
2. Search for `iamrole_jim`
3. Click on the role name
4. Confirm:
   - ✓ Role name: `iamrole_jim`
   - ✓ Entity type: AWS Service
   - ✓ Use case: EC2
   - ✓ Trusted entity: EC2 service
   - ✓ Permissions: `iampolicy_jim` attached

**Expected Output:**
```
Role Details
──────────────────────────────
Role name: iamrole_jim
Entity type: AWS Service
Use case: EC2

Trust relationships:
Service: ec2.amazonaws.com

Permissions:
Policy name: iampolicy_jim
Effect: Allow
```

---

## 💡 Key Learnings

✅ **IAM Roles vs IAM Users**
- Roles are for AWS services
- Users are for people
- Roles use temporary credentials

✅ **Trust Relationships**
- Defines WHO can assume the role
- EC2 service is the trusted entity
- Can be restricted by account, user, or resource

✅ **Principle of Least Privilege**
- Attach only necessary permissions
- Use specific policies, not admin access
- Review regularly

✅ **Why Roles Matter**
- No credentials in application code
- Temporary credentials auto-rotate
- Easier to audit and manage
- Much safer for production

✅ **How EC2 Uses This Role**
- Instance launches
- Assumes the role
- Gets temporary AWS credentials
- Uses credentials to access other services
- Credentials expire automatically

---

## 🎓 Real-World Use Cases

This role enables:
- EC2 → read/write to S3 buckets
- EC2 → connect to RDS databases
- EC2 → write logs to CloudWatch
- EC2 → access Secrets Manager
- EC2 → assume other roles (role chaining)

---

## 📚 Related Concepts

- **Instance Profile:** Container that holds the role for EC2
- **Assume Role:** When a service takes on a role's permissions
- **Temporary Credentials:** Auto-expiring credentials from role assumption
- **Cross-Account Access:** Using roles across AWS accounts

---
