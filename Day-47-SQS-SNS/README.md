
# 🧠 Day 47: Integrating AWS SQS and SNS for Reliable Messaging

---

## 📦 What is Amazon SQS?

Amazon SQS is a **message queue service**.

It stores messages **until another service processes them**.

👉 Simple idea:

* One system sends a message
* SQS stores it safely
* Another system reads and processes it later

✔ Helps systems work independently
✔ Prevents data loss
✔ Supports scaling easily

---

## 📢 What is Amazon SNS?

Amazon SNS is a **message notification service (pub/sub)**.

It sends messages from **one source to many receivers**.

👉 Simple idea:

* One message is published
* SNS sends it to multiple services (SQS, Lambda, email, etc.)

✔ Used for broadcasting messages
✔ Supports filtering based on message type
✔ Helps build event-driven systems

---

## ⚡ What is AWS Lambda?

AWS Lambda runs your code **without managing servers**.

It automatically executes when an event happens.

👉 Simple idea:

* Event happens (like new message in SQS)
* Lambda runs your code automatically
* You only pay when it runs

✔ No servers needed
✔ Fully automatic execution
✔ Great for event-driven apps

---

## 🏗️ What is AWS CloudFormation?

CloudFormation is **Infrastructure as Code (IaC)**.

It lets you create AWS resources using a **YAML file instead of clicking in console**.

👉 Simple idea:

* Write a template
* AWS creates everything automatically
* Same setup can be reused anytime

✔ Automates AWS setup
✔ Reduces manual work
✔ Ensures consistent environments

---

If you want, I can also convert this into:
✔ interview notes
✔ LinkedIn carousel post
✔ or cheat sheet PDF 👍


## Architecture

```
Publisher (AWS CLI)
        │
        ▼
SNS Topic: nautilus-Priority-Queues-Topic
        │
        ├── FilterPolicy: priority = high ──► SQS: nautilus-High-Priority-Queue
        │
        └── FilterPolicy: priority = low  ──► SQS: nautilus-Low-Priority-Queue
                                                        │
                                                        ▼
                                              Lambda Function
                                     (polls HIGH first, then LOW)
```

---

## Resources Created

| Resource | Type | Name |
|---|---|---|
| SQS Queue | AWS::SQS::Queue | nautilus-High-Priority-Queue |
| SQS Queue | AWS::SQS::Queue | nautilus-Low-Priority-Queue |
| SNS Topic | AWS::SNS::Topic | nautilus-Priority-Queues-Topic |
| SNS Subscription | AWS::SNS::Subscription | HighPrioritySubscription |
| SNS Subscription | AWS::SNS::Subscription | LowPrioritySubscription |
| SQS Queue Policy | AWS::SQS::QueuePolicy | HighPriorityPolicy |
| SQS Queue Policy | AWS::SQS::QueuePolicy | LowPriorityPolicy |
| IAM Role | AWS::IAM::Role | lambda_execution_role |
| Lambda Function | AWS::Lambda::Function | nautilus-priorities-queue-function |

---

## CloudFormation Template

**File path:** `/root/nautilus-priority-stack.yml`  
**Stack name:** `nautilus-priority-stack`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Nautilus Priority Queue Processing Stack

Resources:

  HighPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: nautilus-High-Priority-Queue
      VisibilityTimeout: 60

  LowPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: nautilus-Low-Priority-Queue
      VisibilityTimeout: 60

  PriorityQueuesTopic:
    Type: AWS::SNS::Topic
    Properties:
      TopicName: nautilus-Priority-Queues-Topic

  HighPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      TopicArn: !Ref PriorityQueuesTopic
      Protocol: sqs
      Endpoint: !GetAtt HighPriorityQueue.Arn
      FilterPolicy:
        priority:
          - high

  LowPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      TopicArn: !Ref PriorityQueuesTopic
      Protocol: sqs
      Endpoint: !GetAtt LowPriorityQueue.Arn
      FilterPolicy:
        priority:
          - low

  HighPriorityPolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref HighPriorityQueue
      PolicyDocument:
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: SQS:SendMessage
            Resource: !GetAtt HighPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityQueuesTopic

  LowPriorityPolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref LowPriorityQueue
      PolicyDocument:
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: SQS:SendMessage
            Resource: !GetAtt LowPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityQueuesTopic

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
        - arn:aws:iam::aws:policy/AmazonSQSFullAccess
        - arn:aws:iam::aws:policy/AmazonSNSFullAccess

  NautilusLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: nautilus-priorities-queue-function
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Timeout: 60
      Environment:
        Variables:
          high_priority_queue: !Ref HighPriorityQueue
          low_priority_queue: !Ref LowPriorityQueue
      Code:
        ZipFile: |
          import boto3
          import os
          sqs = boto3.client('sqs')
          def delete_message(queue_url, receipt_handle, message):
              sqs.delete_message(QueueUrl=queue_url, ReceiptHandle=receipt_handle)
              return "Message " + "'" + message + "'" + " deleted"
          def poll_messages(queue_url):
              response = sqs.receive_message(
                  QueueUrl=queue_url,
                  AttributeNames=[],
                  MaxNumberOfMessages=1,
                  MessageAttributeNames=['All'],
                  WaitTimeSeconds=3
              )
              if "Messages" in response:
                  receipt_handle = response['Messages'][0]['ReceiptHandle']
                  message = response['Messages'][0]['Body']
                  return delete_message(queue_url, receipt_handle, message)
              else:
                  return "No more messages to poll"
          def lambda_handler(event, context):
              response = poll_messages(os.environ['high_priority_queue'])
              if response == "No more messages to poll":
                  response = poll_messages(os.environ['low_priority_queue'])
              return response

Outputs:
  HighPriorityQueueURL:
    Value: !Ref HighPriorityQueue
  LowPriorityQueueURL:
    Value: !Ref LowPriorityQueue
  SNSTopicARN:
    Value: !Ref PriorityQueuesTopic
  LambdaFunctionName:
    Value: !Ref NautilusLambdaFunction
```

---

## Step-by-Step Deployment

### Step 1: Create the CloudFormation template


```bash
nano /root/nautilus-priority-stack.yml
# Paste the template above, then Ctrl+O → Enter → Ctrl+X
```

### Step 2: Validate the template

```bash
aws cloudformation validate-template \
  --template-body file:///root/nautilus-priority-stack.yml \
  --region us-east-1
```

### Step 3: Deploy the stack

```bash
aws cloudformation deploy \
  --template-file /root/nautilus-priority-stack.yml \
  --stack-name nautilus-priority-stack \
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-east-1
```

> `CAPABILITY_NAMED_IAM` is required because the template creates a named IAM role. Without this flag, CloudFormation will refuse to deploy.

### Step 4: Verify stack creation

```bash
aws cloudformation describe-stacks \
  --stack-name nautilus-priority-stack \
  --region us-east-1 \
  --query "Stacks[0].StackStatus" \
  --output text
```

Expected output: `CREATE_COMPLETE`

### Step 5: Get the SNS Topic ARN

```bash
topicarn=$(aws sns list-topics \
  --query "Topics[?contains(TopicArn, 'nautilus-Priority-Queues-Topic')].TopicArn" \
  --output text)

echo "Topic ARN: $topicarn"
```

### Step 6: Publish test messages to SNS

```bash
aws sns publish --topic-arn $topicarn \
  --message 'High Priority message 1' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}'

aws sns publish --topic-arn $topicarn \
  --message 'High Priority message 2' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}'

aws sns publish --topic-arn $topicarn \
  --message 'Low Priority message 1' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"low"}}'

aws sns publish --topic-arn $topicarn \
  --message 'Low Priority message 2' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"low"}}'
```


> SNS filter policies automatically route messages to the correct SQS queue based on the `priority` message attribute. No manual routing code is needed.

### Step 7: Invoke the Lambda function

The Lambda processes one message per invocation — high-priority queue is always polled first.

```bash
for i in 1 2 3 4; do
  echo "--- Invocation $i ---"
  aws lambda invoke \
    --function-name nautilus-priorities-queue-function \
    --region us-east-1 \
    /tmp/out.json
  cat /tmp/out.json
  echo ""
  sleep 2
done
```


### Step 8: Verify via CloudWatch Logs

```bash
aws logs filter-log-events \
  --log-group-name /aws/lambda/nautilus-priorities-queue-function \
  --region us-east-1 \
  --query 'events[*].message' \
  --output text
```

---

## Expected Output

```
--- Invocation 1 ---   →   High Priority message 1 deleted
--- Invocation 2 ---   →   High Priority message 2 deleted
--- Invocation 3 ---   →   Low Priority message 1 deleted
--- Invocation 4 ---   →   Low Priority message 2 deleted
```

High-priority messages are always processed first, confirming correct priority ordering.

---

## Key Learnings

**SNS Filter Policies** route messages to the correct queue automatically based on message attributes. No application-level routing logic is needed — you just tag messages with `priority=high` or `priority=low`.

**CAPABILITY_NAMED_IAM** must be passed in the CloudFormation deploy command whenever your template creates a named IAM role. Without it, the deployment fails with an InsufficientCapabilities error.

**Lambda Timeout vs SQS WaitTimeSeconds** — the Lambda timeout must always be greater than the SQS `WaitTimeSeconds` (long polling duration). If `WaitTimeSeconds=3` and Lambda timeout is also `3`, the function times out before SQS can return a response. Always set Lambda timeout well above the polling wait time.

