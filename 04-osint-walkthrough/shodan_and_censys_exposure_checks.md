# 🔎 OSINT Investigation Walkthrough: Shodan and Censys Exposure Checks

> This walkthrough demonstrates a passive OSINT workflow for **shodan and censys exposure checks** and shows how an analyst can turn raw enrichment into defender-ready action.

![Investigation](https://img.shields.io/badge/Investigation-Attack%20Surface-blue)
![Threat](https://img.shields.io/badge/Threat-Exposure-orange)
![Risk](https://img.shields.io/badge/Risk-Medium--High-red)
![TLP](https://img.shields.io/badge/TLP-WHITE-lightgrey)

---

## 📌 Table of Contents

- [Case Summary](#-case-summary)
- [Investigation Objective](#-investigation-objective)
- [Evidence Snapshot](#-evidence-snapshot)
- [Step 1](#-step-1-shodan-host-lookup)
- [Step 2](#-step-2-censys-certificate-and-service-pivot)
- [Step 3](#-step-3-version-and-risk-triage)
- [Step 4](#-step-4-defensive-validation)
- [Investigation Summary](#-investigation-summary)
- [Recommended Actions](#-recommended-actions)
- [Sources](#-sources)
- [Analyst Notes](#-analyst-notes)

---

## 🧾 Case Summary

| Field | Details |
|---|---|
| **Target** | `vpn.examplecorp[.]com` |
| **Scenario** | The security team wants to assess whether externally exposed VPN and remote-access services are visible online. |
| **Investigation Type** | External attack surface OSINT |
| **Primary Concern** | Internet-facing VPN, admin, or outdated services |
| **Risk Rating** | Medium-High |
| **Recommended Disposition** | Validate ownership and remediate exposure |

> [!WARNING]
> Use passive OSINT tools and approved internal telemetry. Do not directly browse suspected malicious infrastructure from a corporate endpoint.

---

## 🎯 Investigation Objective

The goal is to identify externally visible services associated with an organization and determine whether any should be prioritized for patching, access restriction, or monitoring.

Key questions:

- Which hosts and services are exposed?
- Are VPN, firewall, or remote-management portals internet-facing?
- Are any versions outdated or known to be targeted?
- Are TLS certificates or banners leaking internal names?
- What should be remediated first?

---

## 📌 Evidence Snapshot

| Evidence Type | Finding | Assessment |
|---|---|---|
| Hostname | `vpn.examplecorp[.]com` | Remote access service |
| IP | `203.0.113[.]44` | External-facing infrastructure |
| Open Ports | `443`, `8443`, `22` | VPN/admin/SSH exposure |
| Product Banner | VPN gateway-style response | Edge device risk |
| TLS CN | `vpn.examplecorp[.]com` | Confirms likely ownership |
| Risk | Medium-High | Edge devices are common initial access targets |

---

## 🛰️ Step 1: Shodan Host Lookup

**Tool:** Shodan  
**Target:** `vpn.examplecorp[.]com` / `203.0.113[.]44`

### What We Checked

We reviewed exposed ports, service banners, certificates, hostnames, and historical observations.

### Observed Findings

| Field | Result |
|---|---|
| Ports | `443`, `8443`, `22` |
| Service | VPN gateway-style HTTPS portal |
| SSH | Open to internet |
| Last Seen | Recent |
| Org | Matches expected provider |

### Assessment

The host appears to expose remote-access services that should be validated against the approved external attack surface inventory.

## 🔭 Step 2: Censys Certificate and Service Pivot

**Tool:** Censys host and certificate search  
**Target:** Certificate CN/SAN and organization fields

### What We Checked

We pivoted from certificate names and service banners to identify additional related hosts.

### Observed Findings

| Field | Result |
|---|---|
| Related Host | `remote.examplecorp[.]com` |
| Related Host | `fw-admin.examplecorp[.]com` |
| Certificate SAN | Multiple remote access names |
| Potential Admin Portal | `8443` |
| Concern | Possible management exposure |

### Assessment

Certificate pivots can reveal additional externally exposed systems even when DNS inventory is incomplete.

## 🧾 Step 3: Version and Risk Triage

**Tool:** Banner review / vendor advisory cross-check  
**Target:** Service banner and headers

### What We Checked

We reviewed whether banners reveal product names, versions, or outdated TLS/service configurations.

### Observed Findings

| Field | Result |
|---|---|
| Product | VPN gateway-style appliance |
| Version | Banner not fully exposed |
| TLS | Certificate valid |
| Admin Path | `/admin` observed |
| Priority | Validate and restrict admin access |

### Assessment

Even without a confirmed vulnerable version, exposed admin paths and VPN portals deserve priority review.

## 🛡️ Step 4: Defensive Validation

**Tool:** Asset inventory / vulnerability management / firewall rules  
**Target:** External services

### What We Checked

We compared OSINT findings to the official asset inventory and vulnerability scanner results.

### Observed Findings

| Field | Result |
|---|---|
| Inventory Match | VPN host known |
| Unexpected Host | `fw-admin.examplecorp[.]com` |
| Control Gap | SSH exposed |
| Owner | Network Security |
| Next Step | Restrict access |

### Assessment

Exposure checks should be tied to ownership, patch status, and access-control validation.

## 📊 Investigation Summary

| Indicator | Value | Significance |
|---|---|---|
| Primary Host | `vpn.examplecorp[.]com` | Internet-facing VPN |
| IP | `203.0.113[.]44` | External asset |
| Open Ports | `443`, `8443`, `22` | Remote access and possible admin exposure |
| Related Host | `fw-admin.examplecorp[.]com` | Needs validation |
| Verdict | Exposed service risk | Validate, restrict, and monitor |

**Verdict:** Validate ownership and remediate exposure.  
**Confidence:** Medium-High for training/demo purposes.  
**Recommended Severity:** Medium-High.

---

## 🛡️ Recommended Actions

| Priority | Action | Owner |
|---:|---|---|
| 1 | Confirm asset ownership and business need | Asset Management |
| 2 | Restrict admin interfaces to VPN or trusted IP ranges | Network Security |
| 3 | Patch or upgrade exposed edge devices | Infrastructure |
| 4 | Disable unnecessary internet-facing SSH | Network Security |
| 5 | Add exposed services to continuous attack surface monitoring | CTI / VM |
| 6 | Alert on new internet-facing VPN or admin portals | Detection Engineering |

---

## 📚 Sources

- Shodan-style host exposure search
- Censys host and certificate search
- Internal asset inventory
- Vulnerability management records

---

## ✅ Analyst Notes

This walkthrough is designed for CTI portfolio use and GitHub rendering. It is written as a safe training example using defanged or placeholder indicators. Validate all findings with current authoritative sources and internal telemetry before blocking, reporting, or making attribution decisions.
