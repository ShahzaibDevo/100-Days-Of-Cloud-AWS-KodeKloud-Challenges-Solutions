
# 🚀 Day 50: Expanding EC2 Instance Storage for Development Needs (AWS 100 Days of Cloud)

## 📘 Overview

In this lab, I worked on expanding storage for an AWS EC2 instance by increasing the attached EBS volume and extending the Linux filesystem.

This is a very common **real-world DevOps and Cloud Engineering task**, where applications require more storage but downtime is not acceptable.

The goal was to safely expand storage from **8 GiB to 12 GiB** and ensure the operating system reflects the updated capacity.

---

## 🎯 Objectives

- Identify the EBS volume attached to EC2 instance
- Increase volume size from **8 GiB → 12 GiB**
- Extend Linux partition to use new space
- Resize filesystem (XFS)
- Verify updated storage inside OS

---

## 📚 Theory: What is EBS Volume Expansion?

### 💡 Amazon EBS (Elastic Block Store)
Amazon EBS provides persistent block storage volumes for EC2 instances.

### 🔄 Key Concept
When you increase an EBS volume size:
- AWS increases the **virtual disk size**
- BUT the operating system does NOT automatically use the new space

You must manually:
1. Extend the partition
2. Extend the filesystem

---

## 🏗️ Architecture Flow

```

EC2 Instance
│
▼
EBS Volume (8 GiB)
│
▼  (Modified in AWS Console)
EBS Volume (12 GiB)
│
▼
Linux Partition Expansion (growpart)
│
▼
Filesystem Expansion (XFS)
│
▼
Root Volume Updated (/)

````

---

## ⚙️ Step-by-Step Implementation

---

### 🔹 1. Check Current Disk

```bash
lsblk
df -hT
````

---

### 🔹 2. Modify EBS Volume (AWS Console)

* Go to EC2 → Volumes
* Select attached volume
* Click **Modify Volume**
* Change size:

```
8 GiB → 12 GiB
```

---

### 🔹 3. Extend Partition

```bash
sudo growpart /dev/xvda 1
```

---

### 🔹 4. Verify Partition Expansion

```bash
lsblk
```

You should now see:

```
/dev/xvda1 → 12G
```

---

### 🔹 5. Extend Filesystem (XFS)

```bash
sudo xfs_growfs -d /
```

---

### 🔹 6. Final Verification

```bash
df -hT
```

Expected output:

```
/dev/xvda1   xfs   12G   /
```

---

## 💻 Key Commands Summary

```bash
lsblk
df -hT
sudo growpart /dev/xvda 1
sudo xfs_growfs -d /
```

---

## 📌 Key Learnings

✔ EBS volume expansion is a **two-step process**
✔ AWS increases disk size but OS does NOT auto-update
✔ Linux tools are required to extend partitions
✔ Filesystem type (XFS/ext4) determines resize command
✔ This is a real-world DevOps storage scaling scenario

---

## 🚀 Real-World Use Case

This concept is widely used in production environments where:

* Databases grow over time
* Application logs increase
* Storage needs scale dynamically
* Zero downtime expansion is required

---

## 🎉 Conclusion

This lab strengthened my understanding of **AWS storage management and Linux system administration**.

It demonstrated how cloud infrastructure and operating systems work together to manage scalable storage systems.

---

## 📂 GitHub Repository

🔗 All 100 Days of Cloud Labs:
[https://github.com/ShahzaibDevo/100-Days-Of-Cloud-AWS-KodeKloud-Challenges-Solutions](https://github.com/ShahzaibDevo/100-Days-Of-Cloud-AWS-KodeKloud-Challenges-Solutions)

---

## 🙏 Acknowledgements

Special thanks to **KodeKloud** and **Mumshad Mannambeth** for providing structured, hands-on cloud learning through real-world DevOps scenarios.

---
