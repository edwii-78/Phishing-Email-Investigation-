# 🎣 Phishing Email Investigation — UPS Brand Impersonation Campaign

A complete SOC-style investigation of a real-world phishing email impersonating **UPS**, covering header forensics, authentication analysis (SPF/DKIM/DMARC/ARC), URL & HTML analysis, threat-intelligence enrichment, IOC extraction, attack-chain reconstruction, and MITRE ATT&CK mapping.

> ⚠️ **Educational / Defensive Security Project.** All indicators in this repository (domains, IPs, URLs) are documented for detection-engineering and SOC training purposes only. Do not visit any listed URL/domain — several are flagged malicious by threat-intel vendors.

---

## 📌 Overview

| | |
|---|---|
| **Sample Type** | Credential phishing email (`.eml`) |
| **Impersonated Brand** | UPS |
| **Attack Technique** | Spearphishing Link (MITRE T1566.002) |
| **Delivery Infrastructure** | Authenticated Microsoft 365 / Exchange Online |
| **Final Landing Domain** | `zoomertar.com` (VT-classified phishing) |
| **Severity** | High |
| **Confidence** | High |

This project walks through a full analyst workflow — from initial visual triage of the rendered email, through header/authentication forensics, to external threat-intelligence correlation (VirusTotal, AbuseIPDB, RDAP/WHOIS) — and closes with an incident assessment and containment recommendations, mirroring how a SOC Tier 1/2 analyst would document a real phishing case.

---

## 🙏 Sample Attribution

The raw `.eml` sample analyzed in this project was sourced from **[rf-peixoto/phishing_pot](https://github.com/rf-peixoto/phishing_pot)**, an open-source, community-maintained archive of real phishing emails collected for research and detection-engineering purposes. All credit for the original sample collection goes to that project and its contributors.

This repository does **not** re-host the sample as a live, openable `.eml` file (to avoid re-triggering the email's tracking pixels or having the raw phishing HTML flagged by GitHub's abuse scanners). If you need the original sample, retrieve it directly from the source repository above.

---

## 🗂️ Repository Structure

```
.
├── README.md
├── report/
│   └── Email_Phishing_Investigation_Report.pdf      # Full written investigation report
├── evidence/
│   └── email-headers-defanged.txt                # Full header dump, URLs defanged (hxxp / [.])
└── screenshots/
    ├── 01-thunderbird-rendered-email.png
    ├── 02-vt-zoomertar-detection-summary.png
    ├── 03-vt-zoomertar-relations-graph.png
    ├── 04-vt-zoomertar-details-whois-ssl.png
    ├── 05-abuseipdb-199.192.27.195-check.png
    ├── 06-abuseipdb-199.192.27.195-whois.png
    ├── 07-vt-199.192.27.195-detection-summary.png
    ├── 08-vt-199.192.27.195-relations-graph.png
    ├── 09-vt-ali001-sarakzit-detection-summary.png
    ├── 10-abuseipdb-52.100.0.236-check.png
    ├── 11-abuseipdb-52.100.0.236-whois.png
    ├── 12-sender-reputation-52.100.0.236.png
    └── 13-vt-52.100.0.236-relations-graph.png
```

---

## 🔬 Methodology

1. **Visual Triage** – Rendered `.eml` inspected in Thunderbird before touching headers, to capture what a real end-user would see.
2. **Header Forensics** – From, Return-Path, Subject, Date, Message-ID analyzed in the correct investigative order.
3. **Authentication Analysis** – SPF, DKIM, DMARC, ARC, CompAuth, and SCL evaluated to determine what authentication does — and does not — prove.
4. **Infrastructure Analysis** – Sending IP / SMTP relay / HELO traced to confirm delivery path.
5. **URL & HTML Analysis** – Extracted all embedded links, resolved the shortener redirect chain, identified tracking pixels and remote-hosted branding assets.
6. **Threat Intelligence Correlation** – Every IOC enriched via **VirusTotal**, **AbuseIPDB**, and **RDAP/WHOIS** lookups.
7. **IOC Extraction & Attack Chain Reconstruction** – Findings correlated into a single attacker-perspective kill chain.
8. **MITRE ATT&CK Mapping** – Observed behavior mapped to Enterprise ATT&CK techniques.
9. **Incident Assessment & Response** – Severity/confidence scoring plus containment, threat-hunting, and hardening recommendations.

---

## 🧰 Tools Used

- Mozilla Thunderbird (rendered `.eml` inspection)
- VirusTotal (detections, relations graph, passive DNS, historical WHOIS/SSL)
- AbuseIPDB (IP reputation & WHOIS)
- RDAP / WHOIS lookups
- Manual email-header and HTML/URL analysis
- MITRE ATT&CK Navigator (technique mapping)

---

## 🧾 Key Indicators of Compromise

| Type | Indicator | Verdict |
|---|---|---|
| Domain | `zoomertar[.]com` | Malicious — Phishing (High Confidence) |
| Domain | `ali001.sarakzit[.]za.com` | Attacker-controlled sender domain |
| IP | `199.192.27[.]195` | Suspicious shared/rotating hosting infrastructure |
| IP | `52.100.0[.]236` | Legitimate Microsoft relay — hunting pivot only, **not malicious** |
| URL Shortener | `t[.]co/f9tVtkdJm3` | Malicious redirect used in this campaign |
| Sender | `Hoffman_John_49321@ali001.sarakzit[.]za.com` | Phishing sender |

Full IOC table, screenshots, and analyst reasoning for each indicator are in [`report/SOC_Phishing_Investigation_Report.md`](.report/Email_Phishing_Investigation_Report.pdf).

---

## 🎯 MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Phishing | T1566 |
| Initial Access | Spearphishing Link | T1566.002 |
| Execution | User Execution | T1204 |
| Credential Access | Input Capture: Web Portal Capture | T1056.007 |
| Defense Evasion | Trusted Relationship | T1199 |
| Reconnaissance | Gather Victim Identity Information | T1589 |

---

## 📄 Full Report

The complete write-up — executive summary, header/authentication forensics, threat-intel enrichment per IOC, attack-chain reconstruction, MITRE mapping, and containment recommendations — is available here:

➡️ **[SOC_Phishing_Investigation_Report.md](./report/SOC_Phishing_Investigation_Report.md)**

---

## ⚖️ Disclaimer

This project is for educational and defensive security purposes only. It analyzes a publicly available phishing sample to demonstrate SOC investigation methodology. No offensive tooling, exploit code, or functional malware is included. All IOCs are historical/defanged where appropriate.

---

## 📬 Author

*Add your name, LinkedIn, and/or portfolio link here.*
