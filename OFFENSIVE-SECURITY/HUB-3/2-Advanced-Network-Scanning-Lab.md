# Advanced Network Scanning Lab (nmap usage)
## 1. Background

Nmap is an open-source cybersecurity tool used for network discovery and security auditing. Created by Gordon Lyon, it helps identify active hosts, open ports, running services, and potential vulnerabilities. It is widely used by penetration testers and network administrators to gather critical information about a network during the reconnaissance phase.

This lab focuses on building practical skills in advanced network scanning using Nmap, a powerful tool for reconnaissance and security assessment. The goal is to simulate a real-world corporate environment by scanning the network range 192.168.10.0/24 to identify active hosts, detect open ports, enumerate services, and uncover potential vulnerabilities.

## 2. Objectives
The aim of this lab is to:
- Use Nmap to perform host discovery within a target network range (192.168.10.0/24).
- Identify open ports on discovered hosts using efficient scanning techniques.
- Enumerate running services and determine their versions on target systems.
- Conduct comprehensive scans using scripts for deeper network analysis.
- Detect potential vulnerabilities within network services using Nmap scripting features.
- Develop the ability to adapt scanning strategies based on findings during reconnaissance.
- Gain practical experience in performing systematic and comprehensive network reconnaissance in a mixed operating system environment.

## 3. Scope and Limitations
### Scope
- This lab focuses on performing network reconnaissance within the defined target range 192.168.10.0/24 using Nmap.
- Activities include host discovery, port scanning, service enumeration, and basic vulnerability detection.
- The lab environment simulates a corporate network containing both Windows and Linux systems, allowing for practical exploration of different services and configurations.
- The objective is limited to identifying potential security issues through scanning techniques only.

### Limitations
- The lab is restricted to a controlled environment and does not involve real-world systems or external networks.
- Only reconnaissance and scanning techniques are permitted—no exploitation or post-exploitation activities are included.
- Additionally, the Nmap version used is a lightweight implementation, meaning some advanced features and scripts may be unavailable.
- Results obtained may not fully reflect real-world scenarios due to simulation constraints and limited network complexity.

## 4. Legal and Ethical Disclaimer
- This lab is conducted strictly for educational and training purposes within an authorized and controlled environment. All activities performed using Nmap are limited to the designated target network (192.168.10.0/24) provided for this exercise.

- Unauthorized network scanning or reconnaissance on systems without explicit permission is illegal and may violate cybersecurity laws and organizational policies. Such actions can lead to severe legal consequences.

- Participants are expected to adhere to ethical guidelines by using the knowledge and skills gained responsibly. 
- All techniques learned in this lab should only be applied in environments where proper authorization has been granted, such as penetration testing engagements, academic labs, or approved security assessments.
- By proceeding with this lab, you acknowledge your responsibility to comply with all applicable laws, regulations, and ethical standards in cybersecurity practice.

## 5. Lab Initialization and Access
To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **`Hub 3`** was started.
2. The running command was executed to validate that the environment was live: `running # expected output: hub-3 running`.
3. The browser was opened and the interface was accessed via `http://localhost`
4. Navigation proceeded to the Advanced Network Scanning section.
5. All the required tasks were initiated and completed.

## 6. Challenge Overview

1. This challenge focuses on applying practical reconnaissance skills using Nmap to analyze a corporate network segment (192.168.10.0/24). The task involves systematically discovering active hosts, identifying open ports, and enumerating services running on both Windows and Linux systems within the network.
2. The Lab will progress through key phases of network scanning, starting with host discovery, followed by port scanning, service/version detection, and finally vulnerability identification using Nmap scripts. Each stage builds on the previous findings, encouraging a dynamic and adaptive approach to scanning.
   
3. The overall aim is to simulate a real-world security assessment by developing the ability to gather detailed network intelligence and identify potential security weaknesses through structured and efficient reconnaissance techniques.

## 7. Tools Used
| Tool | Purpose |
|-----|-----|
| **nmap ( network mapper )** |The primary tool used for network discovery, port scanning, service enumeration, and vulnerability detection. It enables identification of live hosts, open ports, running services, and potential security weaknesses within the target network.|
| **NSE ( nmap scripting engine )** | A built-in feature of Nmap used to execute scripts for advanced scanning and vulnerability detection.|
| **Auxiliary Modules** | Used for reconnaissance and service interaction without exploiting vulnerabilities |
| **Kali Linux Terminal** | A penetration testing operating system used to run Nmap and perform all reconnaissance activities in a controlled lab setup. |
| **Target Network** | 192.168.10.0/24|

## 8. Tasks Completed
### 8.1 Host Discovery Scan
A Nmap host discovery scan is a technique used in network reconnaissance to identify which devices on a network are active (alive) before performing deeper analysis. This type of scan sends different types of probes (such as ICMP echo requests, TCP SYN, or ARP requests) to a range of IP addresses to check if they respond. Any system that responds is marked as “host is up.”

A ping scan is a network scanning technique used to identify active (live) hosts on a network by sending probe requests and checking for responses.

In Nmap, it is performed using the `-sn` flag and does not scan ports, but only determines which systems are online. This helps increase efficiency by filtering out inactive IPs and scanning only live hosts.

The command ```nmap -sn 192.168.10.0/24``` was executed in the terminal and the following result was obtained

```bash
root@kali:~#
nmap -sn 192.168.10.0/24

Execute

Starting Nmap 7.94 ( https://nmap.org ) at Jan 20, 07:20 PM EST
Nmap scan report for 192.168.10.10
Host is up (0.0010s latency).
Nmap scan report for 192.168.10.20
Host is up (0.0060s latency).
Nmap scan report for 192.168.10.30
Host is up (0.0070s latency).
Nmap scan report for 192.168.10.40
Host is up (0.0070s latency).
Nmap scan report for 192.168.10.50
Host is up (0.0080s latency).
Nmap done: 5 IP addresses (5 hosts up) scanned in 2.24 seconds
```

- In conclusion the ping scan performed with Nmap successfully identified 5 active hosts within the 192.168.10.0/24 network. The results confirm that all scanned IP addresses are reachable, providing a clear starting point for further analysis such as port scanning and service enumeration.

### 8.2 Scan Techniques
Nmap uses a variety of scanning techniques to gather detailed information about networks and systems. These techniques include host discovery (ping scans) to identify active devices, TCP SYN scans for quick and stealthy port scanning, TCP connect scans for full connection analysis, and UDP scans to detect services running over UDP. Additionally, Nmap supports service version detection and script-based scanning through its scripting engine to uncover vulnerabilities. Together, these techniques enable comprehensive network reconnaissance and security assessment.

- TCP SYN Scan
    
    A TCP SYN scan is a fast and stealthy port scanning technique used to identify open ports on a target system. In Nmap, it works by sending a SYN packet to a port: if the target replies with SYN-ACK, the port is open; if it responds with RST, the port is closed. The connection is not fully completed, making it a half-open scan that is efficient and less likely to be detected. . It is fast, efficient, and relatively stealthy, making it commonly used for initial port scanning during reconnaissance.

    Nmap TCP SYN scan is run with the -sS flag.

    The command ```nmap -sS 192.168.10.0/24``` was executed in the terminal and the following result was obtained.

    ```bash
    root@kali:~#
    nmap -sS 192.168.10.0/24

    Execute

    Clear
    nmap -sS 192.168.10.0/24

    Starting Nmap 7.94 ( https://nmap.org ) at Jan 20, 07:23 PM EST
    Nmap scan report for 192.168.10.10
    Host is up (0.0050s latency).
    Not shown: 995 closed ports
    PORT     STATE SERVICE
    135/tcp  open  msrpc
    139/tcp  open  netbios-ssn
    389/tcp  open  ldap
    445/tcp  open  microsoft-ds
    3389/tcp  open  ms-wbt-server

    Nmap scan report for 192.168.10.20
    Host is up (0.0070s latency).
    Not shown: 995 closed ports
    PORT     STATE SERVICE
    135/tcp  open  msrpc
    139/tcp  open  netbios-ssn
    445/tcp  open  microsoft-ds
    3389/tcp  open  ms-wbt-server
    5357/tcp  open  http

    Nmap scan report for 192.168.10.30
    Host is up (0.0010s latency).
    Not shown: 994 closed ports
    PORT     STATE SERVICE
    80/tcp  open  http
    135/tcp  open  msrpc
    139/tcp  open  netbios-ssn
    445/tcp  open  microsoft-ds
    1433/tcp  open  ms-sql-s
    3389/tcp  open  ms-wbt-server

    Nmap scan report for 192.168.10.40
    Host is up (0.0060s latency).
    Not shown: 996 closed ports
    PORT     STATE SERVICE
    22/tcp  open  ssh
    80/tcp  open  http
    139/tcp  open  netbios-ssn
    445/tcp  open  netbios-ssn

    Nmap scan report for 192.168.10.50
    Host is up (0.0090s latency).
    Not shown: 998 closed ports
    PORT     STATE SERVICE
    22/tcp  open  ssh
    6667/tcp  open  irc

    Nmap done: 5 IP addresses (5 hosts up) scanned in 13.71 seconds
    ```
- In conclusion, the TCP SYN scan performed revealed multiple open ports and services across all five active hosts in the 192.168.10.0/24 network. The results reveal a mix of services including SMB, RDP, HTTP, SSH, and database services, indicating a diverse environment of Windows and Linux systems. This scan provides valuable insight into the network’s attack surface, highlighting potential entry points for further enumeration and security assessment.

### 8.3 Service Version Detection
Service version detection is a technique used to identify the specific services and their versions running on open ports of a target system. Using Nmap with the -sV flag, probes are sent to open ports to gather detailed information about the software in use, such as web servers, databases, or remote access services. This information is essential for security assessments, as it helps determine whether the services are outdated or vulnerable, enabling more targeted and effective vulnerability analysis.

The command ```nmap -sV 192.168.10.0/24``` was executed in the terminal and the following result was obtained.

```bash
root@kali:~#
nmap -sV 192.168.10.0/24

Execute

Clear
nmap -sV 192.168.10.0/24

Starting Nmap 7.94 ( https://nmap.org ) at Jan 20, 07:24 PM EST
Nmap scan report for 192.168.10.10
Host is up (0.0090s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE VERSION
135/tcp  open  msrpc      Microsoft Windows RPC
139/tcp  open  netbios-ssn Microsoft Windows netbios-ssn
389/tcp  open  ldap       Microsoft Windows Active Directory LDAP
445/tcp  open  microsoft-ds Windows Server 2019 Standard microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
Service Info: OSs: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.20
Host is up (0.0090s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE VERSION
135/tcp  open  msrpc      Microsoft Windows RPC
139/tcp  open  netbios-ssn Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows 10 Home 19041 microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5357/tcp  open  http       Microsoft HTTPAPI httpd 2.0
Service Info: OSs: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.30
Host is up (0.0050s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE VERSION
80/tcp  open  http       Microsoft IIS httpd 10.0
135/tcp  open  msrpc      Microsoft Windows RPC
139/tcp  open  netbios-ssn Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows Server 2016 Standard microsoft-ds
1433/tcp  open  ms-sql-s   Microsoft SQL Server 2016 13.00.1601
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
Service Info: OSs: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.40
Host is up (0.0040s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp  open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.5
80/tcp  open  http       Apache httpd 2.4.41
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X
445/tcp  open  netbios-ssn Samba smbd 4.11.6-Ubuntu
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.50
Host is up (0.0070s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp  open  ssh        OpenSSH 8.0
6667/tcp  open  irc        UnrealIRCd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap done: 5 IP addresses (5 hosts up) scanned in 10.79 seconds
```

- In conclusion, the service version detection scan identified open ports, services and their versions running on all hosts within the network. The results revealed a mix of Windows and Linux systems, including services such as Microsoft IIS, SQL Server, OpenSSH, Apache, and Samba. This detailed information provides deeper insight into the network environment and is crucial for identifying outdated or vulnerable software, thereby enabling more targeted and effective security assessments.

The command ```nmap -sC -sV 192.168.10.0/24``` in Nmap performs a comprehensive network scan across the entire subnet. It combines default scripting (-sC) with service version detection (-sV) to identify active hosts, discover open ports, determine the services running on those ports, and gather additional information such as potential vulnerabilities and misconfigurations. ```-sC``` runs default NSE scripts and performs automated checks (e.g., basic vulnerability detection, service info, misconfigurations) while ```-sV``` enables service version detection and identifies the exact services and versions running on open ports. This makes it a powerful command for in-depth reconnaissance and security assessment.

```bash
root@kali:~#
nmap -sC -sV 192.168.10.0/24

Execute

Clear
nmap -sC -sV 192.168.10.0/24

Starting Nmap 7.94 ( https://nmap.org ) at Jan 20, 07:25 PM EST
Nmap scan report for 192.168.10.10
Host is up (0.0030s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE VERSION
135/tcp  open  msrpc      Microsoft Windows RPC
139/tcp  open  netbios-ssn Microsoft Windows netbios-ssn
389/tcp  open  ldap       Microsoft Windows Active Directory LDAP
445/tcp  open  microsoft-ds Windows Server 2019 Standard microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
Service Info: OSs: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.20
Host is up (0.0050s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE VERSION
135/tcp  open  msrpc      Microsoft Windows RPC
139/tcp  open  netbios-ssn Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows 10 Home 19041 microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5357/tcp  open  http       Microsoft HTTPAPI httpd 2.0
Service Info: OSs: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.30
Host is up (0.0050s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE VERSION
80/tcp  open  http       Microsoft IIS httpd 10.0
135/tcp  open  msrpc      Microsoft Windows RPC
139/tcp  open  netbios-ssn Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows Server 2016 Standard microsoft-ds
1433/tcp  open  ms-sql-s   Microsoft SQL Server 2016 13.00.1601
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
Service Info: OSs: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.40
Host is up (0.0090s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp  open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.5
80/tcp  open  http       Apache httpd 2.4.41
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X
445/tcp  open  netbios-ssn Samba smbd 4.11.6-Ubuntu
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap scan report for 192.168.10.50
Host is up (0.0040s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp  open  ssh        OpenSSH 8.0
6667/tcp  open  irc        UnrealIRCd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap done: 5 IP addresses (5 hosts up) scanned in 13.34 seconds

```

### 8.4 Vulnerabilty Scan
A vulnerability scan is a technique used to identify potential security weaknesses in systems and services on a network. Using Nmap with vulnerability scripts (e.g., --script vuln), the scan checks for known issues such as outdated software, misconfigurations, and exploitable services. This helps security professionals detect risks early and take appropriate measures to strengthen the network’s security.

The command `nmap --script vuln 192.168.10.0/24` was used to scan the network range to discover hosts and services. The command targeted the entire subnet (all IPs from 192.168.10.1 to 192.168.10.254). The argument `--script vuln` tells Nmap to run its NSE (Nmap Scripting Engine) vulnerability scripts, which checks for known security weaknesses in the network range provided.

```bash
nmap --script vuln 192.168.10.0/24

Starting Nmap 7.94 ( https://nmap.org ) at May 03, 07:48 PM EDT
Nmap scan report for 192.168.10.10
Host is up (0.0030s latency).

Nmap scan report for 192.168.10.20
Host is up (0.0040s latency).

Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|       Risk factor: HIGH
|         A critical remote code execution vulnerability exists in Microsoft SMBv1
|         servers (ms17-010).
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx

Nmap scan report for 192.168.10.30
Host is up (0.0020s latency).

Host script results:
| ms-sql-empty-password: 
|   [192.168.10.30:1433]
|_    sa:<empty> => Login Success

Nmap scan report for 192.168.10.40
Host is up (0.0070s latency).

Nmap scan report for 192.168.10.50
Host is up (0.0080s latency).

Nmap done: 5 IP addresses (5 hosts up) scanned in 13.74 seconds
```

- The command nmap --script vuln 192.168.10.0/24 was used to perform a vulnerability scan across all hosts within the 192.168.10.0/24 network. This scan leveraged Nmap’s scripting engine (NSE) to identify known security weaknesses in active systems and services.

    The results revealed that all five hosts in the subnet were reachable. However, critical vulnerabilities were identified on specific systems. Host 192.168.10.20 was found to be vulnerable to the MS17-010 (SMBv1) remote code execution vulnerability, which carries a high risk and could allow attackers to execute arbitrary code on the system. Additionally, host 192.168.10.30 was discovered to have a Microsoft SQL Server instance configured with an empty password for the sa account, representing a severe authentication weakness that allows unauthorized access.

    The remaining hosts (192.168.10.10, 192.168.10.40, and 192.168.10.50) were active but did not show any vulnerabilities with the scripts used in this scan. Overall, the scan demonstrated the effectiveness of automated vulnerability detection in identifying critical security issues, enabling timely remediation to reduce the risk of exploitation and strengthen network security.



    ## 9. Key Findings and Risk Table

| Tool Used | Technique Type | Command(s) Executed | Key Output / Observed Result | Insight Gained | Risk Level | Lessons Learned | Recommended Action / Mitigation | Conclusion |
|---|---|---|---|---|---|---|---|---|
| Nmap | Host Discovery (Ping Scan) | `nmap -sn 192.168.10.0/24` | 5 hosts identified as active on the network | Network contained multiple reachable systems | Low | Attackers can easily map live hosts | Disable ICMP where possible; monitor network scanning | Initial reconnaissance revealed attack surface |
| Nmap | SYN Scan (Stealth Port Scan) | `nmap -sS 192.168.10.0/24` | Multiple open ports (SMB, RDP, HTTP, SSH, SQL) discovered across hosts | Critical services exposed across the network | High | Open ports increase attack surface significantly | Close unused ports; enforce firewall rules; segment network | Port scanning exposed entry points for attacks |
| Nmap | Service & Version Detection | `nmap -sV 192.168.10.0/24` | Identified OS and service versions (Windows Server, IIS, SQL Server, Apache, SSH) | Outdated or specific versions can be targeted for exploits | High | Service fingerprinting enables targeted attacks | Regular patching; hide service banners; update software | Version detection improved attack precision |
| Nmap | Default Script Scanning | `nmap -sC -sV 192.168.10.0/24` | Additional service details gathered using NSE scripts | Automated scripts provide deeper enumeration | Medium | Default scripts quickly reveal misconfigurations | Disable unnecessary services; harden configurations | Script scanning enhanced reconnaissance depth |
| Nmap | Vulnerability Scanning | `nmap --script vuln 192.168.10.0/24` | Detected MS17-010 SMB vulnerability (192.168.10.20) and SQL empty password (192.168.10.30) | Critical weaknesses exist allowing RCE and unauthorized DB access | Critical | Automated scans can identify exploitable vulnerabilities quickly | Patch systems; disable SMBv1; enforce strong passwords; restrict DB access | Vulnerability scan confirmed exploitable security gaps |
| Nmap | SMB Enumeration Insight | (from scans showing port 445 open) | SMB service exposed on multiple hosts | SMB is a high-risk service if not secured | High | SMB is a common attack vector (e.g., ransomware) | Disable SMBv1; restrict access; monitor SMB traffic | SMB exposure increased likelihood of compromise |
| Nmap | Remote Access Exposure | (RDP & SSH from scan results) | RDP (3389) and SSH (22) open on several hosts | Remote access services increase risk of brute-force attacks | High | Weak authentication can lead to system compromise | Enforce MFA; restrict access; use VPNs | Remote access services expanded attack vectors |
| Nmap | Database Exposure | (Port 1433 - SQL Server) | SQL Server detected with weak authentication | Databases are high-value targets for attackers | Critical | Misconfigured databases lead to data breaches | Enforce strong credentials; limit access; audit DB activity | Database exposure created risk of sensitive data loss |


## 10. Troubleshooting
When commands failed or services did not respond:

- The current shell was validated using the command `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.

## 11. Summary and Lesson Learnt
### Summary
The assessment used multiple Nmap scanning techniques to map the network, identify active hosts, and analyze exposed services. Host discovery confirmed five live systems, while port scanning revealed several open and potentially sensitive services such as SMB, RDP, SSH, HTTP, and SQL. Service and version detection provided deeper insight into the operating systems and applications running on each host, enabling targeted analysis. Further enumeration using default scripts enhanced visibility, and vulnerability scanning identified critical issues, including an SMB remote code execution vulnerability (MS17-010) and a misconfigured SQL Server with an empty administrator password. Overall, the scans demonstrated a broad attack surface with both configuration weaknesses and exploitable vulnerabilities.

### Lesson Learnt
- Network reconnaissance using tools like Nmap can quickly reveal active hosts, open ports, and running services, giving attackers a clear view of the attack surface.
- Service and version detection is critical because outdated or identifiable software versions can be directly mapped to known exploits.
- Vulnerability scanning highlights how misconfigurations (such as empty passwords) and unpatched systems (like MS17-010) create severe security risks.
- Exposed services such as SMB, RDP, SSH, and SQL significantly increase the risk of unauthorized access if not properly secured.
- Automated scripts and scanning techniques can uncover security weaknesses faster than manual inspection, emphasizing the need for proactive defense.
- Proper security hygiene—patch management, service hardening, firewall configuration, and strong authentication—is essential to reduce exposure.
- Regular vulnerability assessments are necessary because network security is dynamic and continuously evolving.

## 12. Conclusion
The network exhibited significant security risks, particularly due to exposed services and critical vulnerabilities that could allow unauthorized access or full system compromise. The presence of unpatched systems and weak authentication mechanisms highlights gaps in patch management and security hardening practices. Immediate remediation is required for high-risk findings, alongside improved access control, regular vulnerability assessments, and continuous monitoring. Strengthening these areas will reduce the likelihood of exploitation and improve the overall security posture of the network.