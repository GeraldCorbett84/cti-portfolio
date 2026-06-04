# 🧠 Threat Intelligence Report: Salt Typhoon Telecom and Network Infrastructure Intrusions

> This report converts public reporting on **Salt Typhoon** into a defender-focused intelligence product for SOC triage, threat hunting, detection engineering, and leadership awareness.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Report Type](https://img.shields.io/badge/Report-Nation%20State%20Espionage-blue)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![TLP](https://img.shields.io/badge/TLP-CLEAR-lightgrey)

---

## 📌 Table of Contents

- [Executive Summary](#-executive-summary)
- [Key Judgments](#-key-judgments)
- [Threat Actor / Campaign Overview](#-threat-actor--campaign-overview)
- [Targeting & Victimology](#-targeting--victimology)
- [Attack Lifecycle Assessment](#-attack-lifecycle-assessment)
- [MITRE ATT&CK Highlights](#-mitre-attck-highlights)
- [Detection & Hunting Opportunities](#-detection--hunting-opportunities)
- [Business Impact Assessment](#-business-impact-assessment)
- [Defensive Recommendations](#-defensive-recommendations)
- [Intelligence Gaps](#-intelligence-gaps)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧠 Executive Summary

Salt Typhoon is a PRC state-backed actor associated with compromises of major telecommunications and internet service provider infrastructure. Public advisory reporting emphasizes long-term covert access to network devices and sensitive communications infrastructure. This is a strategic threat because compromise of telecom infrastructure can provide visibility into communications, network topology, call metadata, customer traffic paths, and downstream victim environments.

> [!WARNING]
> This report is intended for defensive use. Validate all indicators and mappings against current reporting before deploying detections or making attribution decisions.

---

## 🔑 Key Judgments

- **Salt Typhoon risk is highest where organizations operate routers, edge devices, telecom infrastructure, or large-scale network management systems.**
- **Network device telemetry is often weaker than endpoint telemetry, allowing long dwell time if logs are incomplete or overwritten.**
- **Configuration theft is strategically important because it reveals routing, trust relationships, credentials, and network topology.**
- **Defenders should prioritize router, firewall, VPN, and management-plane logging instead of relying only on endpoint alerts.**

---

## 🗂️ Threat Actor / Campaign Overview

| Field | Details |
|---|---|
| **Primary Actor / Campaign** | Salt Typhoon |
| **Also Known As** | OPERATOR PANDA, RedMike, UNC5807, GhostEmperor-overlap reporting, PRC state-backed activity |
| **Primary Mission** | Long-term espionage through compromise of telecommunications, ISP, and network infrastructure |
| **Threat Level** | Critical |
| **Report Type** | Nation State Espionage |
| **TLP** | CLEAR |
| **Recommended Audience** | SOC, CTI, Detection Engineering, Incident Response, Security Leadership |

---

## 🎯 Targeting & Victimology

Likely or reported targeting includes:

- Telecommunications providers
- Internet service providers
- Backbone network infrastructure
- Managed network service providers
- Government-linked communications networks
- Critical infrastructure organizations dependent on telecom providers

**Analytic significance:** Targeting should be used to prioritize hunting and exposure review. Organizations in adjacent sectors, supply chains, managed service relationships, or shared technology ecosystems may also face indirect risk.

---

## 🔁 Attack Lifecycle Assessment

- **Reconnaissance:** Identifies exposed network devices, telecom infrastructure, and exploitable services.
- **Initial Access:** Exploits vulnerabilities or weak device configurations in internet-facing infrastructure.
- **Persistence:** Creates or modifies accounts, SSH keys, device configuration, or Linux-level users on compromised devices.
- **Discovery:** Collects configuration files to map upstream and downstream network segments.
- **Collection:** Captures traffic, packet data, configuration files, credentials, and sensitive network information.
- **Exfiltration:** Transfers collected files over protocols such as FTP or TFTP, sometimes from devices with limited monitoring.

### Operational Flow

| Phase | Observed / Assessed Behavior | Useful Telemetry | Why It Matters |
|---|---|---|---|
| Initial Access | Exploit public-facing network devices or services | Device logs, vulnerability scans, edge telemetry | Edge devices are high-value and frequently under-monitored. |
| Persistence | Modify accounts, SSH keys, or local Linux user files | AAA logs, config diffs, file integrity monitoring | Device-level persistence can survive standard endpoint response. |
| Discovery | Dump device configuration and topology data | Config archive logs, device command history | Configuration files expose network architecture and credentials. |
| Collection | Packet capture on remote network devices | Device process lists, file system artifacts, NetFlow | Packet capture can reveal sensitive traffic and communications metadata. |
| Exfiltration | Move configs or packet captures using FTP/TFTP | Firewall logs, device logs, NetFlow | Unencrypted transfer of sensitive files is huntable. |

---

## 🧭 MITRE ATT&CK Highlights

| Technique ID | Technique Name | Report Context |
|---:|---|---|
| T1190 | Exploit Public-Facing Application | Compromise of exposed infrastructure and services. |
| T1098.004 | SSH Authorized Keys | Potential persistence through authorized key modification. |
| T1136 | Create Account | Creation of Linux-level users on compromised devices. |
| T1602.002 | Network Device Configuration Dump | Collection of router or network-device configuration. |
| T1040 | Network Sniffing | Packet capture or traffic collection on devices. |
| T1048.003 | Exfiltration Over Unencrypted Non-C2 Protocol | Use of FTP/TFTP-style transfer for collected files. |

> [!NOTE]
> ATT&CK mappings are meant to support detection design and hunt planning. They should not be treated as a complete record of every possible technique used by this actor.

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | Example Hunt Logic | Primary Data Sources |
|---|---|---|
| Network Device Config Diffs | Compare running config against last known-good baseline for new users, ACL changes, SSH keys, or loopback changes. | Router/firewall configs, network management platforms |
| Unexpected Packet Capture | Search for packet capture tools, `.pcap` files, or unusual long-running capture processes on network devices. | Device shell logs, file system review |
| FTP/TFTP Exfiltration | Identify network devices transferring files externally or to unusual internal staging hosts. | Firewall, NetFlow, device logs |
| Weak Account Encryption | Review device configurations for weak password encryption or reversible secrets. | Configuration management |
| Log Clearing | Look for missing auth logs, cleared shell history, or gaps in device logging. | Syslog, SIEM, device audit logs |

---

## 💼 Business Impact Assessment

| Impact Area | Assessment |
|---|---|
| **Confidentiality** | Potential exposure of sensitive data, credentials, internal communications, customer information, or strategic business information. |
| **Integrity** | Possible manipulation of access paths, identity systems, network devices, or trusted software workflows depending on intrusion path. |
| **Availability** | Ranges from limited for espionage-focused activity to severe for ransomware or destructive campaigns. |
| **Operational Risk** | Requires cross-functional response involving SOC, IAM, network, endpoint, legal, communications, and business owners. |
| **Executive Concern** | Actor activity may create regulatory, reputational, national-security, or business-continuity impact depending on sector. |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Centralize and retain network-device logs | Improves visibility into devices attackers prefer. |
| 2 | Enforce AAA, MFA where supported, and least privilege for device administration | Reduces misuse of admin paths. |
| 3 | Continuously diff device configurations | Detects unauthorized persistence and access-control changes. |
| 4 | Disable insecure management protocols and restrict management planes | Reduces exploit and credential-theft exposure. |
| 5 | Hunt for packet capture artifacts on routers and appliances | Directly addresses Salt Typhoon-style collection behavior. |
| 6 | Rotate credentials stored in device configurations | Configuration theft can expose reusable secrets. |

---

## ❓ Intelligence Gaps

- Which network devices have insufficient logging or no centralized syslog?
- Are any devices running unsupported firmware or exposed management services?
- Were configuration files accessed, copied, or transferred outside standard workflows?
- Do router configs contain weakly encrypted or reusable credentials?
- Can packet capture activity be detected on edge and core devices?

---

## 📚 Sources

- CISA Joint Advisory AA25-239A — Countering Chinese State-Sponsored Actors Compromise of Networks Worldwide: https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-239a
- MITRE ATT&CK — Salt Typhoon / G1045: https://attack.mitre.org/groups/G1045/
- MITRE ATT&CK — JumbledPath / S1206: https://attack.mitre.org/software/S1206/

---

## ✅ Analyst Notes

This report intentionally emphasizes network-device telemetry because telecom-focused intrusions often bypass endpoint-centric security programs. The portfolio value is showing that CTI can drive infrastructure-specific hunt planning.
