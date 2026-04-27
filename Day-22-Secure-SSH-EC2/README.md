# Day 22: Configuring Secure SSH Access to an EC2 Instance

## 📋 Objective

Set up passwordless SSH access from `aws-client` host to EC2 instance `datacenter-ec2` using SSH key `id_rsa`.

---

## 🎯 What You'll Do

1. Create SSH key on `aws-client`
2. Launch EC2 instance (datacenter-ec2)
3. Add SSH public key to instance's authorized_keys
4. Connect via SSH without password

---

## 🛠️ Steps

### **Part 1: Create SSH Key on aws-client**

### Step 1: Generate SSH Key
```bash
ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N ""
```

**Output:**
```
Generating public/private rsa key pair.
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:x9Z+Vph2wvaapXMSplvYv1jMRCoTZt1pPeQuF8jF+rk root@aws-client
```

**Files created:**
- `/root/.ssh/id_rsa` (private key - keep secret!)
- `/root/.ssh/id_rsa.pub` (public key - share with servers)

### Step 2: Display public key
```bash
cat /root/.ssh/id_rsa.pub
```

**Copy the output - you'll need it in Part 2**

---

### **Part 2: Launch EC2 Instance**

### Step 3: Log in to AWS Console
- URL: https://017034197451.signin.aws.amazon.com/console?region=us-east-1
- Username: `kk_labs_user_501911`
- Password: `Ef!4K8u%IbWx`
- Verify region: `us-east-1`

### Step 4: Launch Instance
- Go to **EC2** → **Instances**
- Click **Launch instances**

### Step 5: Choose AMI
- Search: `ubuntu` or `amazon linux`
- Select: **Ubuntu 22.04 LTS** or **Amazon Linux 2023**
- Click **Select**

### Step 6: Instance Type
- Select: `t2.micro`
- Click **Next: Configure Instance Details**

### Step 7: Configure Instance
- Network: Default VPC
- Auto-assign public IP: **Enable**
- Click **Next: Add Storage**

### Step 8: Storage
- Leave default (8 GB)
- Click **Next: Add Tags**

### Step 9: Add Name Tag
- Click **Add Tag**
- Key: `Name`
- Value: `datacenter-ec2`
- Click **Next: Configure Security Group**

### Step 10: Security Group
- Create new: `datacenter-sg`
- Add rule:
  - Type: SSH
  - Port: 22
  - Source: 0.0.0.0/0 (anywhere)
- Click **Review and Launch**

### Step 11: Launch
- Click **Launch**
- Create new key pair if needed (or use existing)
- Click **Launch Instances**

### Step 12: Get Public IP
- Go to **Instances**
- Wait for state: **running**
- Copy the **Public IPv4 address** (e.g., 54.144.125.98)

---

### **Part 3: Add SSH Key to Instance**

### Step 13: SSH into Instance (as ubuntu/ec2-user)
```bash
ssh -i /root/.ssh/id_rsa ubuntu@54.144.125.98
```

**Note:** Replace `54.144.125.98` with your actual Public IP

**If prompted about authenticity:**
```
The authenticity of host '54.144.125.98 (54.144.125.98)' can't be established.
ECDSA key fingerprint is SHA256:wSaKNKN9yR3PiIcGSLO/KfJ86JGys5s4VqQSPboY58Y.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Type `yes` and press Enter

### Step 14: Switch to root user
```bash
sudo su -
```

**Output:**
```
[root@ip-172-31-17-99 ~]#
```

### Step 15: Create .ssh directory
```bash
mkdir -p /root/.ssh
chmod 700 /root/.ssh
```

### Step 16: Add your public key
```bash
cat >> /root/.ssh/authorized_keys << EOF
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDDQxGi2aP/5cvyYlR8CO88WEcXIWncl265P++8F1T3vTXTx89hBf9fav7HiWwa2XPn5lWpv1vXTie3xE4VZfkW91qHeV3bphQDjuiXl8vuaZi4d/inYKNCO3rOXV/Y/yRtYBnE+5WtyNoDdbUEWa8Mmu+q+obDgd+KWBBsdKW6vc78XNqVFUEbyPO1zwfppPndot3LzC/37EMpHYDgxlDRiwKvfS/x+xX7Ms9L00SKUinZc6grtA7HpGpF5/W/9uPYqu6gxr1N0ojEEUDZHGwtnOjtCbQmPggZOJMUEDo+TlA8CfBL9sNOjhjd7c+qG+siMlSAUft5Z8bFvxlvXngASK1M1e3oSQ/HT+Y64n98qoC6lj+TSJu7MYTtT1diU5UjTDsSwWfd8VnUniE3HL1CGrTO67nhh+guLMayjuumML685VPzKSPbXSGVKwwdR/dU0gEyjCy2WBHS/7GuvbAjMxXhKNSNe0oXXXRbr2zx3y9KbGGWHAgM46YBXErSsps= root@aws-client
EOF
```

**Paste your public key between the `EOF` markers**

### Step 17: Set correct permissions
```bash
chmod 600 /root/.ssh/authorized_keys
```

### Step 18: Verify public key was added
```bash
cat /root/.ssh/authorized_keys
```

**Should show your public key**

### Step 19: Disconnect
```bash
exit
exit
```

---

## ✅ Verification

### Step 20: Test SSH as root (from aws-client)
```bash
ssh -i /root/.ssh/id_rsa root@54.144.125.98
```

**Expected:** Logs in directly to `root` without password ✓

**Prompt should show:**
```
[root@ip-172-31-17-99 ~]#
```

---

## 📝 Completion Checklist

- [x] SSH key `id_rsa` created in `/root/.ssh/`
- [x] SSH key `id_rsa.pub` created in `/root/.ssh/`
- [x] EC2 instance `datacenter-ec2` launched (t2.micro)
- [x] Instance type: `t2.micro`
- [x] AMI: Ubuntu 22.04 LTS or Amazon Linux 2023
- [x] Public IP assigned and noted
- [x] Security group allows SSH (Port 22)
- [x] Public key added to `/root/.ssh/authorized_keys`
- [x] Permissions set: 700 for .ssh, 600 for authorized_keys
- [x] SSH access as root works without password

---

## 🔐 Quick Reference

| Item | Details |
|------|---------|
| Private key | `/root/.ssh/id_rsa` |
| Public key | `/root/.ssh/id_rsa.pub` |
| Instance user | `root` |
| Instance name | `datacenter-ec2` |
| Instance type | `t2.micro` |
| Region | `us-east-1` |
| Security group | `datacenter-sg` |
| SSH port | 22 |

---

## 💡 Key Concepts

✅ **SSH Key Pair**
- Private key (secret, on aws-client)
- Public key (on EC2 instance)
- Asymmetric encryption - highly secure

✅ **Authorized Keys File**
- Location: `/root/.ssh/authorized_keys`
- Contains public keys of trusted users/hosts
- Allows passwordless login
- Must have correct permissions (600)

✅ **File Permissions**
- `.ssh` directory: 700 (rwx for owner only)
- `authorized_keys` file: 600 (rw for owner only)
- Wrong permissions = SSH won't work
- Security critical!

✅ **SSH Process**
1. ssh command sends public key
2. Server checks authorized_keys
3. Server challenges with encrypted data
4. Client decrypts with private key
5. If decryption succeeds = access granted
6. No password transmitted (secure!)

---
