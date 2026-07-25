# AWS EC2 Log Backup to S3 using Lambda

## Project Overview

This project automatically backs up Linux log files from an EC2 instance to an S3 bucket using AWS Lambda, SSM, SNS, and EventBridge.

---

# AWS Services Used

- EC2
- Lambda
- S3
- SNS
- EventBridge
- IAM
- Systems Manager (SSM)

---

# Project Workflow

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

---

# Step 1: Create EC2 Instance

1. Open AWS Console
2. Go to EC2
3. Click Launch Instance
4. Select Ubuntu AMI
5. Select t2.micro
6. Create Key Pair
7. Allow SSH
8. Launch Instance

---

# Step 2: Create IAM Role for EC2

1. Open IAM
2. Go to Roles
3. Click Create Role
4. Select AWS Service
5. Select EC2

Attach Policies:

- AmazonSSMManagedInstanceCore
- AmazonS3FullAccess

Role Name:

```text
EC2-Role
```

Create role and attach it to EC2 instance.

---

# Step 3: Connect to EC2

```bash
ssh -i key.pem ubuntu@YOUR_PUBLIC_IP
```

---

# Step 4: Install AWS CLI

```bash
sudo apt update -y

sudo apt install unzip -y

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install
```
Another Waay is 
sudo snap install aws-cli --classic

Check version:

```bash
aws --version
```
---

# Step 5: Install SSM Agent

```bash
sudo snap install amazon-ssm-agent --classic

sudo systemctl enable snap.amazon-ssm-agent.amazon-ssm-agent.service

sudo systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
```

Check status:

```bash
sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service
```

---

# Step 6: Create SNS Topic

1. Open SNS
2. Click Create Topic
3. Select Standard
4. Topic Name:

```text
BackupNotification
```

5. Create Topic

---

# Step 7: Create SNS Subscription

1. Open created topic
2. Click Create Subscription
3. Protocol:

```text
Email
```

4. Enter Email Address
5. Create Subscription
6. Open Email
7. Click Confirm Subscription

---

# Step 8: Create Lambda IAM Role

1. Open IAM
2. Go to Roles
3. Click Create Role
4. Select Lambda

Attach Policies:

- AmazonS3FullAccess
- AmazonSSMFullAccess
- AmazonEC2FullAccess
- AmazonSNSFullAccess

Role Name:

```text
Lambda-Role
```

---

# Step 9: Create Lambda Function

1. Open Lambda
2. Click Create Function
3. Select Author From Scratch

Function Name:

```text
LogBackupFunction
```

Runtime:

```text
Python 3.x
```

Execution Role:

```text
Use Existing Role
```

Select:

```text
Lambda-Role
```

Create function.

---

# Step 10: Paste Lambda Function Code

```python
import boto3
import time
from botocore.exceptions import ClientError

# AWS Clients
s3 = boto3.client('s3', region_name='ap-northeast-1')
ssm = boto3.client('ssm', region_name='ap-northeast-1')
sns = boto3.client('sns', region_name='ap-northeast-1')

# Wait until EC2 appears in SSM
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

    # Bucket Name
    bucket_name = 'atul-cha-log-backup-12345'

    # Region
    region = 'ap-northeast-1'

    # EC2 Instance ID
    instance_id = 'i-0ed33c7e6d6b9ece0'

    # SNS Topic ARN
    sns_topic = 'arn:aws:sns:ap-northeast-1:886274844949:BackupNotification'

    # Linux Log Path
    log_path = "/var/log/"

    # Create bucket if not exists
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

    # Wait for SSM connection
    wait_for_ssm(instance_id)

    # Command to upload Linux logs to S3
    command = f"sudo aws s3 sync {log_path} s3://{bucket_name}/ec2-logs/{instance_id}/"

    # Send command to EC2
    response = ssm.send_command(
        InstanceIds=[instance_id],
        DocumentName="AWS-RunShellScript",
        Parameters={
            'commands': [command]
        }
    )

    print("Backup Command Sent")

    # Send SNS Email Notification
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

Deploy Lambda function.

---
# Lambda Configuration

## Execution Role

Select:

```text
create new role 
```

Choose:

```text
Lambda-Role
```



# Step 11: Add EventBridge Trigger

1. Open Lambda Function
2. Click Add Trigger
3. Select EventBridge
4. Create New Rule

Rule Name:

```text
DailyBackupTrigger
```

Schedule Expression:

```text
cron(0/5 * * * ? *)
```

5. Add Trigger

This runs Lambda every 5 minutes.

---

# Step 12: Test Lambda Function

1. Open Lambda
2. Click Test
3. Create Event:

```json
{}
```

4. Run Test

---

# Step 13: Verify Backup in S3

Open S3 Bucket:

```text
atul-cha-log-backup-12345
```

Backup Path:

```text
s3://atul-cha-log-backup-12345/ec2-logs/i-0ed33c7e6d6b9ece0/
```

You will see Linux log files like:

- syslog
- auth.log
- cloud-init.log

---

# Step 14: Verify Email Notification

Check email inbox.

SNS sends:

```text
Backup Success
EC2 logs copied successfully to S3 bucket
```

---

# Final Output

- Automatic Linux log backup
- S3 storage automation
- Email notification using SNS
- Fully automated cloud workflow

---

