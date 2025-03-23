# Linux File and Directory Management, User Management, and File Permissions

This note provides a comprehensive guide to managing files, directories, users, and file permissions in Linux. It also covers basic file compression, processing, and text manipulation using commands like `grep`, `pipe`, and `sort`.

---

## **File and Directory Management**

### **Basic Commands**
1. **Check Current Directory**  
   - Command: `pwd`  
   - Description: Displays the present working directory.

2. **Change Directory**  
   - Command: `cd Directory_Name`  
   - Description: Changes the current working directory to the specified directory.

3. **Move to Previous Directory**  
   - Command: `cd ..`  
   - Description: Moves back to the parent directory.

4. **Move to Home Directory**  
   - Command: `cd ~`  
   - Description: Moves to the home directory of the current user.

5. **View Hidden Files**  
   - Command: `ls -a`  
   - Description: Lists all files, including hidden ones, in the directory.

6. **Safer Deletion with Confirmation**  
   - Command: `rm -i file_name`  
   - Description: Prompts for confirmation before deleting a file.

7. **Create a New File**  
   - Command: `touch file_name`  
   - Description: Creates an empty file with the specified name.  
   - Alternative: `cat > file_name` (Creates a file and allows writing into it. Press `Ctrl + D` to save).

8. **Open and View File Content**  
   - Command: `cat file_name`  
   - Description: Displays the content of the specified file.

9. **Check Files and Folders in a Directory**  
   - Command: `ls`  
   - Description: Lists files and folders in the current directory.  
   - Recursive Listing: `ls -R` (Lists all files and folders, including those in subdirectories).

10. **Check File Permissions**  
    - Command: `ls -l`  
    - Description: Lists files and directories with detailed permissions and attributes.

11. **Create a Folder**  
    - Command: `mkdir folder_name`  
    - Description: Creates a new directory with the specified name.

12. **Remove a File**  
    - Command: `rm file_name`  
    - Description: Deletes the specified file.

13. **Remove a Folder**  
    - Command: `rm -r folder_name`  
    - Description: Deletes the specified directory and its contents.

14. **Copy File to Another Folder**  
    - Command: `cp file_name folder_name`  
    - Description: Copies the specified file into the target folder.

15. **Rename a File or Folder**  
    - Command: `mv old_name new_name`  
    - Description: Renames a file or directory. This can also be used to move files or folders.

---

## **User Management**

1. **Create a User Account**  
   - Command: `sudo useradd account_name`  
   - Description: Creates a new user account with the specified name.

2. **Set Password for User Account**  
   - Command: `sudo passwd account_name`  
   - Description: Sets or updates the password for the specified user account.

3. **Check All User Accounts**  
   - Command: `sudo cat /etc/passwd`  
   - Description: Displays a list of all user accounts on the system.

4. **Create a Group**  
   - Command: `sudo groupadd group_name`  
   - Description: Creates a new group with the specified name.

5. **Add User to a Group**  
   - Command: `sudo usermod -aG group_name account_name`  
   - Description: Adds a user to the specified group.

6. **Delete a User Account**  
   - Command: `sudo userdel -r account_name`  
   - Description: Deletes the specified user account. The `-r` option removes the user's home directory and mail spool.

7. **Delete a Group**  
   - Command: `sudo groupdel group_name`  
   - Description: Deletes the specified group.

8. **Switch to Another User**  
   - Command: `sudo su user_name`  
   - Description: Switches to the specified user account.

9. **Check User Permissions and Groups**  
   - Command: `id user_name`  
   - Description: Displays the UID, GID, and group memberships of the specified user.

10. **Add User to Kali Group (or Any System Group)**  
    - Command: `sudo usermod -aG kali account_name`  
    - Description: Adds a user to the `kali` group or any other specified system group.

---

## **File Permissions**

### **Permission Types**
- **Read (r)**: View or read the contents of a file or list directory contents.
- **Write (w)**: Modify the file's contents or create/delete files in a directory.
- **Execute (x)**: Run the file as a program or access a directory.

### **Permission Levels**
- **Owner**: The user who owns the file.
- **Group**: A group of users associated with the file.
- **Others**: All other users on the system.

### **Permission Format**
- Example: `-rwxr-xr--`
  - First character: `-` (file), `d` (directory), or `l` (link).
  - Next nine characters: Owner (`rwx`), Group (`r-x`), Others (`r--`).

### **Changing Permissions**
- **Symbolic Mode**:  
  - Example: `sudo chmod u+x file` (adds execute permission for the owner).
  - `u` (owner), `g` (group), `o` (others), `a` (all).
  - `+` (add), `-` (remove), `=` (set).
- **Numeric Mode**:  
  - Example: `sudo chmod 755 file`  
  - Each digit represents permissions: `4` = read, `2` = write, `1` = execute.

### **Changing Ownership**
- **chown**: Change the ownership of a file.  
  - Example: `sudo chown user file` (changes the owner).  
  - `sudo chown user:group file` (changes owner and group).
- **chgrp**: Change the group ownership of a file.  
  - Example: `sudo chgrp group file`.

---

## **File Compression and Processing**

### **ZIP Compression**
- Create ZIP: `zip filename.zip file1 file2 file3 ...`
- Extract ZIP: `unzip filename.zip`
- Compress with gzip: `gzip -d filename.gz`
- Decompress gzip: `gunzip filename.gz`

### **BZIP2 Compression**
- Create BZIP2 Archive: `bzip2 file1 file2 ...`
- Decompress BZIP2: `bunzip2 file1.bz2 file2.bz2 ...`

### **TAR Compression**
- Create TAR Archive: `tar -cvf archive.tar file1 file2 ...`
- Compress TAR with GZIP: `tar -czvf archive.tar.gz file1 file2 ...`
- Extract TAR Archive: `tar -xvf archive.tar`
- Decompress TAR.GZ Archive: `tar -xzvf archive.tar.gz`

### **Create ZIP with Password**
- Encrypt while creating ZIP: `zip -e filename.zip file1 file2 ...`
- Extract Password-Protected ZIP: `unzip filename.zip`

---

## **File Processing**

### **Check Running Processes**
- List Processes with IDs: `ps aux`

### **Stop a Process**
- Kill Process by PID: `kill PID`
- Force Kill: `kill -9 PID`

---

## **Grep, Pipe, and Sorting**

### **Multiple Commands in One Line Using Pipe**
- Example: `cat test.txt | echo "Durjoy" >> test.txt | cat test.txt`

### **Search a Component from a File Using Grep**
- Example: `cat test.txt | grep Durjoy`

### **Sorting a File**
- Sort: `sort test.txt`
- Reverse Sort: `sort -r test.txt`

---

## **Visual Editor (vi) Commands**
- Open: `vi textfile` (default visual mode).
- Insert: Press `i`.
- Save: Go to visual mode (press `Esc`) and then press `:w`.
- Save & Quit: Go to visual mode (press `Esc`) and then press `:wq`.
- Copy: Go to visual mode (press `Esc`) and then press `yy` (for one line copy).
- Paste: Press `p`.
- Delete a Line: Press `dd`.
- Undo a Line: Press `u`.
- Forcefully Quit: `:wq!`.
- Show Line Number: `:set number`.
- Delete Line Number: `:set nonumber`.

---

By following these commands and notes, you can efficiently manage files, directories, users, and permissions in Linux.
