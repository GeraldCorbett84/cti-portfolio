# 🐉 Threat Actor Profile: Salt Typhoon

> **Salt Typhoon** is a PRC state-backed cyber espionage actor associated with long-term compromises of telecommunications, internet service provider, network infrastructure, and government-adjacent targets.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Primary Motivation](https://img.shields.io/badge/Motivation-Espionage-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-G1045-orange)

---

> GitHub-ready threat actor profile modeled after the uploaded Lazarus Group profile.  
> TLP:CLEAR | For defensive research, threat hunting, and CTI portfolio use.

---

## ⚠️ Analyst Notice

Indicators of Compromise are short-lived and should **not** be used as the sole basis for attribution or blocking decisions. Validate all IOCs against current enrichment, passive DNS, malware repositories, internal telemetry, and trusted threat intelligence feeds before deploying detections or blocking controls.

> [!NOTE]
> TTPs generally provide longer-lasting defensive value than raw IOCs. Use IOC tables as enrichment and pivot points, not as the final intelligence product.

---


## 🧠 Executive Summary

Salt Typhoon is a People's Republic of China state-backed threat actor known for targeting telecommunications and network infrastructure. Public reporting describes the actor as focused on long-term access, network device compromise, traffic collection, credential access, and intelligence collection.

Unlike malware-heavy actors, Salt Typhoon activity is often centered around routers, edge devices, network administration systems, packet capture, configuration theft, and abuse of trusted management protocols.

> [!WARNING]
> Salt Typhoon should be considered a **critical threat** to telecommunications providers, ISPs, managed service providers, government networks, and organizations with internet-facing network infrastructure.

---

## 🗂️ At-a-Glance

| Category | Details |
|---|---|
| **Classification** | Nation-State Advanced Persistent Threat |
| **Also Known As** | RedMike, OPERATOR PANDA, GhostEmperor, UNC5807 in overlapping public reporting |
| **First Observed** | Publicly reported as active since at least 2019 |
| **Threat Level** | Critical |
| **Primary Motivation** | Espionage, communications collection, strategic access |
| **Common Targeting** | Telecom, ISPs, network infrastructure, government, transportation, lodging, military-adjacent infrastructure |
| **MITRE ATT&CK Group ID** | G1045 |

---

## 🧭 Origin & Attribution

Salt Typhoon is publicly described by MITRE and government advisories as a PRC state-backed actor. The actor's targeting of telecommunications and network infrastructure aligns with intelligence collection objectives and strategic communications monitoring.

### Attribution Indicators

- Long-term compromises of telecom and ISP infrastructure
- Focus on network devices, routers, and communication paths
- Use of network configuration theft and packet capture
- Abuse of legitimate administration protocols
- Overlap with PRC-linked espionage reporting

---

## 🎯 Motivation

| Motivation | Description |
|---|---|
| **Communications Intelligence** | Access to telecom and ISP networks can enable collection against communications metadata and content |
| **Strategic Espionage** | Targeting supports state intelligence requirements |
| **Network Persistence** | Maintaining access to routers, edge devices, and management systems |
| **Credential Collection** | Collecting credentials and configuration data to expand access |
| **Traffic Monitoring** | Capturing packets, session data, and network flows |

---

## 🏢 Targeted Sectors

| Sector | Examples |
|---|---|
| **Telecommunications** | Carriers, backbone providers, telecom infrastructure |
| **Internet Service Providers** | Regional and national ISPs |
| **Government** | Agencies and government-adjacent infrastructure |
| **Transportation / Lodging** | Sectors referenced in public PRC espionage reporting |
| **Military-Adjacent Infrastructure** | Networks supporting strategic mobility or defense communications |

---

## 🧰 Known TTPs: MITRE ATT&CK

| Tactic | Technique ID | Technique Name | Example Procedure |
|---|---:|---|---|
| Initial Access | T1190 | Exploit Public-Facing Application | Exploits unpatched edge devices and internet-facing systems |
| Discovery | T1046 | Network Service Discovery | Enumerates network services and reachable devices |
| Discovery | T1602.002 | Network Device Configuration Dump | Collects router and network device configurations |
| Credential Access | T1110.002 | Password Cracking | Attempts to recover credentials from collected material |
| Lateral Movement | T1021.004 | Remote Services: SSH | Uses SSH for network device and server access |
| Collection | T1040 | Network Sniffing | Captures traffic from compromised network positions |
| Collection | T1005 | Data from Local System | Collects files, configs, and packet captures |
| Exfiltration | T1048.003 | Exfiltration Over Unencrypted Non-C2 Protocol | Moves captured data over standard network protocols |
| Command and Control | T1090 | Proxy | Uses proxy paths to hide origin and maintain access |

---

## 🛠️ Signature Tools & Malware

| Tool / Malware | Type | Notes |
|---|---|---|
| **Packet Capture Utilities** | Collection Tooling | Used to collect traffic from network infrastructure |
| **SSH / Network Admin Tools** | Legitimate Tool Abuse | Used for access, persistence, and lateral movement |
| **Router Configuration Access** | Network Device Abuse | Used to steal configurations and understand topology |
| **TACACS+ / SNMP Abuse** | Network Management Abuse | May support credential or configuration discovery |
| **SparrowDoor / ShadowPad** | Backdoor Families | Public reporting has discussed overlap with China-nexus espionage tooling |

---

## 🧾 Public IOCs / Pivot Points

| Indicator | Type | Context |
|---|---|---|
| `mycap.pcap` | File Artifact | Packet capture artifact naming observed in public reporting |
| `tac.pcap` | File Artifact | Packet capture artifact naming observed in public reporting |
| `1.pcap` | File Artifact | Packet capture artifact naming observed in public reporting |

> [!NOTE]
> For Salt Typhoon, high-fidelity detection usually comes from network device telemetry, config-change auditing, AAA logs, packet capture artifacts, and unusual management-plane access rather than static malware hashes.

---

## 🗓️ Notable Campaigns

<details>
<summary><strong>Telecommunications and ISP Intrusions</strong></summary>

Salt Typhoon has been publicly associated with compromises of major telecommunications and internet service provider infrastructure, with activity focused on communications intelligence and network infrastructure access.

</details>

<details>
<summary><strong>Network Device Compromise</strong></summary>

Public advisories describe PRC-linked actors abusing network devices, management systems, and edge infrastructure to gain durable access and collect data.

</details>

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Inventory and patch internet-facing network devices | Edge devices are a primary access path |
| 2 | Centralize router, firewall, VPN, and AAA logs | Enables visibility into management-plane abuse |
| 3 | Rotate device credentials and audit local accounts | Limits persistence after config theft |
| 4 | Monitor for unexpected packet capture files | Detects traffic collection activity |
| 5 | Validate SNMP, TACACS+, and SSH exposure | Reduces management-plane attack surface |
| 6 | Compare running configs against known-good baselines | Finds unauthorized ACL, tunnel, and route changes |

---

## 📚 Sources

- MITRE ATT&CK — Salt Typhoon / G1045: https://attack.mitre.org/groups/G1045/
- CISA AA25-239A — Countering Chinese State-Sponsored Actors Compromise of Networks Worldwide: https://www.cisa.gov/news-events/cybersecurity-advisories/aa25-239a
- Picus analysis of CISA AA25-239A: https://www.picussecurity.com/resource/blog/cisa-alert-aa25-239a-analysis-simulation-and-mitigation-of-chinese-apts

---

## ✅ Analyst Notes

For Salt Typhoon, prioritize telemetry from routers, firewalls, VPN appliances, TACACS+/RADIUS, SNMP, NetFlow, and packet capture locations. Traditional endpoint-only detection will miss much of the activity.

---
