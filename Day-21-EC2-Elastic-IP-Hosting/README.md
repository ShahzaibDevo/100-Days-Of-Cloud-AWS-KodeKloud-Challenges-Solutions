# Day 21: EC2 Instance with Elastic IP

## 📋 Objective

Create EC2 instance `devops-ec2` (t2.micro, Ubuntu) and attach Elastic IP `devops-eip`.

---

### 🖥️ What is an EC2 Instance?

An **Amazon EC2 instance** is a virtual server in AWS used to run applications, websites, and backend services in the cloud environment.

---

### 🌐 What is an Elastic IP?

An **Elastic IP (EIP)** is a **static public IPv4 address** provided by AWS.

Unlike automatically assigned public IP addresses that change after stopping or restarting an instance, an Elastic IP remains **permanently associated** with your AWS account.

This ensures users can always access your application using the same IP address.

---

### ✅ Why Use an Elastic IP?

- Provides a **fixed public IP address**
- Prevents downtime caused by IP changes
- Enables reliable application access
- Commonly used for:
  - Application hosting
  - Web servers
  - Production environments
  - DNS configuration

---

### ⚙️ How It Works


## 🎯 Why?

EC2 public IPs change when stopped. Elastic IP stays the same = stable application access.

---

## 🛠️ Steps

### Step 1: Log in
- URL: https://217515478918.signin.aws.amazon.com/console?region=us-east-1
- Username: `kk_labs_user_285137`
- Password: `7DHUXUsvMNhV`
- Verify region: `us-east-1`

### Step 2: Launch Instance
- Go to **EC2** → **Instances**
- Click **Launch instances**

### Step 3: Choose AMI
- Search: `ubuntu`
- Select: **Ubuntu 22.04 LTS**
- Click **Select**

### Step 4: Choose Instance Type
- Find and select: `t2.micro`
- Click **Next: Configure Instance Details**

### Step 5: Configure Instance
- Network: Default VPC
- Auto-assign public IP: **Enable**
- Click **Next: Add Storage**

### Step 6: Storage
- Leave default
- Click **Next: Add Tags**

### Step 7: Add Name Tag
- Click **Add Tag**
- Key: `Name`
- Value: `devops-ec2`
- Click **Next: Configure Security Group**

### Step 8: Security Group
- Create new group: `devops-sg`
- Add rules:
  - SSH (Port 22)
  - HTTP (Port 80)
  - HTTPS (Port 443)
- Click **Review and Launch**

### Step 9: Launch & Download Key
- Click **Launch**
- Create new key pair: `devops-kp`
- Click **Download Key Pair**
- Click **Launch Instances**

### Step 10: Wait for Running
- Go to **Instances**
- Wait for state: **running** (1-2 min)

### Step 11: Create Elastic IP
- Go to **Elastic IPs**
- Click **Allocate Elastic IP address**
- Click **Allocate**

### Step 12: Tag Elastic IP
- Select your Elastic IP
- Click **Actions** → **Edit tags**
- Key: `Name`
- Value: `devops-eip`
- Save

### Step 13: Associate Elastic IP
- Click **Actions** → **Associate Elastic IP address**
- Instance: Select `devops-ec2`
- Click **Associate**

### Step 14: Verify
Go to **Instances**, click `devops-ec2`:
- ✓ Name: `devops-ec2`
- ✓ Type: `t2.micro`
- ✓ State: `running`
- ✓ Public IP: (your Elastic IP)

Go to **Elastic IPs**:
- ✓ Name: `devops-eip`
- ✓ Associated: `devops-ec2`

---

## ✅ Done!

| Item | Status |
|------|--------|
| Instance created | ✓ |
| Instance name | `devops-ec2` ✓ |
| Instance type | t2.micro ✓ |
| AMI | Ubuntu 22.04 ✓ |
| Elastic IP created | ✓ |
| Elastic IP name | `devops-eip` ✓ |
| Associated | ✓ |

---

