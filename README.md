# SOC Threat Intelligence, Phishing Investigation and Wazuh Threat Detection Capstone Project

This project demonstrates practical Security Operations Centre (SOC) skills through the investigation of phishing emails, identification of Indicators of Compromise (IOCs), malware detection and security incident response. Using Wazuh SIEM, VirusTotal, Kali Linux and Ubuntu, the project applies the SOC incident management lifecycle: **Detect, Investigate, Enrich, Respond and Verify**.

## Table of Contents

- Project Overview
- Network Topology
- Tools and Technologies
- Configuration Steps
- Results and Findings
- Author

## Project Overview

This capstone project focuses on developing practical SOC analyst skills through two major security investigation exercises.

**Part A: Phishing Investigation and Threat Intelligence**

Four suspicious emails were analysed to identify phishing techniques, investigate sender infrastructure, extract IOCs and assess potential threats.

The investigation covered:

- Microsoft 365 brand impersonation and credential harvesting.
- Business Email Compromise (BEC) involving fraudulent vendor invoice and banking details.
- DHL delivery impersonation involving payment-card and personal information harvesting.
- Bank of America impersonation involving fraudulent password-reset requests.

Email headers, sender domains, IP addresses, URLs and authentication results were examined. Threat intelligence enrichment was used to assess suspicious infrastructure and identify malicious indicators.

**Part B: Wazuh Detection Exercises**

Three security detection exercises were conducted using Wazuh:

- **YARA Malware Detection:** Investigated suspicious file modifications in a malware test directory.
- **Suspicious Binary Detection:** Used Wazuh Rootcheck to identify a suspected trojaned Linux system binary.
- **Shellshock Detection:** Simulated a Shellshock attack pattern from Kali Linux against an Ubuntu web server and investigated the resulting Wazuh alert.

The overall objective was to demonstrate the ability to detect suspicious activity, investigate security events, enrich threat intelligence, recommend containment measures and identify the evidence required to verify remediation.

## Network Topology

The practical laboratory environment consisted of a Kali Linux test machine, an Ubuntu endpoint and a Wazuh SIEM monitoring platform.

### Network Architecture

The environment included the following components:

- **Kali Linux:** Used as the testing machine to generate controlled security test traffic.
- **Ubuntu 2:** Target endpoint monitored by Wazuh.
- **Wazuh SIEM:** Centralised security monitoring, alert generation, file integrity monitoring and Rootcheck detection.
- **Apache Web Server:** Generated HTTP access logs used during the Shellshock detection exercise.
- **VirusTotal:** Provided external threat intelligence and URL/file reputation analysis.
- **YARA:** Used as part of the malware detection exercise.

### Network IP Address Configuration

| Device | IP Address | Role |
|---|---|---|
| Kali Linux | `192.168.1.53` | Security testing machine |
| Ubuntu Web Server | `192.168.1.52` | Shellshock test target |
| Ubuntu 2 | `192.168.1.55` | Wazuh-monitored endpoint |
| Wazuh Agent | `001` | Ubuntu2 monitoring agent |

**Operating Systems:**

- Kali GNU/Linux Rolling.
- Ubuntu 24.04 LTS.

**Network Communication:**

During the Shellshock exercise, the Kali machine (`192.168.1.53`) sent an HTTP GET request to the Ubuntu web server (`192.168.1.52`). Wazuh analysed the Apache access log and generated a high-severity Shellshock detection alert.

### Security Monitoring Architecture

The monitoring environment supported:

- Endpoint security event collection.
- File Integrity Monitoring (FIM).
- Rootcheck system binary detection.
- YARA-related malware monitoring.
- Apache web access-log monitoring.
- Security alert investigation through the Wazuh dashboard.

**Note:** The document does not provide a complete network diagram or the Wazuh manager's IP address.

## Tools and Technologies

The following tools, platforms and technologies were used throughout the project:

- **Wazuh SIEM:** Centralised security monitoring, threat detection, alert management and endpoint investigation.
- **VirusTotal:** Threat intelligence enrichment and malicious URL/file analysis.
- **AbuseIPDB:** IP address reputation analysis and identification of reported suspicious infrastructure.
- **Kali Linux:** Security testing and controlled attack simulation.
- **Ubuntu 24.04 LTS:** Linux endpoint used for security monitoring and investigation.
- **Oracle VirtualBox:** Virtualisation environment for the laboratory machines.
- **YARA:** Malware identification and file scanning using detection rules.
- **Wazuh Rootcheck:** Detection of suspicious or trojaned system binaries.
- **File Integrity Monitoring (FIM):** Detection of file creation, modification, deletion and checksum changes.
- **Apache HTTP Server:** Web server used for the Shellshock detection exercise.
- **Bash:** Linux command-line shell used during security testing.
- **Linux Terminal:** Execution of commands and system investigations.
- **Email Header Analysis:** Examination of sender information, routing details and authentication results.
- **SPF, DKIM and DMARC:** Email authentication mechanisms used to assess sender legitimacy.
- **SHA-256:** Cryptographic hashing for identifying and comparing suspicious files.
- **IOC Register:** Consolidated record of suspicious IP addresses, domains, URLs and email indicators.
- **MITRE ATT&CK:** Framework for understanding and classifying adversary techniques.

## Configuration Steps

### 1. Investigate Suspicious Phishing Emails

Four suspicious emails were examined to identify potential phishing and financial fraud activities.

The investigation involved:

1. Reviewing email sender addresses and Return-Path information.
2. Examining Reply-To addresses and sender domains.
3. Analysing email headers and routing information.
4. Checking SPF, DKIM and DMARC authentication results.
5. Extracting suspicious IP addresses, domains and URLs.
6. Enriching indicators using VirusTotal and AbuseIPDB.
7. Assessing the suspected objectives and confidence levels.
8. Recommending appropriate containment and response measures.

### 2. Investigate Microsoft 365 Brand Impersonation

The first email impersonated Microsoft 365 and attempted to persuade recipients to verify their accounts through a suspicious website.

**Key indicators:**

- Originating IP: `185.220.101.47`
- Return-Path domain: `mailer-srv-dispatch.xyz`
- Reply-To domain: `account-ms365-verify.top`
- Verification domain: `login-microsoft365-verify.top`
- SPF: Failed.
- DKIM: Failed.
- DMARC: Failed.

The originating IP received a 100% critical AbuseIPDB rating and was associated with a Tor exit node.

**Assessment:** Malicious phishing attempt involving suspected credential harvesting.

**Confidence:** 95%.

### 3. Investigate Business Email Compromise (BEC)

The second email impersonated Delta Supplies Corp and requested urgent payment using recently changed banking details.

**Key indicators:**

- Invoice reference: `DS-2026-04418`
- Subject: Urgent payment due.
- Sender domain: `fast-invoice-send.ru`
- Beneficiary name discrepancy: Delta Supplies Corp versus Delta Supplies Corp LLC.
- IP addresses: `194.165.16.101` and `194.165.16.88`.
- SPF, DKIM and DMARC: Failed.

The investigation identified suspicious payment instructions and inconsistencies in the sender and beneficiary information.

**Assessment:** Suspected Business Email Compromise and invoice-redirection fraud.

**Confidence:** 95%.

### 4. Investigate DHL Delivery Phishing

The third email impersonated DHL and requested a $2.99 delivery administration fee.

**Key indicators:**

- Suspicious domain: `dhl-delivery-info.xyz`
- Rescheduling URL: `https://dhl-delivery-info.xyz/reschedule`
- Support URL: `https://dhl-delivery-info.xyz/support`
- IP addresses: `176.111.174.55` and `176.111.174.33`.
- SPF, DKIM and DMARC: Failed.

VirusTotal identified the supplied URLs as suspicious/phishing links.

**Assessment:** Malicious delivery impersonation and suspected payment-card harvesting.

**Confidence:** 98%.

### 5. Investigate Bank of America Password-Reset Phishing

The fourth email impersonated Bank of America and attempted to persuade the recipient to reset their password through a fraudulent website.

**Key indicators:**

- Suspicious domain: `secure-boa-alerts.cc`
- Phishing URL: `https://secure-boa-alerts.cc/online-banking/verify`
- IP address: `91.108.56.122`
- SPF: Failed.
- DKIM: Failed.
- DMARC: Failed.
- DKIM RSA key size: 512 bits.

VirusTotal flagged the password-reset URL.

**Assessment:** Malicious banking phishing attempt involving suspected credential theft.

**Confidence:** 99%.

### 6. Create a Consolidated IOC Register

The indicators extracted from all four emails were consolidated into an IOC register.

The register included:

- Suspicious IP addresses.
- Sender email addresses.
- Malicious and suspicious domains.
- Phishing URLs.
- Email authentication failures.
- IP reputation findings.
- Threat classifications.

The IOC register provided a central reference for security monitoring, email gateway investigation and SIEM threat-intelligence updates.

### 7. Perform Ubuntu Endpoint Health Checks

Before conducting the detection exercises, the Ubuntu endpoint was checked to establish its baseline health and confirm that the Wazuh agent was active.

The pre-test checks included:

- Reviewing the Wazuh dashboard.
- Confirming Ubuntu agent connectivity.
- Checking the Ubuntu operating system information.
- Reviewing endpoint system information.
- Checking Kali Linux operating system details.
- Confirming Kali network interface configuration.

The Kali network interface `eth0` was configured with IP address `192.168.1.53`.

### 8. Conduct YARA Malware Detection and File Integrity Investigation

The first Wazuh detection exercise focused on identifying suspicious file modifications within the malware test directory.

**Test file:**

```bash
/tmp/yara/malware/mirai
```

**Recorded SHA-256 hashes:**

Before modification:

```text
f69560fda5f15a0a8693208171720cc93643e8de1001f227d7fcc893465df1e1
```

After modification:

```text
2aa2dbae345938386d93ed02d4f3aa3ad1b130c04b04e4e5c5c0d0e4ffe3181
```

**Wazuh detection details:**

- Endpoint: Ubuntu2.
- Date: 6 November 2026.
- Time: 20:44:49.823.
- Detection: File modified in `/tmp/yara/malware/`.

Additional file activity was identified involving:

- `webshell`
- `vpn_filter`
- `xbash`

The different SHA-256 hashes confirmed that the Mirai-labelled file had changed.

The supplied evidence confirmed file modification but did not independently establish that the file was genuine Mirai malware or that it had executed.

### 9. Investigate a Suspicious Linux System Binary

The second Wazuh detection exercise investigated a suspected trojaned Linux system executable.

**Detected binary:**

```bash
/usr/bin/w
```

Wazuh Rootcheck generated an alert identifying a trojaned version of the binary.

**Detection details:**

- Endpoint: Ubuntu2.
- Agent IP: `192.168.1.55`.
- Rule ID: `510`.
- Severity level: `7`.
- Detection time: 6 November 2026, 13:41:35.053.
- Decoder: Rootcheck.
- Signature: `uname -a|proc\.h|bash`.

The investigation identified suspicious process-segfault events shortly before the Rootcheck alert.

The suspected objective was to disguise malicious commands within a legitimate Linux system executable to perform system reconnaissance and execute Bash commands.

### 10. Conduct Shellshock Attack-Pattern Detection

The third exercise tested Wazuh's ability to detect a Shellshock-style HTTP request.

The Kali testing machine sent an HTTP GET request to the Ubuntu web server.

**Test payload:**

```bash
() { :; }; /bin/cat /etc/passwd
```

**Network details:**

- Source IP: `192.168.1.53` (Kali Linux).
- Destination IP: `192.168.1.52` (Ubuntu web server).
- HTTP method: `GET`.
- Requested resource: `/`.
- HTTP response status: `200`.
- Log source: `/var/log/apache2/access.log`.

**Wazuh detection details:**

- Rule ID: `31168`.
- Severity level: `15`.
- Alert: Shellshock attack detected.
- Date: 9 November 2026.
- Alert time: 13:32:26.863.

The alert confirmed that Wazuh successfully detected the Shellshock attack pattern.

The HTTP 200 response confirmed that the request reached the web server, but did not independently prove successful execution of the embedded command or disclosure of `/etc/passwd`.

### 11. Review Cleanup and Restoration Evidence

The final stage involved reviewing Wazuh file integrity monitoring events associated with endpoint restoration activities.

The supplied evidence showed:

- File deletion activity involving `/etc/cups/subscriptions.conf.N`.
- File modification and checksum-change events.
- Process name: `/usr/sbin/cupsd`.
- Parent process: `/usr/lib/systemd/systemd`.
- Executing user: `root`.
- File monitoring mode: `whodata`.

The deletion event was recorded on 6 November 2026 at 13:42:50.625.

**Important:** The deletion event relates to a CUPS configuration file. It does not prove that the suspected trojaned `/usr/bin/w` binary or the YARA sample was removed or restored.

Additional cleanup verification is required:

1. Preserve forensic copies and hashes of suspicious files.
2. Quarantine or remove test artefacts using the authorised lab procedure.
3. Restore `/usr/bin/w` from a trusted Ubuntu package source if tampering is confirmed.
4. Verify package integrity.
5. Run YARA again and retain the scan output.
6. Review authentication, process, Apache and system logs.
7. Repeat the endpoint health check.
8. Confirm the Wazuh agent is active after remediation.
9. Record cleanup timestamps, commands and verification results.

## Results and Findings

The project successfully demonstrated practical SOC investigation and threat detection capabilities across phishing analysis, threat intelligence enrichment and endpoint security monitoring.

### Part A: Phishing Investigation Results

Four suspicious emails were investigated and classified as malicious.

| Email | Attack Technique | Confidence |
|---|---|---|
| Microsoft 365 | Brand impersonation and credential harvesting | 95% |
| Delta Supplies | Business Email Compromise and invoice fraud | 95% |
| DHL | Delivery impersonation and payment-card phishing | 98% |
| Bank of America | Banking credential harvesting | 99% |

**Key findings:**

- All four emails failed the reported SPF, DKIM and DMARC authentication checks.
- Multiple suspicious sender domains and email addresses were identified.
- Suspicious IP addresses were extracted and enriched using AbuseIPDB.
- VirusTotal identified malicious or suspicious phishing URLs.
- Four different suspected attack objectives were established.
- A consolidated IOC register was produced.

### Part B: Wazuh Detection Results

| Investigation | Detection | Result |
|---|---|---|
| YARA malware detection | File modification in `/tmp/yara/malware/` | Modification confirmed |
| Suspicious binary detection | Trojaned `/usr/bin/w` | Rootcheck alert generated |
| Shellshock detection | Malicious HTTP request pattern | Alert successfully generated |

**YARA Investigation**

- Wazuh recorded file integrity changes.
- Different SHA-256 hashes confirmed file modification.
- Additional suspicious filenames were identified in the malware directory.
- Genuine malware attribution and execution were not independently confirmed.

**Suspicious Binary Investigation**

- Wazuh Rootcheck detected a suspected trojaned `/usr/bin/w` binary.
- Rule ID `510` generated a severity level `7` alert.
- Related process-segfault events were identified.
- Complete binary restoration was not verified by the supplied evidence.

**Shellshock Investigation**

- Kali Linux generated a Shellshock-style HTTP request.
- Apache recorded the request in its access log.
- Wazuh generated a high-severity alert.
- Rule ID `31168` recorded the detection at severity level `15`.
- Successful execution of the embedded command was not independently established.

### Consolidated Investigation Timeline

| Date and Time | Activity | Evidence |
|---|---|---|
| Before testing | Ubuntu health check | Wazuh agent and system information |
| 6 Nov 2026, 13:41:29–13:41:33 | Process-segfault events | Wazuh event records |
| 6 Nov 2026, 13:41:35 | Suspicious binary detection | Rootcheck alert |
| 6 Nov 2026, 13:42:14–13:42:50 | File integrity changes | File addition, deletion and checksum events |
| 6 Nov 2026, 20:44:49 | YARA/FIM test | Mirai-labelled file modification |
| 6 Nov 2026, 20:44:54–20:44:55 | Additional malware-directory activity | File additions and modifications |
| 9 Nov 2026, 13:32:24 | Shellshock test request | Apache access log |
| 9 Nov 2026, 13:32:26 | Shellshock alert | Wazuh rule `31168` |
| 9 Nov 2026, 13:33:43 | Listened-port status change | Wazuh event list |

### Key Skills Demonstrated

Through this project, I developed practical experience in:

- Phishing email investigation.
- Email header analysis.
- Business Email Compromise detection.
- Indicators of Compromise identification.
- Threat intelligence enrichment.
- IP reputation analysis.
- SIEM monitoring and alert investigation.
- Wazuh File Integrity Monitoring.
- YARA-related malware detection.
- Linux Rootcheck investigation.
- Suspicious binary analysis.
- Shellshock attack-pattern detection.
- Apache access-log analysis.
- Incident response recommendations.
- Evidence preservation and cleanup verification.

### Security Recommendations

The investigation identified several security improvements:

- Strengthen email security controls and sender authentication.
- Block confirmed malicious domains, URLs and IP addresses.
- Implement phishing awareness training.
- Establish independent verification procedures for vendor banking changes.
- Monitor suspicious account authentication and password-reset activity.
- Maintain updated Linux systems and security patches.
- Verify system binary integrity using trusted package sources.
- Continue Wazuh file integrity and Rootcheck monitoring.
- Preserve forensic evidence during malware investigations.
- Perform post-remediation YARA scans and endpoint health checks.
- Monitor network activity for connections to identified suspicious infrastructure.

### Conclusion

This capstone project demonstrates the practical application of SOC operations across phishing investigation, threat intelligence, malware detection and security incident monitoring.

In Part A, four malicious phishing emails were investigated, resulting in the identification of credential-harvesting attempts, business email compromise, delivery impersonation and banking fraud indicators.

In Part B, Wazuh successfully generated alerts for suspicious file modifications, a suspected trojaned Linux system binary and a Shellshock attack pattern.

The project strengthened my practical understanding of the SOC incident management lifecycle: **Detect, Investigate, Enrich, Respond and Verify**.

**Final SOC Assessment:** The phishing investigations identified four malicious emails, while the Wazuh exercises demonstrated successful detection of three categories of suspicious activity. However, successful malware execution, complete remediation and full endpoint restoration were not established by all the supplied evidence. Further forensic analysis and post-cleanup verification are required before declaring the laboratory environment fully clean.

## Author

**Taofeek Abdulkadir**

**Aspiring SOC Analyst | Cybersecurity Analyst**

**Areas of Interest:**
- Security Operations Centre (SOC)
- Threat Intelligence and Threat Hunting
- Security Information and Event Management (SIEM)
- Phishing Investigation
- Malware Analysis
- Incident Detection and Response
- Digital Forensics
- Vulnerability Management
- Linux Security

**GitHub:** https://github.com/taofeekadri-cmd/Threat-Intelligence-Project/edit/main/README.md

**LinkedIn:www.linkedin.com/in/taofeek-abdulkadir-08a439b4

**Email:** taofeekadri@gmail.com
