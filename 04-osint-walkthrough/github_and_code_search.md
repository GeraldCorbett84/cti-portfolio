# 🔎 OSINT Investigation Walkthrough: GitHub and Code Search

> This walkthrough demonstrates a passive OSINT workflow for **github and code search** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-Code%20Search-blue)
![Threat](https://img.shields.io/badge/Threat-Secret%20Exposure-orange)
![Risk](https://img.shields.io/badge/Risk-High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-public-code-search)
- [Step 2](#-step-2-secret-validation-without-using-the-secret)
- [Step 3](#-step-3-commit-and-exposure-timeline)
- [Step 4](#-step-4-remediation-package)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `examplecorp API token exposure` |
| **Scenario** | The CTI team is checking public code repositories for exposed secrets, internal hostnames, and sensitive project names. |
| **Investigation Type** | Public code exposure review |
| **Primary Concern** | Leaked API keys or internal infrastructure references |
| **Risk Rating** | High |
| **Recommended Disposition** | Validate exposure, rotate secrets, and request removal |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to safely search public code repositories for exposed credentials, internal URLs, cloud bucket names, project identifiers, or other data that could help an attacker.

Key questions:

- Are any API keys, tokens, or credentials publicly exposed?
- Are internal hostnames or infrastructure names visible?
- Does the exposed data belong to the organization?
- Has the secret been used or should it be rotated?
- What removal and remediation steps are required?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Search Term | `examplecorp` | Brand / organization keyword |
| Finding | `EXAMPLECORP_API_KEY=sk_live_...` | Potential secret exposure |
| Repository | `user/demo-integration` | Public repository |
| Internal Hostname | `jira.internal.examplecorp[.]com` | Internal infrastructure reference |
| Commit Age | 12 days | Recent exposure |
| Risk | High | Credential abuse potential |

---

## 💻 Step 1: Public Code Search

**Tool:** GitHub code search / GitLab public search  
**Target:** Brand terms and secret patterns

### What We Checked

We searched for organization name, domain, internal hostnames, API key prefixes, cloud bucket names, and project identifiers.

### Observed Findings

| Field | Result |
|---|---|
| Keyword | `examplecorp` |
| Secret Pattern | `EXAMPLECORP_API_KEY=` |
| Internal Domain | `internal.examplecorp[.]com` |
| Repository | `user/demo-integration` |
| Visibility | Public |

### Assessment

Public code search can reveal exposed data that attackers may use for credential theft, reconnaissance, or social engineering.

## 🔐 Step 2: Secret Validation Without Using the Secret

**Tool:** Internal owner validation / secret scanning workflow  
**Target:** Potential API token

### What We Checked

We did not attempt to use the secret. We validated format, owner, and exposure with the internal application or platform team.

### Observed Findings

| Field | Result |
|---|---|
| Secret Format | Matches production-style key |
| Owner | Integrations team |
| Environment | Possibly production |
| Usage | Requires internal validation |
| Risk | High |

### Assessment

Never test a leaked credential by using it. Validate through approved internal processes and rotate if ownership is confirmed.

## 🧭 Step 3: Commit and Exposure Timeline

**Tool:** Repository history review  
**Target:** Commit history and file path

### What We Checked

We reviewed when the secret first appeared, whether it was removed, and whether it remains in commit history.

### Observed Findings

| Field | Result |
|---|---|
| First Commit | 12 days ago |
| Current Branch | Secret still present |
| Commit History | Secret also in previous commit |
| File Path | `config/prod.env` |
| Exposure Window | 12 days |

### Assessment

Removing a secret from the latest file is not enough if it remains in repository history. Rotation is required.

## 🛡️ Step 4: Remediation Package

**Tool:** Incident ticket / secret management workflow  
**Target:** Confirmed secret exposure

### What We Checked

We documented the affected secret, repo, owner, timeline, and remediation actions.

### Observed Findings

| Field | Result |
|---|---|
| Rotate Secret | Required |
| Revoke Old Token | Required |
| Repository Cleanup | Required |
| Monitor Abuse | API logs |
| Prevention | Enable secret scanning |

### Assessment

The defensive value comes from connecting OSINT discovery to ownership, rotation, logging review, and prevention.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Finding | `EXAMPLECORP_API_KEY=sk_live_...` | Potential exposed secret |
| Repository | `user/demo-integration` | Public repository |
| File Path | `config/prod.env` | Sensitive config file |
| Exposure Window | 12 days | Potential abuse period |
| Verdict | Confirmed exposure pending owner validation | Rotate and revoke immediately if confirmed |

**Verdict:** Validate exposure, rotate secrets, and request removal.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Open an internal incident ticket and identify owner | CTI / AppSec |
| 2 | Rotate and revoke exposed secrets if ownership is confirmed | AppSec / Platform Team |
| 3 | Review API logs for suspicious use during exposure window | SOC / App Team |
| 4 | Request repository removal or history cleanup | Legal / AppSec |
| 5 | Enable secret scanning and push protection | DevSecOps |
| 6 | Add exposed internal hostnames to reconnaissance monitoring | CTI |

---

## 📚 Sources

- GitHub code search workflow
- GitLab public repository search
- Internal secret management process
- Cloud/API audit logs

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
