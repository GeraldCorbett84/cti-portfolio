# 🧬 IOC Analysis: APT29 / Midnight Blizzard RDP Attachment Filename — AWS IAM Compliance Check.rdp

> This analysis examines the indicator **`AWS IAM Compliance Check.rdp`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-Filename-blue)
![Associated Actor](https://img.shields.io/badge/Actor-APT29%20%2F%20Midnight%20Blizzard%20%2F%20Cozy%20Bear-purple)
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

This IOC is associated with **APT29 / Midnight Blizzard / Cozy Bear** activity and is best used as part of a broader investigation rather than as a standalone conclusion. The indicator should be searched across historical telemetry, correlated with endpoint behavior, and enriched before being used for long-term blocking or attribution.

**Primary analytic takeaway:** `AWS IAM Compliance Check.rdp` is most useful for **malicious attachment / rdp lure** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `AWS IAM Compliance Check.rdp` |
| **IOC Type** | Filename |
| **Associated Actor / Malware** | APT29 / Midnight Blizzard / Cozy Bear |
| **Campaign / Activity** | Midnight Blizzard RDP-file spear-phishing campaign |
| **Indicator Category** | Malicious attachment / RDP lure |
| **Malware / Tooling Family** | RDP phishing / credential exposure |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| Filename | `AWS IAM Compliance Check.rdp` | Microsoft listed this `.rdp` filename in hunting logic for identifying possible recipients of Midnight Blizzard RDP attachment phishing. The naming abuses AWS/IAM compliance language to make the attachment appear business-relevant. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Microsoft listed this `.rdp` filename in hunting logic for identifying possible recipients of Midnight Blizzard RDP attachment phishing. The naming abuses AWS/IAM compliance language to make the attachment appear business-relevant.

### Analytic Meaning

- A cloud-compliance-themed lure designed to persuade a user to open an RDP file.
- The danger is behavioral: opening the file can initiate a connection to actor-controlled infrastructure.
- Filename searches are useful for scoping but can be easily changed by an actor.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Malicious attachment / RDP lure |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1566.001 - Spearphishing Attachment
- T1204.002 - Malicious File
- T1021.001 - Remote Services: RDP

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| Email attachment search | Find attachments named `AWS IAM Compliance Check.rdp` | EmailAttachmentInfo |
| Endpoint execution | Look for `mstsc.exe` launched after attachment download | EDR |
| Resource redirection | Review RDP sessions with drive, clipboard, printer, or smart-card redirection | Windows logs / EDR |
| Post-exposure activity | Look for file access, credential use, or remote connections after execution | EDR / identity |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block `.rdp` attachments at the email gateway unless explicitly needed.** | Reduces risk and improves investigation quality |
| 2 | **Review attachment-open telemetry for all recipients.** | Reduces risk and improves investigation quality |
| 3 | **Restrict outbound RDP to the internet.** | Reduces risk and improves investigation quality |
| 4 | **Educate cloud and security teams on fake compliance-themed lures.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Microsoft Security Blog: Midnight Blizzard conducts large-scale spear-phishing campaign using RDP files

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
