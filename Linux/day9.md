
## user and group management

types of users:
* root user (superuser) - has all permissions
* local users - limited permissions
* system users - used by system services

<img width="856" height="553" alt="Screenshot 2026-03-23 at 12 45 28 PM" src="https://github.com/user-attachments/assets/5605be9d-1bd1-4c3f-8028-32a6d8601567" />

useradd command:
* `useradd username` - create a new user
* `passwd username` - set password for the user
* `deluser username` - delete a user  
* `userdel username` - delete a user
  
- useradd vs adduser:
* `useradd` is a low-level utility for adding users.
* `adduser` is a more user-friendly script that uses `useradd` in the background. 

groupadd command:
* `groupadd groupname` - create a new group
* `delgroup groupname` - delete a group    
* `usermod -aG groupname username` - add user to a group
* `gpasswd groupname` - add password to group
* `gpasswd -A username groupname` - add group admin
* `usermod -aG groupname username` - add user to a group
viewing users and groups:
* `cat /etc/passwd` - view all users
* `cat /etc/group` - view all groups    

## Task (in session)
1. login to the server
2. switch to root user using `sudo -i` command 
3. create an user user1 with useradd command
4. crete an user user2 with adduser commnd
5. chek difference form user related file
 - /etc/passwd
 - /etc/shadow
 - /etc/group
 - /etc/gshadow

**whenever new user is added in linux system this files get an entry of user in the files**

## History
```
useradd Ajay
    2  cat /etc/passwd
    3  less /etc/passwd
    4  less -n 10 /etc/passwd
    5  tail -n 5 /etc/passwd
    6  tail -n 5 /etc/shadow
    7  tail -n 5 /etc/group
    8  tail -n 5 /etc/gshadow
    9  adduser prathmesh
   10  tail -n 5 /etc/passwd
   11  tail -n 5 /etc/shadow
   12  tail -n 5 /etc/group
   13  tail -n 5 /etc/gshadow
   14  ls /home
   15  useradd -m dev
   16  ls /home
   17  userdel Ajay
   18  tail -n 5 /etc/passwd
   19  su prathmesh
   20  su dev
   21  adduser pragati
   22  ls /home
   23  tail -n 5 /etc/passwd
   24  su pragati
   25  ls /home/prathmesh
   26  cat /home/prathmesh/config.json 
   27  history
```

## how to cerete an sudo user 
- login as root user **root user has acess of 19 dir**
- make changes in /etc/sudoers file
- `vim /etc/sudoers`
- add an Entry of local user under admin

- `username ALL=(ALL) ALL`
<img width="722" height="88" alt="Screenshot 2026-03-23 at 1 49 40 PM" src="https://github.com/user-attachments/assets/bdb80134-2c6d-41b3-8135-0ab1a5a99a5d" />

**save and quit file !**
- you have crete an local user as sudo user

- https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups












