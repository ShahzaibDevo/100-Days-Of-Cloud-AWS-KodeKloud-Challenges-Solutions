
# Day 34 — Create AWS Lambda Function Using AWS CLI

## Lab Overview

In this lab, the Nautilus DevOps Team deployed a **serverless Lambda function using AWS CLI** instead of the AWS Console.

This demonstrates real DevOps automation where infrastructure is created directly from the command line.

---

##  Simple Theory

**AWS Lambda** allows you to run code without managing servers.

Using **AWS CLI**, engineers can automate deployments, integrate CI/CD pipelines, and avoid manual console work.

 Console = Manual setup  
CLI = Automation & DevOps way

---

##  Lab Objectives

- Create Python script → `lambda_function.py`
- Return message → **Welcome to KKE AWS Labs!**
- Status Code → **200**
- Zip file → `function.zip`
- Create Lambda → `devops-lambda-cli`
- Use IAM Role → `lambda_execution_role`
- Perform everything using **AWS CLI**

---

## 🛠️ Step-by-Step Guide

---

## ✅ Step 1 — Login to aws-client Host

Open terminal on **aws-client** machine.

Verify CLI access:

```bash
aws sts get-caller-identity
````

If account details appear → CLI ready ✅

---

## ✅ Step 2 — Create Python Script

Create file:

```bash
nano lambda_function.py
```

Add code:

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }
```

Save file.

---

## ✅ Step 3 — Zip the Python Script

Create deployment package:

```bash
zip function.zip lambda_function.py
```

Verify:

```bash
ls
```

You should see:

```
lambda_function.py
function.zip
```

---

## ✅ Step 4 — Get IAM Role ARN

Run:

```bash
aws iam get-role --role-name lambda_execution_role
```

Copy the **Role ARN**.

Example:

```
arn:aws:iam::123456789012:role/lambda_execution_role
```

---

## ✅ Step 5 — Create Lambda Function Using CLI

Run command:

```bash
aws lambda create-function \
--function-name devops-lambda-cli \
--runtime python3.14 \
--role arn:aws:iam::ACCOUNT-ID:role/lambda_execution_role \
--handler lambda_function.lambda_handler \
--zip-file fileb://function.zip
```

✅ Lambda function created.

---

## ✅ Step 6 — Verify Lambda Function

Check function:

```bash
aws lambda list-functions
```

You should see:

```
devops-lambda-cli
```

---

## ✅ Step 7 — Test Lambda Function (CLI)

Invoke function:

```bash
aws lambda invoke \
--function-name devops-lambda-cli \
response.json
```

Check output:

```bash
cat response.json
```

Expected result:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

## ✅ Verification Checklist

✔ Python script created
✔ Zip package prepared
✔ IAM role used
✔ Lambda created via CLI
✔ Function executed successfully
✔ Status Code 200 returned

🎉 **Lab Completed Successfully**

---

## 🧠 Key Learning

* AWS CLI enables automation
* Lambda deployment without console
* Used in real DevOps pipelines
* Faster & repeatable infrastructure setup
* Foundation of Infrastructure as Code

---

## 🌍 Real-World DevOps Usage

* CI/CD deployments
* Automated infrastructure provisioning
* Serverless automation
* GitHub Actions / Jenkins integrations

---

