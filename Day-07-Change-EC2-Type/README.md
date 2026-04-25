# Day 7: Change EC2 instance type (AWS UI walkthrough)


## Scenario

The Nautilus DevOps team discovered that a production EC2 instance `datacenter-ec2` was underutilised. The task: resize it from `t2.micro` to `t2.nano` and bring it back to `running` state — all via the AWS Console UI.

## Prerequisites

- AWS Console access — region `us-east-1`
- EC2 instance `datacenter-ec2` must have status checks passing before any changes

---

## Step-by-step — AWS Console UI

### Step 1 — Sign in to AWS Console

Log in and confirm the region is set to `us-east-1` (top-right corner).

### Step 2 — Navigate to EC2 instances

Search bar → `EC2` → left sidebar → **Instances**

### Step 3 — Wait for status checks

Locate `datacenter-ec2`. Check the **Status check** column.  
If it shows **Initializing**, wait until it reads **2/2 checks passed** before proceeding.

### Step 4 — Stop the instance

Select the instance checkbox → **Instance state** → **Stop instance** → confirm.  
Wait for state: `stopped`.

> ⚠️ You cannot change the instance type while it is running. The stop step is mandatory.

### Step 5 — Change instance type

With the instance in `stopped` state:  
**Actions** → **Instance settings** → **Change instance type** → select `t2.nano` → **Apply**

### Step 6 — Start the instance

**Instance state** → **Start instance**. Wait for state: `running`.

### Step 7 — Verify

Click the instance name and confirm in the details panel:
- Instance type = `t2.nano`
- Instance state = `running`
- Status checks = 2/2 checks passed

---

## CLI verification command

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=datacenter-ec2" \
  --query "Reservations[].Instances[].[InstanceId,InstanceType,State.Name,PublicIpAddress]" \
  --output table \
  --region us-east-1
```

---

## Result

```
+----------------------+----------+---------+-----------------+
|  i-0a71621073d2a131f | t2.nano  | running | 13.220.92.167   |
+----------------------+----------+---------+-----------------+
```

| Field          | Value                  | Status             |
|----------------|------------------------|--------------------|
| Instance ID    | i-0a71621073d2a131f    | ✅                 |
| Instance type  | t2.nano                | ✅ changed from t2.micro |
| State          | running                | ✅                 |
| Public IP      | 13.220.92.167          | ✅                 |

---

## Key takeaway

Changing an EC2 instance type requires stopping the instance first.  
The workflow is always: **Stop → Change type → Start**  
Never attempt a resize on a running instance — the option will be greyed out.

---
