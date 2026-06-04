# 🧭 MITRE ATT&CK Mapping: APT29 / Midnight Blizzard / Cozy Bear

> This report maps **APT29 / Midnight Blizzard / Cozy Bear** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G0016-blue)

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

APT29, also tracked as Midnight Blizzard and Cozy Bear, is a Russian state-sponsored espionage actor attributed by MITRE ATT&CK to Russia's Foreign Intelligence Service (SVR). This mapping focuses on observable enterprise behaviors associated with APT29 operations, including phishing, software supply chain compromise, valid account abuse, cloud token theft, mailbox collection, and stealthy command and control.

> [!WARNING]
> APT29 frequently targets identity, email, cloud, and software trust paths. Organizations that only monitor endpoint malware may miss cloud-first intrusion activity.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | APT29 / Midnight Blizzard / Cozy Bear |
| **MITRE Group ID** | G0016 |
| **Primary Mission** | Espionage, strategic intelligence collection, long-term access |
| **Common Initial Access** | Spearphishing, valid accounts, software supply chain compromise |
| **Common Persistence** | Account manipulation, cloud role abuse, application or token abuse |
| **Common Collection Target** | Email, cloud repositories, credentials, diplomatic and government data |
| **Recommended Telemetry** | Email security logs, Entra ID / Azure AD logs, O365 audit logs, EDR, PowerShell logs, DNS, proxy, identity provider logs |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Resource Development | T1583.001 | Acquire Infrastructure: Domains | Registered or used attacker-controlled domains to support phishing, staging, and C2 infrastructure. | Monitor newly registered domains, lookalike infrastructure, and rare destinations tied to executive or government-themed lures. |
| Initial Access | T1566.002 | Phishing: Spearphishing Link | Used phishing links to drive victims toward credential capture or malware delivery. | Detect email links to newly seen domains, credential-harvesting pages, and abnormal click-through patterns. |
| Initial Access | T1195.002 | Supply Chain Compromise: Compromise Software Supply Chain | Leveraged trusted software update paths in campaigns such as SolarWinds-style supply chain compromise. | Monitor signed software spawning unusual processes, new network destinations after updates, and anomalous vendor application behavior. |
| Initial Access | T1078.004 | Valid Accounts: Cloud Accounts | Used compromised cloud identities to access email, SaaS, and cloud resources. | Alert on impossible travel, unfamiliar device sign-ins, suspicious OAuth consent, and new MFA/security info changes. |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Executed PowerShell for discovery, payload staging, and post-compromise actions. | Detect encoded PowerShell, suspicious parent-child process chains, and PowerShell network connections. |
| Persistence | T1098.003 | Account Manipulation: Additional Cloud Roles | Modified cloud permissions or roles to maintain access to cloud resources. | Monitor privileged role assignments, service principal changes, and newly granted application permissions. |
| Credential Access | T1528 | Steal Application Access Token | Stole or abused tokens to access cloud-hosted resources without needing passwords. | Hunt for token use from new IPs, abnormal user agents, and access continuing after password resets. |
| Credential Access | T1552.004 | Unsecured Credentials: Private Keys | Targeted keys and secrets that could provide access to cloud or internal systems. | Detect access to key stores, certificate exports, unusual reads of secrets, and file access to key material. |
| Discovery | T1087 | Account Discovery | Enumerated users, groups, and privileged accounts after gaining access. | Monitor identity enumeration, graph API calls, and bulk directory lookups. |
| Collection | T1114.002 | Email Collection: Remote Email Collection | Collected email remotely through compromised accounts and cloud mail services. | Alert on high-volume mailbox reads, mailbox export activity, and unusual access to sensitive mailboxes. |
| Command & Control | T1568 | Dynamic Resolution | Used dynamic DNS or resilient infrastructure patterns to support C2. | Monitor rare dynamic DNS domains, suspicious domain rotation, and unusual DNS query patterns. |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Exfiltrated collected data through established command-and-control channels. | Look for archive creation followed by outbound transfer to rare external destinations. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **Cloud Identity Abuse** | Alert on new OAuth grants, application consent, or role assignment from unusual users or locations. | APT29 frequently targets cloud identity and trusted access paths. |
| **Mailbox Collection** | Detect abnormal mailbox access volume, export activity, or access from unfamiliar locations. | Email remains a high-value intelligence collection target. |
| **PowerShell Execution** | Alert on encoded PowerShell, PowerShell with network activity, and scripts launched by Office or browser processes. | PowerShell supports post-compromise execution and staging. |
| **Token Misuse** | Correlate token use with device, IP, geolocation, and user-agent anomalies. | Token theft can survive password changes and bypass traditional login detections. |
| **Supply Chain Behavior** | Baseline vendor software and detect new child processes or network traffic after updates. | Trusted software can become a launch point for stealthy compromise. |
| **Rare External Infrastructure** | Detect connections to newly seen domains or dynamic DNS from sensitive systems. | C2 infrastructure may blend into normal web traffic. |

---

## ❓ Priority Hunt Questions

- Are there new privileged cloud role assignments or OAuth grants that lack a business reason?
- Are mailboxes being accessed from unusual IPs, countries, or user agents?
- Are tokens being used after password resets or MFA changes?
- Are signed vendor applications spawning command shells, PowerShell, or network tools?
- Are sensitive users performing unusually high-volume cloud or email reads?
- Are endpoints connecting to newly seen dynamic DNS or low-reputation domains?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Enforce phishing-resistant MFA for cloud, email, VPN, and privileged access. | Reduces the value of stolen passwords. |
| 2 | Audit OAuth consent, service principals, and cloud role assignments regularly. | APT29-style intrusions often target cloud trust relationships. |
| 3 | Enable advanced O365 / Exchange / Entra ID audit logging. | Supports mailbox and identity-based hunt activity. |
| 4 | Monitor token anomalies, not only password-based sign-ins. | Token theft can bypass basic login monitoring. |
| 5 | Restrict PowerShell and script execution through EDR and application control. | Reduces common post-exploitation execution paths. |
| 6 | Harden software update and vendor trust monitoring. | Improves detection of supply chain-style intrusion behavior. |

---

## 📚 Sources

- MITRE ATT&CK — APT29, Group G0016
- Microsoft Security reporting on Midnight Blizzard
- CISA / NSA / FBI reporting on SVR cyber operations
- Public reporting on SolarWinds and cloud identity-focused APT29 campaigns

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
