# 🧬 IOC Analysis: RansomHub C2 IP — 45[.]95[.]67[.]41

> This analysis examines the indicator **`45[.]95[.]67[.]41`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-IPv4-blue)
![Associated Actor](https://img.shields.io/badge/Actor-RansomHub%20Ransomware-purple)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Executive Summary](#-executive-summary)
- [Case Context](#-case-context)
- [IOC Summary](#-ioc-summary)
- [IOC Deep Dive](#-ioc-deep-dive)
- [Analytic Judgment](#-analytic-judgment)
- [MITRE ATT&CK Alignment](#-mitre-attck-alignment)
- [Detection & Hunting Opportunities](#-detection--hunting-opportunities)
- [Defensive Recommendations](#-defensive-recommendations)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧠 Executive Summary

This IOC is associated with **RansomHub Ransomware** activity and is best used as part of a broader investigation rather than as a standalone conclusion. The indicator should be searched across historical telemetry, correlated with endpoint behavior, and enriched before being used for long-term blocking or attribution.

**Primary analytic takeaway:** `45[.]95[.]67[.]41` is most useful for **ransomware c2 / staging infrastructure** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `45[.]95[.]67[.]41` |
| **IOC Type** | IPv4 |
| **Associated Actor / Malware** | RansomHub Ransomware |
| **Campaign / Activity** | RansomHub ransomware activity |
| **Indicator Category** | Ransomware C2 / staging infrastructure |
| **Malware / Tooling Family** | RansomHub RaaS |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| IPv4 | `45[.]95[.]67[.]41` | This IP is present in multiple public RansomHub IOC references and should be used for network hunting and ransomware-stage scoping. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

This IP is present in multiple public RansomHub IOC references and should be used for network hunting and ransomware-stage scoping.

### Analytic Meaning

- Network contact may indicate interaction with suspicious ransomware-related infrastructure.
- Because IPs age quickly, review timestamps and passive DNS before making attribution claims.
- Pair IP hits with endpoint evidence such as tools, scripts, credential dumping, or encryption activity.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Ransomware C2 / staging infrastructure |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1071 - Application Layer Protocol
- T1003 - OS Credential Dumping
- T1021.001 - Remote Services: RDP
- T1486 - Data Encrypted for Impact

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| Firewall/proxy | Connections to `45.95.67.41` | Firewall / proxy |
| EDR process | Process and command line behind the connection | EDR |
| Credential access | LSASS access, credential dumping tools, or unusual authentication failures | EDR / Windows logs |
| Lateral movement | RDP/SMB/WMI activity following the connection | SIEM |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block and alert on the IP.** | Reduces risk and improves investigation quality |
| 2 | **Triage hosts with any observed connection.** | Reduces risk and improves investigation quality |
| 3 | **Force password reset for accounts used on impacted systems if compromise is suspected.** | Reduces risk and improves investigation quality |
| 4 | **Review backup and EDR tamper events.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- IC3/CISA/MS-ISAC/HHS: #StopRansomware RansomHub advisory
- Public RansomHub IOC reporting

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
