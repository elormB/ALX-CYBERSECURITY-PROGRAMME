# Hub 1 Challenge – Target Validation

## 1. Background

**Target Validation** is the process of actively verifying discovered assets to confirm their accessibility, identify their current state, and prioritize them for detailed security assessment. This phase bridges the gap between reconnaissance and vulnerability assessment by providing real-time validation of discovered targets. The strategic importance of target validation lies in its ability to filter discovered assets into actionable targets, ensuring that subsequent testing efforts focus on accessible, relevant systems while identifying immediate security concerns that require attention.

The Target Validation hub introduced automated methods for verifying whether previously discovered hosts and domains are accessible, responsive, and potentially exploitable. This stage formed the bridge between passive reconnaissance and active probing by confirming which targets warrant deeper analysis.

## 2. Objectives

The objective of this lab was to:

- Confirm availability of discovered assets using automated utilities
- Validate live targets across multiple protocols
- Enumerate response codes, redirects, and service behaviors
- Detect misconfigurations, expired certificates, and insecure services
- Identify potential vulnerabilities using rule-based scanning
- Prioritize targets for further exploitation and enumeration

These competencies formed the validation foundation required prior to running deeper scanning or exploitation tools.

## 3. Scope and Limitations

- The scope of this practical covered only challenges under the Target Validation hub, which included httpx, nabuu, and nuclei.
- Tools outside these exercises—such as vulnerability scanners, exploitation frameworks, or brute-force tools—remained out of scope.
- Live exploitation, active intrusion attempts, credential testing, or modification of target systems was not permitted in this controlled lab environment.

## 4. Legal and Ethical Disclaimer

- All actions took place within a controlled training environment configured specifically for ethical cyber security training.
- No scans or probes were executed on public systems or unauthorized infrastructure.
- The activity aligned with safe learning principles and respected responsible hacking guidelines.

## 5. Tools Used

- **httpx –** A Live Host Validator that provides fast, reliable HTTP service validation capabilities that enable efficient verification of web services across large numbers of discovered targets. Its efficient design and comprehensive output make it ideal for the validation phase of reconnaissance, where speed and accuracy are paramount. It was used in tis lab to probe domains/IP lists to confirm whether they are reachable and what services they expose
- **Nabuu –** A Recon Validation Utility that provides fast, reliable port scanning capabilities designed for modern network environments and large-scale target validation. It was used in this lab to cross-check live hosts through concurrent probing and metadata extraction.
- **Nuclei –** A Template-Based Vulnerability Scanner used to detect common security issues such as misconfigurations, weak headers, insecure technologies, and exposed services.

## 6. Challenge Overview

This hub consisted of three validation workflow stages:

- **httpx -** Validate target availability, response codes, and protocols.
- **Nabuu -** Confirm active hosts, extract metadata and behaviors.
- **Nuclei -**  Perform signature-based vulnerability checks.
- The order of workflow was important because running these tools in this order ensured that:
    1. Live hosts were filtered first using httpx.
    2. Then metadata was extracted using nabuu.
    3. Finally only valid and responsive hosts were tested for weaknesses using nuclei.

## 7. Lab Initialization and Access

To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and **`Hub 1`** was started.
2. The `running` command was executed to validate that the environment was live: `running # expected output: hub-1 running`
3. The browser was opened and the interface was accessed via `http://localhost.`
4. Navigation proceeded to the Target Validation section.
5. httpx, nuclei and nabuu tasks were initiated and completed.

## 8. Tasks Completed

### 8.1 httpx

**Purpose:** httpx validated whether discovered domains and subdomains were reachable over the internet. The tool actively probed HTTP and HTTPS endpoints, confirmed service availability, collected response codes, and captured basic fingerprints such as headers and web technologies. This streamlined attack surface mapping by distinguishing live, publicly exposed systems from inactive or non-responsive hosts.

**Outcome:** The httpx scan verified that 10 of 15 subdomains were publicly accessible and returned HTTP 200 responses. The scan also revealed that all reachable endpoints consistently ran on NGINX, and it identified publicly exposed development, staging, and testing environments—systems that typically should not be internet-facing. The process successfully eliminated inactive hosts, refined the target set, and produced a concise list of web-active systems suitable for further assessment.

```bash
pentester@kali-linux:~$ httpx -l subdomains.txt     # Run httpx and load targets from file
[INF] Loading targets from subdomains.txt            # Reading list of subdomains
[INF] Loaded 15 targets from subdomains.txt          # Successfully loaded 15 hostnames

[INF] Starting HTTP probe                            # Begin checking HTTP/HTTPS service availability

[INF] [200] https://www.example.com [nginx]          # Status 200 = site reachable and OK, running nginx
[INF] [200] https://mail.example.com [nginx]         # Mail host responds over HTTPS, nginx detected
[INF] [200] https://blog.example.com [nginx]         # Blog subdomain returns valid page
[INF] [200] https://api.example.com [nginx]          # API endpoint active and online
[INF] [200] https://admin.example.com [nginx]        # Admin interface reachable
[INF] [200] https://dev.example.com [nginx]          # Dev/testing environment live
[INF] [200] https://staging.example.com [nginx]      # Staging site exposed and responding
[INF] [200] https://test.example.com [nginx]         # Test environment returns webpage
[INF] [200] https://support.example.com [nginx]      # Support/helpdesk endpoint active
[INF] [200] https://webmail.example.com [nginx]      # Webmail service accessible via browser

[INF] [404] https://ftp.example.com                  # Host reachable, but no webpage (likely FTP only)
[INF] [404] https://ns1.example.com                  # DNS server responds, but no HTTP content
[INF] [404] https://ns2.example.com                  # Backup DNS hostname active, no web service
[INF] [404] https://mx1.example.com                  # Mail exchanger live but lacks a web page
[INF] [404] https://mx2.example.com                  # Secondary MX host reachable, no web interface

[INF] HTTP probe completed in 12.3s                  # Scan finished for all loaded domains
[INF] Found 10 live hosts                            # Only hosts with HTTP 200 counted as web‑active

```

### 8.2 Nabuu

**Purpose:** Naabu served as a fast TCP port scanner that identified which services were publicly exposed on the target system. It scanned key and configurable port ranges to determine running services, enabling perimeter mapping and exposing potential attack vectors, weak configurations, or unnecessary services reachable from the internet.

**Outcome:** Naabu detected **five open ports** on the target, confirming externally accessible **HTTP/HTTPS services** and an active **email infrastructure** through SMTP, IMAPS, and POP3S. These findings showed that the organization maintained both web and mail services on its public-facing environment, noticeably expanding its exposure footprint. The results emphasized the importance of securing mail protocols and highlighted the need for service minimization or hardening as part of risk reduction measures.

```python
pentester@kali-linux:~$ naabu -host example.com     # Running naabu to scan open ports on example.com
[INF] Starting port scan for example.com            # Scan initialization
[INF] Found 5 open ports                            # Naabu detected 5 reachable services

93.184.216.34:80   # HTTP - standard web traffic
93.184.216.34:443  # HTTPS - secure web traffic
93.184.216.34:25   # SMTP - email sending server
93.184.216.34:993  # IMAPS - secure incoming mail (IMAP over TLS)
93.184.216.34:995  # POP3S - secure incoming mail (POP3 over TLS)

[INF] Port scan completed in 8.2s                   # Total scan time and completion status
```

### 8.3 Nuclei

**Purpose:** Nuclei operated as a vulnerability scanning framework that used signature-based templates to detect misconfigurations, exposed services, technology leaks, and known weaknesses across accessible web assets. Rather than simply confirming availability, the tool actively evaluated hosts against thousands of predefined checks, providing visibility into issues that real attackers could exploit.

**Outcome:** Nuclei scanning identified nine actionable findings across the ten live hosts. These included low-severity server version disclosures and medium-severity exposed development and staging panels that were publicly reachable. Additional informational detections confirmed the web technology stack in use. The results demonstrated that misconfigurations and unnecessary exposure existed within the environment, highlighting areas requiring immediate hardening to reduce exploitability and prevent unauthorized access.

```python
pentester@kali-linux:~$ nuclei -l live_hosts.txt           # Runs nuclei scan against all live hosts listed
[INF] Loading templates from /root/nuclei-templates        # Nuclei loads detection templates
[INF] Loaded 1000+ templates                               # Confirms template pack size
[INF] Starting scan with 10 hosts                          # Total targets being scanned

[LOW] [http] https://www.example.com/ [nginx-version]       # Low risk: Server version disclosed
[LOW] [http] https://mail.example.com/ [nginx-version]      # Low risk: Webmail reveals Nginx version
[LOW] [http] https://blog.example.com/ [nginx-version]      # Low risk: Blog leaking server info
[LOW] [http] https://api.example.com/ [nginx-version]       # Low risk: API leaks server version
[LOW] [http] https://admin.example.com/ [nginx-version]     # Low risk: Admin server version disclosure
[MEDIUM] [http] https://dev.example.com/ [exposed-panel]    # Medium: Likely accessible internal/management panel
[MEDIUM] [http] https://staging.example.com/ [exposed-panel]# Medium: Staging panel possibly exposed publicly
[INFO] [http] https://test.example.com/ [tech-detect:nginx] # Informational: Detected Nginx stack
[INFO] [http] https://support.example.com/ [tech-detect:nginx] # Informational: No vulnerability, just tech fingerprint
[INFO] [http] https://webmail.example.com/ [tech-detect:nginx]  # Informational: Identifies server technology

[INF] Scan completed in 45.2s                               # Nuclei finished processing all hosts
[INF] Found 9 vulnerabilities                                # Total count of exploitable or reportable findings

```

## 9. Findings, Risk and Mitigation table

| **Tool Used** | **Technique Type** | **Command(s) Executed** | **Key Output / Observed Result** | **Insight Gained** | **Risk Level** | **Lessons Learned** | **Recommended Action / Mitigation** | **Conclusion** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **httpx** | Web availability validation | `httpx -l subdomains.txt` | 15 subdomains tested; **10 returned HTTP 200**; **5 returned 404** | Organization runs many active public-facing services | **Medium** | Broad attack surface increases exposure opportunities | Reduce unused public endpoints; restrict staging/dev environments; enforce access controls | Web discovery confirms multiple live hosts accessible to attackers |
| **httpx** | Service & technology fingerprinting | `httpx -l subdomains.txt` | Identified **nginx** running on all responding domains | Consistent and uniform tech stack externally visible | **Low** | Banner exposure reveals backend server type | Obfuscate server banners; disable version disclosure; deploy WAF | Uniform tech stack simplifies patching but aids attackers |
| **httpx** | Test environment discovery | `httpx -l subdomains.txt` | **dev**, **staging**, **test** hosts publicly reachable | Internal/non-production systems are exposed | **High** | Non-production systems often weaker and poorly guarded | Restrict access via VPN/IP allowlisting; remove public exposure | Discovery of staging/dev/test highlights potential weak entry points |
| **Naabu** | TCP port enumeration | `naabu -host example.com` | Found **80, 443, 25, 993, 995** open | Organization hosts both web and mail services | **Medium** | Multi-service exposure multiplies attack paths | Apply firewall rules; minimize exposed ports; enforce TLS everywhere | Port scan confirms external services that must be hardened |
| **Naabu** | Mail security surface analysis | `naabu -host example.com` | SMTP + IMAPS + POP3S open | Public mail servers introduce phishing and relay risks | **High** | Mail infrastructure is a common attacker focus | Enforce SPF/DKIM/DMARC; prevent open-relay; use TLS-only authentication | Exposure of email protocols significantly widens threat vectors |
| **Nuclei** | Vulnerability scanning using templates | `nuclei -l live_hosts.txt` | **9 findings total**: 5 low (server version), 2 medium (exposed panels), 3 info | Proof that exposed hosts have detectable weaknesses | **Medium** | Even low findings contribute to intel that enables attack chaining | Patch systems; remove exposed admin/staging panels; enable MFA | Automated vulns show immediate areas requiring security response |
| **Nuclei** | Server version disclosure | `nuclei -l live_hosts.txt` | Nginx version exposed across **www**, **mail**, **api**, etc. | Software version leakage helps attackers match exploits | **Low** | Banner leakage is easy to fix but highly valuable to attackers | Enable server tokens off; use reverse proxy to sanitize headers | Version disclosure avoidance removes intelligence advantage |
| **Nuclei** | Exposed admin and staging panels | `nuclei -l live_hosts.txt` | **dev.example.com** + **staging.example.com** flagged as exposed-access | High-risk endpoints potentially accessible without authentication | **High** | Misconfigured services are often weakest link | Remove public access, enforce authentication, disable if unnecessary | Vulnerability scan highlights critical systems requiring urgent containment |
| **Nuclei** | Technology fingerprint–not vulnerabilities | `nuclei -l live_hosts.txt` | **tech-detect** findings for support/test/webmail | Confirms technology stack consistency | **Low** | Fingerprinting information still guides attack planning | Apply WAF + header obfuscation policy | Awareness of tech exposure aids tuning of security controls |

## 10. Troubleshooting

When commands failed or services did not respond:

- The current shell was validated using `echo $SHELL`
- If incorrect, the **start_script.sh** was re-run
- The lab state was verified again using `running`

This ensured tool execution remained within the designated training environment.

## 11. Summary, and Lessons Learned

**Summary:** The Target Validation lab successfully verified the accessibility and exposure of previously discovered hosts. **httpx** confirmed that 10 of 15 subdomains were active and reachable over HTTP/HTTPS, revealing a consistent **NGINX web stack** and publicly exposed environments such as **dev**, **staging**, and **test**. **Naabu** identified five open ports, including web services (HTTP/HTTPS) and secure mail protocols (SMTP, IMAPS, POP3S), highlighting an active email infrastructure accessible from the internet. **Nuclei** scanned the live hosts for vulnerabilities and detected nine findings, including low-severity server version disclosures and medium-severity exposed administrative, development, and staging panels. Combined, the tools provided a clear picture of the attack surface, identified misconfigurations, and validated which systems were externally reachable.

**Lessons Learned:**

- Publicly reachable subdomains and services must be regularly validated to prevent unintended exposure.
- Development, staging, and test environments should not remain accessible on the public internet, as they represent significant attack vectors.
- Open ports and exposed protocols, particularly mail services, expand the attack surface and require proper hardening.
- Signature-based vulnerability scanning (Nuclei) effectively identifies configuration leaks and potential weak points that attackers could exploit.
- Continuous monitoring and proactive validation of live assets are crucial to maintain secure operations and reduce the risk of exploitation.

## 12. Conclusion

The lab demonstrated the importance of **target validation** in the reconnaissance and security assessment process. Using httpx, Naabu, and Nuclei provided actionable intelligence, revealed misconfigurations, and exposed high-risk systems that required remediation. The process highlighted that publicly accessible services, exposed panels, and outdated server information increase organizational risk. Effective validation and regular scanning are essential to minimize exposure, enforce security best practices, and ensure the organization maintains a hardened external footprint.