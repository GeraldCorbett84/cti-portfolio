# 🧭 MITRE ATT&CK Mapping: APT28 / Fancy Bear

> This report maps **APT28 / Fancy Bear** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group-G0007-blue)

---

## 📌 Table of Contents

- [Executive Summary](#-executive-summary)
- [Mapping Overview](#-mapping-overview)
- [ATT&CK Technique Mapping](#-attck-technique-mapping)
- [Detection Engineering Opportunities](#-detection-engineering-opportunities)
- [Priority Hunt Questions](#-priority-hunt-questions)
- [Defensive Recommendations](#-defensive-recommendations)
- [Sources](#-sources)

---

## 🧠 Executive Summary

APT28 is a Russian state-sponsored threat group known for credential phishing, malware deployment, political targeting, and long-running espionage campaigns. Mapping its activity to MITRE ATT&CK helps defenders convert threat intelligence into actionable detections and hunt logic.

This mapping emphasizes behaviors that defenders can observe in enterprise telemetry, including **phishing delivery, PowerShell execution, scheduled task persistence, credential dumping, remote email collection, encrypted command and control, and exfiltration over C2 channels**.

> [!WARNING]
> APT28 activity often begins with credential theft. Organizations relying only on malware detections may miss early-stage intrusion activity.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | APT28 / Fancy Bear |
| **MITRE Group ID** | G0007 |
| **Primary Mission** | Espionage, political interference, strategic disruption |
| **Common Initial Access** | Spearphishing attachments and links |
| **Common Persistence** | Scheduled tasks and backdoors |
| **Common Collection Target** | Email, files, credentials, political or defense-related data |
| **Recommended Telemetry** | Email security logs, EDR telemetry, Windows event logs, O365 / Exchange logs, DNS, proxy logs |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Initial Access | T1566.001 | Phishing: Spearphishing Attachment | Malicious Word documents with embedded macros sent to defense or government targets | Office document spawning script interpreter or command shell |
| Initial Access | T1566.002 | Phishing: Spearphishing Link | Fake Outlook Web Access login pages hosted on lookalike domains | Newly registered domains, credential submission pages, abnormal OWA login sources |
| Execution | T1059.001 | PowerShell | Obfuscated PowerShell used to download and execute second-stage payloads | Encoded PowerShell, network connections from PowerShell, suspicious child processes |
| Persistence | T1053.005 | Scheduled Task / Job | Scheduled tasks disguised as Windows Update processes | Task creation events, unusual task names, tasks launching from user-writable paths |
| Credential Access | T1003.001 | LSASS Memory | Mimikatz-like tooling used to dump LSASS and extract NTLM hashes | LSASS access attempts, credential dumping alerts, suspicious handle access |
| Credential Access | T1550.002 | Pass the Hash | Harvested NTLM hashes used for lateral authentication | Lateral SMB/RDP authentication without normal interactive login patterns |
| Discovery | T1083 | File and Directory Discovery | X-Agent enumerated documents, spreadsheets, and email archives | High-volume file enumeration, recursive directory listing, archive staging |
| Lateral Movement | T1021.001 | Remote Services: RDP | Stolen admin credentials used to access additional hosts | RDP logins from unusual hosts, impossible travel, abnormal admin sessions |
| Collection | T1114.002 | Remote Email Collection | Compromised OWA or Exchange accounts used to collect mailboxes | Mailbox export, unusual mailbox access, new inbox rules, high-volume email reads |
| Command & Control | T1573.002 | Encrypted Channel: Asymmetric Cryptography | X-Tunnel used encrypted C2 over port 443 | Long-lived HTTPS sessions, rare destinations, suspicious JA3/JA4 or certificate patterns |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Staged data exfiltrated through established C2 channel | Archive creation followed by outbound transfer to rare external infrastructure |
| Defense Evasion | T1027 | Obfuscated Files or Information | Multi-layer encoding and XOR obfuscation used to evade static signatures | Obfuscated script blocks, high entropy payloads, packed binaries |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **Office → Script Execution** | Alert when Office applications spawn `powershell.exe`, `cmd.exe`, `wscript.exe`, or `mshta.exe` | Common phishing-to-execution chain |
| **Suspicious Scheduled Tasks** | Detect task creation where the action points to user-writable paths or encoded commands | APT28 uses scheduled tasks for persistence |
| **Credential Dumping** | Monitor LSASS access by non-standard processes | Credential theft enables lateral movement |
| **Remote Email Collection** | Alert on abnormal OWA / Exchange mailbox access volume | APT28 frequently targets email content |
| **Rare C2 Destinations** | Detect long-lived outbound HTTPS sessions to low-reputation or newly seen domains | Encrypted C2 can blend into normal web traffic |
| **Archive Before Exfiltration** | Identify archive creation followed by outbound transfer | Collection and staging often precede exfiltration |

---

## ❓ Priority Hunt Questions

- Are any Office applications spawning scripting engines or command shells?
- Are scheduled tasks being created by unusual parent processes?
- Are privileged users authenticating from new endpoints or abnormal geographies?
- Are mailboxes being accessed in bulk through OWA or Exchange APIs?
- Are endpoints making long-lived outbound connections to rare external destinations?
- Are archive files being created shortly before unusual outbound network traffic?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Enforce phishing-resistant MFA for email and remote access | Reduces impact of credential harvesting |
| 2 | Block or restrict Office macros from internet-sourced documents | Disrupts common phishing execution chain |
| 3 | Monitor scheduled task creation across endpoints | Detects persistence activity |
| 4 | Enable advanced audit logging for O365 / Exchange | Supports detection of email collection |
| 5 | Deploy EDR detections for LSASS access and credential dumping | Reduces lateral movement risk |
| 6 | Baseline outbound traffic and monitor rare C2 destinations | Helps identify encrypted C2 channels |

---

## 📚 Sources

- MITRE ATT&CK — **G0007**
- Mandiant APT28 reporting
- CrowdStrike Fancy Bear profile
- Public reporting on Pawn Storm, DNC intrusion, Bundestag intrusion, and related APT28 activity

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against current MITRE ATT&CK entries and current vendor reporting before converting into production detections.
