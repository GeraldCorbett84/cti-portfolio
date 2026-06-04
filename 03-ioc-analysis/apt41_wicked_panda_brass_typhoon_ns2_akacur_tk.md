# 🧬 IOC Analysis: APT41 BEACON C2 Domain — ns2[.]akacur[.]tk

> This analysis examines the indicator **`ns2[.]akacur[.]tk`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

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

**Primary analytic takeaway:** `ns2[.]akacur[.]tk` is most useful for **c2 infrastructure** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `ns2[.]akacur[.]tk` |
| **IOC Type** | Domain |
| **Associated Actor / Malware** | APT41 / Wicked Panda / Brass Typhoon |
| **Campaign / Activity** | APT41 DUSTPAN / DUSTTRAP campaign |
| **Indicator Category** | C2 infrastructure |
| **Malware / Tooling Family** | BEACON C2 |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| Domain | `ns2[.]akacur[.]tk` | Google/Mandiant listed `ns2[.]akacur[.]tk` as a network-based indicator associated with BEACON in an APT41 campaign involving DUSTPAN and DUSTTRAP tooling. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Google/Mandiant listed `ns2[.]akacur[.]tk` as a network-based indicator associated with BEACON in an APT41 campaign involving DUSTPAN and DUSTTRAP tooling.

### Analytic Meaning

- Network-based indicator tied to APT41 BEACON activity.
- Useful for proxy/DNS scoping and confirming suspected DUSTPAN/DUSTTRAP infections.
- Domain may be inactive now; historical telemetry is still valuable.

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

- T1071.001 - Web Protocols
- T1102 - Web Service
- T1505.003 - Web Shell
- T1569.002 - Service Execution

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| DNS query | Look for historical or current queries to `ns2.akacur.tk` | DNS |
| Proxy connection | Find HTTP/S activity to this domain | Proxy / firewall |
| Endpoint process | Identify process responsible for connection | EDR |
| Related IOCs | Pivot to `ns1.akacur.tk`, Cloudflare Workers domains, and DUSTPAN hashes | Threat intel platform |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block the domain and related resolved IPs if still active.** | Reduces risk and improves investigation quality |
| 2 | **Retro-hunt for connections over the last 90-180 days.** | Reduces risk and improves investigation quality |
| 3 | **Prioritize systems that also show web shell, service creation, or DLL trojanization behavior.** | Reduces risk and improves investigation quality |
| 4 | **Enrich with passive DNS before using for attribution.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Google Cloud / Mandiant: APT41 Has Arisen From the DUST

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
