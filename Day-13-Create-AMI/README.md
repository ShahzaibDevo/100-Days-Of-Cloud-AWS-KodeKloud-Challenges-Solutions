# Day 13: Create AMI from EC2 Instance

## What is an AMI? 

**AMI = Amazon Machine Image**

Think of an AMI like a **snapshot or template** of your computer:
- It saves everything on your EC2 instance (operating system, software, settings, files)
- You can use it to create **exact copies** of that instance anytime
- It's like making a backup that you can restore multiple times

**Simple Analogy**: If your EC2 instance is a pizza with all the toppings, the AMI is the **recipe card** that lets you make the exact same pizza again.

---

## Lab Objective

Create an AMI from an existing EC2 instance named `datacenter-ec2` with these requirements:

| Requirement | Value |
|---|---|
| Instance Name | datacenter-ec2 |
| AMI Name | datacenter-ec2-ami |
| Region | us-east-1 |
| Final State | Available |

---

## Step-by-Step Guide

### **Step 1: Login to AWS Console**

```
URL: https://495779504297.signin.aws.amazon.com/console?region=us-east-1
Username: kk_labs_user_528303
Password: ig93U^jT8@Ap
```

Click **Sign In**

---

### **Step 2: Check Your Region**

Look at the **top-right corner** of the console.

Check if it says **us-east-1 (N. Virginia)**

If not:
- Click the region dropdown
- Select **us-east-1**

---

### **Step 3: Go to EC2 Dashboard**

- In the search bar at the top, type **EC2**
- Press **Enter**
- Click on **EC2** service

---

### **Step 4: Find Your Instance**

- In the left sidebar, click **Instances**
- Look for an instance named **datacenter-ec2**
- You can use the search box to filter

---

### **Step 5: Stop the Instance (Recommended)**

This ensures data consistency during AMI creation.

- Select the **datacenter-ec2** instance
- Click **Instance State** button in toolbar
- Click **Stop**
- Wait for status to change to **Stopped**

---

### **Step 6: Create Image from Instance**

- Make sure **datacenter-ec2** is selected
- Click **Image and templates** button in toolbar
- Click **Create image** from the dropdown

---

### **Step 7: Enter AMI Details**

In the **Create image** dialog, enter:

**Image name**: `datacenter-ec2-ami`

**Image description** (optional): 
```
AMI from datacenter-ec2 instance for infrastructure migration
```

**No reboot**: Leave unchecked (recommended)

Click **Create image** button

---

### **Step 8: Note Your AMI ID**

You'll see a success message with your **AMI ID**

Example: `ami-0352359647b146a17`

**Save this ID for your records!**

---

### **Step 9: Check AMI Status**

- In left sidebar, click **Images**
- Click **AMIs**
- Search for **datacenter-ec2-ami**
- Watch the **State** column

Status will change:
- **pending** (yellow) → **available** (green)

This typically takes **3-10 minutes**

---

### **Step 10: Verify AMI is Ready**

When the state shows **available** (green):

✅ AMI name: `datacenter-ec2-ami`  
✅ State: `Available`  
✅ Region: `us-east-1`  
✅ Visibility: `Private`  
✅ Owner: Your AWS Account ID  

---

## Final Result

Your AMI is now **ready to use!**

```
AMI ID:      ami-0352359647b146a17
AMI Name:    datacenter-ec2-ami
State:       Available ✓
Region:      us-east-1
Visibility:  Private
```

---

## What Can You Do With Your AMI?

### 1. **Launch New Instances**
Create 10 more instances with the exact same configuration in seconds

### 2. **Disaster Recovery**
If your instance breaks, launch a new one from the AMI

### 3. **Scale Your Application**
Need more servers? Use the AMI to create them quickly

### 4. **Version Control**
Keep backups of different versions of your infrastructure

### 5. **Share With Team**
Share the AMI with other AWS accounts (optional)

---
