# 🔎 OSINT Investigation Walkthrough: Ransom Leak Site Monitoring

> This walkthrough demonstrates a passive OSINT workflow for **ransom leak site monitoring** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-Ransomware%20Monitoring-blue)
![Threat](https://img.shields.io/badge/Threat-Third--Party%20Risk-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-leak-site-claim-capture)
- [Step 2](#-step-2-vendor-relationship-check)
- [Step 3](#-step-3-proof-of-leak-triage)
- [Step 4](#-step-4-internal-notification-and-tracking)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `ExampleCorp vendor exposure` |
| **Scenario** | A ransomware leak site post names a third-party vendor used by the organization. |
| **Investigation Type** | Ransomware victimology monitoring |
| **Primary Concern** | Vendor compromise and possible downstream data exposure |
| **Risk Rating** | High |
| **Recommended Disposition** | Escalate to vendor risk and legal teams |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to monitor ransomware leak site claims for mentions of the organization, subsidiaries, vendors, customers, or sector peers and turn claims into a risk-based triage package.

Key questions:

- Is the organization or vendor named by a ransomware group?
- What data is claimed?
- Is there proof-of-leak evidence?
- Is the vendor in the organization’s supply chain?
- What internal teams must be notified?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Named Entity | `ExampleCorp Vendor A` | Third-party vendor |
| Claimed Actor | Ransomware leak site operator | Unverified claim |
| Claimed Data | HR and customer support documents | Possible sensitive data |
| Proof Sample | Small screenshot set | Requires validation |
| Relationship | Vendor used for support operations | Potential downstream risk |
| Risk | High | Third-party exposure concern |

---

## 🕵️ Step 1: Leak Site Claim Capture

**Tool:** Ransomware leak monitoring platform / passive collection  
**Target:** Vendor and organization names

### What We Checked

We captured the actor name, victim name, post date, claimed data categories, and any proof-of-leak sample metadata.

### Observed Findings

| Field | Result |
|---|---|
| Victim Name | `ExampleCorp Vendor A` |
| Actor | Ransomware leak site operator |
| Post Date | Recent |
| Claimed Data | HR and support records |
| Proof | Screenshots posted |

### Assessment

Leak site claims should be treated as unverified until validated, but they require fast triage when vendors or sensitive data are involved.

## 🏢 Step 2: Vendor Relationship Check

**Tool:** Vendor inventory / procurement records  
**Target:** Named vendor

### What We Checked

We reviewed whether the named entity is an active vendor, what services it provides, and what data it may access.

### Observed Findings

| Field | Result |
|---|---|
| Vendor Status | Active |
| Service | Customer support tooling |
| Data Access | Customer tickets and limited PII |
| Business Owner | Customer Operations |
| Risk | High |

### Assessment

The impact depends less on the ransomware actor claim and more on what data the vendor handles for the organization.

## 📄 Step 3: Proof-of-Leak Triage

**Tool:** Passive review only / legal-approved workflow  
**Target:** Posted proof sample

### What We Checked

We reviewed visible metadata and document names without downloading sensitive stolen data unless approved by legal.

### Observed Findings

| Field | Result |
|---|---|
| Visible Logos | Vendor branding |
| Document Names | Support exports |
| Dates | Current year |
| Customer Data | Possible |
| Legal Review | Required before deeper handling |

### Assessment

Analysts should avoid unnecessary downloading or handling of stolen data. Legal and privacy teams should guide next steps.

## 🚨 Step 4: Internal Notification and Tracking

**Tool:** Vendor risk / legal / incident response workflow  
**Target:** Vendor incident

### What We Checked

We prepared an internal notification package with evidence, confidence, possible impact, and recommended questions for the vendor.

### Observed Findings

| Field | Result |
|---|---|
| Notify | Vendor Risk, Legal, Privacy, IR |
| Request | Vendor incident statement |
| Ask | Data impact and timeline |
| Track | SLA and remediation |
| Severity | High |

### Assessment

Ransom leak monitoring helps defenders identify third-party incidents early and drive structured vendor follow-up.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Named Entity | `ExampleCorp Vendor A` | Active vendor |
| Threat | Ransomware leak site claim | Unverified but high-risk |
| Claimed Data | HR/support records | Possible sensitive exposure |
| Business Relationship | Customer support tooling | Potential downstream impact |
| Verdict | High-priority third-party risk | Escalate and validate |

**Verdict:** Escalate to vendor risk and legal teams.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Notify vendor risk, legal, privacy, and IR stakeholders | CTI |
| 2 | Ask vendor for formal incident statement and data impact | Vendor Risk |
| 3 | Review contracts and data-processing agreements | Legal / Privacy |
| 4 | Identify internal data shared with the vendor | Business Owner |
| 5 | Monitor for organization name or data appearing in follow-up leaks | CTI |
| 6 | Prepare customer or regulatory response only if impact is confirmed | Legal / Communications |

---

## 📚 Sources

- Ransomware leak site monitoring workflow
- Vendor inventory and procurement records
- Legal-approved proof-of-leak triage process
- Third-party risk management process

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
