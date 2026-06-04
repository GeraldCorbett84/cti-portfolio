# 🧬 IOC Analysis: APT41 DUSTPAN Delivery URL — hxxp://152.89.244[.]185/conn.exe

> This analysis examines the indicator **`hxxp://152.89.244[.]185/conn.exe`** and explains how defenders can convert a raw IOC into investigation, detection, and response actions.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![IOC Type](https://img.shields.io/badge/IOC%20Type-URL-blue)
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

**Primary analytic takeaway:** `hxxp://152.89.244[.]185/conn.exe` is most useful for **malware delivery url** detection and scoping. A match should trigger a timeline review covering initial access, execution, persistence, credential access, lateral movement, exfiltration, and impact indicators where applicable.

> [!WARNING]
> IOCs can expire quickly. Treat this as a starting point for investigation, not the final answer. Validate the indicator against current telemetry, threat-intel feeds, and business context before taking disruptive action.

---

## 🧾 Case Context

| Field | Details |
|---|---|
| **Indicator** | `hxxp://152.89.244[.]185/conn.exe` |
| **IOC Type** | URL |
| **Associated Actor / Malware** | APT41 / Wicked Panda / Brass Typhoon |
| **Campaign / Activity** | APT41 DUSTPAN / DUSTTRAP campaign |
| **Indicator Category** | Malware delivery URL |
| **Malware / Tooling Family** | DUSTPAN downloader / BEACON loader activity |
| **Recommended Severity** | High |
| **Assessment Confidence** | Medium to High, depending on matching telemetry and source validation |

---

## 📍 IOC Summary

| IOC Type | Value | Source / Context | Defensive Use |
|---|---|---|---|
| URL | `hxxp://152.89.244[.]185/conn.exe` | Google/Mandiant identified this URL as being used to deliver DUSTPAN. DUSTPAN was observed disguised as Windows-like binaries including `w3wp.exe` or `conn.exe` and made persistent via Windows services. | Search, block where appropriate, scope affected assets, and correlate with behavior |

> [!NOTE]
> Network indicators are defanged for safe GitHub display. File hashes are safe to display but should still be validated before operational use.

---

## 🔬 IOC Deep Dive

### What This IOC Represents

Google/Mandiant identified this URL as being used to deliver DUSTPAN. DUSTPAN was observed disguised as Windows-like binaries including `w3wp.exe` or `conn.exe` and made persistent via Windows services.

### Analytic Meaning

- High-value delivery indicator for APT41 DUSTPAN activity.
- A connection may represent payload download rather than normal browsing.
- Review both network and host file-write telemetry around the connection time.

### False Positive / Aging Considerations

- **Domains and IPs** can be sinkholed, cleaned up, parked, or reassigned.
- **Filenames** can be renamed by attackers and may collide with legitimate files.
- **Hashes** are high-confidence for exact matches, but actors can easily produce new hashes.
- **URLs and paths** are stronger than domains alone because they preserve more campaign context.

---

## 🧠 Analytic Judgment

| Finding | Assessment |
|---|---|
| **Likely Role** | Malware delivery URL |
| **Primary Risk** | Unauthorized access, malware delivery, credential theft, lateral movement, exfiltration, or ransomware impact depending on environment context |
| **Confidence** | Medium to High when supported by source telemetry and related behavior |
| **Recommended Action** | Hunt first, enrich second, contain affected hosts if correlated malicious behavior is observed |
| **Escalation Threshold** | Escalate if this IOC appears with suspicious process execution, credential activity, lateral movement, or data staging |

---

## 🧭 MITRE ATT&CK Alignment

- T1105 - Ingress Tool Transfer
- T1036.005 - Match Legitimate Name or Location
- T1569.002 - Service Execution

---

## 🔎 Detection & Hunting Opportunities

| Hunt Area | What to Look For | Data Source |
|---|---|---|
| URL access | Proxy or EDR network events for this URL | Proxy / EDR |
| File creation | Creation of `conn.exe` after access to this IP/URL | EDR |
| Service persistence | New Windows service executing `conn.exe` or web-server-like names | Windows logs / EDR |
| Beacon follow-on | Subsequent connections to BEACON domains or Cloudflare Workers | Proxy / DNS |

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | **Block the IP and URL path.** | Reduces risk and improves investigation quality |
| 2 | **Collect endpoint triage from systems that accessed it.** | Reduces risk and improves investigation quality |
| 3 | **Search for DUSTPAN hashes and service persistence.** | Reduces risk and improves investigation quality |
| 4 | **Review exposed web servers for prior web shell activity.** | Reduces risk and improves investigation quality |

---

## 📚 Sources

- Google Cloud / Mandiant: APT41 Has Arisen From the DUST

---

## ✅ Analyst Notes

This file is formatted for CTI portfolio use and GitHub rendering. The goal is to show that an analyst can take one IOC and explain **what it means, where to hunt, how to validate it, and what defenders should do next**.
