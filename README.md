# SOC Portfolio — couple rounds

> Hands-on SOC Analyst portfolio built over a 90-day structured programme (July–October 2026).
> Target role: **SOC Analyst (Tier 1)** in Dubai, UAE.

This repository is the working portfolio I use to demonstrate security operations tradecraft to recruiters
and hiring managers. Every project, detection rule, and write-up is something I built and ran myself — not
something I copied from a walkthrough.

---

## At a glance

| Area | Coverage |
| --- | --- |
| SIEM — Splunk | SPL queries, dashboards, alert investigations |
| SIEM — Microsoft Sentinel | KQL analytics rules, M365 log onboarding, IR runbooks |
| Detection Engineering | Sigma rules mapped to MITRE ATT&CK |
| Threat Hunting | Hypothesis-driven hunt write-ups |
| Incident Response | End-to-end runbooks (phishing → credential compromise → lateral movement) |
| Foundations | Linux/Windows, networking, CTI, kill chain, Diamond Model |

All detection rules are tagged with the relevant [MITRE ATT&CK](https://attack.mitre.org/) technique IDs.

---

## Featured projects

> Each link takes you to the project folder with full code, queries, screenshots, and write-ups.

### 01 — Splunk: BOTS v1 brute force & compromise detection
**Stack:** Splunk Free, SPL, BOTS v1 dataset
Five SPL analytics rules against a real-world attack dataset — failed logon correlation, PowerShell
encoded-command execution, SMB recon, and more. Each rule is mapped to ATT&CK.
`→ [projects/01-splunk-bots-detection/`](projects/01-splunk-bots-detection/)

### 02 — Microsoft Sentinel: M365 analytics rules
**Stack:** Microsoft Sentinel, KQL, M365 data connectors
Five analytics rules covering impossible travel, OAuth consent abuse, MFA tampering, and mailbox-rule abuse.
`→ [projects/02-sentinel-m365-detections/`](projects/02-sentinel-m365-detections/)

### 03 — Sigma rules (vendor-neutral detection)
**Stack:** Sigma YAML
Three Sigma rules with ATT&CK mapping and conversion notes for Splunk + Sentinel.
`→ [projects/03-sigma-rules/`](projects/03-sigma-rules/)

### 04 — Incident response runbook
Phishing → credential compromise → mailbox rule → lateral movement. Full detection-to-recovery playbook.
`→ [projects/04-ir-runbook/`](projects/04-ir-runbook/)

### 05 — Threat hunt: credential dumping hypothesis
Hypothesis-driven hunt using KQL, with a hunt report (null findings accepted and explained).
`→ [projects/05-threat-hunt/`](projects/05-threat-hunt/)

---

## Detection rules index

| Tool | Path |
| --- | --- |
| Splunk SPL | [detection-rules/splunk/](detection-rules/splunk/) |
| Sentinel KQL | [detection-rules/kql/](detection-rules/kql/) |
| Sigma (vendor-neutral) | [detection-rules/sigma/](detection-rules/sigma/) |

---

## THM write-ups

Concise write-ups of every TryHackMe room completed as part of the [SOC Level 1](https://tryhackme.com/path/outline/soclevel1) learning path.

`→ [thm-writeups/](thm-writeups/)`

---

## Cheat sheets

My own notes, written while learning. Plain language, what each concept *means*, not just what it *is*.

`→ [cheatsheets/](cheatsheets/)`

---

## How this portfolio is built

- One or two projects pushed per week
- Every project has a README that walks a recruiter through the *why* and the *outcome*
- Every detection rule is mapped to MITRE ATT&CK technique IDs
- Commit history is kept clean (small, atomic commits)

If you're a recruiter or hiring manager reviewing this — the projects I'd ask you to look at first are:

1. **[`projects/01-splunk-bots-detection/`](projects/01-splunk-bots-detection/)** — shows Splunk depth
2. **[`projects/02-sentinel-m365-detections/`](projects/02-sentinel-m365-detections/)** — shows KQL depth
3. **[`projects/04-ir-runbook/`](projects/04-ir-runbook/)** — shows I understand the end-to-end IR picture

---

## Contact

- **GitHub:** [couple-rounds](https://github.com/couple-rounds)
- **LinkedIn:** *(link to be added in Phase 5)*
- **Based in:** Dubai, UAE — resident visa holder, no sponsorship needed

---

*Built in public. Last updated: 2026-07-21.*