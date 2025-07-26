# Basic Host Discovery Commands

## 1. Ping Scan (Detect live hosts without port scanning)
**Command:** `nmap -sn <target>`  
📌 Sends ICMP echo requests to check for live hosts.  
✅ **Example:** `nmap -sn 192.168.1.0/24`

## 2. ARP Scan (For local networks, more reliable than ICMP)
**Command:** `nmap -PR <target>`  
📌 Sends ARP requests to detect hosts in the network.  
✅ **Example:** `nmap -PR 192.168.1.0/24`

## 3. Disable Ping and Scan for Open Ports (Useful when ICMP is blocked)
**Command:** `nmap -Pn <target>`  
📌 Treats all hosts as online and proceeds with scanning.  
✅ **Example:** `nmap -Pn 192.168.1.1`

## 4. UDP Host Discovery (Find hosts using UDP packets)
**Command:** `nmap -PU <target>`  
📌 Sends UDP packets to detect live hosts that may not respond to ICMP or ARP scans.  
✅ **Example:** `nmap -PU 192.168.1.0/24`

## 5. Aggressive Host Discovery (More detailed scan with additional checks)
**Command:** `nmap -A <target>`  
📌 Performs OS detection, version detection, script scanning, and traceroute.  
✅ **Example:** `nmap -A 192.168.1.1`

## 6. Using NETDISCOVER
**Command:** `sudo netdiscover -r <target_ip>`  
✅ **Example:** `sudo netdiscover -r 172.16.6.128`

## Additional Methods for Active Host Discovery
- **Windows:** Use **Advanced IP Scanner** or the command: `arp -a`
- **Linux:** Use **NBTSCAN** to get results similar to Advanced IP Scanner.  
  **Command:** `nbtscan <network>`  
  ✅ **Example:** `nbtscan 172.16.6.128/16`


## Summary: 
<img width="1570" height="741" alt="image" src="https://github.com/user-attachments/assets/159964c4-bc45-468f-bbf4-9e90761fdf64" />
<img width="1570" height="399" alt="image" src="https://github.com/user-attachments/assets/bb427360-c312-415b-bb00-2e1bf912f5a7" />


