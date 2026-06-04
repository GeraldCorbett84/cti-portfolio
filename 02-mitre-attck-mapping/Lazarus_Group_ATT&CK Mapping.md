# 🧭 MITRE ATT&CK Mapping: Lazarus Group / Hidden Cobra

> This report maps **Lazarus Group / Hidden Cobra** behaviors to MITRE ATT&CK tactics and techniques to support detection engineering, threat hunting, and incident response prioritization.

![Threat Level](https://img.shields.io/badge/Threat%20Level-Critical-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group-G0032-blue)

---

## 📌 Table of Contents

- [Executive Summary](#-executive-summary)
- [Mapping Overview](#-mapping-overview)
- [ATT&CK Technique Mapping](#-attck-technique-mapping)
- [Detection Engineering Opportunities](#-detection-engineering-opportunities)
- [Priority Hunt Questions](#-priority-hunt-questions)
- [Defensive Recommendations](#-defensive-recommendations)
- [Sources](#-sources)

---

## 🧠 Executive Summary

Lazarus Group is a North Korean state-sponsored threat actor associated with cyber espionage, financial theft, cryptocurrency targeting, destructive malware, and software supply chain compromise. Mapping Lazarus activity to MITRE ATT&CK helps defenders focus on behaviors that remain useful even when infrastructure and IOCs rotate.

This mapping emphasizes observable behaviors such as **trojanized software, fake recruiter lures, browser credential theft, service-based persistence, sandbox evasion, RDP abuse, SWIFT/payment system manipulation, and destructive malware deployment**.

> [!WARNING]
> Lazarus Group activity may appear as normal software execution during early stages, especially when the group abuses signed software, trusted applications, or supply chain access.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | Lazarus Group / Hidden Cobra |
| **MITRE Group ID** | G0032 |
| **Primary Mission** | Financial theft, espionage, strategic disruption |
| **Common Initial Access** | Supply chain compromise, spearphishing, fake recruiter lures |
| **Common Targets** | Cryptocurrency, banks, defense contractors, government, healthcare, technology |
| **Recommended Telemetry** | EDR, application execution logs, identity logs, VPN/RDP logs, DNS/proxy, financial platform audit logs |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Resource Development | T1583.001 | Acquire Infrastructure: Domains | Registered cryptocurrency exchange lookalike domains used in AppleJeus campaigns | Newly registered lookalike domains, suspicious TLS certificates, typosquatting patterns |
| Initial Access | T1195.002 | Supply Chain Compromise | Trojanized legitimate cryptocurrency trading or desktop software | Trusted applications spawning unexpected child processes or contacting rare domains |
| Initial Access | T1566.003 | Spearphishing via Service | Fake LinkedIn recruiter profiles delivered malicious job descriptions | Recruiting-themed lures, suspicious attachments, user execution after recruiter contact |
| Execution | T1204.002 | User Execution: Malicious File | Victim manually executed a trojanized app or malicious attachment | First execution of unsigned or rare binaries from user directories |
| Persistence | T1543.003 | Windows Service | FASTCash or related tooling installed as a Windows service | New service creation, unusual service path, service running from temp or user folders |
| Defense Evasion | T1497.001 | Virtualization / Sandbox Evasion | BLINDINGCAN checked for analysis artifacts and halted execution | Process checks for VM artifacts, anti-analysis API calls, execution delay behavior |
| Credential Access | T1555.003 | Credentials from Web Browsers | Extracted saved credentials from Chrome and Firefox profiles | Access to browser credential stores by non-browser processes |
| Discovery | T1018 | Remote System Discovery | Internal scanning to identify servers, payment systems, and high-value assets | Network discovery commands, rapid internal connection attempts |
| Lateral Movement | T1563.002 | RDP Session Hijacking | Hijacked existing RDP sessions with `tscon.exe` | RDP session transitions, `tscon.exe` execution, admin logons without normal patterns |
| Exfiltration | T1048.003 | Exfiltration Over Alternative Protocol | Exfiltrated data via FTP or non-standard transfer channels | Archive staging followed by FTP or unusual outbound transfer |
| Impact | T1485 | Data Destruction | Destover-style wiper malware overwrote data and damaged systems | File overwrite patterns, MBR modification, mass deletion or encryption behavior |
| Impact | T1657 | Financial Theft | Manipulated payment systems or cryptocurrency infrastructure to redirect funds | Abnormal transaction changes, administrative actions outside business workflow |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **Trusted App Abuse** | Signed app spawns `cmd.exe`, `powershell.exe`, or unusual network traffic | Supply chain attacks often abuse trusted binaries |
| **Fake Recruiter Delivery** | Recruiting-themed attachment opens followed by script execution | Lazarus commonly uses employment lures |
| **Browser Credential Theft** | Non-browser process reads browser credential storage | Credential theft enables broader compromise |
| **New Services** | New Windows service created from unusual path | Service creation supports persistence |
| **RDP Session Abuse** | `tscon.exe` use or abnormal RDP session switching | Can indicate lateral movement without new authentication noise |
| **Financial System Integrity** | High-risk transaction changes outside normal workflow | Lazarus financial theft may involve manipulation, not just data theft |

---

## ❓ Priority Hunt Questions

- Are trusted or signed applications spawning shells or contacting rare external infrastructure?
- Are any new Windows services running from user-writable locations?
- Are non-browser processes accessing browser credential stores?
- Are users receiving recruiter-themed lures followed by file execution?
- Are RDP sessions being hijacked or transferred using administrative tools?
- Are financial or cryptocurrency administrative actions occurring outside approved workflows?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Apply strict application control for high-risk systems | Reduces execution of trojanized tools |
| 2 | Monitor signed applications for abnormal child processes | Detects trusted-app abuse and supply chain activity |
| 3 | Disable password saving in browsers for privileged users | Reduces value of browser credential theft |
| 4 | Segment payment, SWIFT, and crypto infrastructure | Limits lateral movement into financial systems |
| 5 | Monitor new service creation and RDP session behavior | Detects persistence and lateral movement |
| 6 | Train staff on fake recruiter and job-themed lures | Disrupts social engineering initial access |

---

## 📚 Sources

- MITRE ATT&CK — **G0032**
- Mandiant APT38 reporting
- CISA AA21-048A
- Kaspersky Lazarus Group analysis
- Public reporting on Operation Dream Job, 3CX, Bangladesh Bank, Sony Pictures, and related Lazarus activity

---

## ✅ Analyst Notes

This mapping is designed for defensive use. Treat ATT&CK mappings as a starting point for detection coverage, not as a complete description of all Lazarus activity. Validate against current reporting before deploying detections or making attribution decisions.
