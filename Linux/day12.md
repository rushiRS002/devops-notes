### password and User management and Group 
- repeated topic from day 10
  
# Linux Password & Group Management

## Using passwd Command
**root user can exicute command**

Change password:
```bash
passwd username  ## root user chnage normall user password 
```

Lock user:
```bash
passwd -l username
```

Unlock user:
```bash
passwd -u username
```
---

## Account Locking & Expiration

```bash
chage -l username   ## Check current policy
chage -M 90 username    ## Set maximum password validity to 90 days
chage -E 2026-10-15 username    ## Set account expiry date ; After 31 Dec 2025, user cannot log in (even with correct password)
```

---

## Understanding /etc/shadow Fields
- `cat` --> to read full file 
- `head` ----> to read file from start 
- `head -n 10 file.txt` ---> read file first 10 lines 
- `tail` ----> to raed file from last 
- `tail -n 10 file.txt` ---> read files from bottom 10 lines 

| Field | Description |
|----|---------|
| Username | Account name |
| Password | Encrypted password |
| Last change | Days since 1970 |
| Min | Min days |
| Max | Max days |
| Warn | Warning |
| Inactive | Inactive days |
| Expire | Expiry date |

---
<img width="1155" height="698" alt="Screenshot 2026-01-28 at 9 25 56 PM" src="https://github.com/user-attachments/assets/92cef543-79e3-4700-a978-cf375933ad78" />

---
<img width="1074" height="666" alt="Screenshot 2026-01-28 at 9 26 20 PM" src="https://github.com/user-attachments/assets/9871cd32-9855-4dad-8054-cad7ded34143" />

---
<img width="951" height="520" alt="Screenshot 2026-01-28 at 9 26 37 PM" src="https://github.com/user-attachments/assets/abe51d0c-0849-4bbe-b87b-ca26186b4ea1" />

---
<img width="927" height="486" alt="Screenshot 2026-01-28 at 9 26 51 PM" src="https://github.com/user-attachments/assets/1c7620c5-91c2-48d1-a191-60408e840e96" />

---

## Using chage Command

```bash
chage username
```

`chage` stands for **"change age."** It manages password aging settings per user.

### View Current Aging Info

```bash
sudo chage -l username
```

**Output example:**
```
Last password change                    : Jan 01, 2024
Password expires                        : Apr 01, 2024
Password inactive                       : May 01, 2024
Account expires                         : never
Minimum number of days between changes  : 0
Maximum number of days between changes  : 90
Number of days of warning before expiry : 7
```

### Common chage Flags

| Command | What it does |
|---------|--------------|
| `chage -l username` | List all aging info |
| `chage -M 90 username` | Set max password age to 90 days |
| `chage -m 5 username` | Set min days before change to 5 |
| `chage -W 7 username` | Warn 7 days before expiry |
| `chage -I 30 username` | Lock account 30 days after expiry |
| `chage -E 2025-06-01 username` | Set account expiry date |
| `chage -E -1 username` | Remove account expiry |
| `chage username` | Interactive mode (prompts for each value) |

### Quick Example

```bash
# Set a full policy for user spiderman
sudo chage -M 90 -m 5 -W 7 -I 30 spiderman

# Force spiderman to change password on next login
sudo chage -d 0 spiderman
```

> 💡 **Real-life analogy:** `chage -l username` is like checking your passport details page — you can see the issue date, expiry date, and all validity information at a glance.

---

## Introduction to Linux Groups

Groups are used to manage permissions for multiple users.

---

## Types of Groups
```
groups username ## to check the user group
```

| Type | Description |
|----|-------------|
| Primary | Default group |
| Secondary | Additional groups |

---

## Creating & Deleting Groups

```bash
groupadd devops  ## New group create
groupdel devops   ## Existing group delete
```
##Creating & Deleting user 
```
userdel username
```
---

## Modifying Groups

```bash
groupmod -n newgroup oldgroup  ## changing group name 
```

---

## Managing Group Membership

```bash
usermod -aG devops devuser  # to add user in group 
gpasswd -d devuser devops   # to remove user from group 
```
- `usermod -aG` safely adds a user to a secondary group, while `gpasswd -d` removes a user from a group.
---

## Viewing Group Info

```bash
groups username
cat /etc/group 
```



---
