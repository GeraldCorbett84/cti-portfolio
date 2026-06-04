# 🕷️ Threat Actor Profile: Scattered Spider / UNC3944 / Octo Tempest

> **Scattered Spider**, also tracked as **UNC3944** and **Octo Tempest**, is a financially motivated cybercriminal group known for social engineering, help desk impersonation, MFA bypass, identity compromise, data theft, and ransomware enablement.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Cybercriminal%20Group-black)
![Primary Motivation](https://img.shields.io/badge/Motivation-Financial%20Extortion-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-G1015-orange)

---

> GitHub-ready threat actor profile modeled after the uploaded Lazarus Group profile.  
> TLP:CLEAR | For defensive research, threat hunting, and CTI portfolio use.

---

## ⚠️ Analyst Notice

Indicators of Compromise are short-lived and should **not** be used as the sole basis for attribution or blocking decisions. Validate all IOCs against current enrichment, passive DNS, malware repositories, internal telemetry, and trusted threat intelligence feeds before deploying detections or blocking controls.

> [!NOTE]
> TTPs generally provide longer-lasting defensive value than raw IOCs. Use IOC tables as enrichment and pivot points, not as the final intelligence product.

---


## 🧠 Executive Summary

Scattered Spider is a financially motivated threat group known for highly effective social engineering. The group frequently targets help desks, identity providers, SaaS platforms, cloud environments, and remote access tools. Its operators are often described as skilled at manipulating enterprise support processes rather than relying only on malware.

The group has been associated with data theft, SIM swapping, MFA fatigue, Okta compromise, SaaS enumeration, cloud abuse, and ransomware deployment through affiliate relationships.

> [!WARNING]
> Scattered Spider is a **critical identity and social engineering threat** for large enterprises, especially those with distributed help desks, cloud environments, and high-value SaaS platforms.

---

## 🗂️ At-a-Glance

| Category | Details |
|---|---|
| **Classification** | Financially Motivated Cybercriminal / Ransomware Affiliate Cluster |
| **Also Known As** | UNC3944, Octo Tempest, Roasted 0ktapus, Storm-0875, Muddled Libra, Starfraud |
| **First Observed** | Publicly reported activity since at least 2022 |
| **Threat Level** | Critical |
| **Primary Motivation** | Data theft, extortion, ransomware, financial gain |
| **Common Targeting** | Telecom, technology, retail, hospitality, finance, SaaS, cloud, BPO organizations |
| **MITRE ATT&CK Group ID** | G1015 |

---

## 🧭 Origin & Attribution

Scattered Spider is generally described as a loosely organized cybercriminal collective. Public reporting often describes members as native English-speaking social engineers who abuse help desk workflows, identity platforms, and remote access tools.

### Attribution Indicators

- Repeated help desk impersonation patterns
- MFA reset and device enrollment abuse
- Targeting of Okta, Azure AD, SaaS, and cloud environments
- SIM swapping and vishing
- Use of legitimate remote access tools
- Links to ransomware deployment and data extortion

---

## 🎯 Motivation

| Motivation | Description |
|---|---|
| **Financial Extortion** | Theft of sensitive data followed by ransom demands |
| **Ransomware Enablement** | Deploying or enabling ransomware through affiliate relationships |
| **Identity Compromise** | Taking over accounts through help desk and MFA abuse |
| **Cloud/SaaS Theft** | Stealing data from SharePoint, OneDrive, GitHub, Slack, Teams, and cloud storage |
| **Operational Manipulation** | Monitoring internal communications to understand defender response |

---

## 🏢 Targeted Sectors

| Sector | Examples |
|---|---|
| **Telecommunications** | Mobile carriers, SIM-related processes, identity workflows |
| **Retail / Hospitality** | Large consumer-facing enterprises |
| **Technology / SaaS** | Cloud providers, identity platforms, software companies |
| **Finance** | Financial services and fintech organizations |
| **Business Process Outsourcing** | Call centers and support organizations |
| **Gaming / Entertainment** | Source code and customer data theft |

---

## 🧰 Known TTPs: MITRE ATT&CK

| Tactic | Technique ID | Technique Name | Example Procedure |
|---|---:|---|---|
| Reconnaissance | T1589 | Gather Victim Identity Information | Collects employee details for impersonation |
| Initial Access | T1566.004 | Spearphishing Voice | Uses vishing and help desk calls |
| Credential Access | T1621 | Multi-Factor Authentication Request Generation | Performs MFA fatigue and push bombing |
| Persistence | T1098.005 | Account Manipulation: Device Registration | Enrolls attacker-controlled MFA devices |
| Defense Evasion | T1078 | Valid Accounts | Uses compromised credentials to appear legitimate |
| Collection | T1530 | Data from Cloud Storage | Collects data from cloud storage and SaaS repositories |
| Collection | T1114 | Email Collection | Searches email for credentials and incident response context |
| Command and Control | T1219 | Remote Access Software | Uses tools such as AnyDesk, TeamViewer, or ScreenConnect |
| Impact | T1486 | Data Encrypted for Impact | Deploys ransomware through affiliate operations |

---

## 🛠️ Signature Tools & Malware

| Tool / Malware | Type | Notes |
|---|---|---|
| **AnyDesk** | Remote Access Tool | Used for remote access and persistence |
| **TeamViewer** | Remote Access Tool | Used for remote control of compromised endpoints |
| **ScreenConnect** | Remote Access Tool | Used for remote access and operations |
| **Mimikatz** | Credential Tool | Used for credential dumping in some intrusions |
| **Raccoon Stealer** | Infostealer | Reported in some public Scattered Spider contexts |
| **BlackCat / ALPHV** | Ransomware | Public reporting linked Scattered Spider affiliates to ransomware deployment |
| **DragonForce / RansomHub** | Ransomware Ecosystem | Public reporting has linked some activity to later ransomware affiliate models |

---

## 🧾 Public IOCs / Pivot Points

| Indicator | Type | Context |
|---|---|---|
| `corp-hubspot[.]com` | Domain | Publicly reported impersonation/phishing-style domain |
| `morningstar-okta[.]com` | Domain | Publicly reported Okta-themed phishing domain |
| `signin-nydig[.]com` | Domain | Publicly reported sign-in themed phishing domain |

> [!NOTE]
> Scattered Spider infrastructure is often short-lived. Defensive value is higher when combining domain monitoring with help desk controls, identity telemetry, and MFA device enrollment auditing.

---

## 🗓️ Notable Campaigns

<details>
<summary><strong>Telecom and BPO Campaigns</strong></summary>

Scattered Spider has been linked to campaigns targeting telecommunications and business process outsourcing organizations using social engineering, SIM swapping, and identity abuse.

</details>

<details>
<summary><strong>Hospitality and Ransomware Incidents</strong></summary>

Public reporting has associated Scattered Spider with major hospitality-sector intrusions involving social engineering and ransomware impact.

</details>

<details>
<summary><strong>Cloud and SaaS Data Theft</strong></summary>

The group frequently searches cloud storage, collaboration platforms, email, and repositories for data that can support extortion or further compromise.

</details>

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Harden help desk identity verification | The group often starts with support process abuse |
| 2 | Require phishing-resistant MFA for privileged users | Reduces MFA push and reset abuse |
| 3 | Alert on new MFA device enrollment | Detects attacker persistence |
| 4 | Restrict remote access tools | Prevents unauthorized RMM use |
| 5 | Monitor SaaS data access anomalies | Detects cloud data theft |
| 6 | Train help desk staff on adversary impersonation | Humans are a primary target in this actor's playbook |

---

## 📚 Sources

- MITRE ATT&CK — Scattered Spider / G1015: https://attack.mitre.org/groups/G1015/
- CISA AA23-320A — Scattered Spider: https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a
- Huntress threat actor profile: https://www.huntress.com/threat-library/threat-actors/scattered-spider

---

## ✅ Analyst Notes

Scattered Spider is a strong example of why identity security and help desk controls matter. Useful hunts include new MFA registrations, password resets followed by VPN login, impossible travel, unusual SaaS downloads, and remote access tool execution.

---
