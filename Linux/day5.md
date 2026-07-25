# Day 5: Delving Deep into the Linux File System

## Navigating the File System
Understanding how to navigate the Linux file system is fundamental to using the operating system effectively. The Linux file system is organized as a hierarchy, starting from the root directory `/`.

### Key Commands for Navigation:
- **`pwd`**: Prints the current working directory.
- **`cd`**: Changes the current directory.
  - Example: `cd /home/user` navigates to the user's home directory.
  - `cd ..` moves one level up in the directory hierarchy.
  - `cd -` returns to the previous directory.
- **`ls`**: Lists the contents of a directory.
  - Options:
    - `ls -l`: Detailed list view.
    - `ls -a`: Shows hidden files (those starting with `.`).

## File and Directory Management
Linux provides powerful commands for creating, renaming, and organizing files and directories.

### Creating Files and Directories:
- **`touch filename`**: Creates an empty file.
- **`mkdir dirname`**: Creates a new directory.
  - Use `mkdir -p /path/to/dir` to create nested directories.

### Renaming Files and Directories:
- **`mv oldname newname`**: Renames files or directories.
  - Example: `mv file1.txt file2.txt` renames `file1.txt` to `file2.txt`.

### Viewing Directory Structure:
- **`tree`**: Displays a tree-like structure of directories (if installed).
  - Example: `tree /home/user`.
- **`ls -R`**: Lists directory contents recursively.

## Viewing and Editing Files
Linux offers several tools for inspecting and modifying files.

### Viewing Files:
- **`cat filename`**: Displays the contents of a file.
- **`less filename`**: Views file contents one page at a time.
  - Navigate with the `Up`/`Down` keys and press `q` to quit.
- **`head filename`**: Displays the first 10 lines of a file.
- **`tail filename`**: Displays the last 10 lines of a file.
  - Use `tail -f filename` to view live updates (e.g., log files).

### Editing Files:
- **`nano filename`**: Opens the file in a simple text editor.
  - Save changes with `Ctrl + O`, and exit with `Ctrl + X`.
- **`vim filename`**: Opens the file in the Vim editor (advanced users).
  - Press `i` to enter insert mode, `Esc` to exit insert mode, and `:wq` to save and quit.

## Copy, Move, and Delete Files
Efficiently manage files using the following commands:

### Copying Files:
- **`cp source destination`**: Copies files or directories.
  - Options:
    - `cp -r`: Copies directories recursively.
    - `cp -i`: Prompts before overwriting existing files.

### Moving Files:
- **`mv source destination`**: Moves files or directories to a new location.
  - Example: `mv file1.txt /home/user/docs`.

### Deleting Files:
- **`rm filename`**: Deletes a file.
  - Use `rm -i` for confirmation before deletion.
  - **`rm -r directory`**: Deletes a directory and its contents.
  - **`rm -rf directory`**: Forcibly deletes a directory without prompting (use with caution).
---

## creating files and dir with commands 
## - Copy, Move, and Delete Files
# 📂 Linux File System & File Management

---

## Navigating the File System

| Command | Purpose |
|--------|--------|
| `pwd` | Show current directory |
| `ls` | List files |
| `cd /path` | Change directory |
| `cd ..` | Go back one level |
| `cd ~` | Go to home directory |

---

## File and Directory Management

| Command | Purpose |
|-------|--------|
| `mkdir dir` | Create directory |
| `rmdir dir` | Delete empty directory |
| `stat file` | File details |
| `tree` | Directory structure |

---

## Viewing and Editing Files

| Command | Purpose |
|------|-------|
| `cat file` | View file |
| `less file` | Scroll view |
| `head file` | First lines |
| `tail file` | Last lines |
| `nano file` | Edit file |
| `vim file` | Advanced editor |

---

## Copy, Move & Delete Files

| Command | Purpose |
|------|--------|
| `cp a b` | Copy file |
| `cp -r dir1 dir2` | Copy directory |
| `mv a b` | Move/rename |
| `rm file` | Delete file |
| `rm -r dir` | Delete directory |

---










