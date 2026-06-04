# 🧭 MITRE ATT&CK Mapping: Sandworm / APT44 / Seashell Blizzard

> This report maps **Sandworm / APT44 / Seashell Blizzard** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G0034-blue)

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

Sandworm, also tracked as APT44 and Seashell Blizzard, is a Russian GRU-linked destructive cyber actor tracked by MITRE ATT&CK as G0034. The group is associated with disruptive and destructive operations, including wipers, attacks against Ukraine, energy-sector targeting, and malware such as NotPetya and Olympic Destroyer. This mapping focuses on destructive tradecraft, credential access, lateral movement, policy abuse, and wipe/encryption-style impact.

> [!WARNING]
> Sandworm should be treated as a destructive threat. Detection strategy should include impact-stage behaviors, not only initial access and malware signatures.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | Sandworm / APT44 / Seashell Blizzard |
| **MITRE Group ID** | G0034 |
| **Primary Mission** | Espionage, disruption, destruction, military/political objectives |
| **Common Initial Access** | Public-facing exploitation, phishing, credential abuse |
| **Common Persistence** | Scheduled tasks, services, web shells, compromised accounts |
| **Common Collection Target** | Operational data, credentials, systems supporting critical services |
| **Recommended Telemetry** | EDR, Windows event logs, domain controller logs, GPO changes, backup logs, OT/ICS monitoring, network flow, DNS, proxy |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Resource Development | T1588.002 | Obtain Capabilities: Tool | Acquired or used tooling to support intrusion and destructive operations. | Monitor known offensive tool transfer, execution, and staging activity. |
| Initial Access | T1190 | Exploit Public-Facing Application | Exploited exposed services and vulnerable applications. | Monitor web logs, appliance logs, and exploit indicators against internet-facing systems. |
| Initial Access | T1566.002 | Phishing: Spearphishing Link | Used phishing links or lures in espionage and intrusion operations. | Detect links to newly registered infrastructure and credential capture pages. |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Used PowerShell or scripts for execution and operational tasks. | Detect encoded PowerShell, suspicious script block logging, and remote execution. |
| Persistence | T1053.005 | Scheduled Task / Job: Scheduled Task | Created tasks to execute payloads or maintain access. | Alert on task creation by unusual users or tasks pointing to user-writable paths. |
| Defense Evasion | T1484.001 | Domain Policy Modification: Group Policy Modification | Modified Group Policy to deploy payloads or weaken controls. | Alert on GPO changes, startup script modifications, and policy changes affecting many hosts. |
| Credential Access | T1003 | OS Credential Dumping | Dumped credentials to support lateral movement and privileged access. | Detect LSASS access, credential dumping tools, and suspicious registry hive access. |
| Lateral Movement | T1021 | Remote Services | Used remote services to move between systems. | Monitor RDP, SMB, WinRM, and admin share use from unusual sources. |
| Collection | T1119 | Automated Collection | Collected data or staged information before disruptive actions. | Detect bulk file discovery, archive creation, and staging directories. |
| Defense Evasion | T1685.001 | Disable or Modify Tools: Disable or Modify Windows Event Log | Modified or disabled logging and security tools during operations. | Detect audit policy changes, log service tampering, and gaps in event collection. |
| Impact | T1561.001 | Disk Wipe: Disk Content Wipe | Used wiper malware to destroy disk contents. | Monitor raw disk access, destructive file overwrite patterns, and sudden system failures. |
| Impact | T1485 | Data Destruction | Destroyed data as part of disruptive operations. | Alert on high-volume delete/overwrite operations and destructive commands. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **GPO Abuse** | Alert on GPO changes that add startup scripts, disable security tools, or execute payloads at scale. | Group Policy can rapidly distribute destructive payloads. |
| **Wiper Behavior** | Detect raw disk writes, MBR modifications, and high-volume overwrite activity. | Impact-stage detections are critical against Sandworm. |
| **Credential Dumping** | Monitor LSASS, registry hive access, and suspicious backup tools. | Credential theft enables rapid lateral movement. |
| **Public-Facing Exploitation** | Correlate web exploit attempts with follow-on command execution. | Initial access may begin on exposed services. |
| **Log and Tool Tampering** | Detect changes to event logging, security service status, or endpoint protection settings. | Destructive actors often reduce visibility before impact. |
| **Backup Targeting** | Detect deletion or tampering of backups and recovery tooling. | Resilience depends on protected backups. |

---

## ❓ Priority Hunt Questions

- Were any Group Policy Objects modified to execute scripts or weaken security controls?
- Are there signs of credential dumping before broad lateral movement?
- Are endpoints showing raw disk access, destructive writes, or wipe-like behavior?
- Are security tools or event logs being disabled across multiple hosts?
- Are backup repositories, snapshots, or recovery systems being deleted or modified?
- Did activity begin from a public-facing system followed by internal remote service use?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Protect and monitor domain controllers and GPO administration. | GPO abuse can enable fast enterprise-wide impact. |
| 2 | Maintain immutable, offline, and tested backups. | Critical for recovery from destructive attacks. |
| 3 | Segment IT, OT, and backup networks. | Limits blast radius. |
| 4 | Monitor impact-stage behaviors such as wiping and backup deletion. | Early-stage alerts may be missed. |
| 5 | Restrict administrative lateral movement paths. | Reduces attacker ability to deploy destructive payloads. |
| 6 | Harden internet-facing systems and patch exploited services quickly. | Reduces common initial access paths. |

---

## 📚 Sources

- MITRE ATT&CK — Sandworm Team, Group G0034
- Mandiant reporting on APT44
- Microsoft reporting on Seashell Blizzard
- Public reporting on NotPetya, Olympic Destroyer, and Ukraine-focused destructive operations

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
