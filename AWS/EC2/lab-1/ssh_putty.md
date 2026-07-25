# SSH into AWS EC2 using PuTTY (Windows)

## 1. Create EC2 Instance

Create an EC2 instance from [Amazon EC2 Console](https://console.aws.amazon.com/ec2?utm_source=chatgpt.com)

While creating the instance:

* Allow **SSH** in Security Group
* Port: **22**
* Source:

  * `My IP` (Recommended)
  * or `0.0.0.0/0` (Not recommended for production)

Example Security Group Rule:

| Type | Protocol | Port | Source |
| ---- | -------- | ---- | ------ |
| SSH  | TCP      | 22   | My IP  |

---

# Install PuTTY and PuTTYgen

## 2. Download PuTTYgen Tool

PuTTYgen is used to convert AWS `.pem` key into `.ppk` format.

Download:

* [PuTTYgen Download](https://puttygen.com/download.php?val=49&utm_source=chatgpt.com)

Install PuTTYgen on Windows.

---

# Convert `.pem` to `.ppk`

## 3. Convert AWS Key Pair

AWS provides key in `.pem` format.

PuTTY requires `.ppk` format.

### Steps

Open **PuTTYgen**

1. Click **Load**
2. Select your `.pem` file
3. Click **Save private key**
4. Save file as:

```bash
mykey.ppk
```

Now your key is ready for PuTTY.

---

# Connect EC2 using PuTTY

## 4. Download PuTTY

Download:

* [PuTTY Download](https://www.putty.org/?utm_source=chatgpt.com)

If PuTTY is already installed, no need to download again.

---

## 5. Configure PuTTY

Open **PuTTY**

### In Host Name

Enter:

```bash
ubuntu@PUBLIC_IP
```

Example:

```bash
ubuntu@13.233.xx.xx
```

### Port

```bash
22
```

### Connection Type

Select:

```bash
SSH
```

---

# Attach `.ppk` Key

## 6. Add Private Key

From left-side menu:

```text
Connection → SSH → Auth → Credentials
```

Then:

1. Click **Browse**
2. Select:

```bash
mykey.ppk
```

---

# Open SSH Connection

## 7. Login to EC2

Click:

```text
Open
```

First time PuTTY will show a security alert.

Click:

```text
Accept
```

Now terminal opens and you are connected to the EC2 instance.

---

# SSH using Terminus / Plink

## 8. Connect using Command Line

Run:

```bash
plink -i "C:\Users\YourUsername\Downloads\mykey.ppk" ubuntu@PUBLIC_IP
```

Example:

```bash
plink -i "C:\Users\Mukund\Downloads\mykey.ppk" ubuntu@13.233.xx.xx
```

---

# Important Notes

## Default Usernames for EC2 AMIs

| AMI          | Username |
| ------------ | -------- |
| Ubuntu       | ubuntu   |
| Amazon Linux | ec2-user |
| RHEL         | ec2-user |
| CentOS       | centos   |
| Debian       | admin    |

---

# Troubleshooting

## Permission Denied Error

Check:

* Correct `.ppk` key attached
* Security Group allows SSH on port 22
* Correct username used
* EC2 instance is running

---

# Verify EC2 Public IP

From EC2 dashboard:

```text
EC2 → Instances → Select Instance → Public IPv4 Address
```

Use this IP in PuTTY.
