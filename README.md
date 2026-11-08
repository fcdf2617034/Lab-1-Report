# WADF104 Lab 1: Network & Service Reconnaissance with Metasploitable 2
**Course:** WADF104 – Network & Service Reconnaissance  
**Student Name:** Kenneth Ukata[cite: 59]  
**Registration Number:** fcdf2617034[cite: 59]  
**Environment:** Debian Linux (Kali Linux `kali@KENNETH`)[cite: 59]  
**Target IP:** `192.168.56.103` (Metasploitable 2 - Ubuntu Linux)[cite: 59]  

---

## 📌 Lab Overview
This repository contains evidence, command logs, scan outputs, and technical documentation for **WADF104 Lab 1**[cite: 59]. The objective of this practical exercise was to perform active network discovery, port scanning, service version enumeration, operating system fingerprinting, and web application analysis against a vulnerable target (`Metasploitable 2`) within a controlled host-only lab environment[cite: 59, 60].

## 🛠️ Tools & Utilities Used
* **Network Discovery:** `ip`, `netdiscover`, `ping`[cite: 60, 61]
* **Port Scanning & Enumeration:** Nmap (`-p-`, `-F`, `-sV`, `-O`, `-A`, `-sU`, `-sC`)[cite: 62, 63, 64, 65]
* **Service Fingerprinting:** `curl`[cite: 66, 67]
* **Web Reconnaissance:** `whatweb` (Levels 1, 3, 4, and redirect testing)[cite: 68, 69, 70, 71]

---

## 📂 Key Practical Modules Covered

### Part 1: Target Identification & Host Discovery
* Verified local Kali interface configurations and subnet routing[cite: 60].
* Executed `sudo netdiscover -r 192.168.56.0/24` to discover active hosts on the host-only network, identifying the target IP at `192.168.56.103`[cite: 61].
* Confirmed network reachability with `ping -c 4 192.168.56.103` (0% packet loss)[cite: 61].

### Part 2: Basic Nmap Port Scanning
* **Default TCP Scan (`nmap`):** Scanned the top 1,000 TCP ports, identifying standard open services (FTP, SSH, Telnet, SMTP, HTTP, NetBIOS, SMB, MySQL)[cite: 62].
* **Full TCP Scan (`nmap -p-`):** Scanned all 65,535 TCP ports to uncover non-standard listening services[cite: 62, 63].
* **Fast Port Scan (`nmap -F`):** Scanned the top 100 most common ports to minimize network traffic and noise[cite: 63].

### Part 3: Service Version, OS & Aggressive Detection
* **Service Version Enumeration (`-sV`):** Banner-grabbed open ports, revealing software versions like `vsftpd 2.3.4`, `OpenSSH 4.7p1`, and `Apache 2.2.8`[cite: 63, 64].
* **OS Detection (`-O`):** Fingerprinted TCP/IP stack behavior to accurately identify the target's Linux 2.6.X kernel[cite: 64].
* **Aggressive Scan (`-A`):** Combined OS detection, version scanning, traceroute, and default scripts into a single run[cite: 64, 65].

### Part 4: UDP & Script Scanning (NSE)
* **UDP Port Scan (`-sU --top-ports 20`):** Enumerated active UDP services such as DNS (53), TFTP (69), and NetBIOS (137)[cite: 65].
* **NSE Scripts (`-sC`, `http-title`, `http-headers`, `smb-os-discovery`):** Queried service details, detected anonymous access privileges, pulled web headers, and retrieved Samba OS build parameters[cite: 65, 66].

### Part 5 & 6: Service Fingerprinting & WhatWeb Enumeration
* Verified Apache web server status and HTTP 200 OK responses using `curl -I`[cite: 67].
* Executed `whatweb` with varying aggression levels (Levels 1, 3, and 4) to fingerprint web applications (such as DVWA and Mutillidae), test redirects, and export scan logs to text files[cite: 68, 69, 70, 71, 72].

---

## 📊 Target Service Inventory Summary

| Port | Protocol | State | Service | Version / Evidence | Command Used |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **21** | TCP | Open | FTP | `vsftpd 2.3.4`[cite: 74] | `nmap -sV 192.168.56.103`[cite: 73] |
| **22** | TCP | Open | SSH | `OpenSSH 4.7p1 Debian`[cite: 74] | `nmap -sV 192.168.56.103`[cite: 73] |
| **23** | TCP | Open | Telnet | `Linux telnetd`[cite: 74] | `nmap -sV 192.168.56.103`[cite: 73] |
| **25** | TCP | Open | SMTP | `Postfix smtpd`[cite: 74] | `nmap -sV 192.168.56.103`[cite: 73] |
| **80** | TCP | Open | HTTP | `Apache httpd 2.2.8 (Ubuntu) PHP/5.2.4`[cite: 74] | `whatweb -v http://192.168.56.103`[cite: 73] |
| **139 / 445** | TCP | Open | NetBIOS / SMB | `Samba smbd 3.X - 4.X`[cite: 74] | `nmap -sV 192.168.56.103`[cite: 73] |

---

## ❓ Lab Review & Assessment Questions

1. **What is reconnaissance?**  
   *The initial penetration testing phase where an attacker or tester gathers comprehensive information (IPs, open ports, running software) about a target prior to vulnerability analysis or exploitation[cite: 74].*

2. **What is the difference between host discovery and port scanning?**  
   *Host discovery determines if a machine is active and reachable on the network (e.g., via ping), whereas port scanning probes an active host to identify which specific communication ports accept connections[cite: 74].*

3. **Explain open, closed, and filtered port states.**  
   * **Open:** An application is actively listening for connections on the port[cite: 74, 75].  
   * **Closed:** The port is accessible, but no application or service is listening on it[cite: 75].  
   * **Filtered:** A firewall or network filter blocks packets, preventing Nmap from determining if the port is open or closed[cite: 75].*

4. **What does `-sV` do?**  
   *Enables service version detection so Nmap can query open ports and identify exact software names and version banners[cite: 75].*

5. **What does `-O` do?**  
   *Instructs Nmap to perform operating system fingerprinting by analyzing the target's unique TCP/IP stack response characteristics[cite: 75].*

6. **Explain what `-A` combines and why it is noisier.**  
   *`-A` enables OS detection (`-O`), version detection (`-sV`), script scanning (`-sC`), and traceroute (`--traceroute`) simultaneously. It generates a high volume of aggressive traffic that is easily flagged by intrusion detection systems (IDS)[cite: 75].*

7. **What does `-p-` mean?**  
   *Tells Nmap to scan all 65,535 TCP ports instead of defaulting to the top 1,000 common ports[cite: 75].*

8. **Why can UDP scans take longer?**  
   *UDP is connectionless and does not feature a handshake. Open or filtered UDP ports often return no response, forcing Nmap to wait for timeouts. Closed UDP ports trigger rate-limited ICMP error responses, slowing scan rates[cite: 75].*

9. **What does `-sC` do?**  
   *Executes Nmap's default set of Nmap Scripting Engine (NSE) scripts to gather basic service information and check for common misconfigurations[cite: 75].*

10. **What information did the HTTP title/headers scripts reveal?**  
    *They identified the web server software (`Apache 2.2.8`), backend scripting language (`PHP 5.2.4`), underlying operating system (`Ubuntu`), and the default page title (`Metas
