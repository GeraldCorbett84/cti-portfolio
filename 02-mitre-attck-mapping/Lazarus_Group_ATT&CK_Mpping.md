# MITRE ATT&CK Mapping: Lazarus Group (Hidden Cobra)

Lazarus Group behavior mapped to MITRE ATT&CK Tactics and Techniques based on publicly documented campaigns including Operation Dream Job, the 3CX supply chain attack, Bangladesh Bank heist, and Sony Pictures breach.

| Tactic | Technique ID | Technique Name | Example Procedure |
|--------|-------------|----------------|-------------------|
| Resource Development | T1583.001 | Acquire Infrastructure: Domains | Registered cryptocurrency exchange lookalike domains (e.g., "bittreх.com") used in AppleJeus campaigns |
| Initial Access | T1195.002 | Supply Chain Compromise: Compromise Software Supply Chain | Trojanized legitimate cryptocurrency trading software (Celas Trade Pro) with backdoored installer distributed via official-looking website |
| Initial Access | T1566.003 | Phishing: Spearphishing via Service | Created fake LinkedIn recruiter profiles targeting aerospace engineers, sending malicious PDF "job descriptions" containing BLINDINGCAN |
| Execution | T1204.002 | User Execution: Malicious File | Victims manually executed trojanized cryptocurrency applications, triggering staged payload delivery |
| Persistence | T1543.003 | Create or Modify System Process: Windows Service | Installed FASTCash as a Windows service on bank payment switch servers to intercept and manipulate ATM transactions |
| Defense Evasion | T1497.001 | Virtualization/Sandbox Evasion: System Checks | BLINDINGCAN checked for virtualization artifacts (registry keys, running processes) and halted execution if analysis environment detected |
| Credential Access | T1555.003 | Credentials from Password Stores: Credentials from Web Browsers | Extracted saved credentials from Chrome and Firefox profiles on compromised workstations |
| Discovery | T1018 | Remote System Discovery | Conducted internal network scanning to identify additional hosts, prioritizing financial systems and servers |
| Lateral Movement | T1563.002 | Remote Service Session Hijacking: RDP Hijacking | Hijacked existing RDP sessions using tscon.exe to move laterally without triggering new authentication events |
| Impact | T1485 | Data Destruction | Deployed Destover wiper malware at Sony Pictures, overwriting MBR and deleting files to destroy evidence and cause damage |
| Impact | T1657 | Financial Theft | Modified SWIFT transaction parameters in real time via FASTCash, redirecting funds to Lazarus-controlled accounts |
| Exfiltration | T1048.003 | Exfiltration Over Alternative Protocol: Exfiltration Over Unencrypted Non-C2 Protocol | Exfiltrated data from Sony Pictures via FTP to staging servers in Bolivia, Singapore, and Thailand |

---
*Sources: MITRE ATT&CK G0032, Mandiant APT38 Report, CISA AA21-048A, Kaspersky Lazarus Analysis*
