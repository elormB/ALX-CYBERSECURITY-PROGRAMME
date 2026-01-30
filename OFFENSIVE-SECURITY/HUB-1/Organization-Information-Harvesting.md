# Hub 1 Challenge – Organization Information Harvesting

## 1. Background

Organizational information harvesting is the process of systematically collecting, analyzing, and evaluating information about an organization’s assets, systems, operations, and external environment to understand potential security risks, support decision-making, and inform the design of cybersecurity strategies. It is a critical part of cybersecurity risk management, enabling organizations to identify vulnerabilities, assess threats, and implement effective controls.

The Organization Information harvesting challenge formed part of the reconnaissance phase of hub 1 practical of the ALX Cybersecurity Programme. The purpose of this hub was to collect publicly available information about a target organization using open-source intelligence (OSINT) tools and methodologies. This module was completed exclusively within **Hub-1 – Organization Information**.

## 2. Objectives

The objective of this exercise was to gather and analyze publicly accessible organizational intelligence through OSINT-based reconnaissance. Specifically, the tasks focused on using:
- theHarvester to gather employee details, corporate email addresses, public contact points, organizational-related metadata and subdomains for example.com
- h8mail to discover email addresses and check for password breaches for example.com.

## 3. Scope & Limitations

- All tasks were restricted to Hub 1 – Organization Information.
- No enumeration or tool activity was performed outside the approved scope.
- All information collected originated from publicly available or lab-simulated data sources.
- Tools were not used against real organizations without authorization.

## 4. Legal & Ethical Disclaimer

- All data collection activities were performed solely inside the ALX lab environment.
- No unauthorized probing, scraping, or scanning of real production systems occurred.
- The tools and techniques were used strictly for training and ethical development purposes.

## 5. Tools Used

| Category | Tools |
| --- | --- |
| Operating System | Kali linux |
| OSINT / Reconnaissance | theHarvester |
| Breach / Credential Analysis | h8mail |
| Domain | example.com |

## 6. Lab Initialization & Access

To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **Hub 1** was started.
2. The `running` command was executed to validate that the environment was live:
    
    ```bash
    running
    # Expected output: 
    hub-1 running
    ```
    
3. The browser was opened and the interface was accessed via `http://localhost`.
4. The Organization Information module was selected and the first task was launched.

## 7. Tasks Completed
The Organization Information section consisted of two practical tools:

### 7.1 theHarvestor

**Purpose:**  To collect emails, hostnames, subdomains, and employee names from public sources (search engines, social media, and domain records)

**Outcome:** Provided an expanded view of the organization’s external footprint, enabling identification of potential attack vectors.

```bash
pentester@kali-linux:~$ theHarvester -d example.com -b all
[*] Target: example.com                      # Domain being scanned
[*] Searching Bing.                          # Source queried
[*] Searching BingAPI.
[*] Searching Bufferoverun.
[*] Searching Censys[*] Searching Certspotter.
[*] Searching Crtsh.
[*] Searching Dnsdumpster.
[*] Searching Duckduckgo.
[*] Searching Fullhunt.
[*] Searching Github.
[*] Searching Google.
[*] Searching Hackertarget.
[*] Searching Hunter.
[*] Searching Intelx.
[*] Searching Linkedin.
[*] Searching Linkedin2.
[*] Searching Netcraft.
[*] Searching Otx.
[*] Searching PTRarchive.
[*] Searching Securitytrails.
[*] Searching Shodan.
[*] Searching Threatcrowd.
[*] Searching Threatminer.
[*] Searching Trello.
[*] Searching Twitter.
[*] Searching Urlscan.
[*] Searching Virustotal.
[*] Searching Yahoo.                         # Shows full breadth of OSINT sources

[*] No IPs found.                            # No server IPs discovered in public records

[*] Emails found:
admin@example.com                            # Likely admin/support contacts
contact@example.com
info@example.com
support@example.com
webmaster@example.com

[*] Hosts found:                             # Discovered subdomains (attack surface)
www.example.com
mail.example.com                             # Possible mail server
ftp.example.com                              # Legacy systems often vulnerable
blog.example.com
api.example.com                               # API endpoints good to test
admin.example.com                             # Could expose dashboards
dev.example.com                               # Internal/test systems often weak
staging.example.com                           # Pre-production — often insecure
```

```python
pentester@kali-linux:~$ theHarvester -d example.com -b google
[*] Target: example.com                        # Same domain
[*] Searching Google.                          # Only one search engine used

[*] No IPs found.                              # No exposed IPs indexed by Google

[*] Emails found:                              # Fewer due to reduced data sources
admin@example.com
contact@example.com
info@example.com

[*] Hosts found:                               # Limited visibility = fewer subdomains
www.example.com
mail.example.com
blog.example.com
api.example.com
admin.example.com
```

### 7.2 h8mail

**Purpose:** To query public breach databases and email exposure repositories to identify whether harvested emails have appeared in known data leaks

**Outcome:** Determined whether harvested emails have appeared in known data breaches, highlighting compromised credentials and potential security risks.

```bash
pentester@kali-linux:~$ h8mail -t example.com
[+] h8mail v2.5.6 - Email OSINT & Password breach hunting
[+] Target: example.com                         # Domain being investigated
[+] Starting email enumeration...               # h8mail tries to find emails anywhere online

[+] Found emails:                               # h8mail found more emails than theHarvester
admin@example.com                               # Likely public contact emails
contact@example.com
info@example.com
support@example.com                              # Support address
webmaster@example.com
john.doe@example.com                             # Possible real employee
jane.smith@example.com
marketing@example.com
sales@example.com

[+] Checking for password breaches...            # Now checking breach databases
[+] admin@example.com: Found in 2 breaches       # Credentials leaked twice
[+] contact@example.com: Found in 1 breach
[+] info@example.com: Found in 3 breaches        # High-risk email
[+] support@example.com: Clean                   # No known leaks
[+] webmaster@example.com: Found in 1 breach
[+] john.doe@example.com: Found in 4 breaches    # Possibly weak password reuse
[+] jane.smith@example.com: Found in 2 breaches
[+] marketing@example.com: Clean
[+] sales@example.com: Found in 1 breach

[+] Email enumeration completed
[+] Total emails found: 9                       # Total discovered
[+] Breached emails: 7                          # High exposure → social engineering risk
```

```bash
pentester@kali-linux:~$ h8mail -t admin@example.com
[+] h8mail v2.5.6 - Email OSINT & Password breach hunting
[+] Target: admin@example.com                    # Single target email
[+] Starting breach analysis...

[+] Email: admin@example.com
[+] Checking password breaches...
[+] Found in 3 data breaches:                    # Specific leak history
                                                # Real breach examples below
- LinkedIn (2012)
- Adobe (2013)
- Collection #1 (2019)

[+] Associated passwords:
- password123                                    # Weak password shows bad hygiene
- admin2021
- welcome123                                     # Password reuse implies easy compromise

[+] Analysis completed
[+] Total breaches: 3
[+] Passwords found: 3                           # Cracked plaintext passwords available
```
These tools allowed enumeration of personnel-related intelligence and validated whether organizational details appeared in third-party datasets.
## 8. Findings and Risk Table

| **Tools Used** | **Technique Type** | **Command(s) Executed** | **Key Output / Observed Result** | **Insight Gained** | **Risk Level** | **Recommended Action / Mitigation** |
| --- | --- | --- | --- | --- | --- | --- |
| `theHarvester` | Email & Company OSINT Enumeration | `theHarvester -d example.com -b all` | Found **5 public emails** and **8 subdomains** including sensitive hosts (`dev`, `staging`, `admin`, `ftp`) | Shows publicly discoverable employees and systems visible to attackers | **Medium** | Conduct OSINT cleanup: remove unused emails from internet-facing pages; restrict exposure of testing/development environments |
| `theHarvester` | Reduced‑scope OSINT Enumeration | `theHarvester -d example.com -b google` | Smaller output: **3 emails**, **5 subdomains** | Verifies that more OSINT sources → more data leakage; Google alone missed assets | **Low–Medium** | Continue to monitor multiple OSINT sources; ensure DNS changes propagate intentionally |
| `h8mail` | Bulk Corporate Email Enumeration & Breach Check | `h8mail -t example.com` | Identified **9 emails**, **7 were in breaches**. Discovered real employee names | Confirms staff identities and usernames are public + breached data usable for phishing | **High** | Implement password resets + MFA for all employees, security awareness training, remove personal emails from public space |
| `h8mail` | Targeted Email Breach Validation | `h8mail -t admin@example.com` | Confirmed **3 historic breaches**, recovered weak reused passwords (`password123`, `welcome123`) | Demonstrates poor password hygiene and real compromise probability | **Critical** | Force immediate password rotation, enforce password policies (length + complexity), require MFA |

## 9. Troubleshooting Actions Taken

Where necessary during execution:

- The Kali environment was verified
- The active shell was confirmed using `echo $SHELL`
- If not in `training-shell`, the setup script was relaunched
- Hub status was re-checked using `running`

---

## 10. Conclusion

This module provided practical experience in gathering organization-level intelligence using OSINT methodologies. Through the use of theHarvester and h8mail, the lab demonstrated how to identify:

- Publicly listed organization emails
- Employee names and roles
- Potential data leakage through breach exposure
- Metadata and external digital presence

The skills acquired supported the broader reconnaissance methodology and formed a critical precursor to later phases such as target validation, vulnerability identification, social engineering, and exploitation.