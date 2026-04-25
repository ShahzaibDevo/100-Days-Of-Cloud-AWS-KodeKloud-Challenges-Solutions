## Day 8: Enable Stop Protection for EC2 Instance

### 🎯 Objective
Enable stop protection for an EC2 instance named `xfusion-ec2` in `us-east-1` region using AWS Console UI.

---

### 📸 Step 1: Login to AWS Console
Go to the AWS Console URL and login with your lab credentials.

---

### 📸 Step 2: Navigate to EC2
Click on **Services** → search **EC2** → click **EC2**

---

### 📸 Step 3: Go to Instances
In the left sidebar click **Instances** → **Instances**

---

### 📸 Step 4: Find the Instance
In the search bar type `xfusion-ec2` and locate the instance in the list.

---

### 📸 Step 5: Check Instance State
Make sure the instance is in **Running** state. If it shows **Stopped**, select it and click **Actions → Instance State → Start Instance** and wait for it to be **Running**.

---

### 📸 Step 6: Select the Instance
Click the **checkbox** next to `xfusion-ec2` to select it.

---

### 📸 Step 7: Open Instance Settings
Click **Actions** (top right) → **Instance Settings** → **Change Stop Protection**

---

### 📸 Step 8: Enable Stop Protection
A dialog box appears. **Check the Enable box** and click **Save**.

---

### 📸 Step 9: Verify
Go back to the instance → scroll down to **Details tab** → confirm **Stop protection** shows **Enabled**.

---
