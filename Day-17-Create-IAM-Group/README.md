# Day 17: Create IAM Group - Step-by-Step Lab Guide

## Objective
Create an IAM group named `iamgroup_kirsty` in AWS using the provided credentials.

---
🧠 Concept — What is IAM Group?

An IAM Group is a collection of IAM users.

Instead of giving permissions individually:

👉 Assign permissions to Group
👉 Add users to group
👉 All users inherit same permissions

🔥 Real DevOps Example
Developers Group → EC2 access
DevOps Group → Full deployment access
ReadOnly Group → Monitoring only

## Step 1: Access AWS Console

1. Open your web browser
2. Navigate to the Console URL: `https://017616195538.signin.aws.amazon.com/console?region=us-east-1`
3. You should see the AWS login page

**Login Credentials:**
- **Username:** `kk_labs_user_998729`
- **Password:** `y!!S0!b0j%qT`
- **Region:** `us-east-1`

---

## Step 2: Navigate to IAM Service

1. Once logged in, you'll be on the AWS Management Console
2. In the search bar at the top, type **"IAM"**
3. Click on **"IAM"** (Identity and Access Management) from the search results
4. This will open the IAM dashboard

---

## Step 3: Access the Groups Section

1. On the left sidebar of the IAM dashboard, locate and click on **"Groups"**
   - If using the newer IAM interface, it might be under "Access management" → "Groups"
2. You should see a list of existing IAM groups (if any)

---

## Step 4: Create a New Group

1. Click the **"Create group"** button (usually blue/orange colored button at the top)
2. You'll be taken to the "Create group" page

---

## Step 5: Configure Group Details

### On the Create Group page:

1. **Group Name Field:**
   - Enter: `iamgroup_kirsty`
   - Ensure the name is exactly as specified (case-sensitive)

2. **Add Users (Optional for now):**
   - You can leave this blank or add users later
   - This step allows you to add existing IAM users to the group

3. **Attach Policies (Optional for now):**
   - You can attach permissions policies to the group
   - For now, you may skip this or attach policies based on your needs

---

## Step 6: Review and Create

1. Review the group name: `iamgroup_kirsty`
2. Click the **"Create group"** button at the bottom of the page
3. You should see a success message confirming the group was created

---

## Step 7: Verify the Group Creation

1. You should be redirected back to the Groups list
2. Look for `iamgroup_kirsty` in the list of groups
3. Confirm that the group appears with:
   - **Name:** `iamgroup_kirsty`
   - **ARN:** Will be displayed (format: `arn:aws:iam::017616195538:group/iamgroup_kirsty`)
   - **Creation Date:** Should show the current date and time

---

## Summary

**What was accomplished:**
✅ Created an IAM group named `iamgroup_kirsty`
✅ Group is created in the `us-east-1` region
✅ Group is ready for users and policies to be assigned

---

