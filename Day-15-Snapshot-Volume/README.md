
# 📘 Day 15 — Create Volume Snapshot (AWS)

## ✅ What is a Volume Snapshot?

A **Volume Snapshot** is a **point-in-time backup** of an EBS volume.

👉 Think of it like:

```
Hard Disk Backup → Stored safely in AWS
```

A snapshot allows you to:

* Restore lost data
* Create new volumes
* Recover from failures
* Implement disaster recovery

In **Amazon Web Services**, snapshots are **incremental**, meaning only changed data blocks are stored.

---

## 🎯 Lab Objective

Create a snapshot from an existing volume.

| Item          | Value             |
| ------------- | ----------------- |
| Volume Name   | nautilus-vol      |
| Region        | us-east-1         |
| Snapshot Name | nautilus-vol-ss   |
| Description   | nautilus Snapshot |
| Final State   | Completed         |

---

## ⚙️ Step-by-Step Guide

### Step 1 — Login to AWS Console

* Open AWS Console URL
* Enter username & password
* Sign in successfully

---

### Step 2 — Select Region

* Check top-right corner
* Select:

```
us-east-1 (N. Virginia)
```

---

### Step 3 — Open EC2 Service

* Search **EC2**
* Open EC2 Dashboard

---

### Step 4 — Open Volumes

Navigate:

```
Elastic Block Store → Volumes
```

---

### Step 5 — Locate Volume

Search:

```
nautilus-vol
```

✅ Select the correct volume.

---

### Step 6 — Create Snapshot

Click:

```
Actions → Create Snapshot
```

Fill details:

```
Name: nautilus-vol-ss
Description: nautilus Snapshot
```

Click **Create Snapshot**.

---

### Step 7 — Monitor Snapshot

Go to:

```
Elastic Block Store → Snapshots
```

Wait until status becomes:

```
Completed ✅
```


## 🧠 Key Concepts Learned

* EBS Volume Backup
* Data Protection Strategy
* Disaster Recovery Basics
* Cloud Storage Management
* DevOps Infrastructure Safety

---

## 🔥 Real DevOps Use Case

DevOps engineers create snapshots to:

* Automate backups
* Protect production data
* Enable fast environment recovery
* Support migration & scaling

---
