# 🧬 IOC Analysis: Emotet C2 IP — 185[.]220[.]101[.]47

> This analysis examines the indicator **`185[.]220[.]101[.]47`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-IPv4-blue)
![Associated Actor](https://img.shields.io/badge/Actor-Emotet%20operators%20%2F%20loader%20ecosystem-purple)
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

This IOC is associated with **Emotet operators / loader ecosystem** activity and is best used as part of a broader investigation rather than as a standalone conclusion. The indicator should be searched across historical telemetry, correlated with endpoint behavior, and enriched before being used for long-term blocking or attribution.

**Primary analytic takeaway:** `185[.]220[.]101[.]47` is most useful for **c2 infrastructure** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `185[.]220[.]101[.]47` |
| **IOC Type** | IPv4 |
| **Associated Actor / Malware** | Emotet operators / loader ecosystem |
| **Campaign / Activity** | Representative Emotet resurgence activity |
| **Indicator Category** | C2 infrastructure |
| **Malware / Tooling Family** | Emotet loader |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| IPv4 | `185[.]220[.]101[.]47` | This IP appears in the user-provided Emotet IOC analysis example as C2 infrastructure. It is included here as a portfolio-style IOC analysis modeled after that template; validate activity in current feeds before operational blocking. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

This IP appears in the user-provided Emotet IOC analysis example as C2 infrastructure. It is included here as a portfolio-style IOC analysis modeled after that template; validate activity in current feeds before operational blocking.

### Analytic Meaning

- Possible command-and-control infrastructure associated with loader activity.
- IP-based IOCs age quickly and may be reassigned.
- A hit should trigger review for phishing, macro execution, and second-stage malware.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | C2 infrastructure |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1566.001 - Spearphishing Attachment
- T1059 - Command and Scripting Interpreter
- T1071 - Application Layer Protocol
- T1105 - Ingress Tool Transfer

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| Network connection | Outbound connections to `185.220.101.47` | Firewall / proxy / EDR |
| Initial access | Inbound Office attachments or suspicious document execution before the connection | Email / EDR |
| Process behavior | Office spawning script interpreters or command shells | EDR |
| Follow-on payloads | Qbot, Cobalt Strike, or ransomware precursor alerts after connection | EDR / SIEM |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block the IP temporarily while validating current ownership.** | Reduces risk and improves investigation quality |
| 2 | **Triage endpoints with any historical connection.** | Reduces risk and improves investigation quality |
| 3 | **Review associated email-delivery logs for malicious attachments.** | Reduces risk and improves investigation quality |
| 4 | **Reset credentials if loader behavior is confirmed.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- User-provided Emotet IOC analysis template
- Feodo Tracker / URLhaus-style IOC context

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
