# 🚀 SSH into EC2 using PuTTY

## 📌 Step 1: Create EC2 Instance

- Launch an Ubuntu EC2 instance
- Enable **SSH Port 22** in Security Group

---

# 🛠️ Step 2: Download PuTTY Tools

## 🔗 PuTTY Download Link

[Download PuTTY & PuTTYGen](https://www.puttygen.com/download.php)

### Required Tools
- PuTTY
- PuTTYGen.

---

# 🔐 Step 3: Convert `.pem` File to `.ppk`

## Open PuTTYGen

Follow these steps:

1. Click **Load**
2. Select your `.pem` file
3. Click **Save private key**
4. Save file as:

```text
mykey.ppk
```

---

# 💻 Step 4: Open PuTTY

In **Host Name**, enter:

```bash
ubuntu@PUBLIC_IP
```

## Example

```bash
ubuntu@13.233.xx.xx
```

---

# 🔑 Step 5: Attach `.ppk` Key

Go to:

```text
Connection → SSH → Auth
```

Click **Browse**

Select:

```text
mykey.ppk
```

---

# ✅ Step 6: Connect to Server

Click:

```text
Open
```
# 💻 SSH in Terminus

Run the following command to connect using SSH:

```bash
plink -i "C:\Users\yogesh\Downloads\mykey.ppk" ubuntu@PUBLIC_IP
```

## Example

```bash
plink -i "C:\Users\yogesh\Downloads\mykey.ppk" ubuntu@13.233.xx.xx
```

# 🧑‍💻 Author

Created for AWS & DevOps Practice 🚀
# SSH-with-puTTy
