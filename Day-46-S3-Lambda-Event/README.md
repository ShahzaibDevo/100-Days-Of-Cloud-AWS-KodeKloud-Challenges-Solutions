Below is a **complete GitHub-ready lab documentation file** (README.md style) with:

* ✔ Full workflow
* ✔ Simple theory
* ✔ Step-by-step AWS UI + CLI flow
* ✔ Architecture
* ✔ Validation steps

You can directly copy this into GitHub.

---

# 📘 AWS DevOps Lab — S3 to S3 Copy using Lambda + DynamoDB Logging

## 🧠 Project Title

**Automated File Transfer Pipeline using AWS S3, Lambda, and DynamoDB**

---

# 🎯 Objective

Build a serverless system where:

* Files uploaded to a **public S3 bucket**
* Automatically trigger a **Lambda function**
* Lambda copies file to a **private S3 bucket**
* Logs are stored in **DynamoDB**

---

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

---

# ☁️ AWS Services Used

* Amazon S3 → File storage
* AWS Lambda → Event processing
* Amazon DynamoDB → Logging database
* Amazon Web Services → Cloud platform

---

# 📌 Step-by-Step Implementation

---

## 🪣 STEP 1 — Create Public S3 Bucket

### Bucket Name:

```
datacenter-public-15676
```

### Settings:

* Region: `us-east-1`
* ❌ Disable Block Public Access

### Purpose:

Stores uploaded files and triggers Lambda.

---

## 🔒 STEP 2 — Create Private S3 Bucket

### Bucket Name:

```
datacenter-private-18753
```

### Settings:

* Keep **Block Public Access ON**

### Purpose:

Secure storage for copied files.

---

## 🗄️ STEP 3 — Create DynamoDB Table

### Table Name:

```
datacenter-S3CopyLogs
```

### Partition Key:

```
LogID (String)
```

### Purpose:

Stores logs of file transfer operations.

---

## ⚙️ STEP 4 — Create IAM Role

### Role Name:

```
lambda_execution_role
```

### Attach Policies:

* AmazonS3FullAccess
* AmazonDynamoDBFullAccess
* AWSLambdaBasicExecutionRole

### Purpose:

Allows Lambda to access S3 + DynamoDB + logs.

---

## ⚡ STEP 5 — Create Lambda Function

### Function Name:

```
datacenter-copyfunction
```

### Runtime:

Python 3.x

### Role:

Use existing → `lambda_execution_role`

---

## 🧾 STEP 6 — Update Lambda Code

Edit file:

```
/root/lambda-function.py
```

### Replace:

```python id="r1"
REPLACE-WITH-YOUR-DYNAMODB-TABLE
```

with:

```python id="r2"
datacenter-S3CopyLogs
```

---

```python id="r3"
REPLACE-WITH-YOUR-PRIVATE-BUCKET
```

with:

```python id="r4"
datacenter-private-18753
```

---

## 🔗 STEP 7 — Add S3 Trigger

Inside Lambda:

* Add Trigger → S3
* Bucket: `datacenter-public-15676`
* Event: PUT (Object Created)

---

## 🧪 STEP 8 — Test Upload

```bash id="t1"
aws s3 cp sample.zip s3://datacenter-public-15676/
```

---

# 🔍 STEP 9 — Validation

## ✔ Check Private Bucket

```bash id="t2"
aws s3 ls s3://datacenter-private-18753/
```

---

## ✔ Check DynamoDB Logs

```bash id="t3"
aws dynamodb scan --table-name datacenter-S3CopyLogs
```

---

# 📊 Expected Output

## S3 Private Bucket:

```
sample.zip
```

## DynamoDB Entry:

* LogID
* SourceBucket
* DestinationBucket
* ObjectKey

---

# 🧠 Simple Theory

## What happens behind the scenes?

1. User uploads file to S3
2. S3 sends event notification
3. Lambda function is triggered
4. Lambda:

   * Reads file from public bucket
   * Copies it to private bucket
   * Writes logs to DynamoDB
5. System completes automatically

---

# 🚀 Key DevOps Concepts

### ✔ Event-driven architecture

System reacts automatically to events

### ✔ Serverless computing

No servers managed (Lambda)

### ✔ Decoupled system

Each service works independently

### ✔ Scalable design

Handles multiple uploads automatically

---

# ⚠️ Common Issues

| Issue                | Fix                         |
| -------------------- | --------------------------- |
| Lambda not triggered | Check S3 event config       |
| Access denied        | Fix IAM role permissions    |
| File not copied      | Check Lambda logs           |
| Public bucket error  | Disable Block Public Access |

---

# 🎯 Final Result

You built a real-world AWS pipeline:

✔ S3 Upload → Lambda → S3 Copy → DynamoDB Logging

---

# 💼 Resume Line (IMPORTANT)

```
Built a serverless AWS pipeline using S3, Lambda, and DynamoDB to automate file transfer and logging with event-driven architecture.
```

---

If you want, I can also:

* convert this into a **beautiful GitHub README with badges + diagrams**
* add **Lambda Python full code**
* or create a **DevOps interview Q&A based on this project**
