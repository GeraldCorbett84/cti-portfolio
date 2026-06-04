# 🧭 MITRE ATT&CK Mapping: Salt Typhoon

> This report maps **Salt Typhoon** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G1045-blue)

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

Salt Typhoon is a PRC state-backed actor tracked by MITRE ATT&CK as G1045. Public reporting describes the actor as focused on telecommunications, ISP, and network infrastructure compromise. This mapping emphasizes behaviors that defenders can observe in network device telemetry, configuration changes, SSH activity, packet capture artifacts, credential abuse, and exfiltration of device configurations.

> [!WARNING]
> Salt Typhoon activity may occur on routers, switches, and other network devices where EDR visibility is limited. Network device logging and configuration baselining are critical.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | Salt Typhoon |
| **MITRE Group ID** | G1045 |
| **Primary Mission** | Espionage, telecommunications access, network infrastructure compromise |
| **Common Initial Access** | Exploitation of public-facing network infrastructure and edge devices |
| **Common Persistence** | Local account creation, SSH key modification, network device configuration changes |
| **Common Collection Target** | Packet captures, network device configs, routing topology, telecom metadata |
| **Recommended Telemetry** | AAA/TACACS/RADIUS logs, router/switch configs, NetFlow, SSH logs, packet capture locations, DNS, syslog, configuration backups |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Reconnaissance | T1590.004 | Gather Victim Network Information: Network Topology | Used configuration files from compromised network devices to identify upstream and downstream network segments. | Compare device configs over time and alert on unexpected reads or exports of topology-revealing configuration. |
| Resource Development | T1588.002 | Obtain Capabilities: Tool | Used publicly available tooling to exploit vulnerabilities. | Monitor exploit tooling, unusual binaries, and scanning artifacts on management systems. |
| Initial Access | T1190 | Exploit Public-Facing Application | Exploited internet-facing network infrastructure and management services. | Prioritize patching and monitor exploit attempts against edge appliances and management interfaces. |
| Persistence | T1136 | Create Account | Created Linux-level users on compromised network devices through modification of account files. | Baseline local accounts and alert on unauthorized changes to `/etc/passwd`, `/etc/shadow`, and device-local users. |
| Persistence | T1098.004 | Account Manipulation: SSH Authorized Keys | Modified SSH authorized keys to maintain access. | Monitor SSH key changes, new authorized keys, and unusual key-based access. |
| Lateral Movement | T1021.004 | Remote Services: SSH | Used modified loopback addresses and SSH connections to access additional devices. | Detect SSH from unusual source interfaces, unexpected device-to-device SSH, and ACL bypass patterns. |
| Collection | T1602.002 | Data from Configuration Repository: Network Device Configuration Dump | Collected network device configuration files. | Alert on config export commands, bulk config reads, and off-cycle backup pulls. |
| Collection | T1040 | Network Sniffing | Used packet capture tooling such as JumbledPath to collect traffic from network devices. | Hunt for pcap creation, tcpdump-like processes, and unusual capture files on network appliances. |
| Discovery | T1049 | System Network Connections Discovery | Enumerated network connections and traffic paths. | Monitor commands or device outputs showing connection tables, routing, and session enumeration. |
| Discovery | T1016 | System Network Configuration Discovery | Collected interface, route, and network configuration details. | Alert on repeated interface/routing table collection from unusual admin sessions. |
| Defense Evasion | T1685.006 | Disable or Modify Tools: Clear Linux or Mac System Logs | Cleared logs such as `.bash_history`, `auth.log`, `lastlog`, `wtmp`, and `btmp`. | Detect log truncation, missing audit events, or gaps in syslog forwarding. |
| Exfiltration | T1048.003 | Exfiltration Over Unencrypted Non-C2 Protocol | Exfiltrated configuration files and collected data using non-C2 transfer paths. | Monitor outbound transfers from management networks and unexpected file movement from network devices. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **Network Device Config Drift** | Compare running configs against known-good baselines and alert on unauthorized changes. | Persistence and discovery may occur through config manipulation. |
| **Unexpected SSH Paths** | Alert when network devices initiate SSH to other devices or management systems unexpectedly. | Device-to-device SSH may indicate lateral movement. |
| **Packet Capture Artifacts** | Hunt for `.pcap` files, tcpdump usage, or suspicious capture binaries. | Salt Typhoon has been associated with packet capture behavior. |
| **Local Account Changes** | Detect new local users or changes to shadow/passwd-style files on network devices. | Unauthorized local users can provide persistence. |
| **Log Clearing** | Monitor syslog continuity and local log integrity. | Log deletion is a key defense evasion signal. |
| **Management Plane Anomalies** | Watch for admin logins from unusual IPs, odd hours, or unexpected interfaces. | Network device management channels are high-value. |

---

## ❓ Priority Hunt Questions

- Have any routers, switches, firewalls, or SD-WAN devices had unauthorized configuration changes?
- Are devices initiating SSH connections to other devices in a way that violates normal management patterns?
- Are there new local accounts or SSH authorized keys on network infrastructure?
- Are packet capture files appearing on appliances or management servers?
- Do syslog records show gaps, truncation, or unexplained loss of authentication logs?
- Are configuration backups being pulled outside approved windows?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Restrict management plane access with ACLs, MFA, and dedicated admin jump hosts. | Reduces direct attacker access to network devices. |
| 2 | Implement configuration baselining and alert on drift. | Identifies stealthy persistence and topology collection. |
| 3 | Centralize network device logs and protect them from local deletion. | Preserves evidence if local logs are cleared. |
| 4 | Rotate device credentials and audit SSH keys. | Reduces persistence through account or key manipulation. |
| 5 | Patch edge and management appliances quickly. | Salt Typhoon-style access often depends on exposed infrastructure. |
| 6 | Monitor for packet capture behavior on network devices. | Packet capture can expose credentials, metadata, and sensitive traffic. |

---

## 📚 Sources

- MITRE ATT&CK — Salt Typhoon, Group G1045
- CISA Advisory AA25-239A — Countering Chinese State-Sponsored Actors Compromise of Networks
- MITRE ATT&CK — JumbledPath, Software S1206
- Public telecom and network infrastructure intrusion reporting

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
