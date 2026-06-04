# 🧬 IOC Analysis: 3CX Supply Chain Payload Container Hash — d3dcompiler_47.dll SHA256

> This analysis examines the indicator **`11be1803e2e307b647a8a7e02d128335c448ff741bf06bf52b332e0bbf423b03`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

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

**Primary analytic takeaway:** `11be1803e2e307b647a8a7e02d128335c448ff741bf06bf52b332e0bbf423b03` is most useful for **malicious payload container / encrypted blob** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `11be1803e2e307b647a8a7e02d128335c448ff741bf06bf52b332e0bbf423b03` |
| **IOC Type** | SHA256 |
| **Associated Actor / Malware** | Lazarus-linked / UNC4736-style 3CX supply-chain activity |
| **Campaign / Activity** | 3CX DesktopApp supply-chain compromise |
| **Indicator Category** | Malicious payload container / encrypted blob |
| **Malware / Tooling Family** | Sideloaded DLL / encrypted second-stage payload |
| **Recommended Severity** | Critical |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| SHA256 | `11be1803e2e307b647a8a7e02d128335c448ff741bf06bf52b332e0bbf423b03` | Fortinet reporting described `d3dcompiler_47.dll` as containing the encrypted second-stage payload used in the 3CX DesktopApp supply-chain compromise. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Fortinet reporting described `d3dcompiler_47.dll` as containing the encrypted second-stage payload used in the 3CX DesktopApp supply-chain compromise.

### Analytic Meaning

- High-confidence host indicator of the trojanized 3CX package.
- The file may appear legitimate by name because `d3dcompiler_47.dll` is commonly seen in Windows applications.
- Detection should combine hash, path, signer, and process-load context.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Malicious payload container / encrypted blob |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1195.002 - Compromise Software Supply Chain
- T1574.002 - DLL Side-Loading
- T1027 - Obfuscated Files or Information

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| File hash search | Search for this SHA256 across endpoints | EDR |
| Suspicious path | Locate `d3dcompiler_47.dll` in 3CX DesktopApp directories | File inventory |
| DLL load telemetry | Find `3CXDesktopApp.exe` loading this DLL | EDR |
| Follow-on payload | Look for browser-data access and unusual outbound traffic from impacted hosts | EDR / proxy |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Quarantine systems containing this hash.** | Reduces risk and improves investigation quality |
| 2 | **Acquire triage package before remediation when possible.** | Reduces risk and improves investigation quality |
| 3 | **Review browser credential stores and authentication logs for affected users.** | Reduces risk and improves investigation quality |
| 4 | **Validate software hashes before redeployment.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Fortinet: 3CX Desktop App Compromised (CVE-2023-29059)
- Trend Micro: Information on Attacks Involving 3CX Desktop App

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
