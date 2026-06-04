# 🧬 IOC Analysis: APT41 DUSTPAN File Hash — conn.exe MD5

> This analysis examines the indicator **`35f650c94faf6a2068e8238dd99edbea`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

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

**Primary analytic takeaway:** `35f650c94faf6a2068e8238dd99edbea` is most useful for **malware hash** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `35f650c94faf6a2068e8238dd99edbea` |
| **IOC Type** | MD5 |
| **Associated Actor / Malware** | APT41 / Wicked Panda / Brass Typhoon |
| **Campaign / Activity** | APT41 DUSTPAN / DUSTTRAP campaign |
| **Indicator Category** | Malware hash |
| **Malware / Tooling Family** | DUSTPAN |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| MD5 | `35f650c94faf6a2068e8238dd99edbea` | Google/Mandiant listed `conn.exe` with MD5 `35f650c94faf6a2068e8238dd99edbea` as a host-based DUSTPAN indicator in APT41 activity. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Google/Mandiant listed `conn.exe` with MD5 `35f650c94faf6a2068e8238dd99edbea` as a host-based DUSTPAN indicator in APT41 activity.

### Analytic Meaning

- High-confidence file hash for DUSTPAN when found in endpoint telemetry.
- Filename resembles a generic connection utility and may be used to blend in.
- Hash-based IOC should be paired with path, service, and parent-process analysis.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Malware hash |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1105 - Ingress Tool Transfer
- T1036.005 - Match Legitimate Name or Location
- T1569.002 - Service Execution
- T1543.003 - Windows Service

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| Hash search | Search EDR file inventory for this MD5 | EDR |
| Filename search | Look for `conn.exe` in unusual directories | EDR / file inventory |
| Service creation | Services launching `conn.exe` or masquerading as Windows components | Windows logs |
| Network behavior | Connections from this binary to APT41 C2 infrastructure | EDR / proxy |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Quarantine hosts with this hash.** | Reduces risk and improves investigation quality |
| 2 | **Preserve file and memory artifacts before remediation if possible.** | Reduces risk and improves investigation quality |
| 3 | **Check for web shells or initial-access artifacts on affected servers.** | Reduces risk and improves investigation quality |
| 4 | **Reset credentials used on impacted systems.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Google Cloud / Mandiant: APT41 Has Arisen From the DUST

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
