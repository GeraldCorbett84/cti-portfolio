# 🐼 Threat Actor Profile: APT41 / Wicked Panda / Brass Typhoon

> **APT41**, also known as **Wicked Panda**, **Brass Typhoon**, and **BARIUM**, is a China-nexus threat actor known for both state-sponsored espionage and financially motivated cybercrime.

![Threat Level](https://img.shields.io/badge/Threat%20Level-High-red)
![Actor Type](https://img.shields.io/badge/Actor%20Type-Nation--State%20%2B%20Cybercrime-purple)
![Primary Motivation](https://img.shields.io/badge/Motivation-Espionage%20%2B%20Financial-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-G0096-orange)

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

APT41 is a prolific China-nexus threat actor known for a rare blend of espionage and financially motivated activity. Public reporting describes APT41 as targeting healthcare, telecom, technology, finance, education, retail, gaming, media, logistics, automotive, and government-related sectors.

The group is known for web shells, supply chain compromise, custom malware, credential access, cloud and infrastructure abuse, DLL side-loading, in-memory loaders, and long-term unauthorized access.

> [!WARNING]
> APT41 is a **high-impact threat** for organizations with internet-facing applications, software supply chain exposure, sensitive IP, government relationships, or high-value customer data.

---

## 🗂️ At-a-Glance

| Category | Details |
|---|---|
| **Classification** | Nation-State APT with Financially Motivated Operations |
| **Also Known As** | Wicked Panda, Brass Typhoon, BARIUM, Double Dragon, Winnti, Earth Baku |
| **First Observed** | Publicly reported activity since at least 2012, with related activity possibly earlier |
| **Threat Level** | High / Critical depending on sector |
| **Primary Motivation** | Espionage, IP theft, financial gain |
| **Common Targeting** | Healthcare, telecom, technology, finance, education, retail, gaming, media, automotive, logistics |
| **MITRE ATT&CK Group ID** | G0096 |

---

## 🧭 Origin & Attribution

APT41 is assessed by multiple security vendors as a China-nexus actor with operations serving both state intelligence objectives and financially motivated activity. This dual mission makes the group unusually flexible and dangerous.

### Attribution Indicators

- China-nexus targeting and operational patterns
- Espionage against sectors of strategic economic interest
- Financially motivated intrusions against gaming and commercial entities
- Use of malware families and tooling associated with Chinese APT ecosystems
- Public reporting by MITRE, Mandiant/Google, DOJ, and multiple vendors

---

## 🎯 Motivation

| Motivation | Description |
|---|---|
| **Espionage** | Collection against strategic industries, governments, and technology targets |
| **Financial Gain** | Theft from gaming, virtual currency, and commercial environments |
| **Intellectual Property Theft** | Targeting source code, research, and proprietary business data |
| **Supply Chain Access** | Compromising trusted software and infrastructure paths |
| **Long-Term Persistence** | Maintaining access for extended collection and follow-on operations |

---

## 🏢 Targeted Sectors

| Sector | Examples |
|---|---|
| **Healthcare** | Medical research and health technology |
| **Telecommunications** | Service providers and telecom infrastructure |
| **Technology** | Software, cloud, and high-tech organizations |
| **Gaming** | Source code, certificates, and virtual currency |
| **Media / Entertainment** | Publicly reported targeting in Asia and Europe |
| **Automotive / Logistics** | Strategic industry targeting |

---

## 🧰 Known TTPs: MITRE ATT&CK

| Tactic | Technique ID | Technique Name | Example Procedure |
|---|---:|---|---|
| Initial Access | T1190 | Exploit Public-Facing Application | Exploits exposed applications and servers |
| Persistence | T1505.003 | Server Software Component: Web Shell | Deploys web shells such as ANTSWORD or BLUEBEAM |
| Execution | T1059 | Command and Scripting Interpreter | Runs commands through web shells and scripts |
| Defense Evasion | T1574.002 | Hijack Execution Flow: DLL Side-Loading | Uses DLL side-loading for stealthy execution |
| Defense Evasion | T1055 | Process Injection | Uses in-memory execution and injection |
| Command and Control | T1071.001 | Web Protocols | Uses HTTP/S for C2 |
| Command and Control | T1105 | Ingress Tool Transfer | Downloads additional payloads |
| Collection | T1005 | Data from Local System | Collects sensitive files |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Exfiltrates collected data through established channels |

---

## 🛠️ Signature Tools & Malware

| Tool / Malware | Type | Notes |
|---|---|---|
| **DUSTPAN** | In-Memory Dropper | Decrypts and executes embedded payloads |
| **DUSTTRAP** | Dropper / Loader | Associated with recent APT41 operations |
| **BEACON** | C2 Framework Payload | Used for post-exploitation activity |
| **ANTSWORD** | Web Shell | Used for persistence and command execution |
| **BLUEBEAM** | Web Shell | Observed in recent Mandiant reporting |
| **Winnti** | Backdoor / Malware Family | Historically associated with APT41 ecosystem |
| **ShadowPad** | Backdoor | Associated with Chinese APT operations and supply chain compromises |

---

## 🧾 Public IOCs / Pivot Points

| Indicator | Type | Context |
|---|---|---|
| `ns2[.]akacur[.]tk` | Domain | Publicly reported APT41-associated infrastructure |
| `ns1[.]akacur[.]tk` | Domain | Publicly reported APT41-associated infrastructure |
| `hxxp[:]//152[.]89[.]244[.]185/conn.exe` | URL | Publicly reported APT41-associated payload location |

---

## 🗓️ Notable Campaigns

<details>
<summary><strong>Global DUSTPAN / DUSTTRAP Campaign</strong></summary>

Google/Mandiant reported APT41 activity involving DUSTPAN, DUSTTRAP, web shells, and prolonged access across multiple global sectors.

</details>

<details>
<summary><strong>Software Supply Chain Compromises</strong></summary>

APT41 has been associated with compromising trusted software distribution paths and abusing legitimate updates.

</details>

<details>
<summary><strong>Gaming and Financially Motivated Intrusions</strong></summary>

The group has historically targeted the video game industry for source code, certificates, and virtual currency manipulation.

</details>

---

## 🛡️ Defensive Recommendations

| Priority | Recommendation | Why It Matters |
|---:|---|---|
| 1 | Harden internet-facing applications | Reduces web shell deployment risk |
| 2 | Monitor web directories for new files | Detects web shell placement |
| 3 | Alert on suspicious `certutil`, `powershell`, and script execution from web processes | Detects post-exploitation behavior |
| 4 | Hunt for DLL side-loading | Detects stealthy execution |
| 5 | Review supply chain and update mechanisms | Reduces trusted software compromise risk |
| 6 | Monitor unusual outbound web traffic from servers | Detects C2 and exfiltration |

---

## 📚 Sources

- MITRE ATT&CK — APT41 / G0096: https://attack.mitre.org/groups/G0096/
- Google/Mandiant — APT41 Has Arisen From the DUST: https://cloud.google.com/blog/topics/threat-intelligence/apt41-arisen-from-dust
- DOJ — APT41-related charges: https://www.justice.gov/opa/pr/seven-international-cyber-defendants-including-apt41-actors-charged-connection-computer

---

## ✅ Analyst Notes

APT41 detection should focus heavily on web shell behavior, application server telemetry, payload downloads, command execution from web services, DLL side-loading, and unusual long-term outbound traffic from servers.

---
