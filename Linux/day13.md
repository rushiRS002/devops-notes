## Archiving and Compressing Files

Archivinng:
- The process of combining multiple files into a single file, often for easier storage or transfer.
tar command:
- `tar -cvf archive_name.tar /path/to/directory_or_files` : Create a tar archive.
- `tar -xvf archive_name.tar` : Extract a tar archive.

Compressing:
- The process of reducing the size of a file or archive to save disk space or reduce transfer time.
- Commonly used tools: `gzip`, `bzip2`, `xz`.
gzip command:
- `gzip filename` : Compress a file, resulting in `filename.gz`.
- `gunzip filename.gz` : Decompress a `.gz` file.

bzip2 command:
- `bzip2 filename` : Compress a file, resulting in `filename.bz2`.
- `bunzip2 filename.bz2` : Decompress a `.bz2` file

xz command:
- `xz filename` : Compress a file, resulting in `filename.xz`.
- `unxz filename.xz` : Decompress a `.xz` file.

tar
- convert tar file to normal file
- `tar -xvf file.tar` conver file in normal file/dir
## cron jobs
[](https://github.com/mukundDeo9325/CDEC-B-65/blob/main/linix-CDEC/images/crontab-explanation-1.png)<img width="800" height="450" alt="image" src="https://github.com/user-attachments/assets/93731bee-3e8e-49c1-b2e1-33db590e8020" />

crontab field structure:
* minute (0-59)
* hour (0-23)
* day of month (1-31)
* month (1-12)
* day of week (0-7) (0 or 7 = Sunday)
* command to be executed

crontab commands:
to install crontab 
update the server 
```bash
sudo apt update -y
```

```bash
sudo apt install cron
```

* `crontab -e` : Edit the crontab file for the current user
* `crontab -l` : List the current user's crontab entries
* `sudo crontab -e` : Edit the crontab file for the root user
* `sudo crontab -l` : List the root user's crontab entries
example cron job to run a script every day at 2am:
```0 2 * * * /path/to/script.sh
```





## to change the edditor in crontab 
- make changes in .bashrc file
```
vim ~/.bashrc
```
```
# Set default editor
export EDITOR=vim
export VISUAL=vim
```
```
source ~/.bashrc
```



---


# Day 13: Automation and Data Handling in Linux

## Overview of Archiving
Archiving is the process of combining multiple files and directories into a single file. This is especially useful for backups, data transfer, and organization.

### Common Use Cases:
1. **Backups:** Safeguard data by creating archives that can be stored securely.
2. **Data Transfer:** Simplify sharing by bundling multiple files into a single archive.
3. **Organization:** Group related files for better management.

## Creating and Extracting Archives with `tar`

### **Creating an Archive:**
```bash
# Syntax:
tar -cvf archive_name.tar file1 file2 directory/

# Example:
tar -cvf project_backup.tar project_folder/
```
- **Options:**
  - `-c`: Create an archive.
  - `-v`: Verbose output (show progress).
  - `-f`: Specify the file name.

### **Extracting an Archive:**
```bash
# Syntax:
tar -xvf archive_name.tar

# Example:
tar -xvf project_backup.tar
```
- **Options:**
  - `-x`: Extract an archive.

### **Managing Archive Contents:**
- **View Contents of an Archive:**
```bash
tar -tvf archive_name.tar
```

## Introduction to Compression
Compression reduces file sizes by encoding data efficiently. It helps save storage and speeds up data transfer.

### **Common Compression Formats:**
- **gzip:** Good compression and widely supported.
- **bzip2:** Better compression than gzip but slower.
- **xz:** Best compression but slowest.

### **Using gzip and gunzip:**
- **Compress a File:**
```bash
gzip file_name
```
- **Decompress a File:**
```bash
gunzip file_name.gz
```

### **Using bzip2 and bunzip2:**
- **Compress a File:**
```bash
bzip2 file_name
```
- **Decompress a File:**
```bash
bunzip2 file_name.bz2
```

### **Using xz and unxz:**
- **Compress a File:**
```bash
xz file_name
```
- **Decompress a File:**
```bash
unxz file_name.xz
```

## Combining Archiving and Compression

### **Create a Compressed tar Archive:**
- Using gzip:
```bash
tar -czvf archive_name.tar.gz file1 file2 directory/
```
- Using bzip2:
```bash
tar -cjvf archive_name.tar.bz2 file1 file2 directory/
```
- Using xz:
```bash
tar -cJvf archive_name.tar.xz file1 file2 directory/
```

### **Extract a Compressed tar Archive:**
- Using gzip:
```bash
tar -xzvf archive_name.tar.gz
```
- Using bzip2:
```bash
tar -xjvf archive_name.tar.bz2
```
- Using xz:
```bash
tar -xJvf archive_name.tar.xz
```

## Practical Examples
### **Compressing Files:**
```bash
# Compress using gzip:
tar -czvf documents.tar.gz Documents/

# Compress using bzip2:
tar -cjvf logs.tar.bz2 Logs/

# Compress using xz:
tar -cJvf archive.tar.xz Folder/
```

### **Decompressing Files:**
```bash
# Decompress using gzip:
tar -xzvf documents.tar.gz

# Decompress using bzip2:
tar -xjvf logs.tar.bz2

# Decompress using xz:
tar -xJvf archive.tar.xz
```

## Introduction to CronTab
CronTab allows scheduling of recurring tasks such as backups, system updates, and cleanup scripts.

### **Understanding the CronTab Syntax:**
CronTab entries have five fields:
```
* * * * * command_to_execute
- - - - -
| | | | |
| | | | +----- Day of the week (0 - 7) (Sunday = 0 or 7)
| | | +------- Month (1 - 12)
| | +--------- Day of the month (1 - 31)
| +----------- Hour (0 - 23)
+------------- Minute (0 - 59)
```

### **Creating and Managing Cron Jobs:**
- **View Existing Cron Jobs:**
```bash
crontab -l
```
- **Edit Cron Jobs:**
```bash
crontab -e
```
- **Delete All Cron Jobs:**
```bash
crontab -r
```

### **Practical Examples:**
1. **Automate Backups Every Day at Midnight:**
```bash
0 0 * * * tar -czvf /backup/home_$(date +\%F).tar.gz /home/user/
```
2. **Clean Temporary Files Every Week:**
```bash
0 0 * * 0 rm -rf /tmp/*
```
3. **Run a System Update Every Month:**
```bash
0 2 1 * * apt-get update && apt-get upgrade -y
```

### **Validate Cron Jobs:**
Use the `journalctl` command to check logs and ensure jobs run as scheduled:
```bash
journalctl -u cron
```

---

# Cron Jobs - Automated Task Scheduling in Linux

This guide demonstrates how to schedule tasks in Linux using `cron`. Below are various examples of creating files, directories, and deleting them automatically based on specific schedules.

---

## 1. **Create a File on 29th Dec 2024**

This cron job creates a file `/abc.txt` on **29th December 2024 at midnight (00:00)**.

### Cron Job:

```bash
0 0 29 dec * /bin/touch /abc.txt
```

### Steps:

1. Change the system date to simulate the execution time:

    ```bash
    sudo date -s "23:59:50 28 dec 2024"
    ```

2. Verify the cron job is added by running:

    ```bash
    crontab -l
    ```

---

## 2. **Delete File on 30th Dec 2024**

This cron job deletes the file `/abc.txt` on **30th December 2024 at midnight (00:00)**.

### Cron Job:

```bash
0 0 30 dec * /bin/rm -f /abc.txt
```

### Steps:

1. Change the system date to simulate the execution time:

    ```bash
    sudo date -s "23:59:50 29 dec 2024"
    ```

---

## 3. **Create Directory on 10th September 2024 at 20:45**

This cron job creates a directory `/root/abc` on **10th September 2024 at 20:45**.

### Cron Job:

```bash
45 20 10 sep * /bin/mkdir -p /root/abc/
```

### Steps:

1. Change the system date to simulate the execution time:

    ```bash
    sudo date -s "2024-09-10 20:45:00"
    ```

---

## 4. **Create Directory Every Weekday (Monday to Friday) at 7:00 AM**

This cron job creates a directory `/home/student/weekly_dir` **every weekday at 7:00 AM** (Monday to Friday).

### Cron Job:

```bash
0 7 * * 1-5 /bin/mkdir -p /home/student/weekly_dir
```

### Steps:

1. Change the system date to simulate the execution time on **Monday at 7:00 AM**:

    ```bash
    sudo date -s "2024-09-09 07:00:00"
    ```

---

## 5. **Create a File Every Minute**

This cron job creates a new file every minute with a unique timestamp in the name. The file is saved under `/home/student/`.

### Cron Job:

```bash
* * * * * /bin/touch /home/student/minute_file_$(date +\%Y\%m\%d_\%H\%M\%S).txt
```

### Explanation:
- `* * * * *`: Runs the task **every minute**.
- `/bin/touch`: Command to create an empty file.
- `/home/student/minute_file_$(date +\%Y\%m\%d_\%H\%M\%S).txt`: 
  - Creates a file with a unique name using the date and time format.
  - `$(date +\%Y\%m\%d_\%H\%M\%S)` generates a timestamp like `20231221_153001`.

---

## Additional Notes:

- **Checking Cron Jobs**: You can check your existing cron jobs with:

    ```bash
    crontab -l
    ```

- **Editing Cron Jobs**: You can edit your cron jobs using:

    ```bash
    crontab -e
    ```

- **Checking Cron Logs**: If your cron job is not working, check the logs to see if there were any issues:

    ```bash
    sudo grep CRON /var/log/syslog
    ```

- **Cron Service**: Ensure the cron service is running:

    ```bash
    sudo systemctl status cron
    ```

    If not running, you can start it using:

    ```bash
    sudo systemctl start cron
    ```

---
## Archiving and compression 
```
root@ubuntu:~$ ls
backup.tar
root@ubuntu:~$ cp /mnt/backup.tar ./backup1.tar
root@ubuntu:~$ cp /mnt/backup.tar ./backup2.tar
root@ubuntu:~$ cp /mnt/backup.tar ./backup3.tar
root@ubuntu:~$ ls
backup.tar  backup1.tar  backup2.tar  backup3.tar
root@ubuntu:~$ ls -lh 
total 480K
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup.tar
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup1.tar
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup2.tar
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup3.tar
root@ubuntu:~$ gzip backup1.tar 
root@ubuntu:~$ ls
backup.tar  backup1.tar.gz  backup2.tar  backup3.tar
root@ubuntu:~$ ls -lh
total 388K
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup.tar
-rw-r--r-- 1 root root  26K Mar 30 07:33 backup1.tar.gz
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup2.tar
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup3.tar
root@ubuntu:~$ bzip2 backup2.tar 
root@ubuntu:~$ ls -lh
total 296K
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup.tar
-rw-r--r-- 1 root root  26K Mar 30 07:33 backup1.tar.gz
-rw-r--r-- 1 root root  25K Mar 30 07:33 backup2.tar.bz2
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup3.tar
root@ubuntu:~$ xz backup3.tar 
root@ubuntu:~$ ls -lh
total 200K
-rw-r--r-- 1 root root 120K Mar 30 07:33 backup.tar
-rw-r--r-- 1 root root  26K Mar 30 07:33 backup1.tar.gz
-rw-r--r-- 1 root root  25K Mar 30 07:33 backup2.tar.bz2
-rw-r--r-- 1 root root  24K Mar 30 07:33 backup3.tar.xz
root@ubuntu:~$ 

```
```
3  mkdir dir 
    4  mkdir test
    5  touch file{1..10}.txt
    6  ls
    7  rm filesystem
    8  ls
    9  cp -r /etc/*.conf ./dir/
   10  ls
   11  ls dir/
   12  cd
   13  ls
   14  tar -cvf backup.tar ./
   15  ls
   16  cp backup.tar /mnt
   17  ls /mnt/
   18  rm ./*
   19  ls
   20  rm -r dir
   21  rm -r test
   22  ls
   23  cp /mnt/backup.tar 
   24  cp /mnt/backup.tar ./
   25  ls
   26  cp /mnt/backup.tar ./backup1.tar
   27  cp /mnt/backup.tar ./backup2.tar
   28  cp /mnt/backup.tar ./backup3.tar
   29  ls
   30  ls -lh 
   31  gzip backup1.tar 
   32  ls
   33  ls -lh
   34  bzip2 backup2.tar 
   35  ls -lh
   36  xz backup3.tar 
   37  ls -lh
   38  history
```





