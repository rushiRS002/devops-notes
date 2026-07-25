# 🚀 AWS EC2 Log Backup to S3 using Lambda

> 📚 Automated AWS Serverless Backup Project  
> ⚡ EC2 Logs Backup using Lambda, S3, SSM, SNS & EventBridge

---

# 🔹 Project Overview

This project automatically backs up Linux log files from an Amazon EC2 instance to an Amazon S3 bucket using AWS Lambda and AWS Systems Manager (SSM).

It also sends real-time email notifications using Amazon SNS after successful backup execution.

---

# 🔹 AWS Services Used

| Service | Purpose |
|---|---|
| EC2 | Linux server |
| Lambda | Automation logic |
| S3 | Backup storage |
| SNS | Email notifications |
| EventBridge | Scheduled trigger |
| SSM | Secure remote command execution |
| IAM | Access management |

---

# 🔹 Architecture Flow

```text
EventBridge
     ↓
AWS Lambda
     ↓
SSM Run Command
     ↓
EC2 Instance
     ↓
Amazon S3
     ↓
SNS Email Alert
```

---

# 🔹 Step 1: Launch EC2 Instance

- Launch Ubuntu EC2 instance
- Allow SSH traffic
- Connect using SSH

```bash
ssh -i "your-key.pem" ubuntu@YOUR_PUBLIC_IP
```

---

# 🔹 Step 2: Create IAM Role for EC2

Attach Policies:

- AmazonSSMManagedInstanceCore
- AmazonS3FullAccess

Role Name:

```text
EC2-Role
```

Attach role to EC2 instance.

---

# 🔹 Step 3: Install AWS CLI

```bash
sudo apt update -y

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

aws --version
```

---

# 🔹 Step 4: Install SSM Agent

```bash
sudo snap install amazon-ssm-agent --classic

sudo systemctl enable snap.amazon-ssm-agent.amazon-ssm-agent.service

sudo systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
```

---

# 🔹 Step 5: Create SNS Topic

Create SNS Topic:

```text
BackupNotification
```

Subscribe your email and confirm subscription.

---

# 🔹 Step 6: Create IAM Role for Lambda

Attach Policies:

- AmazonSSMFullAccess
- AmazonS3FullAccess
- AmazonEC2FullAccess
- AmazonSNSFullAccess

Role Name:

```text
Lambda-Role
```

---

# 🔹 Step 7: Create Lambda Function

Configuration:

- Function Name: LogBackupFunction
- Runtime: Python 3.x
- Execution Role: Lambda-Role

Timeout:

```text
2 Minutes
```

---

# 🔹 Step 8: Lambda Code

```python
import boto3

region = 'ap-south-1'

s3 = boto3.client('s3', region_name=region)
ssm = boto3.client('ssm', region_name=region)
sns = boto3.client('sns', region_name=region)

def lambda_handler(event, context):

    bucket_name = 'your-bucket'
    instance_id = 'your-instance-id'

    command = f"sudo aws s3 sync /var/log/ s3://{bucket_name}/logs/{instance_id}/"

    ssm.send_command(
        InstanceIds=[instance_id],
        DocumentName="AWS-RunShellScript",
        Parameters={
            'commands': [command]
        }
    )

    sns.publish(
        TopicArn='your-sns-topic-arn',
        Subject='Backup Success',
        Message='EC2 logs copied successfully'
    )

    return {
        'statusCode': 200,
        'body': 'Backup Completed'
    }
```

Deploy Lambda.

---

# 🔹 Step 9: Configure EventBridge Trigger

Create EventBridge Rule:

```bash
cron(0/5 * * * ? *)
```

Runs every 5 minutes.

---

# 🔹 Step 10: Test Project

- Open Lambda
- Click Test
- Use event:

```json
{}
```

Expected Result:

```text
Status: Success
```

---

# 🔹 Verify Backup Files in S3

Expected Path:

```text
s3://your-bucket/logs/instance-id/
```

Expected Files:
- syslog
- auth.log
- cloud-init.log

---

# 🔹 Verify Email Alerts

Check inbox for:

```text
Subject: Backup Success
```

---

# 🔥 Key Concepts

- Lambda = Automation
- SSM = Secure command execution
- S3 = Backup storage
- SNS = Notifications
- EventBridge = Scheduler

---

# 🔐 Security Best Practices

- Use IAM least privilege
- Enable S3 encryption
- Restrict public access
- Avoid hardcoded credentials

---

# 🏆 Interview Questions

- What is AWS Lambda?
- What is SSM?
- Why use EventBridge?
- How does SNS work?
- Benefits of serverless architecture?

---

# 🎯 Final Concept

This project automates EC2 log backups using AWS serverless services. EventBridge triggers Lambda, Lambda uses SSM to execute backup commands on EC2, logs are stored in S3, and SNS sends real-time email notifications.
