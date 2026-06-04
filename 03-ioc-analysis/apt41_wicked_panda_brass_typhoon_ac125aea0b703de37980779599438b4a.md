# 🧬 IOC Analysis: APT41 PINEGROVE File Hash — OneDriveUploader.exe MD5

> This analysis examines the indicator **`ac125aea0b703de37980779599438b4a`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-MD5-blue)
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

**Primary analytic takeaway:** `ac125aea0b703de37980779599438b4a` is most useful for **exfiltration tool hash** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `ac125aea0b703de37980779599438b4a` |
| **IOC Type** | MD5 |
| **Associated Actor / Malware** | APT41 / Wicked Panda / Brass Typhoon |
| **Campaign / Activity** | APT41 DUSTPAN / DUSTTRAP campaign |
| **Indicator Category** | Exfiltration tool hash |
| **Malware / Tooling Family** | PINEGROVE |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| MD5 | `ac125aea0b703de37980779599438b4a` | Google/Mandiant listed `OneDriveUploader.exe` with MD5 `ac125aea0b703de37980779599438b4a` as PINEGROVE, a Go-based command-line uploader used for data exfiltration to OneDrive through the OneDrive API. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Google/Mandiant listed `OneDriveUploader.exe` with MD5 `ac125aea0b703de37980779599438b4a` as PINEGROVE, a Go-based command-line uploader used for data exfiltration to OneDrive through the OneDrive API.

### Analytic Meaning

- Host-based indicator associated with cloud exfiltration tooling.
- A file hit should trigger review of OneDrive API usage and staged archives.
- The filename may appear legitimate, making context critical.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Exfiltration tool hash |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1567.002 - Exfiltration to Cloud Storage
- T1105 - Ingress Tool Transfer
- T1020 - Automated Exfiltration

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| Hash search | Search for this MD5 across endpoints | EDR |
| Command line | Look for `OneDriveUploader.exe` with config/auth JSON arguments | EDR |
| Cloud API use | Unusual OneDrive upload volume from server endpoints | Cloud logs |
| Archive staging | Large archive creation before OneDrive upload | EDR / file telemetry |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Isolate affected endpoints and preserve command-line evidence.** | Reduces risk and improves investigation quality |
| 2 | **Review OneDrive audit logs for suspicious uploads.** | Reduces risk and improves investigation quality |
| 3 | **Invalidate exposed OAuth tokens or credentials.** | Reduces risk and improves investigation quality |
| 4 | **Harden cloud storage access with conditional access and least privilege.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Google Cloud / Mandiant: APT41 Has Arisen From the DUST

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
