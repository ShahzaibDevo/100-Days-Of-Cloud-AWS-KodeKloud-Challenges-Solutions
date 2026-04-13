# Day 1: Create an EC2 Key Pair

## 📋 Objective

Create a secure RSA Key Pair named `datacenter-kp` in AWS region `us-east-1`.

---

## 🎯 What is an EC2 Key Pair?

A Key Pair is your login credential for EC2 instances.

- **Public Key** → AWS keeps it on your instance
- **Private Key** → You download it (`.pem` file) and keep it safe

**Think of it like:** AWS has the lock 🔒, you have the key 🔑

---

How It Works


Your Computer                    AWS EC2 Instance
──────────────                   ─────────────────
datacenter-kp.pem ──(SSH)──→     Public Key (stored by AWS)
(Your Private Key)               Verifies you have the matching private key
                                 ✓ Access Granted


## 🛠️ AWS Console Steps

### Step 1: Open EC2 Dashboard
- Go to AWS Console → Search for **EC2**
- Click **EC2** from results

### Step 2: Navigate to Key Pairs
- Left sidebar → **Network & Security** → **Key Pairs**

### Step 3: Create Key Pair
- Click **Create key pair** button

### Step 4: Configure Settings
| Field | Value |
|-------|-------|
| **Name** | `datacenter-kp` |
| **Type** | `RSA` |
| **Format** | `.pem` |


### Step 5: Download
- Click **Create key pair**
- Your `.pem` file downloads automatically
- **Save it immediately — you can only download it once!**

---

## ✅ Verification

Check in AWS Console:
- Go to **EC2 → Key Pairs**
- See `datacenter-kp` in the list ✓

---

## 💡 What You Need to Know

✅ AWS stores the public key (safe in cloud)  
✅ You store the private key (on your computer)  
✅ `.pem` file downloads **ONE TIME ONLY**  
✅ If you lose it, you have to create a new key pair  
✅ Never push `.pem` files to GitHub  

---

## 🎓 Key Learnings

- AWS keeps one half of the key, you keep the other
- Key pairs are region-specific
- The `.pem` file is your secret — protect it
- You'll use this to access EC2 instances in future labs

---

