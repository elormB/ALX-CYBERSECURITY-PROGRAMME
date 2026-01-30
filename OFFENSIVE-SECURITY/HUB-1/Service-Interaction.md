# Hub 1 Challenge – Service Interaction

## 1. Background

Service interaction involves directly communicating with network services to gather detailed information about their configuration, version, and behavior. This active reconnaissance technique provides real-time intelligence about service implementations and potential vulnerabilities. The strategic value of service interaction lies in its ability to reveal current, accurate information about service states, configurations, and responses that cannot be obtained through passive reconnaissance alone.

The Service Interaction challenge introduced techniques for probing and interacting with exposed network services. The lab simulated how an ethical hacker fingerprints network infrastructure, validates service availability, and extracts valuable configuration details without exploiting a target.

## 2. Objectives

The objective of this lab was to:

- Confirm service availability and connectivity.
- Interact with live services using multiple protocols.
- Collect banners and version identifiers.
- Retrieve HTTP and HTTPS responses.
- Examine header metadata and SSL/TLS certificates.
- Build confidence using command‑line interaction tools.

These competencies form the reconnaissance foundation required prior to vulnerability exploitation.

## 3. Scope & Limitations

- All hands-on work was restricted to Hub 1 – Service Interaction tasks within the ALX Cybersecurity Lab.
- No network probes, banner grabs, or service queries were performed outside the approved environment.
- All interactive traffic, including HTTP requests, socket connections, and TLS queries, was directed solely at lab-simulated hosts.
- Tools and commands were not executed against external networks, public-facing systems, or unauthorized infrastructure.

## 4. Legal & Ethical Disclaimer

- All service interrogation activities were conducted exclusively within the authorized ALX training environment.
- No unauthorized scanning, enumeration, banner grabbing, or network communication occurred against any real-world production systems.
- The tools used — including **`netcat`, `telnet`, `curl`, and `wget**` — were executed strictly for education, skill development, and ethical testing.
- All outputs and observations remained within the lab context and were not shared or applied outside of their intended scope.

## 5. Tools Used

The following network interrogation tools were employed during the practical:

| **Category** | **Tool Name** | **Purpose / Function** |
| --- | --- | --- |
| **Operating System** | Kali Linux | Penetration testing platform used to execute all tools and interact with lab services |
| **Raw Socket Interaction** | **netcat (nc)** | Open TCP/UDP connections, verify service availability, banner grabbing |
| **Legacy Terminal-Based Interaction** | **telnet** | Connect to TCP services interactively to observe service responses and banners |
| **HTTP/HTTPS Request Handling** | **curl** | Send custom HTTP requests, inspect responses, retrieve headers, test SSl/TLS, follow redirects |
| **Web Resource Retrieval** | **wget** | Download web files/resources to confirm content availability and server response |

Each tool provided different visibility into how services responded over the network.

### - Assets, Domain and Port Table:

| **IP / Domain** | **Service / Protocol** | **Port** | **Service Type** |
| --- | --- | --- | --- |
| 192.168.56.101 | FTP | 21 | File Transfer |
| 192.168.56.102 | SSH | 22 | Remote Secure Shell |
| 192.168.56.103 | Telnet | 23 | Remote Terminal |
| 192.168.56.104 | SMTP | 25 | Mail Transfer |
| 192.168.56.105 | HTTP | 80 | Web Server |
| 192.168.56.106 | POP3 | 110 | Mail Retrieval |
| 192.168.56.107 | IMAP | 143 | Mail Retrieval |
| 192.168.56.108 | HTTPS | 443 | Secure Web Server |
| 192.168.56.109 | MySQL | 3306 | Database |
| 192.168.56.110 | PostgreSQL | 5432 | Database |
| example.com | Web Domain (HTTPS) | 443 | HTTP/HTTPS |
| self-signed.badssl.com | HTTPS (invalid) | 443 | TLS Test Site |
| httpbin.org | Web API | 80/443 | Redirect/POST test |

## 6. Challenge Overview

The Service Interaction lab consisted of **six (6)** exercises:

1. **Basic Connectivity** — verified hosts were reachable over the network
2. **Banner Grabbing** — captured service greetings and metadata
3. **Basic HTTP Request** — retrieved raw responses from web servers
4. **HTTP Header Analysis** — extracted server identities, cookies, content type, etc.
5. **SSL/TLS Certificate Inspection** — viewed certificate issuer, validity period, expiration
6. **Advanced CURL Operations** — tested custom methods, verbose output, and redirection

These tasks reinforced how reconnaissance yields actionable intelligence without exploitation.

## 7. Lab Initialization and Access

To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **`Hub 1`** was started.
2. The `running` command was executed to validate that the environment was live: `running # expected output: hub-1 running`
3. The browser was opened and the interface was accessed via `http://localhost.`
4. The Organization Information module was selected and the first task was launched.

## 8. Tasks Completed

### 8.1 Basic Connectivity using netcat and telnet

**Purpose:** Netcat was used to establish raw TCP or UDP connections to exposed services in order to verify service availability and retrieve service banners. 

Telnet was used to initiate interactive TCP sessions with remote services to observe their initial responses and manually issue protocol commands.

**Outcome:** Netcat successfully confirmed which ports were live, revealed active services through banner responses, and provided valuable version information that could support vulnerability assessment. Telnet established interactive connectivity to open services, captured initial server messages and banners, and verified that the targeted applications were active and properly responding on their ports.

```bash
pentester@kali-linux:~$ nc -v 192.168.56.101 21
Connection to 192.168.56.101 port 21 [tcp/ftp] succeeded!   # Port 21 is OPEN and accepting TCP connections
220 (vsftpd 2.3.4)                                          # FTP banner leak: running vsftpd version 2.3.4
                                                            # Useful: tells you the exact software and version
Connection closed by foreign host.                          # Server ended the session automatically (no login)
```

```bash
pentester@kali-linux:~$ telnet 192.168.56.103 23
Trying 192.168.56.103...                                   # Attempting TCP connection to Telnet port
Connected to 192.168.56.103.                               # Port 23 OPEN — host running Telnet service
Escape character is '^]'.                                  # Telnet session instructions (control escape)
Welcome to Telnet Server                                   # Banner leak: confirms active Telnet login service
Connection closed by foreign host.                         # Server terminated connection (no credentials entered)

```

### 8.2 Banner Grabbing using netcat and telnet

**Purpose:** Netcat was used to create a flexible TCP/UDP connection to a specified port on a target system and retrieve service banners rapidly, providing insight into exposed applications and potential vulnerabilities. Telnet was used to establish a plain-text connection to a target service, to manually interact with the service and capture its response banner to identify the running application and version.

**Outcome:** Netcat revealed open ports and extracted banner information, that could be used to map active services and evaluate their security exposure. Telnet retrieved service banner details from the target, confirming port accessibility and identifying the running service version for further analysis.

```bash
pentester@kali-linux:~$ nc -v 192.168.56.201 22
Connection to 192.168.56.201 port 22 [tcp/ssh] succeeded!    # Port 22 is open and accepting connections
SSH-2.0-OpenSSH_7.9p1 Debian-10+deb10u2                      # Service banner reveals OpenSSH version and Debian base
Connection closed by foreign host.                           # Server closed the connection automatically
```

```bash
pentester@kali-linux:~$ telnet 192.168.56.206 5432
Trying 192.168.56.206...                                   # Attempting connection to port 5432
Connected to 192.168.56.206.                                # Connection succeeded, port is open
Escape character is '^]'.                                   # Telnet instruction
PostgreSQL DB server ready                                   # Database service banner confirms PostgreSQL is active
Connection closed by foreign host.                           # Server terminated the session
```

```bash
pentester@kali-linux:~$ nc -w 3 192.168.56.202 21
220 (vsftpd 2.3.4)                                          # FTP server detected and version revealed
Connection closed by foreign host.                          # Session closed by the server
```

### 8.3 Basic HTTP Requests with cURL & wget

**Purpose:** `curl` is used to send HTTP requests and view raw responses directly in the terminal, allowing manual inspection of web server behavior, headers, and content. `wget` is used to retrieve web content or entire files over HTTP by downloading and saving the response locally for further analysis or offline access. The main difference here is that, **curl** is designed primarily for sending custom HTTP/HTTPS requests and inspecting responses in the terminal, giving detailed control over headers, methods, and request options ( interacting and inspecting) while **wget** is used to **download content** (files, web pages, or full sites) from a URL, often saving it to disk for later use rather than for interactive inspection ( fetching and saving).

**Outcome:** `curl` retrieved HTTP responses from target web services, providing headers, content, and server behavior information that helped identify server type, version, and configuration details. `wget` downloaded web pages or resources from the target, enabling offline analysis of content and verification of web service availability.

```bash
pentester@kali-linux:~$ curl -I https://example.com
# -I tells curl to make a HEAD request (fetch headers only, no body)

HTTP/1.1 200 OK
# HTTP status code showing success

Content-Type: text/html; charset=UTF-8
# Server tells the browser the page is HTML and uses UTF-8 encoding

Content-Length: 1256
# Size of the webpage in bytes

Server: ECS (nyb/1D2B)
# Web server software responding (ECS - Edge Content Server)

Date: Sat, 01 Jun 2024 10:30:15 GMT
# Timestamp sent by the web server

Connection: keep-alive
# Indicates persistent/TCP connection stays open for efficiency
```

```bash
pentester@kali-linux:~$ wget --server-response https://example.com
# wget downloads the page, and --server-response displays raw HTTP headers

--2024-06-01 10:30:15--  https://example.com/
# Timestamp + target URL

Resolving example.com (example.com)... 93.184.216.34
# DNS resolves domain name to IP address

Connecting to example.com (example.com)|93.184.216.34|:443... connected.
# A TCP session is successfully established on HTTPS port 443

HTTP request sent, awaiting response...
# wget sends the GET request

  HTTP/1.1 200 OK
  # Server responds with HTTP 200 Success

  Content-Type: text/html; charset=UTF-8
  # HTML content using UTF‑8 encoding

  Content-Length: 1256
  # Size of the resource

  Server: ECS (nyb/1D2B)
  # Server software identification

  Date: Sat, 01 Jun 2024 10:30:15 GMT
  # Date from server

  Connection: keep-alive
  # TCP connection stays open

Length: 1256 (1.2K) [text/html]
# wget confirms total size

Saving to: 'index.html'
# File is written locally as index.html

100%[==================================>] 1,256       --.-K/s   in 0s
# Download completed instantly due to small file size

2024-06-01 10:30:15 (1.2 MB/s) - 'index.html' saved [1256/1256]
# Success confirmation: file saved and size matched exactly

```

### 8.4 SSL/TLS Certificate Handling using curl and wget

**Purpose:** `curl` was used with the **`-k` option** to ignore SSL/TLS certificate validation and request content from a secure endpoint that presented an untrusted or self‑signed certificate. 
`wget` was executed with **`--no-check-certificate`** to download content from an HTTPS resource that triggered certificate errors due to an invalid or self‑signed certificate.

**Outcome:** `curl` successfully connected to the HTTPS site and returned the web page content, demonstrating that encrypted traffic was reachable even though the certificate could not be verified. `wget` retrieved the target file/page without terminating due to certificate verification issues, confirming access to the service while bypassing certificate trust requirements.

```bash
pentester@kali-linux:~$ curl -k https://self-signed.badssl.com/
# -k tells curl to ignore SSL certificate validation errors
# This is required because the site uses a self‑signed certificate

<!DOCTYPE html>
<html>
<head><title>self-signed.badssl.com</title></head>
<body>
<h1>self-signed.badssl.com</h1>
<p>This page uses a self-signed certificate.</p>
</body>
</html>
# curl successfully fetches the page content despite the invalid certificate
```

```bash
pentester@kali-linux:~$ wget --no-check-certificate https://self-signed.badssl.com/
# --no-check-certificate tells wget to allow invalid/expired/self‑signed SSL

--2024-06-01 10:30:15--  https://self-signed.badssl.com/
# Timestamp and target URL

Resolving self-signed.badssl.com (self-signed.badssl.com)... 104.154.89.105
# DNS resolves the domain to its IP address

Connecting to self-signed.badssl.com (104.154.89.105)|104.154.89.105|:443... connected.
# TCP handshake on port 443 is successful

WARNING: cannot verify self-signed.badssl.com's certificate, issued by 'CN=badssl.com':
# wget warns that the certificate cannot be validated

Self-signed certificate encountered.
# Confirms that the cert is not signed by a trusted authority

HTTP request sent, awaiting response... 200 OK
# Server replies with HTTP 200 Success

Length: 1234 (1.2K) [text/html]
# wget identifies the size of the file being downloaded

Saving to: 'index.html'
# File will be written to local disk as index.html

100%[======================================>] 1,234       --.-K/s   in 0s
# File fully downloaded

2024-06-01 10:30:15 (1.2 MB/s) - 'index.html' saved [1234/1234]
# Final confirmation: file saved and size matches expected length
```

### 8.5 Advanced cURL Techniques

**Purpose:** `cURL` was used with extended request options (`-L`, `-i`, `-d`, `-X`, and `-A`) to simulate real-world web interactions, including following redirects, submitting POST data, retrieving response headers, and modifying the client identity.

**Outcome:** `cURL` successfully executed multiple HTTP request types, confirmed redirect chains, displayed server response metadata, submitted form-style data, and altered the user‑agent string, demonstrating how web servers respond to different request behaviors and inputs.

```bash
pentester@kali-linux:~$ curl -L http://httpbin.org/redirect/1
# -L tells curl to follow HTTP redirects automatically

{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.68.0"
  },
  "origin": "203.0.113.1",
  "url": "http://httpbin.org/get"
}
# Final response after following the redirect returns JSON from /get

```

```bash
pentester@kali-linux:~$ curl -i -L http://httpbin.org/redirect/1
# -i shows HTTP headers
# -L follows redirects

HTTP/1.1 302 FOUND
Location: /get
Content-Type: text/html; charset=utf-8
Content-Length: 0
Server: gunicorn/19.9.0
Date: Sat, 01 Jun 2024 10:30:15 GMT
# First response: redirect (302), points client to /get

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 234
Server: gunicorn/19.9.0
Date: Sat, 01 Jun 2024 10:30:15 GMT
# Second response: final destination returns success + JSON data

{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.68.0"
  },
  "origin": "203.0.113.1",
  "url": "http://httpbin.org/get"
}
# Same JSON response from /get

```

```bash
pentester@kali-linux:~$ curl -d 'name=test' -X POST https://httpbin.org/post
# -d sends form data (default type: application/x-www-form-urlencoded)
# -X POST explicitly sets the HTTP method to POST

{
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "name": "test"
  },
  "headers": {
    "Accept": "*/*",
    "Content-Length": "9",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.68.0"
  },
  "json": null,
  "origin": "203.0.113.1",
  "url": "https://httpbin.org/post"
}
# httpbin echoes the POSTed form field `name=test`

```

```bash
pentester@kali-linux:~$ curl -A "MyAgent" https://example.com
# -A sets a custom User-Agent string
# Correct quoting, so curl executes normally

<!DOCTYPE html>
<html>
<head>
...
</html>
# Page downloaded successfully from example.com
# Only difference from normal curl: server sees User-Agent: "MyAgent"

```

## 9. Findings and Risk Table

| **Tool Used** | **Technique Type** | **Command(s) Executed** | **Key Output / Observed Result** | **Insight Gained** | **Risk Level** | **Recommended Action / Mitigation** |
| --- | --- | --- | --- | --- | --- | --- |
| `nc` (Netcat) | Basic Service Connectivity | `nc -v 192.168.56.101 21` | Connected -> **vsftpd 2.3.4** banner leak | Confirms FTP active and identifiable by version | **Medium** | Patch/update FTP or replace with SFTP; restrict access |
| `telnet` | Plain‑Text Connectivity Test | `telnet 192.168.56.103 23` | Telnet open -> “Welcome to Telnet Server” | Legacy plaintext protocol exposed to network | **High** | Disable Telnet, enforce SSH-only administration |
| `nc` | SSH Banner Grabbing | `nc -v 192.168.56.201 22` | Revealed **OpenSSH_7.9p1 Debian-10** | Service fingerprinting enabled for attackers | **Medium** | Update OpenSSH, minimize banners, enforce MFA |
| `telnet` | Database Banner Discovery | `telnet 192.168.56.206 5432` | PostgreSQL responded with DB readiness | DB service reachable without auth | **High** | Restrict DB ports to internal network; firewall segmentation |
| `nc -w 3` | Service Timeout + Banner Probe | `nc -w 3 192.168.56.202 21` | FTP again leaks **vsftpd 2.3.4** | Confirms additional exposed FTP instance | **Medium** | Remove duplicate FTP server; enforce FTPS/SFTP |
| `curl` | Basic HTTP GET Fetch | `curl https://example.com` | HTML printed to terminal | Confirms web server accessible & responsive | **Low–Medium** | Ensure web stack is patched; test endpoints for exposure |
| `wget` | HTTP(S) File Retrieval | `wget https://example.com` | Saved **index.html** locally | Verifies download capability, file access path | **Low** | Validate directory/file access restrictions |
| `curl -I` | HTTP Header Inspection | `curl -I https://example.com` | Server headers returned (status, type, server) | Server stack metadata publicly visible | **Medium** | Remove server banners, enable security headers |
| `wget --server-response` | Header + Response Enumeration | `wget --server-response https://example.com` | Full server response printed to console | Information disclosure via header fingerprinting | **Medium** | Implement header sanitization and WAF filtering |
| **`curl -k`** | TLS/SSL Certificate Bypass (Client-side) | `curl -k https://self-signed.badssl.com/` | HTML displayed despite invalid cert | Shows how cert validation can be bypassed for testing | **High** | Never use `-k` in production; install trusted certs; enforce TLS validation |
| **`wget --no-check-certificate`** | TLS/SSL Certificate Bypass (Client-side) | `wget --no-check-certificate https://self-signed.badssl.com/` | File downloaded with warning about self‑signed cert | Demonstrates risk of trusting invalid certificates | **High** | Use only in lab; enforce certificate pinning + proper CA trust |
| **`curl -L`** | HTTP Redirect Following | `curl -L http://httpbin.org/redirect/1` | Redirect handled; JSON returned successfully | Shows how clients automatically follow redirects | **Low** | Monitor redirect chains, avoid insecure redirect loops |
| **`curl -i -L`** | Header + Redirect Analysis | `curl -i -L http://httpbin.org/redirect/1` | Displays both 302 + final 200 response with headers | Reveals full request flow → useful for debugging | **Low–Medium** | Validate redirect behavior; prevent open redirect abuse |
| **`curl -d -X POST`** | HTTP POST Testing | `curl -d 'name=test' -X POST https://httpbin.org/post` | Server echoed form field; full HTTP POST shown | Validates API endpoints and POST data handling | **Medium** | Input validation required server-side to avoid injection |
| **`curl -A`** | User-Agent Impersonation | `curl -A "MyAgent" https://example.com` | Successful request using custom UA header | Demonstrates ability to spoof client identity | **Medium** | Detect UA spoofing; implement logging and anomaly monitoring |

## 10. Troubleshooting

When commands failed or services did not respond:

- The current shell was validated using `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.

## 11. Conclusion

The lab focused on network and web service interactions using tools such as `nc`, `telnet`, `curl`, and `wget`. Key findings included open ports, service banners revealing software versions, legacy protocols like Telnet, and accessible databases. Web interactions demonstrated handling redirects, POST requests, custom user agents, and SSL/TLS certificate validation bypass. The exercises emphasized the importance of secure configurations, patching, and controlled testing to reduce attack surfaces and improve overall cybersecurity posture.