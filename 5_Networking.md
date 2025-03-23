# Understanding HUB, SWITCH, ROUTER, OSI Model, TCP/IP Model, MAC Address, Wireshark, and Metasploitable 2

This note provides a comprehensive guide to understanding networking devices (HUB, SWITCH, ROUTER), the OSI and TCP/IP models, changing MAC addresses, using Wireshark, and setting up Metasploitable 2 in Kali Linux using Docker.

---

## **Networking Devices**

### **HUB**
- **Function**: A hub is a basic networking device that connects multiple devices in a LAN (Local Area Network).
- **Operation**: It broadcasts data to all connected devices, regardless of the intended recipient.
- **Limitations**: 
  - Inefficient as it causes unnecessary traffic.
  - No intelligence to filter or manage data.

### **SWITCH**
- **Function**: A switch is an advanced networking device that connects multiple devices in a LAN.
- **Operation**: It uses MAC addresses to forward data only to the intended recipient.
- **Advantages**:
  - Reduces network congestion.
  - Provides better performance and security compared to a hub.

### **ROUTER**
- **Function**: A router connects multiple networks (e.g., LAN to WAN) and routes data between them.
- **Operation**: It uses IP addresses to determine the best path for data transmission.
- **Advantages**:
  - Enables communication between different networks.
  - Provides features like NAT (Network Address Translation) and firewall.

---

## **OSI Model**

The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes the functions of a communication system into **7 layers**:

### **7 Layers of the OSI Model**
1. **Application Layer (Layer 7)**  
   - Role: Provides network services to end-user applications (e.g., email, web browsing).  
   - Protocols: HTTP, FTP, SMTP, DNS.

2. **Presentation Layer (Layer 6)**  
   - Role: Formats and translates data for the application layer (e.g., encryption, compression).  
   - Examples: SSL/TLS, JPEG, PNG.

3. **Session Layer (Layer 5)**  
   - Role: Manages communication sessions between applications.  
   - Examples: NetBIOS, RPC.

4. **Transport Layer (Layer 4)**  
   - Role: Ensures reliable data transfer between systems.  
   - Protocols: TCP (reliable, connection-oriented), UDP (faster, connectionless).

5. **Network Layer (Layer 3)**  
   - Role: Handles logical addressing (IP addresses) and routing of data packets.  
   - Protocols: IP, ICMP, ARP.

6. **Data Link Layer (Layer 2)**  
   - Role: Ensures error-free data transfer between adjacent network nodes.  
   - Examples: Ethernet, PPP, VLANs.

7. **Physical Layer (Layer 1)**  
   - Role: Defines the physical medium for data transmission (e.g., cables, wireless signals).  
   - Examples: Fiber optics, coaxial cables, radio signals.

---

## **TCP/IP Model**

The TCP/IP model is a simplified version of the OSI model, consisting of **4 layers**:

### **Layers of the TCP/IP Model**
1. **Application Layer**  
   - Role: Provides services directly to user applications.  
   - Examples: HTTP, TLS, DNS.

2. **Transport Layer**  
   - Role: Handles end-to-end communication, error detection, and flow control.  
   - Examples: TCP, UDP.

3. **Internet Layer**  
   - Role: Responsible for routing, addressing, and packet forwarding.  
   - Examples: IP (v4, v6), ICMP, ARP.

4. **Network Access Layer**  
   - Role: Manages physical transmission of data across local networks.  
   - Examples: Ethernet, Wireless LAN.

---

## **Data Transmission Process**

### **Sender Side (Encapsulation)**
1. **Application Layer**: Divides data into parts (D1, D2, D3).  
2. **Transport Layer**: Adds a transport header (TH) to create segments.  
3. **Internet Layer**: Encapsulates segments into packets with network headers.  
4. **Network Access Layer**: Forms frames and transmits them over the network.

### **Receiver Side (Decapsulation)**
1. **Network Access Layer**: Extracts packets by removing frame headers.  
2. **Internet Layer**: Strips network headers to retrieve segments.  
3. **Transport Layer**: Reorders segments and verifies data integrity.  
4. **Application Layer**: Combines data blocks and presents the original message.

---

## **Changing MAC Address**

- **Command**: `macchanger --list`  
  - Description: Lists MAC addresses of various companies.  
- **Usage**:  
  - Change MAC address: `macchanger -m new_mac_address interface_name`.  
  - Example: `macchanger -m 00:11:22:33:44:55 eth0`.

---

## **Wireshark**

Wireshark is a powerful network protocol analyzer used for capturing and inspecting network traffic.

### **Key Features**
- Captures live network traffic.
- Supports various protocols (TCP, UDP, HTTP, DNS, etc.).
- Provides deep packet inspection.
- Offers filtering and analysis tools.
- Exports captured data for further analysis.

### **Usage**
- Launch Wireshark: `wireshark`.
- Select a network interface to capture traffic.
- Apply filters to analyze specific packets.

---

## **Setting Up Metasploitable 2 in Kali Linux Using Docker**

### **Steps**
1. **Pull Metasploitable 2 Image**:  
   - Command: `docker pull tleemcjr/metasploitable2`.

2. **Run Metasploitable 2 Container**:  
   - Command: `sudo docker run -it tleemcjr/metasploitable2`.

3. **Access Metasploitable 2**:  
   - Command: `ssh msfadmin@localhost -p 22`.

4. **Manage Container**:  
   - Stop Container: `sudo docker stop metasploitable2`.  
   - Restart Container: `sudo docker start metasploitable2`.

---

## **Updates to the Note**
- Added detailed explanations for HUB, SWITCH, and ROUTER.
- Included encapsulation and decapsulation processes for the OSI and TCP/IP models.
- Added steps for changing MAC addresses.
- Provided a brief overview of Wireshark and its features.
- Included detailed steps for setting up Metasploitable 2 using Docker.

---

By following this guide, you can gain a deeper understanding of networking concepts, tools, and practical setups in Kali Linux.
