# 🔎 OSINT Investigation Walkthrough: Suspicious Domain

> This walkthrough demonstrates a passive OSINT workflow for investigating a suspicious phishing domain and turning raw enrichment results into a defender-ready assessment.

![Investigation Type](https://img.shields.io/badge/Investigation-Domain%20OSINT-blue)
![Threat Type](https://img.shields.io/badge/Threat-Phishing-orange)
![Risk Rating](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1: VirusTotal](#-step-1-virustotal--reputation-check)
- [Step 2: WHOIS](#-step-2-whois--registration-intelligence)
- [Step 3: Shodan](#-step-3-shodan--infrastructure-analysis)
- [Step 4: URLScan.io](#-step-4-urlscanio--live-page-analysis)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `secure-banklogin[.]net` |
| **Scenario** | User reported a phishing email containing this domain |
| **Investigation Type** | Passive OSINT enrichment |
| **Primary Concern** | Credential harvesting / bank impersonation |
| **Risk Rating** | High |
| **Recommended Disposition** | Treat as malicious pending validation |

> [!WARNING]
> Do not browse directly to suspected phishing domains from a corporate endpoint. Use passive tools such as VirusTotal, URLScan, WHOIS, and sandboxed analysis.

---

## 🎯 Investigation Objective

The goal is to determine whether `secure-banklogin[.]net` is malicious, identify related infrastructure, assess user risk, and produce actionable defensive recommendations.

Key questions:

- Is the domain known to security vendors?
- When was it registered?
- What IP or infrastructure hosts it?
- Does it impersonate a real brand?
- Are there additional related IOCs?
- What actions should the SOC take immediately?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Vendor Detections | 14/92 vendors flagged malicious | Suspicious but not universally detected |
| Domain Age | 3 days old | Strong phishing indicator |
| Hosting IP | `185.220.101.47` | Shared suspicious infrastructure |
| Related Domains | `firstnational-secure[.]com`, `accounts-loginverify[.]net` | Possible phishing kit cluster |
| Collection Endpoint | `/collect.php` | Credential harvesting indicator |
| SSL Certificate | Let's Encrypt | Common for disposable phishing infrastructure |

---

## 🧪 Step 1: VirusTotal — Reputation Check

**Tool:** VirusTotal domain lookup  
**Target:** `secure-banklogin[.]net`

### What We Checked

VirusTotal was used to review vendor detections, passive DNS, related URLs, and first-seen information.

### Observed Findings

| Field | Result |
|---|---|
| Vendor Detection | 14/92 security vendors flagged the domain |
| Category | Phishing / malicious site |
| First Seen | 3 days ago |
| Associated IP | `185.220.101.47` |
| Related Paths | `/login/verify`, `/account/update` |

### Assessment

The vendor count alone is not the only issue. The combination of **new domain age**, **bank-themed naming**, and **login-related URL paths** strongly supports phishing activity.

> [!NOTE]
> Low or moderate vendor detection does not mean a domain is safe. Fresh phishing infrastructure often appears before blocklists fully catch up.

---

## 🌐 Step 2: WHOIS — Registration Intelligence

**Tool:** WHOIS / registrar lookup  
**Target:** `secure-banklogin[.]net`

### Observed Findings

```text
Domain: secure-banklogin.net
Registrar: Namecheap, Inc.
Registered: 2024-03-18
Expires: 2025-03-18
Registrant: Redacted / privacy protected
Registrant Country: PA
Name Servers:
  ns1.cloudflare.com
  ns2.cloudflare.com
```

### Assessment

The registration pattern is consistent with disposable phishing infrastructure.

| Finding | Why It Matters |
|---|---|
| Newly registered domain | Phishing domains are often created shortly before use |
| Privacy-protected registration | Reduces attribution and slows takedown |
| Cloudflare nameservers | Can mask origin hosting and provide free SSL |
| One-year registration | Often consistent with low-cost disposable infrastructure |

---

## 🛰️ Step 3: Shodan — Infrastructure Analysis

**Tool:** Shodan host lookup  
**Target IP:** `185.220.101.47`

### Observed Findings

```text
IP: 185.220.101.47
Organization: Frantech Solutions
Country: Luxembourg
Open Ports:
  80  / HTTP
  443 / HTTPS
  22  / SSH
Hostnames:
  secure-banklogin.net
  firstnational-secure[.]com
  accounts-loginverify[.]net
```

### Assessment

The presence of multiple suspicious banking-themed domains on the same IP indicates this may be a broader phishing kit or campaign infrastructure rather than a single isolated domain.

**New IOCs discovered:**

- `firstnational-secure[.]com`
- `accounts-loginverify[.]net`
- `185.220.101.47`

---

## 🖼️ Step 4: URLScan.io — Live Page Analysis

**Tool:** URLScan.io  
**Target:** `secure-banklogin[.]net`

### Observed Findings

| Field | Finding |
|---|---|
| Page Title | `First National Bank — Secure Login` |
| Visual Appearance | Bank login clone |
| Credential Endpoint | `185.220.101.47/collect.php` |
| External Requests | Legitimate CSS pulled from bank-themed source |
| Certificate | Let's Encrypt |
| Technology | PHP / Bootstrap-style page |

### Assessment

The URLScan evidence supports credential harvesting. The `/collect.php` endpoint is especially important because it indicates where submitted credentials may be sent.

> [!WARNING]
> This is not just suspicious infrastructure. The combination of cloned branding, login paths, and credential collection endpoint supports a **malicious phishing verdict**.

---

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Domain | `secure-banklogin[.]net` | Active phishing domain |
| IP | `185.220.101.47` | Hosting infrastructure |
| Related Domain | `firstnational-secure[.]com` | Possible same actor / phishing kit |
| Related Domain | `accounts-loginverify[.]net` | Possible same actor / phishing kit |
| Endpoint | `/collect.php` | Credential harvesting endpoint |
| Registrar | Namecheap with privacy protection | Disposable infrastructure pattern |

**Verdict:** Confirmed phishing infrastructure.  
**Confidence:** High.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Block all identified domains and IP at email gateway, DNS, web proxy, and firewall | SOC / Network Security |
| 2 | Search email logs for messages containing the domain or related URLs | Email Security |
| 3 | Identify users who clicked the link or submitted credentials | SOC / IAM |
| 4 | Force password reset and session revocation for impacted users | IAM |
| 5 | Report domains to registrar, Google Safe Browsing, and brand abuse teams | CTI / Abuse Desk |
| 6 | Submit IOCs to relevant ISAC or internal threat intel platform | CTI |

---

## ✅ Analyst Notes

This walkthrough is designed for GitHub portfolio display and demonstrates a safe, passive OSINT workflow. The value is not just listing tools — it is explaining **what each finding means, how it changes the assessment, and what defenders should do next**.
