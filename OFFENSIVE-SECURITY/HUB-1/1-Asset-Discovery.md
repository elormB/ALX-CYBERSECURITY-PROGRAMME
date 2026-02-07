# Hub 1 Challenge – Asset Discovery Lab
## 1. Background
Asset discovery is a foundational phase of offensive security and reconnaissance. The purpose of this lab was to identify and enumerate publicly discoverable assets belonging to a target, ranging from DNS records and subdomains to hidden infrastructure and metadata. This exercise focused solely on the Asset Discovery module within Hub-1 of the ALX Cybersecurity programme.
## 2. Objectives
The objective of this module was to understand, enumerate, and map digital assets using a variety of open-source intelligence (OSINT) techniques and tools. Specifically, the tasks aimed to:
1. Resolve DNS information
2. Enumerate name servers
3. Attempt DNS zone transfers
4. Discover subdomains through active and passive enumeration
5. Query external intelligence sources, such as Shodan, to identify internet-wide exposures and public-facing services
   
## 3. Scope & Limitations
1. All activities were performed strictly within Hub 1 – Asset Discovery.
2. No exercises or targets outside this hub were attempted unless explicitly instructed.
3. All enumeration was restricted to the designated lab environment.
4. All tools were used only within the approved scope of the lab and were not deployed against live or external systems without written authorization.

## 4. Legal & Ethical Disclaimer
1. All activities were conducted strictly within the authorized lab environment provided by the ALX Cybersecurity Programme.
2. No real-world or production systems were targeted at any point.
3. The environment and tools were used solely for learning and the development of cybersecurity skills.
## 5. Tools Used
| Category | Tools |
| --- | --- |
| Operating System | Kali linux |
| DNS Record Extration | Dig |
| Manual DNS Querying | NSlookup |
| DNS Enumeration and Zone Transfer | DNS Zone Transfer |
| Passive Subdomain Discovery | Subfinder |
| Passive and Active Subdomain Discovery | Amass |
| Internet-Wide Scanning & Exposure Search | Shodan |
| Domain | example.com |

## 6. Lab Environment Setup
To begin the assessment, the lab environment was initialized and verified as operational.
1. The Linux terminal was opened and `Hub 1` was started to deploy the lab environment.
2. The `running` command was executed to confirm that the environment was active.
3. The challenge hub status was verified to ensure it was ready for use.
```bash
running
Expected output: hub-1 running
```
4. The lab graphical interface was accessed by navigating to `http://localhost` in a web browser.
5. The Asset Discovery category was opened, and the first task was initiated from the menu.
## 7. Methodology and Outcomes

The Asset Discovery module used **six reconnaissance techniques**, each relying on a different tools to enumerate DNS data, exposed infrastructure, and public-facing assets.

### 7.1 DNS Dig

**Purpose: `dig`** (Domain Information Groper) is a command‑line network administration tool used to look up Domain Name System (DNS) records (A, AAA, MX, TXT, NS, etc.)

**Goal:** Build a baseline understanding of the target’s exposed DNS footprint and guide further reconnaissance by revealing assets and potential misconfigurations.

```bash
pentester@kali-linux:~$ dig example.com
; <<>> DiG 9.16.1-Ubuntu <<>> example.com         # DiG version and the queried domain
;; global options: +cmd                             # Default options for the query
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
# qr = query response, rd = recursion desired, ra = recursion available
# QUERY/ANSWER/AUTHORITY/ADDITIONAL = number of records in each section
;; QUESTION SECTION:
;example.com.                   IN      A           # Asking for A record (IPv4 address)
;; ANSWER SECTION:
example.com.            86400   IN      A       93.184.216.34
# 86400 = TTL in seconds (time DNS can cache this record)
# A = IPv4 address record
# 93.184.216.34 = resolved IP of example.com
;; Query time: 15 msec                              # Time to get the response
;; SERVER: 127.0.0.53#53(127.0.0.53)              # Local DNS resolver handling the query
;; WHEN: Mon Jan 01 12:00:00 UTC 2024             # Date & time of query
;; MSG SIZE  rcvd: 56                               # Size of DNS response in bytes
```

```bash
pentester@kali-linux:~$ dig example.com TXT
; <<>> DiG 9.16.1-Ubuntu <<>> example.com TXT        # DiG version & TXT record query
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 98765
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
# qr = query response, rd = recursion desired, ra = recursion available
# QUERY/ANSWER/AUTHORITY/ADDITIONAL = record counts
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494               # EDNS extension info
;; QUESTION SECTION:
;example.com.                    IN      TXT         # Requesting TXT records
;; ANSWER SECTION:
example.com.         86400       IN      TXT     "v=spf1 include:_spf.example.com ~all"
# 86400 = TTL (24 hours)
# TXT record contains SPF policy for email authentication
# v=spf1 ... ~all = SoftFail for unauthorized mail sources
;; Query time: 18 msec                               # Time taken for DNS query
;; SERVER: 127.0.0.53#53(127.0.0.53)                # Local DNS resolver
;; WHEN: Mon Jan 01 12:00:00 UTC 2024               # Timestamp of query
;; MSG SIZE  rcvd: 89                                # DNS response size

```

```bash
pentester@kali-linux:~$ dig example.com NS
; <<>> DiG 9.16.1-Ubuntu <<>> example.com NS        # Querying NS (Name Server) records
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 13579
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
# qr = query response, rd = recursion desired, ra = recursion available
# ANSWER: 2 = Two NS records returned
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494              # EDNS extension support info
;; QUESTION SECTION:
;example.com.                    IN      NS         # Requesting authoritative name servers
;; ANSWER SECTION:
example.com.         86400       IN      NS      ns1.example.com.
example.com.         86400       IN      NS      ns2.example.com.
# 86400 = TTL (24 hours)
# NS records identify the authoritative DNS servers for the domain
# ns1.example.com and ns2.example.com handle DNS queries for example.com
;; Query time: 12 msec                               # Time taken for DNS resolution
;; SERVER: 127.0.0.53#53(127.0.0.53)                # Local DNS resolver used
;; WHEN: Mon Jan 01 12:00:00 UTC 2024               # Timestamp of DNS query
;; MSG SIZE  rcvd: 67                                # Size of DNS response packet
```

```bash
pentester@kali-linux:~$ dig example.com MX
; <<>> DiG 9.16.1-Ubuntu <<>> example.com MX        # Querying MX (Mail Exchange) records
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 54321
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
# qr = query response, rd = recursion desired, ra = recursion available
# ANSWER: 2 = Two MX records found
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494              # Indicates EDNS support
;; QUESTION SECTION:
;example.com.                    IN      MX         # Requesting mail server records
;; ANSWER SECTION:
example.com.         86400       IN      MX      10 mail.example.com.
example.com.         86400       IN      MX      20 mail2.example.com.
# 86400 = TTL (24 hours)
# MX = Mail Exchange record
# 10 and 20 = priority values (lower = higher priority)
# mail.example.com is the primary mail server
# mail2.example.com is the secondary/fallback server
;; Query time: 20 msec                               # Time needed for DNS response
;; SERVER: 127.0.0.53#53(127.0.0.53)                # Local system DNS resolver
;; WHEN: Mon Jan 01 12:00:00 UTC 2024               # Timestamp of query
;; MSG SIZE  rcvd: 78                                # Total size of DNS response
```

### 7.2 DNS nslookup

**Purpose:** `nslookup` is a command-line tool used to manually query DNS servers and retrieve information about domain names, IP addresses, and name server configurations.
**Goal:** To use `nslookup` to manually query DNS servers, compare results with `dig`, and validate authoritative DNS responses.

```bash
pentester@kali-linux:~$ nslookup 93.184.216.34
Server:  192.168.56.1
Address: 192.168.56.1#53
93.184.216.34.in-addr.arpa    name = example.com
# Reverse DNS lookup (PTR record)
# Confirms that IP 93.184.216.34 maps back to example.com
# Useful for verifying mail servers or identifying hosts
```

```bash
pentester@kali-linux:~$ nslookup -type=A example.com
Server:  192.168.56.1                   # DNS server used for the lookup
Address: 192.168.56.1#53                # IP and port (53 = DNS)
Name:    example.com                    # Queried domain name
Address: 93.184.216.34                  # Resolved IPv4 address (A record)
# nslookup displays only basic DNS results compared to dig
# This confirms the domain resolves to the same IP as the dig command
```

```bash
pentester@kali-linux:~$ nslookup -type=MX example.com
Server:  192.168.56.1                     # DNS server performing the lookup
Address: 192.168.56.1#53                  # DNS server IP and port (53)

example.com      mail exchanger = mail.example.com
# MX = Mail Exchange record
# Shows the mail server responsible for handling email for the domain
# Only one MX record is displayed by this DNS server
```

```bash
pentester@kali-linux:~$ nslookup -type=NS example.com

Server:  192.168.56.1                     # Local DNS resolver
Address: 192.168.56.1#53

example.com    nameserver = ns1.example.com
example.com    nameserver = ns2.example.com
# NS records identify the authoritative name servers for the domain
# ns1 and ns2.example.com handle DNS queries for example.com

```

```bash
pentester@kali-linux:~$ nslookup -type=TXT example.com
Server:  192.168.56.1
Address: 192.168.56.1#53
example.com     text = "v=spf1 include:_spf.example.com ~all"
# TXT record contains SPF policy information for email security
# v=spf1 … ~all = SoftFail for unauthorized mail senders
```

```python
pentester@kali-linux:~$ nslookup -type=AXFR example.com ns1.example.com
Server:  ns1.example.com
Address: 192.168.56.10#53
# Querying the zone transfer directly from ns1.example.com
example.com	zone = 5 records
# ns1 allows AXFR (zone transfer) and returns 5 records
example.com.	3600	IN	A	93.184.216.34
# A record → main domain IP
example.com.	3600	IN	MX	10 mail.example.com.
# MX record → mail server
example.com.	3600	IN	TXT	"v=spf1 include:_spf.example.com ~all"
# TXT record → SPF policy for email authentication
example.com.	3600	IN	NS	ns1.example.com.
example.com.	3600	IN	NS	ns2.example.com.
# NS records → authoritative name servers
;; Received 5 records.
# Confirms successful full zone transfer (AXFR)
```

```python
pentester@kali-linux:~$ nslookup -type=AXFR example.com ns2.example.com
Server:  ns2.example.com
Address: 192.168.56.11#53
# Querying the secondary nameserver
** Can't list domain example.com: Query refused
# ns2.example.com does NOT allow AXFR (security best practice)
;; Transfer failed.
# AXFR failure expected for secure DNS configuration
```

### 7.3 DNS Zone Transfer Attempt

**Purpose:** Test whether the DNS server allows zone transfers (AXFR) due to misconfiguration, which could expose all domain records.

**Goal:** Attempt to retrieve full domain records if the zone transfer is permitted (a common misconfiguration exploitation technique).

```bash
pentester@kali-linux:~$ dig @ns1.example.com example.com AXFR
; <<>> DiG 9.18.1-1ubuntu1.3-Ubuntu <<>> @ns1.example.com example.com AXFR
; (1 server found)
# AXFR = DNS Zone Transfer request
# @ns1.example.com = Explicitly query the nameserver for full zone data
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12350
;; flags: qr aa; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1
# qr = query response
# aa = authoritative answer (server is authoritative for the zone)
# ANSWER: 5 = Five DNS records returned from the zone
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
# Indicates modern EDNS support
;; QUESTION SECTION:
;example.com.                IN      AXFR
# Requesting a full zone transfer
;; ANSWER SECTION:
example.com.        3600    IN      A       93.184.216.34
# A record (IPv4 address)
example.com.        3600    IN      MX      10 mail.example.com.
# MX record (mail server priority 10)
example.com.        3600    IN      TXT     "v=spf1 include:_spf.example.com ~all"
# TXT record containing SPF policy
example.com.        3600    IN      NS      ns1.example.com.
example.com.        3600    IN      NS      ns2.example.com.
# NS records (authoritative nameservers)
;; Query time: 25 msec
;; SERVER: ns1.example.com#53(ns1.example.com)
;; WHEN: Mon Jun 24 12:00:00 UTC 2024
;; MSG SIZE  rcvd: 234
; Transfer completed.
# This means the zone transfer succeeded (VERY rare in real environments)
# Successful AXFR is considered a serious misconfiguration and security risk.
```

```bash
pentester@kali-linux:~$ dig @ns2.example.com example.com AXFR
; <<>> DiG 9.18.1-1ubuntu1.3-Ubuntu <<>> @ns2.example.com example.com AXFR
; (1 server found)
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: REFUSED, id: 12351
# status: REFUSED → ns2.example.com is denying the AXFR (zone transfer) request
# AXFR is restricted; unauthorized clients get REFUSED
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1
# qr = response; rd = recursion desired
# ANSWER: 0 → no records returned
# AUTHORITY: 0 → no SOA/NS included
# ADDITIONAL: 1 → only the OPT (EDNS) section appears
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
# EDNS present → modern DNS extension used, not related to the refusal
;; QUESTION SECTION:
;example.com.		IN	AXFR
# AXFR = full zone transfer request
;; Query time: 15 msec
# round‑trip time; normal for a refused response
;; SERVER: ns2.example.com#53(ns2.example.com)
# confirms the server that refused the AXFR
;; WHEN: Mon Jun 24 12:00:00 UTC 2024
# timestamp when query was executed
;; MSG SIZE  rcvd: 45
# small message → typical for a refused response
; Transfer failed.
# zone transfer denied — correct secure DNS behavior
```

### 7.4 Subfinder

**Purpose:** Perform passive subdomain enumeration by collecting domain assets from publicly available or open sources.

**Goal:** Identify publicly indexed subdomains that expand the target’s external footprint without directly interacting with its infrastructure.

```bash
pentester@kali-linux:~$ subfinder -d example.com -all
[INF] Loading provider config from /root/.config/subfinder/provider-config.yaml
# Subfinder is loading API keys and configuration for passive enumeration sources
[INF] Enumerating subdomains for example.com
[INF] Using all available sources
# -all = Use every passive source available
# This maximizes discovery of publicly known subdomains
[INF] Found 23 subdomains for example.com
# Total subdomains discovered across all passive data sources
www.example.com
mail.example.com
ftp.example.com
blog.example.com
api.example.com
admin.example.com
dev.example.com
staging.example.com
test.example.com
support.example.com
ns1.example.com
ns2.example.com
mx1.example.com
mx2.example.com
webmail.example.com
app.example.com
cdn.example.com
images.example.com
static.example.com
www2.example.com
old.example.com
archive.example.com
backup.example.com
# These are the identified subdomains
# Includes functional, administrative, development, and legacy targets
# Useful for attack surface mapping and vulnerability assessment
[INF] Enumeration completed in 4.8s
# Total time taken for the passive scan
```

### 7.5 Amass

**Purpose:** Enumerate subdomains using a combination of passive intelligence sources and active probing techniques, including brute‑forcing and scraping.

**Goal:** Expand the list of discovered assets by uncovering additional subdomains and infrastructure that may not be visible through passive tools alone.

```bash
pentester@kali-linux:~$ amass enum -d example.com
[INFO] Configuration loaded from: /root/.config/amass/config.ini
# Amass config found and loaded

[INFO] Starting enumeration of example.com
# Beginning passive + active subdomain discovery

[INFO] Found: www.example.com
[INFO] Found: mail.example.com
[INFO] Found: ftp.example.com
[INFO] Found: blog.example.com
[INFO] Found: api.example.com
[INFO] Found: admin.example.com
[INFO] Found: dev.example.com
[INFO] Found: staging.example.com
[INFO] Found: test.example.com
[INFO] Found: support.example.com
[INFO] Found: ns1.example.com
[INFO] Found: ns2.example.com
[INFO] Found: mx1.example.com
[INFO] Found: mx2.example.com
[INFO] Found: webmail.example.com
[INFO] Found: cdn.example.com
[INFO] Found: assets.example.com
[INFO] Found: images.example.com
# Subdomain enumeration results — discovered via combined resolvers, APIs, and scraping

[INFO] The enumeration has finished
# Amass finished processing all sources

----------------------------------------------------------------------------------------------------------------------
```

```python
pentester@kali-linux:~$ amass intel -d example.com
[INFO] Configuration loaded from: /root/.config/amass/config.ini
# Intelligence mode loads same config

[INFO] Starting intelligence gathering for example.com
# Looks for ASN, IP ranges, and related domains

[INFO] ASN: 12345 - Example Corp
# ASN (Autonomous System Number) hosting the domain

[INFO] CIDR: 192.168.1.0/24
# IP block associated with the organization

[INFO] Found related domain: example.org
[INFO] Found related domain: example.net
[INFO] Found related domain: examplecorp.com
# Related domains → discovered from WHOIS, SSL, DNS, certificates, reverse DNS, etc.

[INFO] Intelligence gathering finished
# End of intel mode
```

---

### 7.6 Shodan

**Purpose:** Search the internet for exposed devices, services, and technologies associated with the target domain by querying Shodan’s global scanning database.

**Goal:** Identify publicly accessible systems, open ports, vulnerable services, and metadata leaks that expand the attack surface.

## 8. Findings & Risk Table

| **Tool Used** | **Technique Type** | **Command(s) Executed** | **Key Output / Observed Result** | **Insight Gained** | **Risk Level** | **Recommended Action / Mitigation** |
| --- | --- | --- | --- | --- | --- | --- |
| `dig` | DNS Enumeration | `dig example.com`<br>`dig example.com NS`<br>`dig example.com MX`<br>`dig example.com TXT` | Returned core DNS records | Identified DNS servers, MX mail, IP structure | **Low** | No action needed; ensure DNS info reveals only intended records |
| `nslookup` | DNS Lookup | `nslookup example.com`<br>`nslookup -type=A example.com`<br>`nslookup -type=MX example.com`<br>`nslookup -type=TXT example.com`<br>`nslookup -type=NS example.com`<br>`nslookup -type=AXFR example.com ns1.example.com`<br>`nslookup -type=AXFR example.com ns2.example.com` | DNS resolution validated | - Confirmed authoritative server responses<br>- ns1.example.com allowed full DNS zone transfer which is a security misconfiguration.<br>- ns2.example.com did not allow  DNS zone transfer which is a security best practice. | Low | Maintain DNS hygiene; monitor for unintended record leaks |
| `dig AXFR` | DNS Zone Transfer Attempt | `dig AXFR example.com @ns1.example.com`<br>`dig AXFR example.com @ns2.example.com` | DNS Zone Transfer Attempt was successful for ns1.example.com<br>DNS Zone Transfer Attempt was unsuccessful for ns2.example.com | Successful AXFR is considered a serious misconfiguration and security risk.<br>Unsuccessful AXFR is a correct secure DNS configuration. | High for ns1.example.com<br>Low for ns2.example.com | Restrict AXFR to internal trusted hosts only<br>Disable zone transfers publicly. |
| `subfinder` | Passive Subdomain Enumeration | `subfinder -d example.com` | Found 23 subdomains | Expanded potential perimeter that could be leveraged for vector attack mapping | **Medium** | Review all discovered domains<br>Decommission unused hosts<br>Ensure TLS and patching |
| `amass` | Active + Passive Enumeration | `amass enum -d example.com`<br>`amass intel -d example.com` | Found 18 subdomains<br>Revealed ASN, CIDR, and other domains using SSL, WHOIS, reverse DNS and DNS. | Identified hidden/dev infrastructure | **Medium** | Inventory all assets; ensure unknown subdomains are secured, removed, or monitored |
| `shodan` | Public Exposure Scan | `shodan domain example.com` | Listed exposed IPs, open ports, banners using nginx, https, web and ssl. | Revealed exposed services and versions | **High** | Patch exposed services, enforce firewalls, enable IDS/IPS, remove unnecessary public ports |


## 9. Troubleshooting

If lab tools or browser do not initialize:

1. Confirm correct environment

```bash
echo $SHELL
#expected outcome
training-shell
```

2. If incorrect

```bash
cd ~/lab
./start_script.sh
```

3. Confirm lab status

```bash
running
```

4.  Ensure Kali VM is active
5. Restart session if necessary.

## 10. Conclusion

This module provided hands‑on exposure to fundamental reconnaissance techniques that form the cornerstone of real‑world penetration testing engagements. Through the execution of asset discovery tasks, the lab helped to develop practical skills in identifying:

- Publicly exposed DNS information
- Subdomains and associated digital footprint
- Misconfigurations, including unsecured DNS zone transfers
- Internet‑facing services and infrastructure identified via external intelligence platforms such as Shodan

The knowledge and skills acquired in this phase establish a critical foundation for subsequent offensive security activities, including vulnerability assessment, exploitation, and post‑exploitation operations.