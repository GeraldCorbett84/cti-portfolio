# 🧬 IOC Analysis: 3CX Supply Chain Malicious DLL Hash — ffmpeg.dll SHA256

> This analysis examines the indicator **`7986bbaee8940da11ce089383521ab420c443ab7b15ed42aed91fd31ce833896`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![IOC Type](https://img.shields.io/badge/IOC%20Type-SHA256-blue)
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

**Primary analytic takeaway:** `7986bbaee8940da11ce089383521ab420c443ab7b15ed42aed91fd31ce833896` is most useful for **malicious dll / sideloaded library** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `7986bbaee8940da11ce089383521ab420c443ab7b15ed42aed91fd31ce833896` |
| **IOC Type** | SHA256 |
| **Associated Actor / Malware** | Lazarus-linked / UNC4736-style 3CX supply-chain activity |
| **Campaign / Activity** | 3CX DesktopApp supply-chain compromise |
| **Indicator Category** | Malicious DLL / sideloaded library |
| **Malware / Tooling Family** | Sideloaded DLL / SmoothOperator activity |
| **Recommended Severity** | Critical |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| SHA256 | `7986bbaee8940da11ce089383521ab420c443ab7b15ed42aed91fd31ce833896` | Public reporting on the 3CX compromise identified a malicious `ffmpeg.dll` loaded by the legitimate 3CX DesktopApp. Fortinet reporting listed this SHA256 for the malicious DLL. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Public reporting on the 3CX compromise identified a malicious `ffmpeg.dll` loaded by the legitimate 3CX DesktopApp. Fortinet reporting listed this SHA256 for the malicious DLL.

### Analytic Meaning

- Strong host-based indicator for systems affected by the trojanized 3CX DesktopApp.
- Represents malicious code loaded through DLL sideloading by a trusted signed application.
- Hash detections are high-confidence but narrow; pair with behavior analytics.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Malicious DLL / sideloaded library |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1195.002 - Compromise Software Supply Chain
- T1574.002 - DLL Side-Loading
- T1204 - User Execution
- T1105 - Ingress Tool Transfer

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| File hash search | Search endpoint file inventory for this SHA256 | EDR / asset inventory |
| Application path | Look for `ffmpeg.dll` under 3CX DesktopApp directories | EDR / file inventory |
| Process chain | Detect `3CXDesktopApp.exe` loading unusual DLLs or spawning shells | EDR |
| Network follow-on | Review affected hosts for connections to known 3CX C2 infrastructure | DNS / proxy / firewall |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Immediately isolate hosts where this hash is present.** | Reduces risk and improves investigation quality |
| 2 | **Remove affected 3CX DesktopApp versions and install a clean version from a trusted source.** | Reduces risk and improves investigation quality |
| 3 | **Hunt for browser credential theft and follow-on payloads.** | Reduces risk and improves investigation quality |
| 4 | **Review third-party software update controls.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Fortinet: 3CX Desktop App Compromised (CVE-2023-29059)
- CISA: Supply Chain Attack Against 3CXDesktopApp
- Mandiant: 3CX Software Supply Chain Compromise

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
