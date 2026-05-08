
## Day 32 — Snapshot and Restoration of an RDS Instance


---

#  Day 32: Snapshot and Restoration of an RDS Instance (AWS)

## Lab Scenario

The Nautilus Development Team is preparing for a major database upgrade.

Before performing any production change, DevOps engineers must:

 Create a backup (Snapshot)  
 Restore database from backup  
 Validate restored environment  

This ensures **data safety**, **rollback capability**, and **deployment confidence**.

---

##  Lab Objective

As a DevOps Engineer, perform:

1. Take snapshot of existing RDS instance
2. Restore snapshot into a new database
3. Configure instance size
4. Verify restored database availability

---

## Simple Theory

### What is an RDS Snapshot?

An **RDS Snapshot** is a backup of your database stored in Amazon S3.

It allows you to:

- Recover deleted databases
- Test upgrades safely
- Clone environments
- Perform disaster recovery

Real Production Rule:
**Never modify production database without a backup.**

---

##  Architecture Flow

```

Existing RDS (nautilus-rds)
│
▼
Create Snapshot (nautilus-snapshot)
│
▼
Restore Snapshot
│
▼
New RDS Instance (nautilus-snapshot-restore)

```

---

## Lab Prerequisites

- AWS Console Access
- Existing RDS Instance: `nautilus-rds`
- Region: **us-east-1**

 Only create resources in **us-east-1**

---

##  Step-by-Step Lab Guide

---

## 🔹 Step 1 — Login to AWS Console

Open Console URL:

https://390964011108.signin.aws.amazon.com/console?region=us-east-1

Login using provided credentials.

Set Region → **N. Virginia (us-east-1)**

---

## 🔹 Step 2 — Open RDS Dashboard

1. Search **RDS**
2. Open **Databases**
3. Confirm instance:

```

nautilus-rds
Status: Available

```

 Wait until status becomes **Available**

---

## 🔹 Step 3 — Create Snapshot

1. Select **nautilus-rds**
2. Click **Actions**
3. Choose **Take snapshot**

### Snapshot Details
```

Snapshot name: nautilus-snapshot

```

Click **Take Snapshot**

---

### Verification

Go to:

RDS → Snapshots → Manual Snapshots

Check:

```

nautilus-snapshot
Status: Available

```

Wait until snapshot completes.

---

## 🔹 Step 4 — Restore Snapshot

1. Select snapshot **nautilus-snapshot**
2. Click **Actions**
3. Select **Restore snapshot**

---

## 🔹 Step 5 — Configure Restored Database

Update configuration:

```

DB Instance Identifier:
nautilus-snapshot-restore

```

### Instance Settings
```

DB Instance Class: db.t3.micro

```

Keep remaining settings default.

---

## 🔹 Step 6 — Launch Restored Instance

Click:

**Restore DB Instance**

AWS now creates a new database from backup.

---

## 🔹 Step 7 — Monitor Restoration

Go to:

RDS → Databases

You will see:

```

nautilus-snapshot-restore
Status: Creating

```

Wait several minutes.

---

## 🔹 Step 8 — Verify Completion

Final verification:

```

DB Identifier: nautilus-snapshot-restore
Status: Available
Instance Class: db.t3.micro

```

Lab Completed Successfully

---

## Expected Final Resources

| Resource | Name |
|----------|------|
| Original DB | nautilus-rds |
| Snapshot | nautilus-snapshot |
| Restored DB | nautilus-snapshot-restore |

---

## Key DevOps Learnings

- Database backup strategy
- Safe production upgrade workflow
- Disaster recovery preparation
- Snapshot restoration process
- Environment cloning best practices

---

## Real World DevOps Use Cases

DevOps teams use snapshots for:

- Production backups
- Testing database upgrades
- Migration validation
- Rollback strategy
- Blue/Green deployments

---

## 🏁 Conclusion

This lab demonstrates an essential DevOps principle:

> **Backup → Restore → Validate → Deploy**

Snapshots protect applications from data loss and allow safe experimentation without risking production systems.

---
