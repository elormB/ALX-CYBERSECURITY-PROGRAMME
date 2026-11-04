# ALX Cybersecurity Programme – Learning Portfolio

 _Author: Benny Elorm Mensah  
Date: 2026-05-30_

## 1. Overview

This repository documents my learning journey and practical experience throughout the **ALX Cybersecurity Programme**.

It serves as a technical portfolio showcasing the concepts, tools, techniques, practical exercises, and hands-on cybersecurity labs explored during the programme.

The documentation focuses on developing practical cybersecurity skills through structured exercises, security assessments, technical investigations, and lab-based challenges. Each document captures the objectives, methodology, tools used, observations, findings, lessons learned, and security recommendations from the exercises.

The primary goal of this repository is to demonstrate continuous learning, practical application of cybersecurity concepts, and the development of skills across offensive security, defensive security, and governance, risk, and compliance (GRC).

## 2. Learning Objectives

Throughout the programme, the following areas are explored:

- **Cybersecurity Fundamentals:** Understanding core security concepts, threats, vulnerabilities, and security principles.
- **Offensive Security:** Learning reconnaissance, information gathering, vulnerability assessment, and controlled security testing.
- **Defensive Security:** Understanding security monitoring, threat detection, log analysis, incident response, and defensive security controls.
- **Governance, Risk, and Compliance (GRC):** Exploring information security governance, risk management, security policies, compliance requirements, and cybersecurity frameworks.
- **Network Security:** Understanding network architecture, segmentation, scanning, wireless security, and network defense.
- **Web Application Security:** Exploring web application vulnerabilities, HTTP requests, client-side validation, and security testing techniques.
- **Security Tools and Frameworks:** Gaining practical experience with industry-relevant cybersecurity tools and frameworks.
- **Technical Documentation:** Developing the ability to document security assessments, interpret findings, and recommend appropriate mitigations.

## 3. Repository Structure

The repository is organized by security domain, hub, and practical lab to make the documentation easy to navigate.

```text
ALX-CYBERSECURITY-PROGRAMME/
│
├── OFFENSIVE-SECURITY/
│   │
│   ├── [HUB-1](./OFFENSIVE-SECURITY/HUB-1/)/
│   │   ├── [1-Asset-Discovery.md](./OFFENSIVE-SECURITY/HUB-1/1-Asset-Discovery.md)
│   │   ├── 2-Organization-Information-Harvesting.md
│   │   ├── 3-Service-Interaction.md
│   │   ├── 4-OSINT-Framework.md
│   │   ├── 5-Target-Validation.md
│   │   ├── 6-Shells-and-Tunneling.md
│   │   └── 7-Metasploit-Framework.md
│   │
│   ├── HUB-2/
│   │   └── 1-Burp-Suite-Client-side-Bypass.md
│   │
│   └── HUB-3/
│       ├── 1-Network-Segmentation-Assessment.md
│       ├── 2-Advanced-Network-Scanning-Lab.md
│       └── 3-Wireless-Networks-Lab/
│
├── DEFENSIVE-SECURITY/
│   └── (Practical exercises and documentation)
│
├── GOVERNANCE-RISK-AND-COMPLIANCE/
│   └── (Practical exercises and documentation)
│
├── LICENSE
└── README.md
```

*The Defensive Security and GRC directories will be expanded as additional exercises and documentation are completed.*

## 4. Practical Learning Areas

### 1. Offensive Security

This section documents practical exercises involving reconnaissance, information gathering, vulnerability assessment, and controlled security testing within authorized lab environments.

| Lab | Learning Focus |
|---|---|
| Asset Discovery | Identifying and enumerating assets within a target environment |
| Organization Information Harvesting | Gathering publicly available organizational information |
| Service Interaction | Investigating and interacting with network services |
| OSINT Framework | Applying Open-Source Intelligence techniques |
| Target Validation | Validating discovered targets and collected information |
| Shells and Tunneling | Understanding remote shells, communication channels, and tunneling |
| Metasploit Framework | Exploring exploitation workflows and framework capabilities |
| Burp Suite and Client-side Bypass | Examining client-side validation and HTTP request manipulation |
| Network Segmentation Assessment | Evaluating network accessibility and segmentation controls |
| Advanced Network Scanning | Discovering hosts, identifying services, and examining potential vulnerabilities |
| Wireless Networks Lab | Exploring wireless discovery, monitoring, and security assessment |

### 2. Defensive Security

This section documents practical exercises and learning activities focused on identifying, monitoring, detecting, analyzing, and responding to cybersecurity threats.

Key learning areas include:

- **Security Monitoring:** Understanding how systems, networks, and applications are monitored for suspicious activities.
- **Threat Detection and Analysis:** Identifying indicators of compromise, suspicious behaviour, and potential security incidents.
- **Log Analysis:** Examining system, application, and network logs to investigate security events.
- **Incident Response:** Understanding the processes involved in identifying, containing, investigating, and recovering from security incidents.
- **Network Defense:** Exploring security controls used to protect network infrastructure and prevent unauthorized access.
- **Endpoint Security:** Understanding endpoint protection, system hardening, and host-based security controls.
- **Security Operations:** Exploring Security Operations Centre (SOC) processes, workflows, and tools.
- **Vulnerability Management:** Understanding how vulnerabilities are identified, prioritized, tracked, and remediated.

### 3. Governance, Risk, and Compliance (GRC)

This section documents learning activities related to cybersecurity governance, organizational risk management, security policies, regulatory requirements, and compliance frameworks.

Key learning areas include:

- **Information Security Governance:** Understanding how organizations establish and manage information security responsibilities, policies, and controls.
- **Risk Management:** Identifying, assessing, evaluating, and treating information security risks.
- **Security Policies and Procedures:** Understanding the development and implementation of organizational security policies and standards.
- **Security Frameworks:** Exploring cybersecurity frameworks and standards such as the NIST Cybersecurity Framework and ISO/IEC 27001.
- **Compliance Management:** Understanding how organizations meet applicable regulatory, legal, and contractual security requirements.
- **Security Auditing:** Examining how security controls are assessed to determine their effectiveness and compliance.
- **Business Continuity and Disaster Recovery:** Understanding how organizations prepare for disruptions and maintain critical operations.
- **Third-Party Risk Management:** Assessing security risks associated with vendors, suppliers, and external service providers.

## 5. Tools and Technologies

The following tools and technologies are explored through practical exercises and technical investigations.

| Category | Tools |
|---|---|
| Operating Systems | Kali Linux |
| Network Scanning | Nmap |
| Web Application Security | Burp Suite, Browser Developer Tools |
| Exploitation Frameworks | Metasploit Framework |
| OSINT and Reconnaissance | OSINT Framework, Reconnaissance Tools |
| Wireless Security | Aircrack-ng Suite, Wash, Reaver |
| Network Analysis | Wireshark, tcpdump |
| Scripting and Automation | Python, Bash, JavaScript |

*The tools listed reflect technologies explored or intended for use across the documented learning activities. Not every tool was used in every lab. Additional tools will be included as new exercises are completed.*

## 6. Lab Setup and Deployment

This repository includes a lab deployment script that can be used to initialize the cybersecurity training environment and practise selected exercises documented in this portfolio.

Visitors can download the deployment script, configure the required environment, and launch the lab to follow along with the practical exercises.

### Getting Started

**1. Clone the repository**

```bash
git clone https://github.com/elormB/ALX-CYBERSECURITY-PROGRAMME.git
```

**2. Navigate to the lab directory**

```bash
cd ALX-CYBERSECURITY-PROGRAMME
```

**3. Download the deployment script**

The lab deployment script can be downloaded from the repository.

[Download Lab Deployment Script](INSERT-DIRECT-SCRIPT-LINK-HERE)

**4. Make the script executable**

```bash
chmod +x start_script.sh
```

**5. Start the lab environment**

```bash
./start_script.sh
```

**6. Verify the lab status**

Follow the instructions provided by the script to confirm that the environment has started successfully.

### Requirements

Before deploying the lab, ensure that the following are available:

- Kali Linux (preferred) or another supported Linux environment.
- Bash shell.
- Required dependencies and tools specified by the deployment script.
- A stable local environment for running the training exercises.

*Additional requirements may apply depending on the specific lab.*

### Important Notes

- Review the deployment script before executing it.
- Follow the setup instructions provided in the relevant lab documentation.
- Use the environment strictly for educational purposes and authorized cybersecurity practice.
- Some exercises may require additional tools, configurations, or dependencies.
- The deployment script and associated labs are provided for learning and experimentation. Use them in an isolated, controlled environment.

## 7. Documentation Approach

Each practical exercise is documented using a structured technical reporting approach. Depending on the nature of the activity, documentation may include:

- **Background:** Introduction to the security concept or technology being investigated.
- **Objectives:** The learning goals and expected outcomes.
- **Scope and Limitations:** The boundaries of the assessment and any constraints encountered.
- **Legal and Ethical Considerations:** The authorized testing context and responsible use of security techniques.
- **Tools and Methodology:** Tools, commands, and procedures used during the exercise.
- **Practical Implementation:** Step-by-step explanation of the activities performed.
- **Observations and Findings:** Relevant outputs, results, and security implications.
- **Lessons Learned:** Technical knowledge and practical skills gained.
- **Recommendations:** Appropriate security controls and mitigation strategies.
- **Conclusion:** Summary of the exercise and its outcomes.

## 8. Skills Developed

Through the programme and associated practical exercises, the following skills are being developed:

- Network reconnaissance and host discovery.
- Information gathering and OSINT.
- Network scanning and service enumeration.
- Basic vulnerability assessment.
- Web application security testing.
- HTTP request inspection and manipulation.
- Understanding of client-side and server-side security controls.
- Wireless network security assessment.
- Security monitoring and threat analysis.
- Understanding of incident response and security operations.
- Information security risk assessment and management.
- Familiarity with cybersecurity governance and compliance frameworks.
- Linux command-line operations.
- Technical analysis and security documentation.
- Identification of security weaknesses and mitigation strategies.

## 9. Learning Philosophy

This repository represents learning as a continuous process. The emphasis is not only on completing cybersecurity challenges but also on understanding the underlying security concepts, interpreting technical results, identifying limitations, and documenting lessons from each exercise.

Where challenges, tools, or configurations present difficulties, alternative approaches and troubleshooting steps are documented to reflect the actual learning experience.

The repository will continue to evolve as additional practical exercises are completed and new cybersecurity skills are developed across offensive security, defensive security, and GRC.

## 10. Legal and Ethical Disclaimer

All practical exercises documented in this repository were conducted within authorized training environments, simulated labs, or systems for which permission was granted.

The tools, commands, and techniques presented are intended strictly for educational purposes, ethical security research, and authorized security assessments.

Unauthorized access, scanning, exploitation, interception, or testing of systems without appropriate permission is prohibited. The author does not endorse or encourage the misuse of the information contained in this repository.

## 11. Author

**Aspiring Cybersecurity Professional |AWS Certified Solutions Archtect | Cloud Security**

This portfolio reflects my commitment to continuous learning, practical skill development, and building a strong foundation in cybersecurity.

- GitHub: [@elormB](https://github.com/elormB)
- Portfolio Repository: [ALX Cybersecurity Programme](https://github.com/elormB/ALX-CYBERSECURITY-PROGRAMME)

---

*Learning by doing. Documenting the process. Building practical cybersecurity skills.*