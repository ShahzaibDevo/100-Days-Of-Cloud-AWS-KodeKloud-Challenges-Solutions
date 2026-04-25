# Day 10: Attach Elastic IP to EC2 Instance 🚀

---

## 📖 Theory

### What is an Elastic IP (EIP)?
An **Elastic IP address** is a **static, public IPv4 address** designed for dynamic cloud computing on AWS.

### Why Use Elastic IP?
| Regular Public IP | Elastic IP |
|---|---|
| Changes on every stop/start | Stays the same always |
| Lost when instance stops | Persists until you release it |
| Not remappable | Can remap to any instance |

### Key Concepts:
- **Elastic IP** is allocated to your **AWS Account**, not directly to an instance
- You **associate** it to an EC2 instance when needed
- If EIP is **not associated** to a running instance, AWS **charges** for it
- One EIP = One instance at a time
- EIP stays in your account until you explicitly **release** it

### Real-World Use Case:
> Imagine you host a website on EC2. If the instance crashes, you launch a new one and simply **remap the same EIP** — your users never notice a change because the IP stays the same.

---

## 🛠️ Lab Task

> **Goal:** Attach the Elastic IP named `datacenter-ec2-eip` to the EC2 instance named `datacenter-ec2` in `us-east-1` region.

---

## 📋 Step-by-Step Guide (AWS Console UI)

---

### ✅ Step 1: Login to AWS Console

1. Open your browser and go to:
```
https://116115856632.signin.aws.amazon.com/console?region=us-east-1
```
2. Enter your credentials:
   - **Account ID:** `116115856632`
   - **Username:** `kk_labs_user_879238`
   - **Password:** `7z7l038Z3@wr`
3. Click **Sign In**

![AWS Login Page]

> ⚠️ Make sure the region is set to **US East (N. Virginia) us-east-1** in the top-right corner

---

### ✅ Step 2: Navigate to EC2 Dashboard

1. In the **AWS Management Console**, click on the **Search bar** at the top
2. Type `EC2`
3. Click on **EC2** from the results

![Search EC2]

---

### ✅ Step 3: Verify the EC2 Instance

1. In the **EC2 Dashboard**, click **Instances** from the left sidebar
2. Search for `datacenter-ec2` in the search box
3. Click on the instance to view its details
4. Note down the **Instance ID** (e.g., `i-06924b8e39d63bea2`)
5. Confirm the instance **State** is `Running` ✅

![EC2 Instance List]

> 📝 **Note:** Keep the Instance ID handy — you'll need it during association

---

### ✅ Step 4: Navigate to Elastic IPs

1. In the **EC2 left sidebar**, scroll down to the section **"Network & Security"**
2. Click on **Elastic IPs**

![Elastic IP Navigation]

You will see the list of Elastic IPs in your account.

---

### ✅ Step 5: Find the Elastic IP

1. Look for the EIP named **`datacenter-ec2-eip`** in the list
2. Click on it to **select** it (checkbox)
3. Note the **Allocation ID** (e.g., `eipalloc-xxxxxxxxx`)
4. Confirm that the **Association ID** column is currently **empty** (not yet attached)

![EIP List]

---

### ✅ Step 6: Associate the Elastic IP

1. With `datacenter-ec2-eip` selected, click **Actions** button (top right)
2. From the dropdown, click **"Associate Elastic IP address"**

![Actions Dropdown]

---

### ✅ Step 7: Fill Association Details

A new page/dialog will appear:

1. **Resource type** → Select `Instance`
2. **Instance** → Click the dropdown and search for `datacenter-ec2`
   - Select the instance `i-06924b8e39d63bea2`
3. **Private IP address** → It will auto-populate, leave as default
4. **Reassociation** → Leave unchecked (unless EIP is already attached elsewhere)

![Associate EIP Form]

5. Click the **"Associate"** button (orange button bottom right)

---

