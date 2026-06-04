# 🧬 IOC Analysis: APT41 DUSTTRAP Domain — www[.]eloples[.]com

> This analysis examines the indicator **`www[.]eloples[.]com`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-Domain-blue)
![Associated Actor](https://img.shields.io/badge/Actor-APT41%20%2F%20Wicked%20Panda%20%2F%20Brass%20Typhoon-purple)
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

This IOC is associated with **APT41 / Wicked Panda / Brass Typhoon** activity and is best used as part of a broader investigation rather than as a standalone conclusion. The indicator should be searched across historical telemetry, correlated with endpoint behavior, and enriched before being used for long-term blocking or attribution.

**Primary analytic takeaway:** `www[.]eloples[.]com` is most useful for **dusttrap network infrastructure** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `www[.]eloples[.]com` |
| **IOC Type** | Domain |
| **Associated Actor / Malware** | APT41 / Wicked Panda / Brass Typhoon |
| **Campaign / Activity** | APT41 DUSTPAN / DUSTTRAP campaign |
| **Indicator Category** | DUSTTRAP network infrastructure |
| **Malware / Tooling Family** | DUSTTRAP |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| Domain | `www[.]eloples[.]com` | Google/Mandiant listed `www[.]eloples[.]com` as a DUSTTRAP-related network indicator, with reporting noting first and last observed dates in 2024. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Google/Mandiant listed `www[.]eloples[.]com` as a DUSTTRAP-related network indicator, with reporting noting first and last observed dates in 2024.

### Analytic Meaning

- Indicator is more specific to DUSTTRAP than generic suspicious infrastructure.
- Useful for historical detection and campaign scoping.
- Should be enriched with passive DNS and endpoint process telemetry.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | DUSTTRAP network infrastructure |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1071.001 - Web Protocols
- T1027.013 - Encrypted/Encoded File
- T1567.002 - Exfiltration to Cloud Storage

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| DNS/proxy | Queries or connections to `www.eloples.com` | DNS / proxy |
| Related IP | Connections to IPs associated with this FQDN in passive DNS | Firewall / proxy |
| Endpoint activity | DLL trojanization or memory-only plugin behavior on systems that connected | EDR |
| Cloud exfiltration | OneDrive API upload activity near connection time | Proxy / cloud logs |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block the domain and investigate all historical connections.** | Reduces risk and improves investigation quality |
| 2 | **Correlate with APT41 DUSTTRAP host indicators.** | Reduces risk and improves investigation quality |
| 3 | **Inspect affected hosts for plugin-style malware and trojanized DLLs.** | Reduces risk and improves investigation quality |
| 4 | **Review cloud storage access for suspicious uploads.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Google Cloud / Mandiant: APT41 Has Arisen From the DUST

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
