# 🧭 MITRE ATT&CK Mapping: APT41 / Wicked Panda / Brass Typhoon

> This report maps **APT41 / Wicked Panda / Brass Typhoon** behaviors to MITRE ATT&CK tactics and techniques to support threat hunting, detection engineering, and SOC triage.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-orange)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State%20+%20Financial-purple)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Group ID](https://img.shields.io/badge/Group%20ID-G0096-blue)

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

APT41, also known as Wicked Panda and Brass Typhoon, is a China-nexus threat group tracked by MITRE ATT&CK as G0096. Public reporting describes APT41 as a Chinese state-sponsored espionage actor that has also conducted financially motivated operations. This mapping focuses on public-facing exploitation, web shells, DLL side-loading, credential access, discovery, C2 over web protocols, ingress tooling, and exfiltration to cloud services.

> [!WARNING]
> APT41 has historically used a broad toolkit. Defenders should prioritize behavior-based detections around web shells, DLL side-loading, and post-exploitation discovery rather than relying only on static signatures.

---

## 🗂️ Mapping Overview

| Category | Details |
|---|---|
| **Threat Actor** | APT41 / Wicked Panda / Brass Typhoon |
| **MITRE Group ID** | G0096 |
| **Primary Mission** | Espionage, intellectual property theft, and some financially motivated activity |
| **Common Initial Access** | Public-facing application exploitation, web shells, supply chain or service compromise |
| **Common Persistence** | Web shells, services, DLL side-loading, compromised accounts |
| **Common Collection Target** | Healthcare, telecom, technology, finance, education, retail, gaming, credentials, IP |
| **Recommended Telemetry** | Web server logs, EDR, PowerShell logs, DLL load telemetry, DNS, proxy, cloud storage logs, authentication logs |

---

## 🧰 ATT&CK Technique Mapping

| Tactic | Technique ID | Technique Name | Example Procedure | Detection / Hunt Focus |
|---|---:|---|---|---|
| Resource Development | T1583.001 | Acquire Infrastructure: Domains | Registered or used domains to support C2 and campaign infrastructure. | Monitor newly registered domains and rare destinations from servers. |
| Initial Access | T1190 | Exploit Public-Facing Application | Exploited internet-facing applications and services for initial access. | Correlate exploit attempts with new files, shells, or suspicious child processes on web servers. |
| Persistence | T1505.003 | Server Software Component: Web Shell | Deployed web shells to maintain access to compromised servers. | Detect suspicious server-side script creation and web server processes spawning shells. |
| Execution | T1059.001 | Command and Scripting Interpreter: PowerShell | Used PowerShell for execution and post-compromise tasks. | Alert on PowerShell execution from service accounts or web server parent processes. |
| Execution | T1059.003 | Command and Scripting Interpreter: Windows Command Shell | Used command shell for discovery, staging, and execution. | Monitor `cmd.exe` launched by IIS, Apache, Nginx, Tomcat, or unusual service processes. |
| Defense Evasion | T1574.002 | Hijack Execution Flow: DLL Side-Loading | Used DLL side-loading to execute payloads through legitimate applications. | Detect unsigned or unusual DLLs loaded by signed applications from non-standard paths. |
| Defense Evasion | T1027 | Obfuscated Files or Information | Used packed or obfuscated malware to evade static detection. | Monitor high-entropy files, suspicious script obfuscation, and packed binaries. |
| Defense Evasion | T1036 | Masquerading | Named files, services, or processes to appear legitimate. | Detect binaries with trusted names executing from unusual paths. |
| Credential Access | T1003 | OS Credential Dumping | Dumped credentials to expand access. | Monitor LSASS access, credential dumping alerts, and abnormal registry hive reads. |
| Discovery | T1018 | Remote System Discovery | Enumerated remote hosts and internal systems. | Detect net view, ping sweeps, LDAP queries, and endpoint discovery from servers. |
| Command & Control | T1071.001 | Application Layer Protocol: Web Protocols | Used HTTP/S for command and control communications. | Monitor long-lived or periodic web traffic from servers to rare external domains. |
| Exfiltration | T1567.002 | Exfiltration Over Web Service: Exfiltration to Cloud Storage | Exfiltrated data to cloud storage services. | Detect large uploads from servers to cloud storage platforms or unfamiliar external services. |

---

## 🔎 Detection Engineering Opportunities

| Detection Area | Example Logic Concept | Why It Matters |
|---|---|---|
| **Web Shell Activity** | Alert when web server processes spawn command shells or scripting interpreters. | Web shells are a high-value APT41 detection point. |
| **DLL Side-Loading** | Monitor signed executables loading unsigned or unexpected DLLs from writable directories. | APT41 has repeatedly used side-loading patterns. |
| **Server Discovery** | Detect internal reconnaissance commands running from public-facing servers. | Compromised web servers often become staging points. |
| **Cloud Exfiltration** | Alert on large outbound uploads from servers to cloud storage services. | Cloud storage may be abused for data theft. |
| **Credential Dumping** | Monitor LSASS access and registry hive extraction. | Credential theft enables lateral movement. |
| **Rare Web C2** | Detect periodic HTTP/S beacons from servers to rare or newly seen domains. | C2 can blend into normal web traffic. |

---

## ❓ Priority Hunt Questions

- Are web server processes spawning `cmd.exe`, `powershell.exe`, or scripting engines?
- Are new server-side scripts appearing in web-accessible directories?
- Are signed applications loading DLLs from unusual or user-writable paths?
- Are public-facing servers conducting internal network discovery?
- Are servers uploading large volumes of data to cloud storage platforms?
- Are credential dumping alerts clustered after exploitation of a public-facing application?

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Patch and harden public-facing applications aggressively. | Reduces common APT41 initial access paths. |
| 2 | Monitor web server child processes and file creation. | Improves web shell detection. |
| 3 | Deploy DLL load monitoring and application control. | Reduces side-loading opportunities. |
| 4 | Limit outbound internet access from servers. | Constrains C2 and exfiltration. |
| 5 | Protect privileged credentials and monitor LSASS access. | Reduces lateral movement risk. |
| 6 | Baseline normal server-to-internet traffic. | Makes rare C2 and cloud exfiltration easier to detect. |

---

## 📚 Sources

- MITRE ATT&CK — APT41, Group G0096
- Google Cloud / Mandiant reporting on APT41
- Public reporting on Wicked Panda / Brass Typhoon tooling and campaigns
- MITRE ATT&CK technique entries for web shells, DLL side-loading, and cloud exfiltration

---

## ✅ Analyst Notes

This mapping is designed to turn CTI into operational detection guidance. Validate technique mappings against the latest MITRE ATT&CK entries, vendor reporting, and your own telemetry before converting them into production detections.
