# 🔎 OSINT Investigation Walkthrough: Typosquatting and Brand Monitoring

> This walkthrough demonstrates a passive OSINT workflow for **typosquatting and brand monitoring** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-Brand%20Monitoring-blue)
![Threat](https://img.shields.io/badge/Threat-Typosquatting-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-generate-lookalike-patterns)
- [Step 2](#-step-2-certificate-and-dns-validation)
- [Step 3](#-step-3-passive-content-review)
- [Step 4](#-step-4-brand-protection-package)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `examplecorp` |
| **Scenario** | The CTI team is monitoring for domains impersonating the company brand and login workflows. |
| **Investigation Type** | Brand impersonation monitoring |
| **Primary Concern** | Credential harvesting and customer impersonation |
| **Risk Rating** | High |
| **Recommended Disposition** | Block suspicious domains and initiate takedown when active |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to identify domains that impersonate the organization or its login workflows using typosquatting, lookalike words, homoglyphs, or suspicious authentication terms.

Key questions:

- What new domains resemble the brand?
- Are they using login, support, VPN, HR, invoice, or payment themes?
- Are any active or hosting cloned content?
- Are certificates issued for suspicious lookalikes?
- Which domains require blocking or takedown?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Brand Term | `examplecorp` | Protected brand keyword |
| Suspicious Domain | `examp1ecorp-login[.]com` | Digit substitution |
| Suspicious Domain | `examplecorp-support[.]net` | Support impersonation |
| Suspicious Domain | `examplecorp-payments[.]org` | Payment-themed domain |
| Certificate | Issued for support-themed domain | Possible activation |
| Risk | High | Credential or payment fraud potential |

---

## 🔤 Step 1: Generate Lookalike Patterns

**Tool:** DNSTwist-style permutation logic  
**Target:** `examplecorp`

### What We Checked

We generated common typo, substitution, insertion, hyphenation, and TLD variants of the brand name.

### Observed Findings

| Field | Result |
|---|---|
| Substitution | `examp1ecorp-login[.]com` |
| Hyphenation | `examplecorp-support[.]net` |
| Theme | `examplecorp-payments[.]org` |
| Potential Homoglyph | `examp1ecorp` |
| Risk | Brand impersonation |

### Assessment

Lookalike patterns can identify phishing infrastructure before it is reported by users or customers.

## 📜 Step 2: Certificate and DNS Validation

**Tool:** CT logs and passive DNS  
**Target:** Generated domain candidates

### What We Checked

We checked which generated domains were registered, had certificates, or resolved to IP addresses.

### Observed Findings

| Field | Result |
|---|---|
| Registered | `examplecorp-support[.]net` |
| Certificate Issued | `examplecorp-support[.]net` |
| Resolves | `examp1ecorp-login[.]com` |
| No DNS | `examplecorp-payments[.]org` |
| Priority | Support and login domains |

### Assessment

Registered domains with active DNS or certificates should be prioritized for blocking and takedown review.

## 🖼️ Step 3: Passive Content Review

**Tool:** URLScan.io / screenshot lookup  
**Target:** Active lookalike domains

### What We Checked

We reviewed screenshots, titles, redirects, forms, and external requests.

### Observed Findings

| Field | Result |
|---|---|
| Active Page | `examplecorp-support[.]net` |
| Page Title | ExampleCorp Help Desk |
| Login Form | Present |
| Redirect | Real brand website after submit |
| Verdict | Likely phishing |

### Assessment

A branded help desk page with a login form strongly supports a credential-harvesting assessment.

## 📦 Step 4: Brand Protection Package

**Tool:** TIP / abuse workflow  
**Target:** Confirmed and suspected lookalikes

### What We Checked

We prepared an IOC package for blocking, monitoring, and abuse reporting.

### Observed Findings

| Field | Result |
|---|---|
| Domain | `examplecorp-support[.]net` |
| Domain | `examp1ecorp-login[.]com` |
| Theme | Support / login |
| Takedown | Recommended |
| Monitoring | Continue for brand + auth terms |

### Assessment

Brand monitoring is most useful when it connects discovery to immediate block, user protection, and takedown action.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Brand | `examplecorp` | Protected brand term |
| Lookalike | `examp1ecorp-login[.]com` | Digit substitution and login theme |
| Lookalike | `examplecorp-support[.]net` | Support impersonation |
| Active Page | Help desk login clone | Credential harvesting risk |
| Verdict | Likely brand abuse | Block and submit takedown |

**Verdict:** Block suspicious domains and initiate takedown when active.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Block confirmed lookalike domains | SOC / Network Security |
| 2 | Submit active phishing pages for takedown | CTI / Legal |
| 3 | Search email and proxy logs for user exposure | SOC |
| 4 | Monitor CT logs for brand + login/support terms | CTI |
| 5 | Create brand-abuse alerting rules | Detection Engineering |
| 6 | Notify customer support or fraud teams if customers may be targeted | Fraud / Support |

---

## 📚 Sources

- DNSTwist-style domain permutation
- Certificate Transparency monitoring
- Passive DNS enrichment
- URLScan-style page analysis

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
