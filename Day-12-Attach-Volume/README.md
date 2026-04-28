# Day 12: Attach Volume to EC2 Instance 📦

## 🎯 Objective

Attach `xfusion-volume` to `xfusion-ec2` instance with device name `/dev/sdb` in the `us-east-1` region.

---

## 📚 Volume Definition & Concepts

### What is an EBS Volume?

**EBS (Elastic Block Store)** is AWS block storage service that works like a virtual hard drive. It provides persistent storage that can be attached to EC2 instances.

### Key Volume Characteristics

- **Virtual Hard Drive:** Acts as additional storage for EC2 instances
- **Persistent Storage:** Data remains even when instance is stopped or restarted
- **Flexible:** Can be attached and detached from instances
- **Scalable:** Can be increased in size without stopping the instance
- **Independent:** Exists separately from EC2 instances

### Volume States Explained

| State | Meaning |
|-------|---------|
| **Available** | Volume exists but not attached to any instance - ready to attach |
| **In-use** | Volume is currently attached and working with an instance |
| **Deleting** | Volume is being permanently removed from AWS |
| **Deleted** | Volume has been permanently removed |

### Device Name Explained

A **Device Name** is the path where the volume appears inside the operating system:

- **Root Device:** `/dev/xvda` or `/dev/sda1` - System/boot drive
- **Secondary Devices:** `/dev/sdb`, `/dev/sdc`, `/dev/sdd` - Additional storage
- **Example:** `/dev/sdb` = Second block device attached to the instance

### Why Attach Volumes?

- **Expand Storage:** Add more space to instance
- **Data Persistence:** Keep data separate from instance
- **Performance:** Different volumes for different workloads
- **Backup:** Detach and snapshot volumes

---

## 📝 Step-by-Step Guide

### Step 1: Access AWS Management Console

1. Open the AWS Console URL in your browser:
   ```
   https://873139289675.signin.aws.amazon.com/console?region=us-east-1
   ```

2. Sign in with credentials:
   - **Username:** `kk_labs_user_690652`
   - **Password:** `7RIo39Gw09J%`

3. Verify region is **us-east-1** (top-right corner)

### Step 2: Navigate to EC2 Dashboard

1. Click on **Services** (top-left)
2. Search for **EC2**
3. Click **EC2** to open the dashboard
4. Confirm you're in **us-east-1** region

### Step 3: Locate Your Instance

1. In the left sidebar, click on **Instances**
2. Search for `xfusion-ec2` in the instances list
3. Verify instance status is **Running**
4. Note the Instance ID for reference

**Expected:**
```
Instance: xfusion-ec2
State: Running
Region: us-east-1
```

### Step 4: Locate Your Volume

1. In the left sidebar, click on **Volumes** (under "Elastic Block Store")
2. Search for `xfusion-volume` in the volumes list
3. Verify the volume status shows **Available**

**Expected:**
```
Volume: xfusion-volume
State: Available
Region: us-east-1
```

### Step 5: Attach Volume to Instance

#### **Method A: From Volumes Console (Recommended)** 

1. Click on `xfusion-volume` to select it
2. Click **Actions** dropdown menu (top-right)
3. Select **Attach Volume**
4. In the dialog box:
   - **Instance:** Select `xfusion-ec2`
   - **Device:** Enter `/dev/sdb`
5. Click **Attach** button
6. Wait for attachment to complete

#### **Method B: From Instance Console**

1. Click on `xfusion-ec2` instance
2. Click **Actions** → **Security** → **View Volume Attachments**
3. Click **Edit Volume Attachments**
4. Click **Add Volume**
5. Select `xfusion-volume` from dropdown
6. Set Device Name to `/dev/sdb`
7. Click **Attach**

### Step 6: Verify Attachment

#### **Check via Volumes Console:**

1. Go to **Volumes** in the sidebar
2. Click on `xfusion-volume`
3. Look at the **Details** tab:
   - **State:** Should show `In-use` 
   - **Attachments:** Should show `xfusion-ec2` with device `/dev/sdb`

#### **Check via Instance Console:**

1. Go to **Instances** in the sidebar
2. Click on `xfusion-ec2`
3. Scroll to **Block Devices** section
4. You should see:
   - Root device: `/dev/xvda`
   - Attached volume: `/dev/sdb` → `xfusion-volume` (In-use)

**Expected Output:**
```
Block Devices:
  /dev/xvda (root) - In-use
  /dev/sdb (xfusion-volume) - In-use
```

---

