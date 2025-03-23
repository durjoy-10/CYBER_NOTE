# Port Scanning in Network Scanning and Enumeration

Port scanning is a critical phase in network scanning and enumeration, where the goal is to identify open ports and services running on a target system. This information is essential for understanding the attack surface and potential vulnerabilities. Below is a detailed analysis of the port scanning process using the provided Nmap commands and their outputs.

## 1. Basic Port Scan
**Command:**
```bash
nmap 172.16.6.128
```
**Output:**
```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 19:59 +06
Nmap scan report for 172.16.6.128
Host is up (0.0016s latency).
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

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```
**Explanation:**
- Scanned all common TCP ports (1-1024 and some higher ports).
- Identified multiple open ports, including:
  - **21/tcp (FTP):** File Transfer Protocol, often targeted for unauthorized access.
  - **22/tcp (SSH):** Secure Shell, used for remote administration.
  - **23/tcp (Telnet):** Unencrypted remote login service, vulnerable to sniffing.
  - **80/tcp (HTTP):** Web server, commonly exploited for web-based attacks.
  - **445/tcp (Microsoft-DS):** SMB protocol, often targeted for exploits like EternalBlue.
  - **3306/tcp (MySQL):** Database service, potentially vulnerable to SQL injection.
  - **5900/tcp (VNC):** Remote desktop service, susceptible to brute-force attacks.
- Services like Telnet (23/tcp) and FTP (21/tcp) are particularly risky due to their lack of encryption.

---

## 2. Targeted Port Scan
**Command:**
```bash
nmap -p 22 172.16.6.128
```
**Output:**
```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:00 +06
Nmap scan report for 172.16.6.128
Host is up (0.00040s latency).
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.33 seconds
```
**Explanation:**
- Only port **22/tcp (SSH)** was scanned and found open.
- Useful for verifying the status of specific ports.
- SSH is a critical service; ensuring it is properly secured is essential.

---

## 3. Multiple Port Scan
**Command:**
```bash
nmap -p 22,23,80,443 172.16.6.128
```
**Output:**
```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:00 +06
Nmap scan report for 172.16.6.128
Host is up (0.00051s latency).
PORT    STATE  SERVICE
22/tcp  open   ssh
23/tcp  open   telnet
80/tcp  open   http
443/tcp closed https
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.26 seconds
```
**Explanation:**
- **22/tcp (SSH):** Open.
- **23/tcp (Telnet):** Open.
- **80/tcp (HTTP):** Open.
- **443/tcp (HTTPS):** Closed.
- HTTP (80/tcp) is open, but HTTPS (443/tcp) is closed, indicating potential misconfiguration or lack of encryption for web traffic.
- Telnet (23/tcp) should be disabled due to its insecure nature.

---

## 4. Range-Based Port Scan
**Command:**
```bash
nmap -p 20-50 172.16.6.128
```
**Output:**
```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:01 +06
Nmap scan report for 172.16.6.128
Host is up (0.00069s latency).
Not shown: 27 closed tcp ports (reset)
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
23/tcp open  telnet
25/tcp open  smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.38 seconds
```
**Explanation:**
- **21/tcp (FTP):** Open.
- **22/tcp (SSH):** Open.
- **23/tcp (Telnet):** Open.
- **25/tcp (SMTP):** Open.
- **SMTP (25/tcp)** is often targeted for email-related attacks like phishing or spam.
- **FTP (21/tcp) and Telnet (23/tcp)** are high-risk services.

---


## 5. Full Port Scan

**Command:**
```bash
nmap -p- 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:02 +06
Nmap scan report for 172.16.6.128
Host is up (0.00041s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
23/tcp    open  telnet
25/tcp    open  smtp
53/tcp    open  domain
80/tcp    open  http
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
512/tcp   open  exec
513/tcp   open  login
514/tcp   open  shell
1099/tcp  open  rmiregistry
1524/tcp  open  ingreslock
2049/tcp  open  nfs
2121/tcp  open  ccproxy-ftp
3306/tcp  open  mysql
3632/tcp  open  distccd
5432/tcp  open  postgresql
5900/tcp  open  vnc
6000/tcp  open  X11
6667/tcp  open  irc
6697/tcp  open  ircs-u
8009/tcp  open  ajp13
8180/tcp  open  unknown
8787/tcp  open  msgsrvr
35293/tcp open  unknown
36600/tcp open  unknown
40253/tcp open  unknown
43333/tcp open  unknown
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 4.60 seconds
```

**Note:**
- Scanned all 65535 TCP ports.
- Identified additional open ports, including:
  - **3632/tcp (distccd)**: Distributed compiler service, often exploited.
  - **5900/tcp (VNC)**: Open.
  - **6667/tcp (IRC)**: Internet Relay Chat, sometimes used for C2 (Command and Control) by attackers.
  - **8009/tcp (AJP13)**: Apache JServ Protocol, potentially vulnerable to exploits.
- A full port scan is time-consuming but provides a comprehensive view of all open ports.

---

## 6. TCP Connect Scan

**Command:**
```bash
nmap -p 20-25 -sT 172.16.6.128
```

**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:03 +06
Nmap scan report for 172.16.6.128
Host is up (0.00075s latency).

PORT   STATE  SERVICE
20/tcp closed ftp-data
21/tcp open   ftp
22/tcp open   ssh
23/tcp open   telnet
24/tcp closed priv-mail
25/tcp open   smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

**Note:**
- **TCP Connect Scan (-sT)** establishes a full connection to the target, making it more reliable but also more detectable by intrusion detection systems (IDS).

---

## Nmap Scan Report

### 7. SYN Scan
**Command:**
```bash
nmap -p 20-25 -sS 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:04 +06
Nmap scan report for 172.16.6.128
Host is up (0.00066s latency).

PORT   STATE  SERVICE
20/tcp closed ftp-data
21/tcp open   ftp
22/tcp open   ssh
23/tcp open   telnet
24/tcp closed priv-mail
25/tcp open   smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.25 seconds
```
**Note:**
- Similar to the TCP Connect Scan, but faster and stealthier.
- SYN Scan (-sS) is the default scan type for Nmap when run with root privileges.
- It is less likely to be logged by the target system compared to a full TCP Connect Scan.

- The SYN scan is stealthier and faster than the TCP Connect scan. It only sends SYN packets to the target system, and if a SYN-ACK response is received, the port is open. This scan is more difficult to detect, especially when run with root privileges.

**State of SYN Scan:**
- **Open →** SYN-ACK response received.
- **Closed →** RST response received.
- **Filtered →** No response or ICMP unreachable received.

---


### 8. ACK Scan
**Command:**
```bash
nmap -p 20-25 -sA 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:04 +06
Nmap scan report for 172.16.6.128
Host is up (0.00057s latency).

PORT   STATE      SERVICE
20/tcp unfiltered ftp-data
21/tcp unfiltered ftp
22/tcp unfiltered ssh
23/tcp unfiltered telnet
24/tcp unfiltered priv-mail
25/tcp unfiltered smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.26 seconds
```
**Note:**
- ACK Scan (-sA) is used to determine if ports are filtered by a firewall.
- It does not indicate whether the port is open or closed, only if it is reachable.

**State of ACK Scan:**
- An ACK Scan is used to identify whether the scanned ports are filtered by a firewall. It doesn't provide information about whether a port is open or closed, only if it is reachable.
- **Unfiltered →** Port is not blocked by a firewall (no response or TCP connection reset).
- **Filtered →** No response received, indicating the presence of a firewall.

---

### 9. FIN, XMAS, and NULL Scans
**Commands:**
```bash
nmap -p 20-25 -sF 172.16.6.128  # FIN Scan
nmap -p 20-25 -sX 172.16.6.128  # XMAS Scan
nmap -p 20-25 -sN 172.16.6.128  # NULL Scan
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:05 +06
Nmap scan report for 172.16.6.128
Host is up (0.00043s latency).

PORT   STATE         SERVICE
20/tcp closed        ftp-data
21/tcp open|filtered ftp
22/tcp open|filtered ssh
23/tcp open|filtered telnet
24/tcp closed        priv-mail
25/tcp open|filtered smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 1.58 seconds
```
**Note:**
- These scans are used to evade detection by firewalls or IDS.
- They are less reliable than SYN or TCP Connect Scans but can be useful in certain scenarios.

- These scans are used to evade detection by firewalls or IDS. These scans don’t establish a complete TCP connection, making them stealthy. However, they are less reliable than SYN or TCP Connect scans.

- **FIN Scan:** Uses a FIN flag to complete the scan. It can evade detection from systems that don't handle FIN packets properly.
  
  **State of FIN Scan:**
  - **Open/Filtered →** No response received.
  - **Closed →** RST response received from the target.
  
- **XMAS Scan:** Sends a packet with the FIN, URG, and PSH flags set.
  
  **State of XMAS Scan:**
  - **Open/Filtered →** No response received.
  - **Closed →** RST response received from the target.
  
- **NULL Scan:** Sends a packet with no flags set.
  
  **State of NULL Scan:**
  - **Open/Filtered →** No response received.
  - **Closed →** RST response received from the target.

---

### 10. UDP Scan
**Command:**
```bash
nmap -p 20-25 -sU 172.16.6.128
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:07 +06
Nmap scan report for 172.16.6.128
Host is up (0.00057s latency).

PORT   STATE  SERVICE
20/udp closed ftp-data
21/udp closed ftp
22/udp closed ssh
23/udp closed telnet
24/udp closed priv-mail
25/udp closed smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.39 seconds
```
**Note:**
- UDP scans (-sU) are slower and less reliable than TCP scans due to the connectionless nature of UDP.
- They are important for identifying services like DNS (53/udp) or SNMP (161/udp).

- UDP scans are generally slower and less reliable than TCP scans due to UDP’s connectionless nature. However, they are useful for discovering services like DNS (53/udp) or SNMP (161/udp).

**State of UDP Scan:**
- **Open →** No response from the target.
- **Closed →** ICMP "Destination Unreachable" response received.
- **Filtered →** No response received, suggesting firewall filtering.

---

### 11. Timing and Performance
**Commands:**
```bash
nmap -p 20-25 -sT 172.16.6.128 -T 4  # Aggressive timing
nmap -p 20-25 -sT 172.16.6.128 -T 3  # Normal timing
```
**Output:**
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-23 20:08 +06
Nmap scan report for 172.16.6.128
Host is up (0.00068s latency).

PORT   STATE  SERVICE
20/tcp closed ftp-data
21/tcp open   ftp
22/tcp open   ssh
23/tcp open   telnet
24/tcp closed priv-mail
25/tcp open   smtp
MAC Address: 00:0C:29:0E:61:63 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```
**Note:**
- Aggressive timing (-T 4) completes the scan in 0.19 seconds.
- Normal timing (-T 3) takes 13.19 seconds.
- Timing templates (-T) control the speed and stealth of the scan.
- Aggressive timing is faster but more likely to be detected.





## Summary and Recommendations
- **Identify High-Risk Services:** Services like FTP (21/tcp), Telnet (23/tcp), and VNC (5900/tcp) should be disabled or secured.
- **Use Stealthy Scans:** SYN scans (`-sS`) are preferred for their balance of speed and stealth.
- **Perform Full Port Scans:** Use `-p-` to identify all open ports, especially less common ones that may be overlooked.
- **Check for Filtered Ports:** Use ACK scans (`-sA`) to detect firewall rules.
- **Scan UDP Ports:** Don't forget to scan UDP ports, as they can reveal critical services like DNS or SNMP.
- **Adjust Timing:** Use aggressive timing (`-T 4`) for quick scans, but be aware of the increased risk of detection.

By following these steps, you can effectively enumerate open ports and services, identify potential vulnerabilities, and secure the target system.
