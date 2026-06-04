# 🧬 IOC Analysis: 3CX Supply Chain C2 Domain — officestoragebox[.]com

> This analysis examines the indicator **`officestoragebox[.]com`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![IOC Type](https://img.shields.io/badge/IOC%20Type-Domain-blue)
![Associated Actor](https://img.shields.io/badge/Actor-Lazarus-linked%20%2F%20UNC4736-style%203CX%20supply-chain%20activity-purple)
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

This IOC is associated with **Lazarus-linked / UNC4736-style 3CX supply-chain activity** activity and is best used as part of a broader investigation rather than as a standalone conclusion. The indicator should be searched across historical telemetry, correlated with endpoint behavior, and enriched before being used for long-term blocking or attribution.

**Primary analytic takeaway:** `officestoragebox[.]com` is most useful for **c2 / office-themed impersonation domain** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `officestoragebox[.]com` |
| **IOC Type** | Domain |
| **Associated Actor / Malware** | Lazarus-linked / UNC4736-style 3CX supply-chain activity |
| **Campaign / Activity** | 3CX DesktopApp supply-chain compromise |
| **Indicator Category** | C2 / Office-themed impersonation domain |
| **Malware / Tooling Family** | Supply-chain C2 infrastructure |
| **Recommended Severity** | Critical |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| Domain | `officestoragebox[.]com` | This Office-themed domain appears in public 3CX IOC reporting and is useful for identifying endpoints that may have communicated with actor-controlled infrastructure after running the trojanized application. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

This Office-themed domain appears in public 3CX IOC reporting and is useful for identifying endpoints that may have communicated with actor-controlled infrastructure after running the trojanized application.

### Analytic Meaning

- The name imitates normal Microsoft/Office storage language.
- Connections from 3CX hosts should be treated as suspicious.
- Domain-based detection is useful for scoping but may be stale if infrastructure has changed.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | C2 / Office-themed impersonation domain |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1071.001 - Web Protocols
- T1102 - Web Service
- T1195.002 - Compromise Software Supply Chain

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| DNS search | Queries for `officestoragebox.com` | DNS |
| Proxy search | HTTP/S requests to this domain | Proxy |
| Process linkage | Network connection from `3CXDesktopApp.exe` or child process | EDR |
| Timeline | Compare first connection to 3CX installation/update time | EDR / asset inventory |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block and sinkhole the domain where possible.** | Reduces risk and improves investigation quality |
| 2 | **Investigate any endpoint connection as potential compromise.** | Reduces risk and improves investigation quality |
| 3 | **Review parent-child process activity from 3CXDesktopApp.** | Reduces risk and improves investigation quality |
| 4 | **Update/remediate affected 3CX software.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- User-provided 3CX IOC analysis template
- CISA: Supply Chain Attack Against 3CXDesktopApp
- Volexity 3CX reporting

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
