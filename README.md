# cybersecurity-labs
Medium articles I've written documenting the cyber security labs I've completed. 
# Cybersecurity Lab Portfolio & Write-ups

Welcome! This repository serves as an index for my hands-on cybersecurity labs, write-ups, and exercises. I currently work in the supply chain field and want to transition into a SOC Analyst style role. My full articles, detailed methodologies, and analysis are chronicled over on my Medium Profile: (https://medium.com/@anthonysito1).

This is a collection of some of the highlights. Please feel free to look at my profile and see the 20+ other labs and LetsDefend cases I've done. 

## 📂 Indexed Labs & Write-ups

### 🔹 Blue Team SOC Labs:
### 🔹 Windows Event Log Parsing & RDP Brute Force Analysis
*   **Lab Write-up:** [SOC Lab Write-up — Introduction & First Attack](https://medium.com/@anthonysito1/soc-lab-writeup-introduction-first-attack-f0fdedbabab5?postPublishedType=repub)
*   **Objective:** Deploy Winlogbeat and Sysmon to monitor a Windows 10 endpoint, simulate an RDP brute-force attack from a Kali Linux VM, and perform incident triage in Kibana.
*   **Related MITRE ATT&CK Framework:** Brute Force: Password Guessing (T1110.001), Remote Services: Remote Desktop Protocol (T1021.001)
*   **Key Skills Demonstrated:**
    *   **SIEM Log Analysis** Elasticsearch/Kibana (ELK)
    *   **Windows Security Event ID Tracking** Event code 4625 = Authentication Failures, Event Code 4624 = Success
    *   **Network Log Correlation** Sysmon Event ID `3` via Port 3389
    *   Distinguishing true malicious attacks from user-induced False Positives.
*   **Defensive Takeaways:** Proposed implementing standard Group Policy Account Lockout thresholds and alerting rules for Event ID `4740`.
### 🔹 Advanced Endpoint Detection: Backdoors, Privilege Escalation, & Persistence
*   **Lab Write-up:** [SOC Lab 2 — Backdoor, Group Add, & Scheduled Task Detection](https://medium.com/@anthonysito1/soc-lab-2-backdoor-group-add-scheduled-task-detection-31898690f0da?postPublishedType=repub)
*   **Objective:** Analyze a complete post-exploitation lifecycle within Kibana. Detect local account creation, privilege escalation to the Administrators group, and persistence mechanisms.
*   **Related MITRE ATT&CK Framework:** User Execution: Malicious File (T1204.002), C2 Connection: Application Layer Protocol (T1071), Scheduled Task/Job: Scheduled Task (T1053.005)
*   **Key Skills Demonstrated:**
    *   **Initial Access Triage:** Used Sysmon Event ID `15` to extract the malicious source URL (`hxxp://192[.]168[.]56[.]101:8080/payload2[.]exe`).
    *   **Privilege Escalation Tracking:** Monitored Windows Security Event IDs `4720` (Account Created) and `4732` (Privileged Group Addition) to trace the unauthorized `backdoor` user.
    *   **Persistence Analysis:** Parsed Windows Security Event ID `4698` to locate a masqueraded scheduled task (`WindowsUpdate2`) executing with `SYSTEM` privileges.
    *   **Attacker Behavior Mapping:** Correlated a surge in `whoami` discovery commands with privilege escalation events.
*   **Defensive Takeaways:** Recommended immediate host isolation/quarantine to cut off active Command and Control (C2) communication.
### 🔹 Fileless Malware: Obfuscated PowerShell & Network Forensics
*   **Lab Write-up:** [SOC Lab 10 — Fileless PowerShell via Base64 Encoding Attack](https://medium.com/@anthonysito1/soc-lab-10-fileless-powershell-via-base64-encoding-attack-a6068f85d124?postPublishedType=repub)
*   **Objective:** Investigate a malicious connection with Remote Desktop Protocol (RDP) and de-obfuscate a fileless PowerShell reverse shell payload executing in memory.
*   **Related MITRE ATT&CK Framework:** Remote Services: Remote Desktop Protocol (T1021.001), Command and Scripting Interpreter: Powershell (T1059.001), Obfuscated Files or Information: Command Obfuscation (T1027.010)
*   **Key Skills Demonstrated:**
    *   **Network Packet Analysis:** Utilized Wireshark to parse unencrypted RDP initialization packets, identifying the rogue user string (`Cookie: mstshash=backdoor`).
    *   **SIEM Triage Execution:** Filtered 104,000+ cluster events down to Windows Security Event ID `4624` (Logon Type 10) to validate true positive network connections.
    *   **De-obfuscation & Code Analysis:** Filtered PowerShell Script Block Logging (Event ID `4104`) for the `-enc` flag, using CyberChef to decode a Base64 string executing a `.NET` TCP Socket connection.
    *   **Egress Traffic Validation:** Queried Sysmon Event ID `3` to cross-reference network connections, verifying the connection to determine if C2 sessions or data exfiltration occurred.
*   **Defensive Takeaways:** Recommended narrowing corporate RDP exposure to specific VPN entry gateways, enabling PowerShell Constrained Language Mode (CLM), and building SIEM alerts for command-line encoding arguments.
### 🔹 Linux Host Forensics: SSH Compromise & Log Destruction Triage
*   **Lab Write-up:** [SOC Lab 20 — Log Destruction](https://medium.com/@anthonysito1/soc-lab-20-log-destruction-0e59f75aeba8?postPublishedType=repub)
*   **Objective:** Shift security telemetry monitoring to a native Linux environment (`Linux Mint`), analyzing an active credential abuse vector over SSH followed by anti-forensics log tampering actions.
*   **Related MITRE ATT&CK Framework:** Defense Evasion: Indicator Removal (T1070)
*   **Key Skills Demonstrated:**
    *   **Linux Audit Logging:** Monitored logs via `system.auth.ssh` and `system.auth.sudo` inside Kibana.
    *   **Anti-Forensics Triage:** Detected malicious execution of the `truncate` command targeting critical system audit files (`/var/log/auth.log` and `/var/log/syslog`).
    *   **SIEM Pipeline Validation:** Analyzed the behavior of the SIEM pipeline, verifying that real-time log ingestion preserves forensic evidence despite local system wiping.
    *   **Post-Exploitation Timeline Auditing:** Audited shell command executions to successfully verify attacker containment and scope of impact.
*   **Defensive Takeaways:** Advised immediate enforcement of key-based SSH access, disabling native root SSH authorization flags, and implementing append-only system logging attributes.
### 🔹 Phishing, Malware Execution, & C2 Log Correlation Analysis with Splunk
*   **Lab Write-up:** [SOC Lab 23 — Phishing & C2 Triage](https://medium.com/@anthonysito1/soc-lab-23-user-downloaded-malicious-exe-b98a18cc2d16)
*   **Objective:** Simulate a phishing-driven malware delivery pipeline using `msfvenom` against a Windows 10 target, triage the infection timeline using Splunk, and track endpoint-to-network actions using Sysmon events.
*   **Related MITRE ATT&CK Framework:** Phishing: Malicious Link (T1566.002), User Execution: Malicious File (T1204.002), C2 Connection: Application Layer Protocol (T1071)
*   **Key Skills Demonstrated:**
    *   **SIEM Log Correlation:** Used Splunk transaction-based queries to chronologically map network and host-based events back to a single malicious IP.
    *   **Sysmon Event Code Tracking:** Analyzed `Event ID 15` (File Creation) and `Event ID 1` (Process Creation) to establish proof of download and binary execution.
    *   **Network Behavioral Mapping:** Identified anomalous outbound network connections originating from untrusted administrative and user environments (`\Downloads\`).
*   **Defensive Takeaways:** Recommended deploying automated email gateway attachment sandboxing, implementing strict web content filtering, enforcing host isolation policies, and blocking verified malicious Command and Control (C2) IPs at the perimeter.
### 🔹 Privilege Escalation: Unquoted Service Path & Runtime Compilation with Splunk
*   **Lab Write-up:** [SOC Lab 24 — Unquoted Service Path Attack](https://medium.com/@anthonysito1/soc-lab-24-unquoted-service-path-attack-d19d1e7f54f5)
*   **Objective:** Emulate and investigate local privilege escalation via a Windows path interception vulnerability, tracking lateral movement and runtime compilation behaviors via SIEM logging.
*   **Related MITRE ATT&CK Framework:** Hijack Execution Flow: Path Interception by Unquoted Path (T1574.009), Command and Scripting Interpreter: Powershell (T1059)
*   **Key Skills Demonstrated:**
    *   **Windows Internal Forensics:** Exploited and triaged Windows Service Control Manager parsing logic by detecting an issue with how Windows parses unquoted paths with spaces.
    *   **In-Memory Compilation Triage:** Parsed Sysmon Event ID `1` logs, including PowerShell execution of `.NET` compilation arguments (`Add-Type`, `OutputAssembly`).
    *   **Lateral Movement Auditing:** Reconstructed a `wmiexec` remote administrative pipeline by correlating `WmiPrvSE.exe` processes, loopback address communication states, and Windows Event ID `5145` (SMB/ADMIN$ Share access).
*   **Defensive Takeaways:** Recommended registry-level auditing of service binary paths to enforce string quotation encapsulations, hardening root folder file-write system permissions, and restricting interactive host SMB access.

### 🔹 LetsDefend Case Writeups:
* https://medium.com/@anthonysito1/soc338-lumma-stealer-dll-side-loading-via-click-fix-phishing-1bc2dca4a9e4 – LetsDefend SIEM challenge for alert on DLL Side-Loading
* https://medium.com/@anthonysito1/soc167-ls-command-detected-in-requested-url-99cd0cfbd54b – LetsDefend SIEM challenge for false alarm alert where "ls" is in the URL.
* https://medium.com/@anthonysito1/soc173-follina-0-day-detected-cdb036b88548 - LetsDefend SIEM challenge for a CVE called Follina.
* https://medium.com/@anthonysito1/soc326-impersonating-domain-mx-record-change-detected-94da4c5e1e76 - LetsDefend SIEM challenge involving an attacker who typosquatted a domain and eventually lead to a reverse shell
* https://medium.com/@anthonysito1/macos-malware-letsdefend-challenge-53b312381eca - LetsDefend Challenge analyzing Mac Stealthware
* https://medium.com/@anthonysito1/pdf-analysis-letsdefend-challenge-82e36868598a - LetsDefend challenge analyzing a malicious PDF.
* https://medium.com/@anthonysito1/soc202-fakegpt-malicious-chrome-extension-8a1208468ce2 - LetsDefend SIEM challenge involving a malicious Chrome extension. 

There are dozens of more case writeups on my medium as well, these are just what I believe are some of the best.

---
## 📬 Connect with Me
* **LinkedIn:** (https://www.linkedin.com/in/anthony-sito-253985193)
