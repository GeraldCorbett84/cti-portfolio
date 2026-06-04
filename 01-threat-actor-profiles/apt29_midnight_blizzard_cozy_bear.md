# 🐻 Threat Actor Profile: APT29 / Midnight Blizzard / Cozy Bear

> **APT29**, also known as **Midnight Blizzard** and **Cozy Bear**, is a Russian state-sponsored cyber espionage group associated with intelligence collection, cloud targeting, credential theft, and long-term access operations.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Primary Motivation](https://img.shields.io/badge/Motivation-Espionage-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-G0016-orange)

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

APT29 is a Russian state-sponsored advanced persistent threat group widely associated with Russia's Foreign Intelligence Service, the SVR. The group is known for targeting government, diplomatic, defense, academic, policy, technology, and NGO organizations.

APT29 frequently uses phishing, identity abuse, cloud-focused tradecraft, stealthy persistence, and credential access to maintain long-term access to high-value targets. Recent reporting has highlighted the group's use of malicious RDP configuration files, device-code phishing, cloud tenant abuse, and compromised infrastructure to support intelligence collection.

> [!WARNING]
> APT29 should be treated as a **critical espionage threat** for organizations with sensitive policy, defense, diplomatic, research, cloud, or identity infrastructure.

---

## 🗂️ At-a-Glance

| Category | Details |
|---|---|
| **Classification** | Nation-State Advanced Persistent Threat |
| **Also Known As** | Midnight Blizzard, Cozy Bear, The Dukes, NOBELIUM, UNC2452, Dark Halo, BlueBravo |
| **First Observed** | Publicly reported activity since at least 2008 |
| **Threat Level** | Critical |
| **Primary Motivation** | Espionage, intelligence collection, strategic access |
| **Common Targeting** | Government, diplomacy, defense, academia, NGOs, IT providers, cloud environments |
| **MITRE ATT&CK Group ID** | G0016 |

---

## 🧭 Origin & Attribution

APT29 is attributed by the U.S. and U.K. governments and major security vendors to Russia's Foreign Intelligence Service, the SVR. The group has been publicly linked to long-running intelligence operations against Western governments and strategically important organizations.

### Attribution Indicators

- Alignment with Russian strategic intelligence priorities
- Targeting of diplomatic, government, defense, and policy organizations
- Use of stealthy long-term access operations
- Cloud and identity-focused tradecraft consistent with intelligence collection
- Public attribution by government and private-sector reporting

---

## 🎯 Motivation

| Motivation | Description |
|---|---|
| **Espionage** | Collection against governments, diplomatic entities, policy organizations, and defense targets |
| **Cloud Access** | Targeting Microsoft 365, identity providers, OAuth workflows, and cloud tenants |
| **Credential Theft** | Harvesting credentials, session tokens, and authentication material |
| **Long-Term Persistence** | Maintaining access for future collection and strategic tasking |
| **Infrastructure Abuse** | Use of compromised websites, legitimate services, and cloud infrastructure to blend in |

---

## 🏢 Targeted Sectors

| Sector | Examples |
|---|---|
| **Government / Diplomacy** | Ministries, embassies, foreign policy organizations |
| **Defense / Aerospace** | Contractors, research entities, defense-adjacent organizations |
| **Academia / Think Tanks** | Policy, science, and international relations institutions |
| **NGOs** | Human rights, humanitarian, and geopolitical organizations |
| **Technology / Cloud** | Service providers and cloud-hosted enterprise environments |

---

## 🧰 Known TTPs: MITRE ATT&CK

| Tactic | Technique ID | Technique Name | Example Procedure |
|---|---:|---|---|
| Reconnaissance | T1589 | Gather Victim Identity Information | Collects identity and organizational details before targeted phishing |
| Resource Development | T1583.003 | Acquire Infrastructure: Virtual Private Server | Uses cloud/VPS infrastructure for campaign operations |
| Initial Access | T1566 | Phishing | Sends spear-phishing emails and malicious RDP-themed lures |
| Initial Access | T1133 | External Remote Services | Abuses exposed remote access and identity paths |
| Persistence | T1098.003 | Account Manipulation: Additional Cloud Roles | Abuses cloud permissions to maintain access |
| Defense Evasion | T1078 | Valid Accounts | Uses stolen or abused accounts to blend into normal activity |
| Credential Access | T1110 | Brute Force | Password spraying and credential attacks against identity services |
| Command and Control | T1071.001 | Web Protocols | Uses HTTPS and legitimate services for communications |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Exfiltrates collected intelligence through established channels |

---

## 🛠️ Signature Tools & Malware

| Tool / Malware | Type | Notes |
|---|---|---|
| **WellMess** | Malware / Backdoor | Historically associated with APT29 activity |
| **WellMail** | Malware / Backdoor | Used for command execution and collection |
| **SoreFang** | Malware / Post-compromise tool | Associated with APT29-linked operations |
| **SUNBURST** | Supply chain backdoor | Associated with the SolarWinds compromise cluster |
| **Malicious RDP Files** | Access Vector | Used in spear-phishing campaigns to initiate remote access connections |
| **OAuth / Device Code Abuse** | Identity Abuse | Used to gain access to cloud accounts without traditional malware |

---

## 🧾 Public IOCs / Pivot Points

| Indicator | Type | Context |
|---|---|---|
| `findcloudflare[.]com` | Domain | Reported in APT29 watering-hole/device-code phishing activity |
| `cloudflare[.]redirectpartners[.]com` | Domain | Reported redirect infrastructure used to mimic Cloudflare verification workflows |
| `sellar[.]co[.]uk` | Domain / Compromised Site Context | Publicly reported in APT29 watering-hole infrastructure context |

> [!CAUTION]
> These indicators are provided for research and enrichment. Validate before blocking because some infrastructure may represent compromised legitimate sites rather than actor-owned infrastructure.

---

## 🗓️ Notable Campaigns

<details>
<summary><strong>SolarWinds / SUNBURST</strong></summary>

A major software supply chain compromise that impacted government and enterprise organizations. Activity was associated with APT29/NOBELIUM reporting and demonstrated the actor's ability to compromise trusted software delivery channels.

</details>

<details>
<summary><strong>Cloud and Identity Targeting</strong></summary>

APT29 has increasingly focused on cloud environments, identity systems, OAuth workflows, device-code phishing, and authentication abuse to gain and maintain access.

</details>

<details>
<summary><strong>RDP Spear-Phishing Campaign</strong></summary>

Microsoft reported APT29/Midnight Blizzard using signed RDP configuration files in spear-phishing operations against government, academia, defense, NGOs, and other targets.

</details>

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Enforce phishing-resistant MFA | Reduces effectiveness of credential and device-code phishing |
| 2 | Restrict outbound RDP | Helps prevent malicious RDP file abuse |
| 3 | Monitor OAuth consent grants and new cloud roles | Detects cloud persistence and privilege abuse |
| 4 | Audit service principals and enterprise apps | Identifies suspicious application-based access |
| 5 | Centralize identity logs | Supports detection of impossible travel, token abuse, and suspicious sign-ins |
| 6 | Hunt for unusual RDP file attachments | Detects recent spear-phishing tradecraft |

---

## 📚 Sources

- MITRE ATT&CK — APT29 / G0016: https://attack.mitre.org/groups/G0016/
- CISA AA24-057A — SVR Cyber Actors Adapt Tactics for Initial Cloud Access: https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-057a
- Microsoft — Midnight Blizzard RDP spear-phishing campaign: https://www.microsoft.com/en-us/security/blog/2024/10/29/midnight-blizzard-conducts-large-scale-spear-phishing-campaign-using-rdp-files/
- AWS — Amazon disrupts APT29 watering-hole campaign: https://aws.amazon.com/blogs/security/amazon-disrupts-watering-hole-campaign-by-russias-apt29/

---

## ✅ Analyst Notes

APT29 is best hunted through identity, cloud, and behavioral telemetry. Prioritize suspicious OAuth activity, unusual cloud role assignments, sign-ins from unexpected infrastructure, RDP file handling, and anomalous access to sensitive mailboxes or document repositories.

---
