# Network Enumeration Notes

## Target IP: 172.16.6.128

### 1. Basic Port Scan with Service Detection

**Command:**
```bash
nmap -p 20-25 -sT 172.16.6.128 -T 5 -sV
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 09:21 +06
Nmap scan report for 172.16.6.128
Host is up (0.00065s latency).

PORT   STATE  SERVICE   VERSION
20/tcp closed ftp-data
21/tcp open   ftp       vsftpd 2.3.4
22/tcp open   ssh       OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp open   telnet    Linux telnetd
24/tcp closed priv-mail
25/tcp open   smtp      Postfix smtpd
MAC Address: 00:0C:29:0E:61:63 (VMware)
Service Info: Host:  metasploitable.localdomain; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.55 seconds
```

**Summary:**
- **Open Ports:** 21 (FTP), 22 (SSH), 23 (Telnet), 25 (SMTP)
- **Closed Ports:** 20 (FTP Data), 24 (Private Mail)
- **FTP Version:** vsftpd 2.3.4 (known to have a backdoor vulnerability)

---

### 2. OS Detection

**Command:**
```bash
nmap -p 20-25 -sT 172.16.6.128 -T 5 -sV -O
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 09:22 +06
Nmap scan report for 172.16.6.128
Host is up (0.00053s latency).

PORT   STATE  SERVICE   VERSION
20/tcp closed ftp-data
21/tcp open   ftp       vsftpd 2.3.4
22/tcp open   ssh       OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp open   telnet    Linux telnetd
24/tcp closed priv-mail
25/tcp open   smtp      Postfix smtpd
MAC Address: 00:0C:29:0E:61:63 (VMware)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: Host:  metasploitable.localdomain; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.66 seconds
```

**Summary:**
- **OS:** Linux 2.6.9 - 2.6.33
- **Device Type:** General purpose

---

### 3. Aggressive Scan on FTP Port (21)

**Command:**
```bash
nmap -A -p 21 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 09:23 +06
Nmap scan report for 172.16.6.128
Host is up (0.00046s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 172.16.6.1
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
MAC Address: 00:0C:29:0E:61:63 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: OS: Unix

TRACEROUTE
HOP RTT     ADDRESS
1   0.46 ms 172.16.6.128

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2.48 seconds
```

**Summary:**
- **Anonymous FTP Login:** Allowed
- **FTP Server Status:** Plain text connections, session timeout of 300 seconds

---

### 4. SMB Enumeration

**Command:**
```bash
sudo nmblookup -A 172.16.6.128
```

**Output:**
```
Looking up status of 172.16.6.128
        METASPLOITABLE  <00> -         B <ACTIVE> 
        METASPLOITABLE  <03> -         B <ACTIVE> 
        METASPLOITABLE  <20> -         B <ACTIVE> 
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE> 
        WORKGROUP       <00> - <GROUP> B <ACTIVE> 
        WORKGROUP       <1d> -         B <ACTIVE> 
        WORKGROUP       <1e> - <GROUP> B <ACTIVE> 

        MAC Address = 00-00-00-00-00-00
```

**Summary:**
- **SMB Host Name:** METASPLOITABLE
- **Workgroup:** WORKGROUP
- **MAC Address:** 00-00-00-00-00-00
