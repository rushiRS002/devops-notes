# AWS EC2 Log Backup to S3 using Lambda


<img width="1774" height="887" alt="ChatGPT Image May 27, 2026, 04_08_16 PM" src="https://github.com/user-attachments/assets/840b2e2b-7b82-4c3f-a023-47863c4c3993" />

## 📌 Project Overview

This project automatically backs up Linux log files from an EC2 instance to an S3 bucket using AWS Lambda, SSM, SNS, and EventBridge without SSH access.

---

# 🛠 AWS Services Used

- Amazon EC2
- AWS Lambda
- Amazon S3
- Amazon SNS
- Amazon EventBridge
- AWS IAM
- AWS Systems Manager (SSM)

---

# 🏗 Architecture Workflow



```text
EventBridge Trigger
        ↓
Lambda Function
        ↓
SSM Command
        ↓
EC2 Instance
        ↓
S3 Bucket Backup
        ↓
SNS Email Notification
```

---

# 🚀 Step 1: Create EC2 Instance

1. Open AWS Console
2. Go to EC2
3. Click Launch Instance
4. Select Ubuntu AMI
5. Select t2.micro
6. Create Key Pair
7. Allow SSH
8. Launch Instance

---

# 🔐 Step 2: Create IAM Role for EC2

## Create Role

1. Open IAM
2. Go to Roles
3. Click Create Role
4. Select:
   - AWS Service
   - EC2

## Attach Policies

- AmazonSSMManagedInstanceCore
- AmazonS3FullAccess

## Role Name

```text
EC2-Role
```

Attach this role to the EC2 instance.

---

# ⚙️ Step 3: Install AWS CLI Automatically using User Data

While launching EC2:

1. Go to Advanced Details
2. Add this User Data script

```bash
#!/bin/bash

apt update -y

snap install aws-cli --classic

snap install amazon-ssm-agent --classic

systemctl enable snap.amazon-ssm-agent.amazon-ssm-agent.service

systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
```

This installs:
- AWS CLI
- SSM Agent

No manual SSH connection required.

---

# 📧 Step 4: Create SNS Topic

1. Open SNS
2. Click Create Topic
3. Select Standard

## Topic Name

```text
BackupNotification
```

4. Create Topic

---

# 📩 Step 5: Create SNS Subscription

1. Open created topic
2. Click Create Subscription

## Configure

- Protocol: Email
- Enter Email Address

3. Create Subscription
4. Open Email
5. Click Confirm Subscription

---

# 🛡 Step 6: Create Lambda IAM Role

## Create Role

1. Open IAM
2. Go to Roles
3. Click Create Role
4. Select:
   - AWS Service
   - Lambda

## Attach Policies

- AmazonS3FullAccess
- AmazonSSMFullAccess
- AmazonEC2FullAccess
- AmazonSNSFullAccess

## Role Name

```text
Lambda-Role
```

---

# ⚡ Step 7: Create Lambda Function

1. Open Lambda
2. Click Create Function
3. Select Author From Scratch

## Configure

### Function Name

```text
LogBackupFunction
```

### Runtime

```text
Python 3.x
```

### Execution Role

- Use Existing Role
- Select:

```text
Lambda-Role
```

4. Click Create Function

---

# 🧠 Step 8: Lambda Function Code

```python
import boto3
import time
from botocore.exceptions import ClientError

# AWS Clients
s3 = boto3.client('s3', region_name='ap-south-1')
ssm = boto3.client('ssm', region_name='ap-south-1')
sns = boto3.client('sns', region_name='ap-south-1')

def wait_for_ssm(instance_id):

    while True:

        response = ssm.describe_instance_information()

        ids = [
            i['InstanceId']
            for i in response['InstanceInformationList']
        ]

        if instance_id in ids:
            print("SSM Online")
            break

        print("Waiting for SSM...")
        time.sleep(15)

def lambda_handler(event, context):

    bucket_name = 'dinesh-bucket'

    region = 'ap-south-1'

    instance_id = 'i-02e92165a1ee00d71'

    sns_topic = 'arn:aws:sns:ap-northeast-1:886274844949:BackupNotification'

    log_path = "/var/log/"

    try:

        s3.head_bucket(Bucket=bucket_name)

        print("Bucket already exists")

    except ClientError:

        s3.create_bucket(
            Bucket=bucket_name,
            CreateBucketConfiguration={
                'LocationConstraint': region
            }
        )

        print("Bucket created successfully")

    wait_for_ssm(instance_id)

    command = f"sudo aws s3 sync {log_path} s3://{bucket_name}/ec2-logs/{instance_id}/"

    response = ssm.send_command(
        InstanceIds=[instance_id],
        DocumentName="AWS-RunShellScript",
        Parameters={
            'commands': [command]
        }
    )

    print("Backup Command Sent")

    sns.publish(
        TopicArn=sns_topic,
        Subject='Backup Success',
        Message='EC2 logs copied successfully to S3 bucket'
    )

    return {
        'statusCode': 200,
        'body': 'EC2 Logs Backup Completed Successfully'
    }
```

Deploy the Lambda function.

---

# ⏰ Step 9: Add EventBridge Trigger

1. Open Lambda Function
2. Click Add Trigger
3. Select EventBridge
4. Create New Rule

## Rule Name

```text
DailyBackupTrigger
```

## Schedule Expression

```text
cron(0/5 * * * ? *)
```

This runs the Lambda function every 5 minutes.

---

# 🧪 Step 10: Test Lambda Function

1. Open Lambda
2. Click Test
3. Create Event:

```json
{}
```

4. Run Test

---

# 📂 Step 11: Verify Backup in S3

## Open S3 Bucket

```text
dinesh-backup
```

## Backup Path

```text
s3://dinesh-backup-12345/ec2-logs/i-0ed33c7e6d6b9ece0/
```

You should see log files like:

- syslog
- auth.log
- cloud-init.log

---

# 📬 Step 12: Verify Email Notification

Check your email inbox.

SNS will send:

```text
Subject: Backup Success

EC2 logs copied successfully to S3 bucket
```

---

# ✅ Final Output

- Automatic Linux log backup
- Fully automated backup workflow
- No SSH required
- S3 storage automation
- Email notification using SNS

---
---

# 🧾 Author

AWS DevOps Automation Project  
EC2 Log Backup to S3 using Lambda + SSM + SNS + EventBridge

```<img width="1774" height="887" alt="ChatGPT Image May 27, 2026, 04_08_16 PM" src="https://github.com/user-attachments/assets/22ced0c6-2e36-4bc4-8da5-d0f5b103dc95" />
