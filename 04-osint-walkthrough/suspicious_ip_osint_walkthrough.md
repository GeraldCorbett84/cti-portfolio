# 🔎 OSINT Investigation Walkthrough: Suspicious IP

> This walkthrough demonstrates a passive OSINT workflow for investigating a suspicious outbound connection from an internal workstation to a potentially malicious command-and-control IP.

![Investigation Type](https://img.shields.io/badge/Investigation-IP%20OSINT-blue)
![Threat Type](https://img.shields.io/badge/Threat-C2%20Beaconing-red)
![Risk Rating](https://img.shields.io/badge/Risk-Critical-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1: VirusTotal](#-step-1-virustotal--reputation-and-context)
- [Step 2: WHOIS](#-step-2-whois--ownership-research)
- [Step 3: Shodan](#-step-3-shodan--port-and-service-analysis)
- [Step 4: URLScan.io](#-step-4-urlscanio--historical-scan-data)
- [Investigation Summary](#-investigation-summary--incident-response)
- [Immediate Actions](#-immediate-actions)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target IP** | `91.108.4.203` |
| **Scenario** | SIEM alert for repeated outbound connections from internal workstation to IP over port 443 |
| **Investigation Type** | Passive IP OSINT and infrastructure enrichment |
| **Primary Concern** | Possible Cobalt Strike / C2 beaconing |
| **Risk Rating** | Critical |
| **Recommended Disposition** | Escalate as suspected active intrusion |

> [!WARNING]
> Repeated outbound connections to confirmed or suspected C2 infrastructure should be treated as a potential active compromise until disproven.

---

## 🎯 Investigation Objective

The goal is to determine whether `91.108.4.203` represents malicious infrastructure, assess whether the internal workstation may be compromised, and define immediate containment and scoping actions.

Key questions:

- Is the IP known to security vendors?
- Is it associated with malware or C2 frameworks?
- What services are exposed?
- Are there Cobalt Strike indicators?
- How far back should telemetry be reviewed?
- What containment steps should the SOC take?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Vendor Detections | 22/92 vendors flagged malicious | Strong malicious signal |
| Malware Tags | C2, Emotet, Cobalt Strike | Indicates potential intrusion activity |
| Open Ports | 443, 50050, 8080 | Possible Cobalt Strike service pattern |
| Certificate | Self-signed, Cobalt Strike-themed CN | Strong C2 infrastructure indicator |
| Related URLs | `/jquery-3.3.1.min.js` | Malleable C2 profile mimicry |
| First Seen | 31 days ago | Supports historical telemetry review |

---

## 🧪 Step 1: VirusTotal — Reputation and Context

**Tool:** VirusTotal IP lookup  
**Target:** `91.108.4.203`

### Observed Findings

| Field | Result |
|---|---|
| Vendor Detection | 22/92 security vendors flagged malicious |
| Tags | C2, Emotet, Cobalt Strike |
| Associated Domains | `update-service[.]org`, `cdn-telecom[.]net` |
| Last Seen | Active today |
| WHOIS Country | Netherlands |

### Assessment

The vendor detection count is concerning, but the malware tags are more important. Cobalt Strike is commonly used for post-exploitation, lateral movement preparation, and ransomware precursor activity.

**Priority:** Escalate immediately and isolate the affected workstation while preserving forensic evidence.

---

## 🌐 Step 2: WHOIS — Ownership Research

**Tool:** WHOIS / IP ownership lookup  
**Target:** `91.108.4.203`

### Observed Findings

```text
NetRange: 91.108.0.0 - 91.108.7.255
Organization: Serverius Holding B.V.
Country: Netherlands
Abuse Contact: abuse@serverius.net
ASN: AS50673
```

### Assessment

The hosting provider context helps determine whether this is isolated infrastructure or part of a broader malicious hosting block. The next step is to pivot across adjacent infrastructure and determine whether other nearby IPs are also flagged.

| Finding | Why It Matters |
|---|---|
| Hosting provider | Helps with abuse reporting and infrastructure pivoting |
| ASN | Allows broader reputation and clustering analysis |
| NetRange | Supports scoped investigation of nearby IPs |
| Abuse Contact | Provides takedown or reporting path |

---

## 🛰️ Step 3: Shodan — Port and Service Analysis

**Tool:** Shodan host lookup  
**Target:** `91.108.4.203`

### Observed Findings

```text
IP: 91.108.4.203
Open Ports:
  443   HTTPS - Self-signed certificate
  50050 TCP   - Cobalt Strike Team Server default port
  8080  HTTP  - Cobalt Strike listener
SSL Certificate:
  CN=Major Cobalt Strike
  Issuer=Major Cobalt Strike self-signed
```

### Assessment

Port `50050` and a self-signed certificate resembling known Cobalt Strike defaults are high-confidence indicators of operator-controlled C2 infrastructure.

> [!WARNING]
> If an internal workstation is repeatedly connecting to this IP, the host should be treated as compromised until endpoint evidence proves otherwise.

---

## 🖼️ Step 4: URLScan.io — Historical Scan Data

**Tool:** URLScan.io  
**Target:** `91.108.4.203`

### Observed Findings

| Field | Finding |
|---|---|
| Scan Count | 14 scans over 30 days |
| Associated URL | `hxxps://91.108.4.203/jquery-3.3.1.min.js` |
| Pattern | C2 traffic disguised as jQuery library request |
| First Seen | 31 days ago |
| Scan Locations | Germany, UK, USA |

### Assessment

The `jquery-3.3.1.min.js` path is consistent with malleable C2 profile behavior, where malicious beacon traffic is disguised as normal web asset traffic.

**Telemetry implication:** Pull at least 31+ days of EDR, DNS, proxy, and firewall logs for the affected workstation.

---

## 📊 Investigation Summary & Incident Response

| Indicator | Value | Significance |
|---|---|---|
| IP | `91.108.4.203` | Confirmed / suspected Cobalt Strike C2 |
| Domain | `update-service[.]org` | Associated C2 domain |
| Domain | `cdn-telecom[.]net` | Associated C2 domain |
| Port | `50050` | Cobalt Strike Team Server port |
| C2 Profile | `/jquery-3.3.1.min.js` | Malleable C2 masquerading as jQuery |

**Verdict:** Suspected active Cobalt Strike C2.  
**Confidence:** High.  
**Recommended Severity:** Critical.

---

## 🚨 Immediate Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Isolate the affected workstation from the network without powering it off | SOC / IT Operations |
| 2 | Preserve volatile evidence and collect memory if forensic process allows | DFIR |
| 3 | Pull EDR telemetry covering at least 31+ days | SOC / Detection Engineering |
| 4 | Search for lateral movement from the affected workstation | SOC / IR |
| 5 | Block IP and associated domains at firewall, proxy, and DNS layers | Network Security |
| 6 | Escalate to incident response leadership | SOC Manager / IR Lead |

### Additional Hunt Focus

- Outbound connections to `91.108.4.203` over ports 443, 8080, or 50050
- Similar requests to `/jquery-3.3.1.min.js`
- PowerShell, rundll32, or regsvr32 activity before first beacon
- New scheduled tasks or services on the affected workstation
- SMB/RDP connections from the workstation to internal systems

---

## ✅ Analyst Notes

This walkthrough is designed for GitHub portfolio use and demonstrates how a CTI/SOC analyst can move from a single IP alert to a full operational assessment. The key skill shown is **turning enrichment into incident response decisions**.
