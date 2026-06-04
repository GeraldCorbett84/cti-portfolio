# 🔎 OSINT Investigation Walkthrough: Certificate Transparency Hunting

> This walkthrough demonstrates a passive OSINT workflow for **certificate transparency hunting** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-Certificate%20Transparency-blue)
![Threat](https://img.shields.io/badge/Threat-Brand%20Impersonation-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-ct-log-search-brand-and-auth-terms)
- [Step 2](#-step-2-certificate-clustering)
- [Step 3](#-step-3-passive-page-validation)
- [Step 4](#-step-4-defensive-ioc-package)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `*.corp-login-verification[.]com` |
| **Scenario** | CT logs revealed newly issued certificates for domains impersonating an enterprise login workflow. |
| **Investigation Type** | Certificate transparency monitoring |
| **Primary Concern** | Phishing domains discovered before user reports |
| **Risk Rating** | High |
| **Recommended Disposition** | Monitor, block, and validate for takedown |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to use Certificate Transparency logs to identify suspicious certificates issued for domains that impersonate corporate login, VPN, Okta, Microsoft 365, or help desk workflows.

Key questions:

- What suspicious certificates were issued recently?
- Which domains use brand or authentication terms?
- Are certificates clustered by issuer, timestamp, or naming pattern?
- Are any domains already active?
- Which domains should be blocked or reported?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Certificate CN/SAN | `corp-login-verification[.]com` | Authentication-themed domain |
| Related SAN | `okta-corp-login[.]com` | Identity provider impersonation |
| Related SAN | `vpn-corp-access[.]net` | Remote access theme |
| Issuer | Let's Encrypt | Common for fast phishing setup |
| Issue Time | Within last 24 hours | Possible pre-launch infrastructure |
| Risk | High | Likely phishing preparation |

---

## 📜 Step 1: CT Log Search — Brand and Auth Terms

**Tool:** crt.sh, Censys Certificates, or Google CT-style search  
**Target:** Brand and login-related keywords

### What We Checked

We searched for certificate subjects and SAN entries containing brand terms plus `login`, `verify`, `vpn`, `sso`, `okta`, and `m365`.

### Observed Findings

| Field | Result |
|---|---|
| Domain | `corp-login-verification[.]com` |
| Domain | `okta-corp-login[.]com` |
| Domain | `vpn-corp-access[.]net` |
| Issuer | Let's Encrypt |
| Issued | Last 24 hours |

### Assessment

CT logs can reveal phishing infrastructure before the first user reports a suspicious email.

## 🧭 Step 2: Certificate Clustering

**Tool:** Censys certificate pivots / CT search  
**Target:** Issuer, SAN entries, and issue timestamp

### What We Checked

We grouped certificates by issue time, naming pattern, and shared certificate metadata.

### Observed Findings

| Field | Result |
|---|---|
| Cluster Size | 6 related domains |
| Common Theme | SSO and VPN login |
| Issuer | Let's Encrypt |
| Time Window | All issued within 90 minutes |
| Likely Purpose | Phishing kit staging |

### Assessment

A tight issuance window and similar naming patterns suggest coordinated infrastructure setup.

## 🖼️ Step 3: Passive Page Validation

**Tool:** URLScan.io / screenshot service  
**Target:** Newly discovered domains

### What We Checked

We used passive page-analysis tools to determine whether the domains were serving login pages without browsing directly from a corporate host.

### Observed Findings

| Field | Result |
|---|---|
| Active Site | `okta-corp-login[.]com` |
| Page Title | Corporate SSO Verification |
| Login Form | Present |
| External Assets | Okta-style CSS references |
| Collection Endpoint | `/submit.php` |

### Assessment

The active page content confirms that at least one CT-discovered domain is likely being used for credential harvesting.

## 📌 Step 4: Defensive IOC Package

**Tool:** Threat intel platform / SIEM case  
**Target:** Discovered certificate domains

### What We Checked

We prepared a blocklist and monitoring package based on CT findings.

### Observed Findings

| Field | Result |
|---|---|
| Domain | `corp-login-verification[.]com` |
| Domain | `okta-corp-login[.]com` |
| Domain | `vpn-corp-access[.]net` |
| Endpoint | `/submit.php` |
| Tactic | Brand impersonation |

### Assessment

CT hunting provides early warning and gives defenders time to block, monitor, and request takedowns before phishing reaches users.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Primary Domain | `corp-login-verification[.]com` | Suspicious certificate subject |
| Related Domain | `okta-corp-login[.]com` | Identity impersonation |
| Related Domain | `vpn-corp-access[.]net` | Remote access impersonation |
| Issuer | Let's Encrypt | Fast, common certificate issuance |
| Verdict | Likely phishing setup | Block and monitor |

**Verdict:** Monitor, block, and validate for takedown.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Block CT-discovered domains at DNS/proxy | SOC |
| 2 | Monitor CT logs daily for brand + auth keywords | CTI |
| 3 | Search email logs for newly discovered domains | Email Security |
| 4 | Submit active phishing pages for takedown | CTI / Abuse Desk |
| 5 | Create detections for visits to CT-discovered domains | Detection Engineering |
| 6 | Document naming patterns for future hunts | CTI |

---

## 📚 Sources

- crt.sh-style certificate transparency search
- Censys certificate search
- URLScan passive page validation
- Internal DNS and email telemetry

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
