# Hub 1 Challenge - OSINT Framework Lab

## 1. Background

**OSINT (Open Source Intelligence) Frameworks** are integrated platforms that automate the collection, processing, and correlation of open-source intelligence from multiple sources. These frameworks provide modular architectures that enable systematic intelligence gathering while maintaining comprehensive databases of findings and their relationships.

The strategic value of OSINT frameworks lies in their ability to automate repetitive tasks, correlate findings across multiple sources, and provide comprehensive intelligence pictures that would be difficult to achieve through manual collection alone.

The OSINT Framework Lab introduced the practical use of OSINT frameworks to collect open-source intelligence from publicly available sources.

The challenges in the **OSINT Frameworks**  lab guided the exploration of techniques and tools for systematic information gathering across multiple platforms. The lab emphasized the use of structured methodologies to automate and streamline intelligence collection.

## 2. Objectives

The objectives of this lab were to:

- Confirm the availability and status of OSINT frameworks and related lab services.
- Interact with OSINT tools, including Recon-ng and SpiderFoot, to perform structured reconnaissance.
- Collect domain, DNS, and host metadata from training targets.
- Retrieve and analyze WHOIS data and public exposure information.
- Examine collected intelligence for potential risks and indicators of weak configurations.
- Build confidence using command-line and GUI interaction tools for OSINT data collection.

These competencies established the foundation for effective reconnaissance and risk assessment prior to deeper cybersecurity investigations.

## 3. Scope and Limitations

- Activities were restricted to the **OSINT Frameworks section** of Hub 1.
- Two core OSINT frameworks were explored: Recon-ng and SpiderFoot.
- Tasks focused on **structured data collection, domain reconnaissance, DNS and WHOIS analysis, and public exposure intelligence**.
- Practical exercises emphasized the **automation of intelligence gathering** and **analysis of findings for potential risks**.
- No real-world systems, organizations, or individuals were investigated.
- Offensive penetration testing and exploitation of vulnerabilities were excluded.
- Data collection and reconnaissance were limited to **training targets provided in the lab environment**.
- Results reflected **controlled lab conditions**, not actual network environments.
- Findings were **educational in nature**, intended to build OSINT skills rather than produce actionable intelligence outside the lab

## 4. Legal and Ethical Disclaimer

- All actions were performed within the isolated lab environment.
- No real-world systems, domains, or third-party assets were probed.
- OSINT tools were used solely for educational purposes.
- Ethical and legal boundaries were observed at all times.
- Misuse of OSINT techniques outside controlled environments may violate data protection, privacy, and cybersecurity laws.

## 5. Tools Used

The tools utilized in this lab are:

- **Recon-ng:** a modular OSINT framework for structured reconnaissance and intelligence gathering.
- **SpiderFoot:** an automated OSINT scanner used to collect domain, IP, and public-source intelligence.
- **Kali Linux:** an operating system offered a controlled sandbox for all activities.
- **Kali Firefox Browser:** used to access the local dashboard and frameworks at `http://localhost`.
- **Domain:** example.com
- **IP-** 93.184.216.34

## 6. Challenge Overview

The lab contained **two main OSINT tasks**:

- **Recon-ng Framework:** Modules were added, workspaces configured, and domain metadata and host intelligence were collected.
- **SpiderFoot:** Automated scans were run to gather DNS, WHOIS, and public exposure data.

## 7. Lab Initialization and Access

To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **`Hub 1`** was started.
2. The `running` command was executed to validate that the environment was live: `running # expected output: hub-1 running`
3. The browser was opened and the interface was accessed via `http://localhost.`
4. Navigation proceeded to the **OSINT Frameworks** section.
5. Recon-ng and SpiderFoot tasks were initiated and completed.

## 8. Tasks Completed

### 8.1 Recong -ng

**Purpose:** Recon‑ng was used to perform structured reconnaissance through a modular framework that supports workspace creation, data storage, and targeted intelligence gathering. It enabled systematic collection of information such as domains, hosts, emails, and associated metadata from publicly available sources without requiring manual tool configuration.

**Outcome:** Recon‑ng successfully enumerated domain details, identified related hosts and subdomains, and extracted email information linked to the target. The collected data formed a foundation for further analysis and highlighted how publicly accessible metadata may expose organizations to profiling, phishing, and mapping threats during early reconnaissance phases.

```bash
pentester@kali-linux:~$ recon-ng
[*] Recon-ng v5.2.0
[*] No workspace selected.
[*] No modules loaded.
# recon-ng launches successfully but requires a workspace before running modules

[recon-ng][default] >

```

```bash
pentester@kali-linux:~$ workspaces add example_target
[+] Workspace 'example_target' added.
[+] Workspace 'example_target' selected.
# Creates a new workspace and switches to it
# Workspaces store data separately for each engagement

[recon-ng][example_target] >

```

```bash
pentester@kali-linux:~$ use recon/profiles-profiles/name_to_domain
[recon-ng][example_target][name_to_domain] >
# Loads the "name_to_domain" module
# Module attempts to discover domain information related to the target

```

```bash
pentester@kali-linux:~$ set SOURCE example.com
SOURCE => example.com
# Sets example.com as the input target for the module

```

```bash
pentester@kali-linux:~$ run
[*] Running module 'name_to_domain'...
[*] Searching for domain information...
[+] www.example.com - Found subdomain
[+] mail.example.com - Found subdomain
[+] api.example.com - Found subdomain
[+] admin.example.com - Found subdomain
[+] ftp.example.com - Found subdomain
[*] Module execution completed.
# Module performs reconnaissance and discovers related subdomains
# Results are automatically inserted into the workspace database

```

```bash
pentester@kali-linux:~$ show hosts

  +-------------------------------------------------------------------+
  | rowid | host              | ip_address     | region | country | latitude | longitude |
  +-------------------------------------------------------------------+
  | 1     | www.example.com    | 93.184.216.34  | US     | US      | 37.4056  | -122.0775 |
  | 2     | mail.example.com   | 93.184.216.34  | US     | US      | 37.4056  | -122.0775 |
  | 3     | api.example.com    | 93.184.216.34  | US     | US      | 37.4056  | -122.0775 |
  | 4     | admin.example.com  | 93.184.216.34  | US     | US      | 37.4056  | -122.0775 |
  | 5     | ftp.example.com    | 93.184.216.34  | US     | US      | 37.4056  | -122.0775 |
  +-------------------------------------------------------------------+
# Displays all discovered hosts now stored in the active workspace database
# All subdomains resolve to the same IP, suggesting a shared hosting setup

[recon-ng][example_target][name_to_domain] >

```

### 8.2 SpiderFoot:

**Purpose:** SpiderFoot was used to automate OSINT reconnaissance across multiple sources, including DNS records, IP ranges, WHOIS data, and social media footprints. It enabled comprehensive scanning with minimal manual configuration, aggregating intelligence into a single report for analysis of public exposure and potential risk indicators.

**Outcome:** SpiderFoot successfully collected domain and IP intelligence, retrieved WHOIS and DNS records, and discovered public exposure data including social links and potential vulnerabilities. The scan provided a detailed overview of the target’s footprint, highlighting areas where misconfigurations or exposed data could create security risks.

```bash
pentester@kali-linux:~$ spiderfoot -s example.com
[*] SpiderFoot 4.0.0
[*] Starting scan for: example.com
# SpiderFoot begins an automated OSINT scan against the target domain

[*] Using modules: sfp_whois, sfp_dnsresolve, sfp_dnsdb, sfp_subdomain_enum, sfp_ssl_certificate, sfp_web_framework, sfp_email_extractor
# SpiderFoot lists the enabled modules it will run:
# - WHOIS lookup
# - DNS resolution
# - Passive DNS database checks
# - Active subdomain enumeration
# - SSL certificate scraping
# - Web technology detection
# - Email harvesting
```

```bash
[+] sfp_whois: Found WHOIS data for example.com
# Successfully retrieves registrar, ownership, and registration info

[+] sfp_dnsresolve: Resolved 15 subdomains
# Performs DNS lookups and confirms which subdomains actively resolve

[+] sfp_dnsdb: Found 8 additional subdomains from passive DNS
# Queries DNS history databases to collect older or cached hostnames

[+] sfp_subdomain_enum: Found 12 subdomains via enumeration
# Uses brute force or wordlists to identify hidden or lesser-known subdomains

[+] sfp_ssl_certificate: Found SSL certificate data
# Extracts certificate info such as issuer, alternate names, expiry dates

[+] sfp_web_framework: Detected nginx web server
# Fingerprints web server technology based on HTTP responses

[+] sfp_email_extractor: Found 6 email addresses
# Identifies email patterns from webpages or DNS metadata

```

```bash
[*] Scan completed in 45.2s
# Total runtime of the automated scan

[*] Total results: 47
# Raw data points SpiderFoot gathered across all modules

[*] Unique hosts: 23
# Distinct IPs or name records discovered

[*] Email addresses: 6
# Total harvested email addresses from spidering and pattern matching

[*] Subdomains: 35
# Combined result of all subdomain sources (DNS resolve + passive DNS + enumeration)
```

## 9. Findings, Risk and Mitigation table

| **Tool Used** | **Technique Type** | **Command(s) Executed** | **Key Output / Observed Result** | **Insight Gained** | **Risk Level** | **Lessons Learned** | **Recommended Action / Mitigation** | **Conclusion** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Recon-ng** | Workspace initialization & scoped recon | `recon-ng` → `workspaces add example_target` | Framework launched and workspace created successfully | OSINT engagement structured and isolated for target | **Low** | Proper workspace management is critical for organized recon | Maintain separate workspaces per target to avoid data leakage | Lab preparation and structured setup reduces errors and ensures data integrity |
| **Recon-ng** | Subdomain discovery module | `use recon/profiles-profiles/name_to_domain` → `set SOURCE example.com` → `run` | Identified **www**, **mail**, **api**, **admin**, **ftp** subdomains | Domain uses multiple exposed services; expands attack surface | **Medium** | Recon-ng modules simplify targeted recon | Remove unused subdomains, apply DNS monitoring and DNSSEC | Subdomain enumeration exposes potential attack vectors early in recon |
| **Recon-ng** | Host and geo‑mapping | `show hosts` | All discovered subdomains resolve to **one IP (93.184.216.34)** | Indicates shared hosting or shared web infrastructure | **Medium** | Geo-location info aids in footprint understanding | Segregate critical services; avoid hosting admin/FTP on shared IPs | Identifying IP overlaps helps assess service exposure |
| **SpiderFoot** | Automated WHOIS & registrar enumeration | `spiderfoot -s example.com` | WHOIS data publicly retrieved | Ownership and registrant details assist attackers in profiling | **Medium** | Public registrant info is often overlooked | Enable WHOIS privacy masking; avoid personal identifiers | Protecting domain registration info reduces social engineering risk |
| **SpiderFoot** | Active DNS resolution | Auto module: `sfp_dnsresolve` | Resolved **15 active subdomains** | Confirms live DNS records attackers may probe | **Medium** | DNS resolution quickly identifies active targets | Audit DNS regularly; remove unused or forgotten records | Continuous DNS monitoring is crucial for attack surface management |
| **SpiderFoot** | Passive DNS enumeration | Auto module: `sfp_dnsdb` | Returned **8 historic subdomains** | Legacy or abandoned systems remain publicly visible | **High** | Historical data can still expose old infrastructure | Decommission obsolete domains; monitor passive DNS databases | Regular cleanup and monitoring of passive DNS prevents forgotten exposure |
| **SpiderFoot** | Subdomain brute‑force enumeration | Auto module: `sfp_subdomain_enum` | **12 additional subdomains** identified | Hidden/obscure subdomains detectable through brute force | **High** | Attackers can find subdomains not visible through normal queries | Block wildcard enumeration; secure DNS; apply subdomain monitoring | Subdomain brute-force shows the need for subdomain hygiene and monitoring |
| **SpiderFoot** | SSL/TLS certificate scraping | Auto module: `sfp_ssl_certificate` | Extracted cert SANs, issuer and expiry | Attackers can infer infrastructure and expiry windows | **Medium** | Certificate info can reveal service structure | Rotate certificates, minimize SAN leakage, enforce TLS best practices | Proper SSL management reduces risk of reconnaissance exploitation |
| **SpiderFoot** | Technology fingerprinting | Auto module: `sfp_web_framework` | Identified **nginx web server** | Web stack fingerprint aids targeted exploit attempts | **Medium** | Server fingerprinting provides attack vectors | Obfuscate server banners; harden NGINX and keep patched | Obscuring stack information mitigates tailored attacks |
| **SpiderFoot** | Email harvesting | Auto module: `sfp_email_extractor` | Discovered **6 email addresses** | Enables phishing and social engineering attack chains | **High** | Email leakage is a common and underestimated risk | Implement email obfuscation, anti-scraping measures, staff awareness | Protecting publicly visible emails is essential for organizational security |
| **SpiderFoot** | Overall OSINT aggregation | Automated execution across all modules | **47 data points**, **23 unique hosts**, **35 combined subdomains** | Attack surface significantly broader than visible externally | **High** | Automation accelerates reconnaissance and highlights exposure | Perform continuous OSINT monitoring & external attack surface management | Combined use of automated and modular OSINT tools improves visibility and risk awareness |

## 10. Troubleshooting

When commands failed or services did not respond:

- The current shell was validated using `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.

## 11. Summary and Lessons Learned

- Recon-ng is excellent for **modular, controlled reconnaissance**, allowing step-by-step data gathering and workspace management.
- SpiderFoot provides **broad, automated scanning**, exposing additional subdomains, emails, SSL info, and web technologies.
- Together, they highlight **publicly exposed infrastructure, potential social engineering risks, and attack vectors**.
- Proper workspace isolation, monitoring, and mitigation reduce risk and improve OSINT effectiveness.

## 12. Conclusion

The lab demonstrated that combining **manual modular tools** (Recon-ng) and **automated scanners** (SpiderFoot) provides a **comprehensive understanding of a target’s public footprint**. Findings revealed multiple subdomains, shared hosting exposure, WHOIS/email leakage, and SSL/web server metadata.

Applying mitigation strategies, such as DNS monitoring, certificate management, and privacy protections, significantly reduces exposure and strengthens organizational security posture.