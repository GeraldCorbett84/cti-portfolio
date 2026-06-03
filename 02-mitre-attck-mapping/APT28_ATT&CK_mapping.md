# MITRE ATT&CK Mapping: APT28 (Fancy Bear)

APT28 behavior mapped to MITRE ATT&CK Tactics and Techniques based on publicly documented campaigns including Operation Pawn Storm, the DNC breach, and Bundestag intrusion.

| Tactic | Technique ID | Technique Name | Example Procedure |
|--------|-------------|----------------|-------------------|
| Initial Access | T1566.001 | Phishing: Spearphishing Attachment | APT28 sent malicious Word documents with embedded macros to NATO defense officials, masquerading as legitimate conference invitations |
| Initial Access | T1566.002 | Phishing: Spearphishing Link | Deployed fake Outlook Web Access login pages hosted on lookalike domains to harvest credentials from government staff |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Used obfuscated PowerShell scripts to download and execute second-stage payloads from C2 servers post-compromise |
| Persistence | T1053.005 | Scheduled Task/Job: Scheduled Task | Created scheduled tasks disguised as Windows Update processes to maintain persistence across reboots |
| Credential Access | T1003.001 | OS Credential Dumping: LSASS Memory | Used a custom variant of Mimikatz embedded in X-Agent to dump LSASS memory and extract NTLM hashes |
| Credential Access | T1550.002 | Use Alternate Authentication Material: Pass the Hash | Leveraged harvested NTLM hashes to authenticate laterally to file servers without cracking plaintext passwords |
| Discovery | T1083 | File and Directory Discovery | X-Agent component systematically enumerated file systems searching for documents, spreadsheets, and email archives |
| Lateral Movement | T1021.001 | Remote Services: Remote Desktop Protocol | Used stolen admin credentials to RDP into additional hosts within the compromised network segment |
| Collection | T1114.002 | Email Collection: Remote Email Collection | Accessed Office 365 and Exchange mailboxes via OWA using compromised credentials to collect communications |
| Command & Control | T1573.002 | Encrypted Channel: Asymmetric Cryptography | X-Tunnel used RSA encryption for C2 communications, blending with legitimate HTTPS traffic on port 443 |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Staged collected files in encrypted archives before exfiltrating through the existing C2 channel to avoid detection |
| Defense Evasion | T1027 | Obfuscated Files or Information | Payloads delivered with multiple layers of base64 encoding and XOR obfuscation to evade static AV signatures |

---
*Sources: MITRE ATT&CK G0007, Mandiant APT28 Report, CrowdStrike Fancy Bear Profile*
