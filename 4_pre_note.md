# Managing Processes, SSH, FTP, TELNET, IP Configuration, and System Information

This note provides a guide to managing processes, using remote access tools (SSH, FTP, TELNET), configuring IP addresses, and gathering system and user information in Linux. It also includes instructions for installing software and learning about tools.

---

## **Managing Processes and Tasks**

### **Basic Process Management**
1. **Start a Process**  
   - Example: `ping facebook.com`  
   - Description: Starts a process (e.g., pinging a website).

2. **Pause a Process**  
   - Command: `Ctrl + Shift + Z`  
   - Description: Pauses the current foreground process.

3. **Resume a Process**  
   - Command: `fg ping`  
   - Description: Resumes a paused process (e.g., `ping`).

4. **Check Running Processes**  
   - Command: `top`  
   - Description: Displays real-time information about running processes.

5. **Stop a Process**  
   - Command: `sudo kill 10`  
   - Description: Stops a process by its Process ID (PID).

6. **Check User Logins and Running Processes**  
   - Command: `ps ux`  
   - Description: Displays processes running under the current user.

---

## **Remote Access Tools (SSH, FTP, TELNET)**

### **SSH (Secure Shell)**
1. **Check SSH Usage**  
   - Command: `ssh -h`  
   - Description: Displays all available options for the `ssh` command.

2. **Remote Access via SSH**  
   - Command: `ssh username@ip_address`  
   - Example: `ssh msfadmin@192.168.57.130`  
   - Description: Connects to a remote system using SSH. Replace `username` and `ip_address` with the target system's credentials.

### **FTP (File Transfer Protocol)**
1. **Check FTP Usage**  
   - Command: `ftp -h`  
   - Description: Displays all available options for the `ftp` command.

2. **Remote Access via FTP**  
   - Command: `ftp ip_address`  
   - Example: `ftp 192.168.57.130`  
   - Description: Connects to a remote system using FTP.

3. **List Available FTP Commands**  
   - Command: `?` or `help`  
   - Description: Displays a list of commands that can be executed in the FTP session.

### **TELNET**
1. **Check TELNET Usage**  
   - Command: `telnet -h`  
   - Description: Displays all available options for the `telnet` command.

2. **Remote Access via TELNET**  
   - Command: `telnet ip_address`  
   - Example: `telnet 192.168.57.130`  
   - Description: Connects to a remote system using TELNET.

---

## **Setting IP Address and Internet Configuration**

### **Network Configuration**
1. **Check Connected Devices**  
   - Command: `ifconfig`  
   - Description: Displays network interface details.

2. **Disable a Network Device**  
   - Command: `sudo ifconfig lo down`  
   - Description: Disables the specified network interface (`lo`, `wlan0`, `eth0`, etc.).

3. **Enable a Network Device**  
   - Command: `sudo ifconfig lo up`  
   - Description: Enables the specified network interface.

4. **Assign IP Address Manually**  
   - Commands:  
     - `sudo ifconfig lo 127.0.0.1` (Assign IP address).  
     - `sudo ifconfig lo netmask 255.0.0.0` (Assign netmask).  
     - `sudo ifconfig lo broadcast ____` (Assign broadcast address for `wlan0` or `eth0`).

5. **Check Connectivity**  
   - Command: `ping google.com`  
   - Description: Tests network connectivity to a website.

---

## **Enumerating User and System Information**

1. **Check Current User**  
   - Command: `whoami`  
   - Description: Displays the current logged-in user.

2. **Check User ID and Groups**  
   - Command: `id`  
   - Description: Displays the UID, GID, and group memberships of the current user.

3. **Check Hostname**  
   - Command: `hostname`  
   - Description: Displays the system's hostname.

4. **Change Hostname**  
   - Command: `sudo nano /etc/hostname`  
   - Description: Edits the hostname file to change the system's hostname.

5. **Check Linux Kernel Version**  
   - Command: `uname -a`  
   - Description: Displays the current Linux kernel version.

6. **Check Linux Release Version**  
   - Command: `hostnamectl`  
   - Description: Displays detailed system information, including the exact Linux release version.

---

## **Installing Software and Learning Tools**

1. **Install Software**  
   - Command: `sudo apt install software_name`  
   - Description: Installs the specified software using the package manager.

2. **Search for Tools in Package Manager**  
   - Command: `sudo apt search tool_name`  
   - Description: Searches for available tools in the package manager.

3. **View Tool Manual**  
   - Command: `man tool_name`  
   - Description: Displays the manual for the specified tool.

4. **Get Tool Description**  
   - Command: `whatis tool_name`  
   - Description: Provides a brief description of the specified tool.

---

## **Updates to the Note**
- Added detailed descriptions for each command.
- Included examples for better understanding.
- Organized sections for clarity.
- Added instructions for changing hostname and checking Linux versions.
- Included commands for enabling/disabling network interfaces and assigning IP addresses manually.

---

By following these commands and notes, you can efficiently manage processes, configure network settings, access remote systems, and gather system information in Linux.
