# 🧭 MITRE ATT&CK Mapping: Scattered Spider / UNC3944 / Octo Tempest

> This report maps **Scattered Spider / UNC3944 / Octo Tempest** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Financially%20Motivated-blue)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G1015-blue)

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

Scattered Spider, also tracked as UNC3944 and Octo Tempest, is a financially motivated intrusion group tracked by MITRE ATT&CK as G1015. The actor is known for social engineering, help desk impersonation, MFA manipulation, SIM-swapping-style tactics, SaaS compromise, remote access tooling, and ransomware enablement. This mapping focuses on identity, cloud, help desk, and remote access telemetry.

> [!WARNING]
> Scattered Spider often attacks the identity layer before ransomware appears. Help desk workflows and MFA reset processes are high-priority control points.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | Scattered Spider / UNC3944 / Octo Tempest |
| **MITRE Group ID** | G1015 |
| **Primary Mission** | Financial gain, data theft, extortion, ransomware enablement |
| **Common Initial Access** | Social engineering, help desk impersonation, phishing, valid account abuse |
| **Common Persistence** | MFA manipulation, remote access tools, identity provider changes |
| **Common Collection Target** | SaaS data, cloud storage, email, identity data, privileged credentials |
| **Recommended Telemetry** | IdP logs, MFA logs, help desk tickets, phone/SMS reset logs, SaaS audit logs, EDR, VPN, remote access tool telemetry |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Reconnaissance | T1589.003 | Gather Victim Identity Information: Employee Names | Collected employee and help desk information to support impersonation. | Monitor external exposure of employee directories and sensitive support process details. |
| Initial Access | T1656 | Impersonation | Impersonated employees, contractors, or help desk staff to persuade targets to reset credentials or MFA. | Review help desk calls, ticket notes, and identity reset requests for social engineering indicators. |
| Initial Access | T1566.004 | Phishing: Spearphishing Voice | Used voice-based social engineering to trick staff or support teams. | Correlate help desk calls with password resets, device enrollments, and high-risk sign-ins. |
| Credential Access | T1621 | Multi-Factor Authentication Request Generation | Generated repeated MFA prompts to pressure users into approval. | Detect MFA fatigue patterns, repeated push attempts, and denied-then-approved sequences. |
| Defense Evasion | T1556.006 | Modify Authentication Process: Multi-Factor Authentication | Modified MFA settings or enrolled attacker-controlled devices. | Alert on MFA method changes, new device enrollments, and security info updates. |
| Initial Access | T1078 | Valid Accounts | Used compromised credentials to access identity, VPN, SaaS, and cloud services. | Detect unfamiliar devices, new geographies, and unusual user-agent patterns. |
| Initial Access | T1133 | External Remote Services | Accessed environments through VPN and other remote access services. | Monitor VPN logins after password or MFA reset events. |
| Command & Control | T1219 | Remote Access Software | Used legitimate remote access tools for persistence and control. | Detect unauthorized AnyDesk, TeamViewer, ScreenConnect, or similar tool installation and use. |
| Credential Access | T1539 | Steal Web Session Cookie | Stole or abused web session material to access SaaS platforms. | Monitor session cookie reuse, impossible travel, and access continuing after credential resets. |
| Discovery | T1087 | Account Discovery | Enumerated accounts, groups, and privileges after access. | Detect excessive directory queries and admin portal enumeration. |
| Collection | T1530 | Data from Cloud Storage | Collected data from cloud storage and SaaS repositories. | Alert on mass downloads, new OAuth apps, and abnormal access to sensitive folders. |
| Impact | T1486 | Data Encrypted for Impact | Enabled or partnered with ransomware operators to encrypt victim environments. | Detect ransomware staging, mass file rename, encryption behavior, and backup targeting. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **MFA Reset Abuse** | Alert on MFA method changes followed by VPN/SaaS login from a new device. | Identity reset abuse is a core Scattered Spider pattern. |
| **Help Desk Social Engineering** | Correlate help desk tickets with password resets, device changes, and privileged access events. | Help desk workflows are often targeted. |
| **Remote Access Tooling** | Detect new or unauthorized remote management software on endpoints and servers. | Legitimate tools can bypass malware-centric controls. |
| **SaaS Mass Download** | Alert on high-volume file downloads from cloud storage or collaboration platforms. | Data theft often occurs before extortion. |
| **Session Anomalies** | Detect session reuse from new locations or impossible travel with the same token. | Cookie/session theft can bypass MFA. |
| **Ransomware Staging** | Detect backup deletion, high-volume file modification, and encryption tooling. | Scattered Spider activity may lead to ransomware impact. |

---

## ❓ Priority Hunt Questions

- Were any MFA methods changed shortly before suspicious SaaS, VPN, or cloud access?
- Are help desk password resets followed by high-risk sign-ins?
- Are users receiving repeated MFA prompts followed by eventual approval?
- Are remote access tools installed on systems that do not normally use them?
- Are SaaS repositories showing mass export or bulk download behavior?
- Did privileged account activity begin after a phone-based or ticket-based identity reset?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Require strong identity verification for help desk resets. | Disrupts impersonation-based account takeover. |
| 2 | Use phishing-resistant MFA and number matching where possible. | Reduces MFA fatigue and push abuse. |
| 3 | Alert on MFA device enrollment and security info changes. | Detects identity-layer persistence. |
| 4 | Restrict remote access tools by policy and application control. | Limits misuse of legitimate administration tools. |
| 5 | Monitor SaaS audit logs for mass download and admin changes. | Surfaces extortion preparation activity. |
| 6 | Train help desk staff on caller impersonation and escalation procedures. | Human process hardening is essential against this actor. |

---

## 📚 Sources

- MITRE ATT&CK — Scattered Spider, Group G1015
- MITRE ATT&CK Campaign C0027 — Scattered Spider-linked telecom and BPO targeting
- Microsoft reporting on Octo Tempest
- Mandiant / Google Cloud reporting on UNC3944

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
