# 🧬 IOC Analysis: APT29 / Midnight Blizzard RDP Phishing Sender Domain — sellar[.]co[.]uk

> This analysis examines the indicator **`sellar[.]co[.]uk`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-Domain-blue)
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

**Primary analytic takeaway:** `sellar[.]co[.]uk` is most useful for **phishing infrastructure / compromised sender domain** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `sellar[.]co[.]uk` |
| **IOC Type** | Domain |
| **Associated Actor / Malware** | APT29 / Midnight Blizzard / Cozy Bear |
| **Campaign / Activity** | Midnight Blizzard RDP-file spear-phishing campaign |
| **Indicator Category** | Phishing infrastructure / compromised sender domain |
| **Malware / Tooling Family** | RDP phishing / credential exposure |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| Domain | `sellar[.]co[.]uk` | Microsoft reported that Midnight Blizzard sent targeted spear-phishing emails containing signed Remote Desktop Protocol configuration files. The domain appears in Microsoft hunting logic as a sender domain associated with the campaign. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Microsoft reported that Midnight Blizzard sent targeted spear-phishing emails containing signed Remote Desktop Protocol configuration files. The domain appears in Microsoft hunting logic as a sender domain associated with the campaign.

### Analytic Meaning

- May indicate an email sender domain abused or compromised for targeted phishing delivery.
- Useful for mailbox scoping, recipient identification, and targeted-user notification.
- Domain alone should not be treated as proof of compromise; review message headers, attachments, and user interaction.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Phishing infrastructure / compromised sender domain |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1566 - Phishing
- T1204 - User Execution
- T1021.001 - Remote Services: RDP
- T1041 - Exfiltration Over C2 Channel

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| Email delivery | Inbound messages where SenderFromDomain equals `sellar.co.uk`, especially with `.rdp` attachments | Email gateway / Microsoft 365 |
| Attachment analysis | RDP configuration files attached to emails from this domain | Email security / sandbox |
| User exposure | Recipients who opened or downloaded attached `.rdp` files | Email telemetry / EDR |
| Outbound RDP | External RDP sessions shortly after message delivery | Firewall / EDR / Windows logs |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Quarantine matching messages and preserve headers for investigation.** | Reduces risk and improves investigation quality |
| 2 | **Identify all recipients and confirm whether `.rdp` attachments were opened.** | Reduces risk and improves investigation quality |
| 3 | **Block outbound RDP to internet destinations unless explicitly required.** | Reduces risk and improves investigation quality |
| 4 | **Train users that `.rdp` attachments can expose local resources and credentials.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Microsoft Security Blog: Midnight Blizzard conducts large-scale spear-phishing campaign using RDP files
- MITRE ATT&CK: APT29 / Cozy Bear / Midnight Blizzard

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
