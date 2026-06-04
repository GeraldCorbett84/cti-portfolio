# 🌩️ Threat Actor Profile: Volt Typhoon

> **Volt Typhoon** is a PRC state-sponsored actor known for stealthy living-off-the-land activity, long-term persistence, and pre-positioning inside U.S. critical infrastructure networks.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Primary Motivation](https://img.shields.io/badge/Motivation-Strategic%20Access-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-G1017-orange)

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

Volt Typhoon is a PRC state-sponsored threat actor publicly associated with targeting critical infrastructure, especially in the United States and Guam. Public government reporting assesses that the actor seeks to pre-position inside IT networks in ways that could enable disruption of operational technology or critical services during a geopolitical crisis.

Volt Typhoon is notable for heavy use of living-off-the-land techniques, valid accounts, compromised SOHO routers, and minimal malware. This approach allows the actor to blend into normal administrative traffic and remain undetected for long periods.

> [!WARNING]
> Volt Typhoon should be treated as a **critical national-security threat** for communications, energy, transportation, water, government, and other critical infrastructure organizations.

---

## 🗂️ At-a-Glance

| Category | Details |
|---|---|
| **Classification** | Nation-State Advanced Persistent Threat |
| **Also Known As** | Vanguard Panda, Bronze Silhouette, Insidious Taurus, Dev-0391, Storm-0391, UNC3236, Voltzite |
| **First Observed** | Active since at least 2021 |
| **Threat Level** | Critical |
| **Primary Motivation** | Strategic access, espionage, potential disruption preparation |
| **Common Targeting** | Communications, energy, transportation, water/wastewater, government, critical infrastructure |
| **MITRE ATT&CK Group ID** | G1017 |

---

## 🧭 Origin & Attribution

Volt Typhoon has been publicly attributed by U.S. and allied government agencies to PRC state-sponsored cyber activity. Its focus on critical infrastructure and long-term persistence suggests strategic objectives beyond ordinary espionage.

### Attribution Indicators

- Targeting of U.S. critical infrastructure and Guam
- Use of compromised SOHO routers and proxy infrastructure
- Living-off-the-land activity with minimal custom malware
- Valid account abuse and long-term persistence
- Public attribution by CISA, NSA, FBI, Microsoft, and allied agencies

---

## 🎯 Motivation

| Motivation | Description |
|---|---|
| **Strategic Pre-Positioning** | Maintaining access that could support future disruption |
| **Espionage** | Collecting information about infrastructure and operations |
| **Credential Access** | Obtaining credentials to maintain stealthy access |
| **Operational Resilience** | Using legitimate tools and proxies to reduce detection |
| **Critical Infrastructure Access** | Establishing footholds in sectors that support national security and daily life |

---

## 🏢 Targeted Sectors

| Sector | Examples |
|---|---|
| **Communications** | Telecom and network service providers |
| **Energy** | Electric utilities and supporting IT networks |
| **Transportation** | Logistics, transportation systems, and support networks |
| **Water / Wastewater** | Public utility infrastructure |
| **Government** | Government agencies and supporting service providers |
| **Critical Infrastructure** | Organizations with OT/ICS dependencies |

---

## 🧰 Known TTPs: MITRE ATT&CK

| Tactic | Technique ID | Technique Name | Example Procedure |
|---|---:|---|---|
| Initial Access | T1190 | Exploit Public-Facing Application | Exploits vulnerable edge devices and internet-facing systems |
| Resource Development | T1090.003 | Proxy: Multi-hop Proxy | Uses compromised SOHO routers and proxy chains |
| Defense Evasion | T1078 | Valid Accounts | Uses stolen credentials to blend into normal access |
| Credential Access | T1003.003 | OS Credential Dumping: NTDS | Attempts to collect domain credential material |
| Discovery | T1087 | Account Discovery | Enumerates users and privileged accounts |
| Discovery | T1016 | System Network Configuration Discovery | Maps network configuration and routes |
| Lateral Movement | T1021.001 | Remote Services: RDP | Uses remote access paths for lateral movement |
| Collection | T1560 | Archive Collected Data | Archives data before exfiltration |
| Exfiltration | T1048 | Exfiltration Over Alternative Protocol | Moves collected data through network protocols |

---

## 🛠️ Signature Tools & Malware

| Tool / Malware | Type | Notes |
|---|---|---|
| **PowerShell** | LOLBin | Used for command execution and discovery |
| **WMI** | LOLBin | Used for remote execution and discovery |
| **net.exe / nltest.exe** | Windows Admin Tools | Used for account and domain discovery |
| **wevtutil** | Log Utility | May be used to clear or inspect logs |
| **Ntdsutil / vssadmin** | Credential Access Support | May support collection of directory data |
| **Compromised SOHO Routers** | Infrastructure | Used to proxy traffic and obscure origin |

---

## 🧾 Public IOCs / Pivot Points

| Indicator | Type | Context |
|---|---|---|
| `104[.]161[.]54[.]203` | IP Address | Publicly reported Volt Typhoon infrastructure indicator |
| `109[.]166[.]39[.]139` | IP Address | Publicly reported Volt Typhoon infrastructure indicator |
| `23[.]227[.]198[.]247` | IP Address | Publicly reported Volt Typhoon infrastructure indicator |

---

## 🗓️ Notable Campaigns

<details>
<summary><strong>U.S. Critical Infrastructure Pre-Positioning</strong></summary>

CISA and partner agencies reported that Volt Typhoon compromised IT environments of multiple U.S. critical infrastructure organizations with concern that access could support disruptive effects during a future crisis.

</details>

<details>
<summary><strong>Living-off-the-Land Critical Infrastructure Activity</strong></summary>

Microsoft reported Volt Typhoon activity focused on stealthy post-compromise credential access and network discovery against critical infrastructure organizations.

</details>

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Patch and replace unsupported edge devices | Volt Typhoon frequently abuses edge infrastructure |
| 2 | Implement phishing-resistant MFA | Reduces valid-account abuse |
| 3 | Hunt for LOTL command patterns | Detects stealthy hands-on-keyboard activity |
| 4 | Centralize Windows, VPN, identity, and network logs | Prevents log loss and supports timeline reconstruction |
| 5 | Monitor abnormal RDP, SMB, and WMI use | Detects lateral movement |
| 6 | Segment IT from OT | Reduces risk of IT footholds impacting operations |

---

## 📚 Sources

- MITRE ATT&CK — Volt Typhoon / G1017: https://attack.mitre.org/groups/G1017/
- CISA AA24-038A — PRC State-Sponsored Actors Compromise and Maintain Persistent Access to U.S. Critical Infrastructure: https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-038a
- Microsoft — Volt Typhoon targets U.S. critical infrastructure: https://www.microsoft.com/en-us/security/blog/2023/05/24/volt-typhoon-targets-us-critical-infrastructure-with-living-off-the-land-techniques/

---

## ✅ Analyst Notes

Volt Typhoon hunting should focus on abnormal administrative behavior, not just malware alerts. Prioritize identity logs, VPN logs, remote access patterns, Windows event telemetry, domain controller activity, and evidence of staged credential or archive files.

---
