# Day 48: Automating Infrastructure Deployment with AWS CloudFormation

## Task Overview

The Nautilus DevOps team needs to implement a Lambda function using a CloudFormation stack on the AWS client host.

| Requirement | Value |
|---|---|
| Template File | `/root/datacenter-lambda.yml` |
| Stack Name | `datacenter-lambda-app` |
| Lambda Function Name | `datacenter-lambda` |
| Runtime | `Python 3.9` |
| IAM Role | `lambda_execution_role` |
| Response Body | `Welcome to KKE AWS Labs!` |
| Status Code | `200` |

---

## CloudFormation Template

Create the file `/root/datacenter-lambda.yml` with the following content:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Lambda function to print welcome message.

Resources:

  LambdaExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

  LambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: datacenter-lambda
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Timeout: 5
      Code:
        ZipFile: |
          def lambda_handler(event, context):
              message = "Welcome to KKE AWS Labs!"
              return {
                  'statusCode': 200,
                  'body': message
              }
```

---

## Step-by-Step Guide

### Step 1 — Create the Template File

```bash
vi /root/datacenter-lambda.yml
```

Press `i` to insert → Paste content → Press `Esc` → Type `:wq` → Press `Enter`

---

### Step 2 — Verify the File

```bash
cat /root/datacenter-lambda.yml
```

---

### Step 3 — Deploy the CloudFormation Stack

```bash
aws cloudformation create-stack \
  --stack-name datacenter-lambda-app \
  --template-body file:///root/datacenter-lambda.yml \
  --capabilities CAPABILITY_NAMED_IAM
```

Expected output:
```json
{
    "StackId": "arn:aws:cloudformation:us-east-1:XXXXXXXXXXXX:stack/datacenter-lambda-app/..."
}
```

---

### Step 4 — Wait for Stack to Complete

```bash
aws cloudformation describe-stacks \
  --stack-name datacenter-lambda-app \
  --query 'Stacks[0].StackStatus'
```

Keep running until you see `"CREATE_COMPLETE"`

---

### Step 5 — Invoke the Lambda Function

```bash
aws lambda invoke --function-name datacenter-lambda response.json
```

Expected output:
```json
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```

---

### Step 6 — Verify the Response

```bash
cat response.json
```

Expected output:
```json
{"statusCode": 200, "body": "Welcome to KKE AWS Labs!"}
```

---




The deployment returned:

{
"statusCode": 200,
"body": "Welcome to KKE AWS Labs!"
}

💡 Key takeaways from this lab:

→ CloudFormation enables infrastructure to be managed as version-controlled code
→ CAPABILITY_NAMED_IAM is required when creating named IAM roles through CloudFormation
→ Understanding the relationship between stacks, resources, and services is essential in AWS architecture
→ A single YAML template can automate the deployment of an entire serverless environment within minutes

This lab reinforced the real value of Infrastructure as Code:
Consistent. Repeatable. Automated.

✅ Day 48 completed.

#DevOps #AWS #CloudFormation #Lambda #InfrastructureAsCode #IaC #CloudComputing #Serverless #AWSCloud #100DaysOfCloud #DevOpsJourney #CloudEngineer
