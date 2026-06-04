# 🌊 Threat Actor Profile: Sandworm / APT44 / Seashell Blizzard

> **Sandworm**, also tracked as **APT44** and **Seashell Blizzard**, is a Russian GRU-linked threat actor known for destructive malware, wiper attacks, cyber-physical operations, influence activity, and attacks against Ukraine and critical infrastructure.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Primary Motivation](https://img.shields.io/badge/Motivation-Disruption%20%2B%20Espionage-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-G0034-orange)

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

Sandworm is a Russian military intelligence-linked threat group associated with GRU Unit 74455. The group is known for some of the most destructive cyber operations publicly reported, including attacks involving power disruption, wiper malware, destructive payloads, and operations against Ukrainian government, energy, media, and critical infrastructure organizations.

APT44/Sandworm combines espionage, disruptive attacks, destructive malware, and influence operations. The actor has demonstrated capability across enterprise IT, industrial environments, and geopolitical conflict zones.

> [!WARNING]
> Sandworm should be treated as a **critical destructive threat**, especially for energy, government, defense, media, transportation, and critical infrastructure organizations.

---

## 🗂️ At-a-Glance

| Category | Details |
|---|---|
| **Classification** | Nation-State Advanced Persistent Threat |
| **Also Known As** | APT44, Seashell Blizzard, Voodoo Bear, Telebots, ELECTRUM, IRON VIKING, FROZENBARENTS |
| **First Observed** | Publicly reported activity since at least 2009 |
| **Threat Level** | Critical |
| **Primary Motivation** | Disruption, destruction, espionage, military support, influence |
| **Common Targeting** | Ukraine, government, energy, ICS/OT, media, military, transportation, critical infrastructure |
| **MITRE ATT&CK Group ID** | G0034 |

---

## 🧭 Origin & Attribution

Sandworm is publicly linked to Russia's GRU Unit 74455, also known as the Main Centre for Special Technologies. The group has been associated with disruptive and destructive operations aligned with Russian military and geopolitical objectives.

### Attribution Indicators

- Targeting aligned with Russian geopolitical and military objectives
- Destructive malware use during conflict periods
- ICS/OT interest and power-disruption operations
- Public attribution by governments and major security vendors
- Tooling overlap across Ukrainian destructive campaigns

---

## 🎯 Motivation

| Motivation | Description |
|---|---|
| **Disruption** | Interrupting critical services, media, government, and infrastructure operations |
| **Destruction** | Deploying wipers and destructive payloads |
| **Espionage** | Maintaining access to strategic targets |
| **Military Support** | Cyber operations supporting geopolitical and kinetic objectives |
| **Influence / Psychological Impact** | Using cyber effects to signal capability or create instability |

---

## 🏢 Targeted Sectors

| Sector | Examples |
|---|---|
| **Energy / Utilities** | Power distribution, grid operators, OT support networks |
| **Government** | Ukrainian and allied government entities |
| **Media** | News agencies and information operations targets |
| **Military / Defense** | Defense organizations and military-adjacent targets |
| **Transportation** | Strategic logistics and infrastructure |
| **Critical Infrastructure** | ICS/SCADA and operational support systems |

---

## 🧰 Known TTPs: MITRE ATT&CK

| Tactic | Technique ID | Technique Name | Example Procedure |
|---|---:|---|---|
| Initial Access | T1566 | Phishing | Uses phishing for initial access in some operations |
| Initial Access | T1190 | Exploit Public-Facing Application | Exploits vulnerable external systems |
| Persistence | T1543.003 | Create or Modify System Process: Windows Service | Creates services to persist after reboot |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Uses scripting for execution and administration |
| Defense Evasion | T1070 | Indicator Removal | Clears evidence or deletes logs |
| Defense Evasion | T1484.001 | Group Policy Modification | Uses GPO to deploy payloads at scale |
| Impact | T1485 | Data Destruction | Destroys data with wiper malware |
| Impact | T1561.001 | Disk Wipe: Disk Content Wipe | Wipes file or disk content |
| Impact | T1490 | Inhibit System Recovery | Deletes backups or recovery paths |

---

## 🛠️ Signature Tools & Malware

| Tool / Malware | Type | Notes |
|---|---|---|
| **BlackEnergy** | Malware Framework | Associated with earlier Ukraine-focused operations |
| **Industroyer / Industroyer2** | ICS Malware | Designed to impact electric power operations |
| **NotPetya** | Wiper / Destructive Malware | Globally disruptive destructive malware |
| **CaddyWiper** | Wiper | Used in Ukrainian destructive operations |
| **SwiftSlicer** | Wiper | Reported against Ukrainian organizations |
| **NikoWiper / ZeroWipe** | Wipers | Associated with destructive campaigns |
| **BadPilot Tradecraft** | Access Operation | Microsoft-reported long-running access activity |

---

## 🧾 Public IOCs / Pivot Points

| Indicator | Type | Context |
|---|---|---|
| `CDA9310715B7A12F47B7C134260D5FF9200C147FC1D05F030E507E57E3582327` | SHA-256 | Publicly reported wiper-related indicator |
| `FC0E6F2EFFBFA287217B8930AB55B7A77BB86DBD923C0E8150551627138C9CAA` | SHA-256 | Publicly reported wiper-related indicator |
| `195[.]230[.]23[.]19` | IP Address | Publicly reported infrastructure indicator in Sandworm-related reporting |

---

## 🗓️ Notable Campaigns

<details>
<summary><strong>Ukraine Power Grid Operations</strong></summary>

Sandworm has been linked to operations against Ukrainian electric power infrastructure, including activity involving Industroyer-family malware.

</details>

<details>
<summary><strong>NotPetya</strong></summary>

NotPetya caused widespread global disruption and is one of the most damaging destructive cyber incidents publicly attributed to Russian military cyber operations.

</details>

<details>
<summary><strong>BadPilot Campaign</strong></summary>

Microsoft reported a long-running Seashell Blizzard subgroup campaign focused on access operations that could enable follow-on espionage or destructive activity.

</details>

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Segment IT and OT networks | Limits cyber-physical impact |
| 2 | Maintain offline and immutable backups | Reduces wiper and ransomware impact |
| 3 | Monitor GPO changes | Detects large-scale destructive deployment paths |
| 4 | Alert on service creation and suspicious `sc.exe` usage | Detects persistence |
| 5 | Harden remote access and edge systems | Reduces initial access options |
| 6 | Conduct destructive-malware tabletop exercises | Improves response to high-impact events |

---

## 📚 Sources

- MITRE ATT&CK — Sandworm Team / G0034: https://attack.mitre.org/groups/G0034/
- Google/Mandiant — APT44: Unearthing Sandworm: https://cloud.google.com/blog/topics/threat-intelligence/apt44-unearthing-sandworm
- Microsoft — BadPilot campaign: https://www.microsoft.com/en-us/security/blog/2025/02/12/the-badpilot-campaign-seashell-blizzard-subgroup-conducts-multiyear-global-access-operation/
- HHS HC3 — Seashell Blizzard Threat Actor Profile: https://www.hhs.gov/sites/default/files/seashell-blizzard-threat-actor-profile-tlpclear.pdf

---

## ✅ Analyst Notes

Sandworm hunting should prioritize evidence of destructive preparation: backup deletion, GPO changes, service creation, remote deployment, suspicious PowerShell, disk-wiping behavior, and abnormal activity near OT support systems.

---
