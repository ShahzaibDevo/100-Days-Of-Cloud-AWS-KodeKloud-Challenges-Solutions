# Day 18: Create Read-Only IAM Policy for EC2 Console Access

## Objective
Create an IAM policy named `iampolicy_yousuf` that allows read-only access to EC2 console in the `us-east-1` region.

---

## Lab Information

**AWS Account Details:**
- **Console URL:** `https://972345760678.signin.aws.amazon.com/console?region=us-east-1`
- **Username:** `kk_labs_user_972602`
- **Password:** `N^@0%5%%^Sk^`
- **Region:** `us-east-1`
- **Policy Name:** `iampolicy_yousuf`
- **Lab Duration:** 1 hour (07:13:08 UTC to 08:13:08 UTC)

---

## Method 1: AWS Console (GUI) - Step by Step

### Step 1: Access AWS Console

1. Open your web browser
2. Navigate to: `https://972345760678.signin.aws.amazon.com/console?region=us-east-1`
3. Log in with the provided credentials:
   - **Username:** `kk_labs_user_972602`
   - **Password:** `N^@0%5%%^Sk^`

---

### Step 2: Navigate to IAM Service

1. Once logged in, you'll be on the AWS Management Console
2. In the search bar at the top, type **"IAM"**
3. Click on **"IAM"** (Identity and Access Management) from the search results
4. This opens the IAM Dashboard

---

### Step 3: Access the Policies Section

1. On the left sidebar, locate and click on **"Policies"**
   - If using the new interface, it may be under "Access management" → "Policies"
2. You'll see a list of existing policies (AWS managed and customer managed)

---

### Step 4: Create a New Policy

1. Click the **"Create policy"** button (usually blue/orange at the top right)
2. You'll see the policy creation page with two options:
   - **Visual editor** (point-and-click interface)
   - **JSON editor** (code-based)

---

### Step 5: Option A - Using Visual Editor (Recommended for Beginners)

#### Sub-Step 5.1: Select Service
1. Under "Service" field, click on the dropdown
2. Search for and select **"EC2"**

#### Sub-Step 5.2: Configure Permissions

1. **Actions:**
   - Expand the "Actions" section
   - Select **"Read"** category (or expand it to see read-only actions)
   - Check the following actions:
     - `DescribeInstances` - View all instances
     - `DescribeImages` - View all AMIs
     - `DescribeSnapshots` - View all snapshots
     - `DescribeInstanceAttribute`
     - `DescribeInstanceStatus`
     - `DescribeReservedInstances`
     - `DescribeSecurityGroups`
     - `DescribeVolumes`
     - `DescribeVpcs`

2. **Resources:**
   - Select **"All resources"** (or specify specific resource ARNs if needed)

3. **Request conditions:**
   - Leave empty for this basic read-only policy

#### Sub-Step 5.3: Review and Create

1. Click **"Next"** to proceed to the Review page
2. Enter the policy name: **`iampolicy_yousuf`**
3. (Optional) Add description: *"Read-only access to EC2 console for viewing instances, AMIs, and snapshots"*
4. Click **"Create policy"**

---


## Key Takeaways

🔑 **Best Practices:**
1. Use **read-only policies** for auditing and monitoring roles
2. Apply **principle of least privilege** - grant minimum necessary permissions
3. Use **wildcards carefully** - `Describe*` is safe for read-only access
4. **Test policies** before assigning to production users
5. **Document policies** for compliance and audit trails

---
