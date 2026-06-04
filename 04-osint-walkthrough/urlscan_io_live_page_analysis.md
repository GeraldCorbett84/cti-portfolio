# 🔎 OSINT Investigation Walkthrough: URLScan.io Live Page Analysis

> This walkthrough demonstrates a passive OSINT workflow for **urlscan.io live page analysis** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-URLScan-blue)
![Threat](https://img.shields.io/badge/Threat-Credential%20Harvesting-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-urlscan-submission-review)
- [Step 2](#-step-2-redirect-and-request-chain)
- [Step 3](#-step-3-ioc-extraction)
- [Step 4](#-step-4-user-impact-assessment)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `account-review-m365[.]net` |
| **Scenario** | A suspicious Microsoft 365 account review URL was reported by an employee. |
| **Investigation Type** | Passive page analysis |
| **Primary Concern** | Cloned login page and credential submission endpoint |
| **Risk Rating** | High |
| **Recommended Disposition** | Treat as malicious |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to use URLScan-style analysis to safely inspect the page behavior, identify external requests, determine whether the page impersonates a brand, and extract related IOCs.

Key questions:

- What does the page look like?
- Does it contain a login form?
- Where are credentials submitted?
- What domains and IPs are loaded by the page?
- Are there additional URLs, scripts, or redirects?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| URL | `hxxps://account-review-m365[.]net/login` | Microsoft 365-themed URL |
| Page Title | `Microsoft Account Review` | Brand impersonation |
| Form Action | `/api/submit` | Credential collection endpoint |
| Redirect | `hxxps://login.microsoftonline[.]com` | Post-capture legitimacy redirect |
| Hosting IP | `91.92.240[.]18` | Infrastructure IOC |
| Risk | High | Credential harvesting indicators present |

---

## 🖼️ Step 1: URLScan Submission Review

**Tool:** URLScan.io  
**Target:** `hxxps://account-review-m365[.]net/login`

### What We Checked

We reviewed the screenshot, page title, request chain, DOM indicators, and external resource requests.

### Observed Findings

| Field | Result |
|---|---|
| Page Title | Microsoft Account Review |
| Visual | Microsoft-style login clone |
| Login Form | Present |
| Hosting IP | `91.92.240[.]18` |
| Certificate | Let's Encrypt |

### Assessment

The visual clone and Microsoft-themed language strongly indicate credential harvesting.

## 🔁 Step 2: Redirect and Request Chain

**Tool:** URLScan request tab  
**Target:** Network requests

### What We Checked

We inspected redirects, POST endpoints, script sources, and final landing page.

### Observed Findings

| Field | Result |
|---|---|
| Credential Endpoint | `/api/submit` |
| Post-Capture Redirect | `login.microsoftonline[.]com` |
| External Script | `cdn-jquery-cache[.]com/app.js` |
| HTTP Method | POST |
| User Impact | Credential theft risk |

### Assessment

The attacker likely captures credentials and then redirects the victim to a real Microsoft page to reduce suspicion.

## 🧩 Step 3: IOC Extraction

**Tool:** URLScan indicators tab  
**Target:** Domains, IPs, URLs, and paths

### What We Checked

We extracted all page-related observables for defensive action.

### Observed Findings

| Field | Result |
|---|---|
| Domain | `account-review-m365[.]net` |
| IP | `91.92.240[.]18` |
| URL | `hxxps://account-review-m365[.]net/login` |
| Path | `/api/submit` |
| Domain | `cdn-jquery-cache[.]com` |

### Assessment

URLScan can turn a single user-reported URL into several network indicators for blocklisting and hunting.

## 🛡️ Step 4: User Impact Assessment

**Tool:** SIEM / proxy / identity logs  
**Target:** Internal telemetry

### What We Checked

We prepared internal searches for users who visited the URL or submitted data.

### Observed Findings

| Field | Result |
|---|---|
| Proxy Search | Domain and full URL |
| DNS Search | Domain and IP |
| Identity Search | Failed/successful logins after click time |
| Email Search | Messages containing the URL |
| Action | Password reset if submitted |

### Assessment

The OSINT result should be connected to internal telemetry to determine actual exposure.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| URL | `hxxps://account-review-m365[.]net/login` | Reported phishing URL |
| Domain | `account-review-m365[.]net` | Microsoft-themed phishing domain |
| Endpoint | `/api/submit` | Credential capture path |
| Redirect | `login.microsoftonline[.]com` | Post-capture trust-building redirect |
| Verdict | Confirmed phishing | Block, scope, and reset impacted users |

**Verdict:** Treat as malicious.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Block domain, URL, and extracted infrastructure | SOC |
| 2 | Search proxy logs for visits to the URL | SOC |
| 3 | Search email gateway for messages containing the domain | Email Security |
| 4 | Review identity logs for suspicious sign-ins after click time | IAM |
| 5 | Force password reset and session revocation for exposed users | IAM |
| 6 | Submit URL to takedown and safe browsing services | CTI / Abuse Desk |

---

## 📚 Sources

- URLScan.io-style passive page analysis
- Internal proxy and DNS logs
- Microsoft 365 / identity sign-in telemetry
- Email gateway telemetry

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
