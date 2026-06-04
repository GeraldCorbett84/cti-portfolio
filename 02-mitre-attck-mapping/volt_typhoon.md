# 🧭 MITRE ATT&CK Mapping: Volt Typhoon

> This report maps **Volt Typhoon** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G1017-blue)

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

Volt Typhoon is a PRC state-sponsored actor tracked by MITRE ATT&CK as G1017. Public reporting describes Volt Typhoon as targeting U.S. critical infrastructure and territories, with behavior assessed as pre-positioning for potential disruption. This mapping focuses on living-off-the-land activity, valid account abuse, SOHO-router proxying, external remote services, discovery, credential access, and stealthy lateral movement.

> [!WARNING]
> Volt Typhoon commonly emphasizes stealth and living-off-the-land behavior. Absence of malware does not mean absence of compromise.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | Volt Typhoon |
| **MITRE Group ID** | G1017 |
| **Primary Mission** | Espionage, critical infrastructure access, potential pre-positioning |
| **Common Initial Access** | Public-facing appliance exploitation, valid accounts, remote services |
| **Common Persistence** | Valid account abuse, web shells, remote access paths, network appliance access |
| **Common Collection Target** | Credentials, network topology, system information, operationally relevant data |
| **Recommended Telemetry** | EDR, Windows event logs, VPN logs, firewall logs, identity logs, NetFlow, DNS, proxy, appliance logs |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Initial Access | T1190 | Exploit Public-Facing Application | Exploited public-facing appliances and services to gain access. | Monitor edge devices for exploit attempts, unusual web shell behavior, and post-exploitation commands. |
| Initial Access | T1078 | Valid Accounts | Used compromised credentials to access target systems. | Detect logins from unusual devices, IPs, and impossible travel patterns. |
| Initial Access | T1133 | External Remote Services | Used external-facing remote access services to enter victim environments. | Monitor VPN, RDP gateway, and remote management logs for anomalies. |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Used PowerShell and native commands for discovery and post-compromise actions. | Alert on encoded commands, suspicious script block logging, and PowerShell network connections. |
| Execution | T1047 | Windows Management Instrumentation | Used WMI for command execution and remote administration. | Detect WMI process creation from unusual accounts or endpoints. |
| Credential Access | T1003.003 | OS Credential Dumping: NTDS | Accessed domain credential material such as NTDS.dit in some intrusions. | Monitor volume shadow copy creation, NTDS access, and credential dumping alerts. |
| Discovery | T1087 | Account Discovery | Enumerated accounts and privileged users. | Detect excessive account enumeration, domain queries, and directory discovery. |
| Discovery | T1016 | System Network Configuration Discovery | Collected IP configuration, routes, and interface details. | Monitor repeated `ipconfig`, `route print`, `netsh`, and network configuration discovery commands. |
| Discovery | T1049 | System Network Connections Discovery | Listed active connections and network sessions. | Alert on `netstat`, session enumeration, and unusual connection discovery from compromised hosts. |
| Lateral Movement | T1021.001 | Remote Services: RDP | Used RDP with valid credentials for movement between systems. | Alert on unusual RDP source-destination pairs and admin logins outside normal patterns. |
| Command & Control | T1090.003 | Proxy: Multi-hop Proxy | Used compromised SOHO infrastructure to proxy traffic and hide origin. | Detect access from residential or rare proxy infrastructure and unusual long-lived sessions. |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Moved collected data through established channels. | Detect archive creation followed by outbound transfer to rare destinations. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **Living-off-the-Land Chains** | Correlate native tool usage across PowerShell, WMI, net, netstat, ipconfig, and certutil. | Volt Typhoon may avoid obvious malware. |
| **Valid Account Abuse** | Baseline privileged account behavior and alert on new source hosts or abnormal times. | Credential abuse is a key access method. |
| **SOHO Proxy Indicators** | Review connections from residential ISP space or rare VPN/proxy infrastructure. | Proxying can obscure attacker location. |
| **Edge Device Exploitation** | Monitor appliance logs for exploit paths, web shell behavior, or new admin accounts. | Initial access may occur through exposed devices. |
| **NTDS Access** | Detect volume shadow copy and unauthorized access to domain credential stores. | Credential theft enables long-term control. |
| **Remote Service Lateral Movement** | Alert on unusual RDP/WMI activity between non-admin workstations or sensitive servers. | Helps surface stealthy movement. |

---

## ❓ Priority Hunt Questions

- Are valid accounts logging in from new devices, new regions, or residential proxy ranges?
- Are native Windows tools being used in suspicious sequences across multiple systems?
- Are there signs of web shell activity on exposed appliances or servers?
- Has NTDS.dit, SAM, SECURITY, or SYSTEM registry hive data been accessed or copied?
- Are critical infrastructure or OT-adjacent hosts receiving unusual RDP/WMI connections?
- Are sensitive hosts communicating with rare external infrastructure or SOHO-router IP ranges?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Enforce phishing-resistant MFA and conditional access for remote services. | Limits the value of stolen credentials. |
| 2 | Reduce internet exposure of edge appliances and management services. | Shrinks initial access paths. |
| 3 | Centralize and retain appliance, VPN, and firewall logs. | Improves visibility where EDR cannot run. |
| 4 | Monitor living-off-the-land tools with behavior-based detections. | Volt Typhoon may not rely on custom malware. |
| 5 | Segment IT and OT networks and restrict lateral movement paths. | Reduces pre-positioning risk. |
| 6 | Review privileged accounts and rotate credentials after suspected compromise. | Disrupts valid-account persistence. |

---

## 📚 Sources

- MITRE ATT&CK — Volt Typhoon, Group G1017
- CISA Advisory AA24-038A — PRC State-Sponsored Actors Compromise and Maintain Persistent Access to U.S. Critical Infrastructure
- MITRE ATT&CK Campaign C0035 — KV Botnet Activity
- MITRE ATT&CK Campaign C0039 — Versa Director Zero Day Exploitation

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
