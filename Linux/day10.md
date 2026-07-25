## Managing Users and Permissions in Linux

Umask
```
root user = 022 
local user = 002
```
file permission calculation:
* full permission - umask value = defalt permission


1. System Defined File Types
    - Regular files
    - Directories
    - Symbolic links
    - Device files (block and character)
    - Pipes and sockets


2. User Defined File Types
    - Custom file extensions (e.g., .txt, .jpg, .exe)
    - Configuration files (e.g., .conf, .ini)

## Linux File System Security
Linux File System Security restricts user to access the files and directories. User require permissions to access files or directories. “ls -l” or “ll” command can be used to check security of any file or directory. Above command will display contents from directory along with its security details. 

### Linux
1. File type.
2. Owner Permissions.
3. Group Permissions.
4. Other User Permission.
5. Link Count.
6. Owner of file/directory.
7. Group Owner of File or Directory. 
8. File Size.
9. Creation Date and Time. 
10. File/Directory Name.

file types in Linux 
- `-` : Regular file
- `d` : Directory
- `l` : Symbolic link
- `b` : Block device file
- `c` : Character device file
- `p` : Pipe


(-) Regular File: The regular file is a most common file type found on the Linux system. It governs all different files such us text files, images, binary files, shared libraries, etc. 

(d) Directory: Directory is second most common file type found in Linux. Directory can be created with the mkdir command.

(l) Symbolic Link: With symbolic links an administrator can assign a file or directory multiple identities. Symbolic link can be thought of as a pointer to an original file.

## File Permissions
read (r) (4) : Read permission allows a user to open and read the contents of a file. In case of directory, read permission allows user to list the contents of the directory.

write (w) (2) : Write permission allows a user to modify or delete the contents of a file. In case of directory, write permission allows user to add, delete, or rename files within the directory.

execute (x) (1) : Execute permission allows a user to run a file as a program or script. In case of directory, execute permission allows user to access files and subdirectories within the directory.


---
## Importance of File Permissions in Linux
-rw-rw-r-- 1 new new 0 Mar 25 08:16 file.txt
|-> file type 
rw- ---> owners permission of that file 
rw- ---> Group permisssion of that file 
r-- ---> other than owner and group whoever is the user got only reading 

r - read - 4  
w - write - 2
x - exicute - 1 

file full permission 666 
rw-rw-rw-  full permission - umask (local 002 / root 022) = defalt permission 
666 - 002 = 664 rw-rw-r--

---
umask 
defalt umask local 002 | root 022

## pile permission using chmod 
```
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
---------- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod u+r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-r-------- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod u+w test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw------- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod g+r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-r----- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod g+w test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-rw---- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod o+r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-rw-r-- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod o+w test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-rw-rw- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ 
```
```
ubuntu@ip-172-31-8-7:~$ chmod o-w test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-rw-r-- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod o-r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-rw---- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod o=r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rw-rw-r-- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod u=r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-r--rw-r-- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod u=rwx test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-rwxrw-r-- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ chmod u=r test
ubuntu@ip-172-31-8-7:~$ ls -l
total 0
-r--rw-r-- 1 ubuntu ubuntu 0 Mar 25 08:32 test
ubuntu@ip-172-31-8-7:~$ 
```

















## Explanation of rwx (read, write, execute) Permissions
```
r ---> read ---> 4 
w ----> write ---> 2
x ---> exicute ---> 1

u ---> user 
g ----> group 
o ---> other 

+ ---> add 
- ---> remove 
= ---> at least this level 

r --> read 
w ---> write 
x ---> exicute permission 
```
```
chmod ugo +-= rwx file.txt
```
## How Permissions are Displayed with ls -l
```
777 dir full permission rwxrwxrwx
                         7  7  7
666 file full permission rw-rw-rw-
                          6  6  6
```
## Example: Breaking Down the Permission String (e.g., -rwxr-xr--)
```
drwx(owner)rwx(group)r-x(other)
|
file type 
```
## Introduction to File Types
- system define file ## system has an acess to this file and managing them
```
d ----> dir 
- ----> normal file 
l ----> link file 
p ----> pipe file 
s ----> soket file 
```
- user define file 
`.txt .mp4  .mp3 .md .yaml ---> user has an acess to this file `

---






