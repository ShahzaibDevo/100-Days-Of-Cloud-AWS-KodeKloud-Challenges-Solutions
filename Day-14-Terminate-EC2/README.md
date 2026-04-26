# Day 14: Terminate EC2 Instance 

---

## What is EC2 Termination? (Simple Definition)

**Termination = Permanent Deletion of an EC2 Instance**

Think of it like this:
- **Stop**: Pause your server (you can turn it back on later)
- **Terminate**: Delete your server permanently (can't get it back)


---

## Lab Objective

Delete the EC2 instance named `nautilus-ec2` in the `us-east-1` region and verify it reaches `terminated` state.

| Item | Value |
|---|---|
| Instance Name | nautilus-ec2 |
| Region | us-east-1 |
| Final State | terminated |
| Action | Delete instance |

---

## AWS Credentials

```
Console URL: https://097227072133.signin.aws.amazon.com/console?region=us-east-1
Username: kk_labs_user_687358
Password: RbtSWvQ2hVj@
```

---

## Simple Step-by-Step Guide

### **Step 1: Login to AWS Console**

- Go to the Console URL
- Enter username: `kk_labs_user_687358`
- Enter password: `RbtSWvQ2hVj@`
- Click **Sign In**

---

### **Step 2: Check Region**

- Look at **top-right corner**
- Verify it says **us-east-1 (N. Virginia)**
- If not, click region dropdown and select **us-east-1**

---

### **Step 3: Go to EC2 Service**

Two ways:

**Option A**: Search bar
- Click search bar at top
- Type: `EC2`
- Press Enter
- Click EC2 service

**Option B**: Services menu
- Click **Services**
- Find **Compute** section
- Click **EC2**

---

### **Step 4: Click Instances**

- In left sidebar, click **Instances**
- You'll see a list of all instances

---

### **Step 5: Find the Instance**

- Look for instance named **`nautilus-ec2`**
- Use the search box to filter
- Type: `nautilus-ec2`

**You should see:**
- Name: nautilus-ec2 ✓
- State: running (or stopped)
- Region: us-east-1 ✓

---

### **Step 6: Select the Instance**

- Click on **nautilus-ec2**
- It will be highlighted
- Details will appear below

---

### **Step 7: Start Termination**

**Three ways to terminate:**

**Method 1 (Easiest):**
- Click **Instance State** button in toolbar
- Click **Terminate instance**

**Method 2:**
- Right-click on the instance
- Click **Instance State**
- Click **Terminate instance**

**Method 3:**
- Click **Actions** button
- Click **Instance State**
- Click **Terminate instance**

---

### **Step 8: Confirm Termination**

A confirmation dialog will appear:

`

- Click **Terminate** button to confirm
- Do NOT click Cancel (unless you want to stop)

---

### **Step 9: Watch the Instance Shutdown**

The instance will go through states:

1. **shutting-down** (yellow) - Instance is stopping
2. **terminated** (gray) - Instance is deleted ✓

This takes about **10-30 seconds**

---

### **Step 10: Verify Terminated State**

Check the instance in the list:

- State column should show: **terminated** (gray icon)
- Instance is no longer running
- Instance no longer costs money

