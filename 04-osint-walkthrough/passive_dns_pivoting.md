# 🔎 OSINT Investigation Walkthrough: Passive DNS Pivoting

> This walkthrough demonstrates a passive OSINT workflow for **passive dns pivoting** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-Passive%20DNS-blue)
![Threat](https://img.shields.io/badge/Threat-Phishing-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-passive-dns-lookup-resolution-history)
- [Step 2](#-step-2-shared-infrastructure-pivot)
- [Step 3](#-step-3-naming-pattern-analysis)
- [Step 4](#-step-4-ioc-expansion)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `login-m365-security[.]com` |
| **Scenario** | A suspicious Microsoft-themed domain was found in a phishing report. |
| **Investigation Type** | Passive DNS enrichment |
| **Primary Concern** | Shared infrastructure and related phishing domains |
| **Risk Rating** | High |
| **Recommended Disposition** | Treat as suspicious pending validation |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to determine whether `login-m365-security[.]com` is part of a larger phishing cluster by using passive DNS to identify historical resolutions, shared IP infrastructure, and related domains.

Key questions:

- What IP addresses has the domain resolved to?
- Are other suspicious domains hosted on the same IP?
- Did the infrastructure appear recently?
- Are there naming patterns that suggest a phishing kit or actor-controlled cluster?
- What additional IOCs should be added to the case?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Domain | `login-m365-security[.]com` | Microsoft-themed login domain |
| Current IP | `45.83.64[.]19` | Shared hosting infrastructure |
| Historical IP | `185.225.73[.]44` | Prior resolution observed |
| Related Domain | `secure-o365-verify[.]com` | Same IP and login theme |
| Related Domain | `microsoft-auth-check[.]net` | Related naming pattern |
| First Seen | 4 days ago | Fresh infrastructure indicator |

---

## 🧪 Step 1: Passive DNS Lookup — Resolution History

**Tool:** Passive DNS provider such as SecurityTrails, RiskIQ/PassiveTotal, VirusTotal, or DomainTools  
**Target:** `login-m365-security[.]com`

### What We Checked

We reviewed A/AAAA history, first-seen dates, last-seen dates, and related hostnames that resolved to the same infrastructure.

### Observed Findings

| Field | Result |
|---|---|
| Current A Record | `45.83.64[.]19` |
| Previous A Record | `185.225.73[.]44` |
| First Seen | 4 days ago |
| Last Seen | Current |
| Resolution Pattern | Short-lived rotation between two VPS providers |

### Assessment

The short domain age and rapid IP movement are consistent with disposable phishing infrastructure. Passive DNS gives the analyst a way to understand the campaign without browsing directly to the domain.

## 🌐 Step 2: Shared Infrastructure Pivot

**Tool:** Passive DNS reverse-IP or co-hosted domain search  
**Target:** `45.83.64[.]19`

### What We Checked

We pivoted from the IP address to other domains that recently resolved to the same host.

### Observed Findings

| Field | Result |
|---|---|
| Related Domain | `secure-o365-verify[.]com` |
| Related Domain | `microsoft-auth-check[.]net` |
| Related Domain | `office365-session[.]org` |
| Theme | Microsoft 365 authentication |
| Time Window | All observed within one week |

### Assessment

Multiple Microsoft-themed domains appearing on the same IP within a short time window indicates likely campaign infrastructure rather than a one-off suspicious domain.

## 🧭 Step 3: Naming Pattern Analysis

**Tool:** Manual analysis of domain strings and pivots  
**Target:** Related domain cluster

### What We Checked

We reviewed lexical patterns across related domains.

### Observed Findings

| Field | Result |
|---|---|
| Brand Terms | `microsoft`, `m365`, `office365`, `o365` |
| Action Terms | `login`, `verify`, `auth`, `session` |
| TLD Mix | `.com`, `.net`, `.org` |
| Likely Purpose | Credential harvesting |
| Confidence | Medium-High |

### Assessment

The repeated use of authentication terms suggests the infrastructure is designed to capture credentials, not simply host generic malware.

## 🧩 Step 4: IOC Expansion

**Tool:** Internal case notes / TIP enrichment  
**Target:** All related domains and IPs

### What We Checked

We converted the pivot results into a defender-ready IOC set.

### Observed Findings

| Field | Result |
|---|---|
| Domain | `login-m365-security[.]com` |
| Domain | `secure-o365-verify[.]com` |
| Domain | `microsoft-auth-check[.]net` |
| Domain | `office365-session[.]org` |
| IP | `45.83.64[.]19` |

### Assessment

Passive DNS transformed a single suspicious domain into a small infrastructure cluster that can be searched across DNS, proxy, firewall, and email telemetry.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Primary Domain | `login-m365-security[.]com` | Suspicious Microsoft-themed domain |
| Current IP | `45.83.64[.]19` | Shared hosting infrastructure |
| Related Domain | `secure-o365-verify[.]com` | Same infrastructure and theme |
| Related Domain | `microsoft-auth-check[.]net` | Likely same campaign |
| Verdict | Suspicious / likely phishing | Requires blocking and internal scoping |

**Verdict:** Treat as suspicious pending validation.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Block related domains at DNS and proxy layers | SOC / Network Security |
| 2 | Search DNS logs for all identified domains | SOC |
| 3 | Search email logs for inbound messages containing these domains | Email Security |
| 4 | Identify users who resolved or visited the domains | SOC / IAM |
| 5 | Submit the cluster to internal threat intel platform | CTI |
| 6 | Monitor for new domains sharing the same naming pattern | CTI / Detection Engineering |

---

## 📚 Sources

- SecurityTrails-style passive DNS workflow
- VirusTotal domain relations
- DomainTools / RiskIQ-style infrastructure pivoting
- Internal DNS and proxy telemetry

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
