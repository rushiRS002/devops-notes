## AWS CLI 
 - Introduction to AWS CLI
 - https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
 - Configuring and using AWS CLI for S3 and EC2 Management
 - Practical Demo of CLI Commands
---

## AWS CLI installation and use 

```bash
sudo apt update -y
apt install unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

 - Configuring and using AWS CLI for S3 and EC2 Management
```
sudo snap install aws-cli --classic
```
```bash
aws sts get-caller-identity
```


```bash
aws configure
``` 
    - Access Key ID: YOUR_ACCESS_KEY
    - Secret Access Key: YOUR_SECRET_KEY
    - Default region name: us-east-1
    - Default output format:


- https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html ## refrance page to use command 
```bash
aws s3 ls
aws s3 mb s3://your-bucket-name
aws s3 cp your-file.txt s3://your-bucket-name/
aws s3 cp s3://your-bucket-name/your-file.txt .
```

 - EC2 Management
```bash
aws ec2 describe-instances
aws ec2 start-instances --instance-ids <instanceid>
aws ec2 stop-instances --instance-ids <instanceid>
```


--- 
# Introduction to AWS CLI

AWS Command Line Interface (CLI) is a powerful tool that enables users to interact with AWS services using commands. It helps automate tasks and manage AWS services efficiently.

---

## AWS CLI Setup and Configuration

### Step 1: Install AWS CLI
1. Download the AWS CLI installer for your operating system.
2. Follow the installation instructions provided [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

### Step 2: Configure AWS CLI
Run the following command to configure AWS CLI:
```bash
aws configure
```
Provide the following details:
- Access Key ID
- Secret Access Key
- Default Region
- Output Format (e.g., JSON, table, text)

### Step 3: Verify Installation
Run this command to verify:
```bash
aws --version
```

---

## AWS CLI Commands for S3 Management

### 1. List S3 Buckets
```bash
aws s3 ls
```

### 2. Create a Bucket
```bash
aws s3 mb s3://<bucket-name>
```

### 3. Upload a File to S3
```bash
aws s3 cp <file-path> s3://<bucket-name>/
```

### 4. List Objects in a Bucket
```bash
aws s3 ls s3://<bucket-name>/
```

### 5. Delete an Object
```bash
aws s3 rm s3://<bucket-name>/<object-key>
```

### 6. Sync Local Folder to S3
```bash
aws s3 sync <local-folder-path> s3://<bucket-name>/
```
### 7. empty  bucket to S3
```
aws s3 rm s3://cbz14buck  --recursive
```
 ### 8. remove empty to S3
 ```
 
aws s3api delete-bucket --bucket cbz14buck  --region  ap-south-1
```
---

## AWS CLI Commands for EC2 Management

### 1. List EC2 Instances
```bash
aws ec2 describe-instances
```

### 2. Start an EC2 Instance
```bash
aws ec2 start-instances --instance-ids <instance-id>
```

### 3. Stop an EC2 Instance
```bash
aws ec2 stop-instances --instance-ids <instance-id>
```

### 4. Terminate an EC2 Instance
```bash
aws ec2 terminate-instances --instance-ids <instance-id>
```

### 5. Create a Key Pair
```bash
aws ec2 create-key-pair --key-name <key-name>
```

### 6. Create a Security Group
```bash
aws ec2 create-security-group --group-name <group-name> --description "<description>"
```

### 7. Add Inbound Rule to Security Group
```bash
aws ec2 authorize-security-group-ingress --group-id <group-id> --protocol tcp --port 22 --cidr 0.0.0.0/0
```

---

## AWS CLI Commands for IAM Management

### 1. List IAM Users
```bash
aws iam list-users
```

### 2. Create an IAM User
```bash
aws iam create-user --user-name <user-name>
```

### 3. Attach a Policy to an IAM User
```bash
aws iam attach-user-policy --user-name <user-name> --policy-arn <policy-arn>
```

```
arn:aws:iam::aws:policy/aws-service-role/AccessAnalyzerServiceRolePolicy

arn:aws:ec2:ap-south-1:779926947982:instance/i-0dd3a384be52e130f

```



### 4. Create an IAM Group
```bash
aws iam create-group --group-name <group-name>
```

### 5. Add a User to a Group
```bash
aws iam add-user-to-group --user-name <user-name> --group-name <group-name>
```

### 6. Create a Custom IAM Policy
```bash
aws iam create-policy --policy-name <policy-name> --policy-document file://<policy-document.json>
```

---

## AWS CLI Commands for VPC Management

### 1. List VPCs
```bash
aws ec2 describe-vpcs
```

### 2. Create a VPC
```bash
aws ec2 create-vpc --cidr-block <CIDR-block>
```

### 3. Create a Subnet
```bash
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block <CIDR-block>
```

### 4. Create an Internet Gateway
```bash
aws ec2 create-internet-gateway
```

### 5. Attach Internet Gateway to VPC
```bash
aws ec2 attach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <igw-id>
```

### 6. Create a Route Table
```bash
aws ec2 create-route-table --vpc-id <vpc-id>
```

### 7. Add Route to Route Table
```bash
aws ec2 create-route --route-table-id <route-table-id> --destination-cidr-block 0.0.0.0/0 --gateway-id <igw-id>
```

### 8. Associate Route Table with Subnet
```bash
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <route-table-id>
```

---
.

## Refer:https://docs.aws.amazon.com/cli/latest/

These hands-on tasks will help reinforce the understanding and practical usage of AWS CLI commands.




