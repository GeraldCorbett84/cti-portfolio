# 🔎 OSINT Investigation Walkthrough: WHOIS and RDAP Research

> This walkthrough demonstrates a passive OSINT workflow for **whois and rdap research** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-WHOIS%20RDAP-blue)
![Threat](https://img.shields.io/badge/Threat-Phishing-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-rdap-lookup-registration-timeline)
- [Step 2](#-step-2-registration-pattern-review)
- [Step 3](#-step-3-brand-and-workflow-impersonation-check)
- [Step 4](#-step-4-registration-based-ioc-development)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `vpn-helpdesk-reset[.]com` |
| **Scenario** | A user reported a fake VPN reset email containing a suspicious domain. |
| **Investigation Type** | Domain registration enrichment |
| **Primary Concern** | Disposable domain registration and impersonation infrastructure |
| **Risk Rating** | High |
| **Recommended Disposition** | Treat as suspicious pending validation |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to determine whether `vpn-helpdesk-reset[.]com` has suspicious registration characteristics that support a phishing or social-engineering assessment.

Key questions:

- When was the domain registered?
- Is the registration privacy-protected?
- Which registrar and nameservers are used?
- Does the domain name mimic enterprise support workflows?
- Are there related domains registered with similar patterns?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Domain | `vpn-helpdesk-reset[.]com` | VPN/help desk-themed domain |
| Registrar | Namecheap-style low-cost registrar | Common in disposable infrastructure |
| Created | 2 days ago | Strong phishing indicator |
| Registrant | Privacy protected | Limits attribution |
| Nameservers | `ns1.cloudflare[.]com`, `ns2.cloudflare[.]com` | Origin masking / fast setup |
| Similar Domain | `vpn-ticket-reset[.]com` | Possible related registration |

---

## 🌐 Step 1: RDAP Lookup — Registration Timeline

**Tool:** RDAP / WHOIS lookup  
**Target:** `vpn-helpdesk-reset[.]com`

### What We Checked

We reviewed creation date, expiration date, registrar, registrant fields, and nameserver configuration.

### Observed Findings

| Field | Result |
|---|---|
| Created | 2 days ago |
| Expires | 1 year from creation |
| Registrar | Namecheap-style registrar |
| Registrant | Redacted for privacy |
| Nameservers | Cloudflare |

### Assessment

A newly registered, privacy-protected support-themed domain is a strong phishing signal, especially when paired with a VPN reset lure.

## 🧾 Step 2: Registration Pattern Review

**Tool:** WHOIS history / domain intelligence  
**Target:** Domain registration metadata

### What We Checked

We looked for repeated registrar, registrant, nameserver, and timing patterns across related domains.

### Observed Findings

| Field | Result |
|---|---|
| Related Domain | `vpn-ticket-reset[.]com` |
| Related Domain | `helpdesk-vpn-auth[.]net` |
| Creation Window | Same week |
| Name Pattern | VPN + help desk + reset/auth |
| Likely Use | Credential capture |

### Assessment

Repeated domain registration patterns can show campaign behavior even when registrant data is privacy protected.

## 🔍 Step 3: Brand and Workflow Impersonation Check

**Tool:** Manual OSINT review  
**Target:** Domain string

### What We Checked

We evaluated whether the domain name imitates a real internal workflow, such as VPN reset, help desk ticketing, or identity verification.

### Observed Findings

| Field | Result |
|---|---|
| Impersonated Workflow | VPN reset / help desk authentication |
| Social Engineering Theme | Urgent remote access problem |
| Target User Group | Remote employees |
| Potential Data Collected | Username, password, MFA code |
| Risk | High |

### Assessment

Attackers often use workflow-themed domains because they look plausible to users and can bypass quick analyst review.

## 🧩 Step 4: Registration-Based IOC Development

**Tool:** Threat intel platform / notes  
**Target:** Related registered domains

### What We Checked

We converted registration pivots into a monitorable IOC set.

### Observed Findings

| Field | Result |
|---|---|
| Domain | `vpn-helpdesk-reset[.]com` |
| Domain | `vpn-ticket-reset[.]com` |
| Domain | `helpdesk-vpn-auth[.]net` |
| Nameserver Pattern | Cloudflare |
| Creation Window | Same week |

### Assessment

Registration intelligence supports proactive monitoring for new domains using the same theme and setup pattern.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Domain | `vpn-helpdesk-reset[.]com` | VPN/help desk-themed suspicious domain |
| Created | 2 days ago | Fresh infrastructure |
| Registrar | Namecheap-style registrar | Common in disposable campaigns |
| Nameservers | Cloudflare | Masks origin and enables fast setup |
| Verdict | Likely phishing | Block and scope internally |

**Verdict:** Treat as suspicious pending validation.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Block the domain and related domains at DNS/proxy | SOC |
| 2 | Search email logs for the exact domain and similar strings | Email Security |
| 3 | Search identity logs for users authenticating after clicking VPN-reset lures | IAM |
| 4 | Create watchlist for new VPN/helpdesk/auth domains | CTI |
| 5 | Report abuse to registrar and hosting provider | CTI / Abuse Desk |
| 6 | Add workflow-themed phishing examples to awareness training | Security Awareness |

---

## 📚 Sources

- WHOIS / RDAP registration review
- DomainTools-style WHOIS history
- Internal email, DNS, and IAM telemetry
- Registrar abuse reporting workflow

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
