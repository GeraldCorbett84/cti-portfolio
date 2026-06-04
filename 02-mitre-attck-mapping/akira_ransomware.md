# 🧭 MITRE ATT&CK Mapping: Akira Ransomware

> This report maps **Akira Ransomware** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Ransomware-blue)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G1024-blue)

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

Akira is a ransomware variant and ransomware deployment entity tracked by MITRE ATT&CK as G1024. Public advisories describe Akira operations as double extortion activity, with initial access often involving compromised credentials and external remote services such as VPNs. This mapping focuses on identity abuse, remote services, credential dumping, remote access tools, discovery, exfiltration, backup disruption, and encryption impact.

> [!WARNING]
> Akira activity may begin as ordinary remote access using compromised credentials. VPN, identity, and backup telemetry are high-priority data sources.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | Akira Ransomware |
| **MITRE Group ID** | G1024 |
| **Primary Mission** | Financial extortion through data theft and ransomware encryption |
| **Common Initial Access** | External remote services, compromised VPN credentials, exploitation of public-facing systems |
| **Common Persistence** | Valid accounts, remote access software, scheduled tasks, created accounts |
| **Common Collection Target** | Files, backups, sensitive business data, virtual machine storage, credentials |
| **Recommended Telemetry** | VPN logs, identity logs, EDR, Windows event logs, backup logs, file server logs, remote access tool telemetry, firewall and proxy logs |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Initial Access | T1133 | External Remote Services | Used remote access services such as RDP or VPN connections to gain access. | Monitor VPN/RDP logins from unusual locations, unfamiliar devices, or after credential events. |
| Initial Access | T1078 | Valid Accounts | Used compromised credentials for initial access and lateral movement. | Detect impossible travel, new device sign-ins, and unusual privileged account use. |
| Initial Access | T1190 | Exploit Public-Facing Application | Exploited vulnerable internet-facing services in some intrusions. | Monitor exploit attempts and patch high-risk edge systems quickly. |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Used PowerShell for execution and administrative tasks. | Detect encoded PowerShell and suspicious script execution from remote sessions. |
| Execution | T1059.003 | Command and Scripting Interpreter: Windows Command Shell | Used command shell for discovery, staging, and tooling execution. | Alert on `cmd.exe` executing from remote logon sessions or administrative shares. |
| Persistence | T1136 | Create Account | Created or used accounts to maintain access. | Monitor new local/domain accounts and privilege group changes. |
| Defense Evasion | T1562.001 | Impair Defenses: Disable or Modify Tools | Disabled or modified security tools before ransomware deployment. | Alert on EDR tampering, service stops, and security policy changes. |
| Credential Access | T1003.001 | OS Credential Dumping: LSASS Memory | Dumped credentials to support lateral movement. | Detect LSASS handle access, dump file creation, and credential theft tools. |
| Discovery | T1083 | File and Directory Discovery | Enumerated file shares and directories to identify data for theft/encryption. | Detect high-volume file enumeration and recursive directory listing. |
| Lateral Movement | T1021.001 | Remote Services: RDP | Used RDP to move laterally across victim environments. | Monitor RDP logons from unusual systems or by accounts that do not normally use RDP. |
| Command & Control | T1219 | Remote Access Software | Used legitimate tools such as remote monitoring and management software. | Detect unauthorized AnyDesk, LogMeIn, ScreenConnect, TeamViewer, or similar tools. |
| Impact | T1486 | Data Encrypted for Impact | Encrypted victim data as part of ransomware operations. | Detect mass file modification, new ransomware extensions, and abnormal encryption-like behavior. |
| Impact | T1490 | Inhibit System Recovery | Targeted backups and recovery mechanisms. | Monitor backup deletion, VSS deletion, and changes to backup infrastructure. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **VPN Credential Abuse** | Alert on VPN access from new countries, devices, autonomous systems, or after credential reset events. | Akira commonly abuses remote access services. |
| **Remote Access Tool Installation** | Detect newly installed or portable RMM tools on servers and workstations. | RMM tools may support hands-on-keyboard activity. |
| **Credential Dumping** | Monitor LSASS, registry hive, and credential dumping tool activity. | Credential theft enables fast lateral movement. |
| **Backup Tampering** | Alert on deletion or modification of backups, VSS, or backup jobs. | Ransomware actors often impair recovery. |
| **File Enumeration** | Detect mass file listing or archive staging before encryption. | Data theft often precedes encryption. |
| **Mass Encryption** | Detect rapid file rename/write activity across shares. | Provides early warning of impact stage. |

---

## ❓ Priority Hunt Questions

- Did suspicious VPN access occur before internal discovery or lateral movement?
- Are newly created accounts or privilege changes tied to remote access sessions?
- Are RMM tools present on systems where they are not approved?
- Are backups, snapshots, or VSS copies being deleted or modified?
- Is there archive creation followed by outbound transfer?
- Are file shares experiencing mass rename, write, or encryption-like activity?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Enforce MFA on VPN, RDP, and all external remote services. | Reduces credential-only access risk. |
| 2 | Patch internet-facing systems and VPN appliances quickly. | Reduces exploit-driven initial access. |
| 3 | Restrict and monitor remote access tools. | Limits attacker-operated RMM activity. |
| 4 | Protect backups with immutability and separate credentials. | Improves ransomware recovery. |
| 5 | Monitor LSASS and credential dumping behavior. | Disrupts lateral movement. |
| 6 | Segment file servers and restrict admin shares. | Limits encryption blast radius. |

---

## 📚 Sources

- MITRE ATT&CK — Akira, Group G1024
- CISA / FBI / DC3 / HHS Advisory AA24-109A — #StopRansomware: Akira Ransomware
- Public reporting on Akira double extortion operations
- MITRE ATT&CK technique entries for external remote services, credential dumping, and ransomware impact

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
