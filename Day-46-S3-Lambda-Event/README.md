


# 🏗️ Architecture

```text id="arch1"
User Upload File
        ↓
Public S3 Bucket (datacenter-public-15676)
        ↓ (Event Trigger)
AWS Lambda Function (datacenter-copyfunction)
        ↓
Private S3 Bucket (datacenter-private-18753)
        ↓
DynamoDB Table (datacenter-S3CopyLogs)
```

# 🚀 Day 46: Event-Driven Processing with Amazon S3 and Lambda

## 🧠 Objective

Build a **serverless event-driven pipeline** where:

* File upload in S3 triggers Lambda
* Lambda processes event
* File is copied to another S3 bucket
* Logs are stored in DynamoDB

---

# 🏗️ Architecture Flow

```text id="flow1"
User Uploads File
        ↓
S3 Public Bucket (datacenter-public-15676)
        ↓ (Event Trigger)
AWS Lambda (datacenter-copyfunction)
        ↓
S3 Private Bucket (datacenter-private-18753)
        ↓
DynamoDB (datacenter-S3CopyLogs)
```

---

# ☁️ AWS Services Used

* Amazon S3 → File storage + event trigger
* AWS Lambda → Event processing
* Amazon DynamoDB → Logging database
* Amazon Web Services → Cloud provider

---

# 📌 STEP 1 — CREATE PUBLIC S3 BUCKET (UI)

## Go to:

S3 Console → Create bucket

### Settings:

* Bucket name:

```
datacenter-public-15676
```

* Region:

```
us-east-1
```

### Permissions:

❌ Uncheck **Block all public access**

✔ Confirm warning checkbox

### Click:

👉 Create bucket

---

# 📌 STEP 2 — CREATE PRIVATE S3 BUCKET

Repeat same steps:

* Bucket name:

```
datacenter-private-18753
```

### Permissions:

✔ Keep Block Public Access ENABLED

### Click:

👉 Create bucket

---

# 📌 STEP 3 — CREATE DYNAMODB TABLE

Go to DynamoDB → Create table

### Configuration:

* Table name:

```
datacenter-S3CopyLogs
```

* Partition key:

```
LogID (String)
```

### Click:

👉 Create table

---

# 📌 STEP 4 — CREATE IAM ROLE FOR LAMBDA

Go to IAM → Roles → Create Role

### Step 1:

* Trusted entity: AWS Service
* Use case: Lambda

### Step 2: Attach Policies

Attach:

* AmazonS3FullAccess
* AmazonDynamoDBFullAccess
* AWSLambdaBasicExecutionRole

### Step 3:

Role name:

```
lambda_execution_role
```

Click:
👉 Create role

---

# 📌 STEP 5 — CREATE LAMBDA FUNCTION

Go to Lambda → Create Function

### Settings:

* Function name:

```
datacenter-copyfunction
```

* Runtime:

```
Python 3.x
```

* Execution role:

```
Use existing role → lambda_execution_role
```

Click:
👉 Create function

---

# 📌 STEP 6 — UPDATE LAMBDA CODE

Open:

```
/root/lambda-function.py
```

(or inside Lambda editor)

---

## Replace values:

### ❌ Old:

```python id="old1"
REPLACE-WITH-YOUR-DYNAMODB-TABLE
```

### ✅ New:

```python id="new1"
datacenter-S3CopyLogs
```

---

### ❌ Old:

```python id="old2"
REPLACE-WITH-YOUR-PRIVATE-BUCKET
```

### ✅ New:

```python id="new2"
datacenter-private-18753
```

---

### Click:

👉 Deploy

---

# 📌 STEP 7 — ADD S3 TRIGGER TO LAMBDA

Inside Lambda:

### Click:

👉 Add Trigger

### Configure:

* Source: S3
* Bucket:

```
datacenter-public-15676
```

* Event type:

```
ObjectCreated (PUT)
```

### Enable trigger:

✔ Yes

Click:
👉 Add

---

# 📌 STEP 8 — UPLOAD TEST FILE

From AWS CLI:

```bash id="cli1"
aws s3 cp sample.zip s3://datacenter-public-15676/
```

---

# 📌 STEP 9 — VERIFY OUTPUT

## ✔ Check Private Bucket

```bash id="cli2"
aws s3 ls s3://datacenter-private-18753/
```

Expected:

```
sample.zip
```

---

## ✔ Check DynamoDB Logs

```bash id="cli3"
aws dynamodb scan --table-name datacenter-S3CopyLogs
```

Expected:

* LogID
* SourceBucket
* DestinationBucket
* ObjectKey

---

# 🧠 SIMPLE THEORY (IMPORTANT FOR INTERVIEWS)

This lab is based on **Event-Driven Architecture**.

## What happens?

1. User uploads file to S3
2. S3 generates event (ObjectCreated)
3. Lambda is triggered automatically
4. Lambda:

   * Reads event data
   * Copies file to private bucket
   * Writes log into DynamoDB
5. Process completes without servers

---

# ⚡ KEY CONCEPTS

## ✔ Serverless

No EC2 required

## ✔ Event-driven

Actions triggered automatically

## ✔ Decoupled system

Each service works independently

## ✔ Scalable architecture

Handles multiple uploads easily

---

# ⚠️ COMMON ISSUES & FIXES

| Problem               | Solution                       |
| --------------------- | ------------------------------ |
| Lambda not triggering | Check S3 event notification    |
| Access denied         | Fix IAM role permissions       |
| File not copied       | Check Lambda logs (CloudWatch) |
| DynamoDB empty        | Check table name in code       |
| Public bucket error   | Disable Block Public Access    |

---

# 🧪 DEBUG COMMANDS

### Check logs:

```bash id="dbg1"
aws logs describe-log-groups
```

### Check S3:

```bash id="dbg2"
aws s3 ls
```

---

# 🎯 FINAL RESULT

✔ File uploaded to public S3
✔ Lambda triggered automatically
✔ File copied to private S3
✔ Logs stored in DynamoDB

---

# 💼 RESUME PROJECT LINE

```text id="resume1"
Built an event-driven serverless pipeline using AWS S3, Lambda, and DynamoDB to automate file transfer and logging with real-time processing.
```

---

# 🚀 If you want next upgrade

I can also help you:

* Add **CloudWatch logging section (advanced)**
* Convert this into **professional GitHub README with badges + diagram**
* Give **interview Q&A (very important for DevOps jobs)**
* Or explain **how this becomes production architecture**

Just tell 👍
