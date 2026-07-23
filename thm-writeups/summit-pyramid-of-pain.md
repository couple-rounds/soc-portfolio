# TryHackMe Write-Up — Summit: Pyramid of Pain

| Field | Detail |
| --- | --- |
| Learning path | SOC Level 1 |
| Room | [Summit](https://tryhackme.com/room/summit) |
| Completion date | 23 July 2026 |
| Exercise type | Defensive analysis and detection engineering simulation |

> This write-up excludes flags, challenge answers, malware indicators, and exact rule values.

## Objective and outcome

I completed a six-stage simulation in which an adversary repeatedly adapted malware to evade earlier controls. I analyzed sandbox and log evidence, then applied hash, firewall, DNS, and guided Sigma-based detections at progressively more durable levels of the Pyramid of Pain.

The exercise demonstrated how to select controls from available evidence and balance detection durability, telemetry requirements, false positives, and adversary effort.

## Tools and approach

The room provided a **Malware Sandbox**, **Manage Hashes**, **Firewall Rule Manager**, **DNS Rule Manager**, **Sigma Rule Builder**, and simulated email and log evidence.

For each sample, I:

1. Reviewed the case context and available telemetry.
2. Separated direct observations from assumptions.
3. Selected an observable and an appropriate control.
4. Applied the control and assessed how the adversary adapted.

## Detection progression

| Level | Defensive application | Operational trade-off |
| --- | --- | --- |
| Hash values | Identify or block an exact file | Fast and precise, but invalidated by minor file changes |
| IP addresses | Block known malicious infrastructure | Useful for containment, but infrastructure may rotate or be shared |
| Domain names | Deny access to malicious domains | More durable than one IP, but domains can still be replaced |
| Network and host artifacts | Detect distinctive activity on systems or networks | Forces tool changes, but requires suitable telemetry and tuning |
| Tools | Detect stable characteristics across tool variants | Broader coverage, with potential version drift and legitimate lookalikes |
| Tactics, techniques, and procedures | Detect how the adversary operates | Harder to evade, but more demanding to engineer, validate, and maintain |

Lower-level indicators remain useful for immediate response. Strong detection combines their speed with more durable behavioral analytics.

## What challenged me

### Sample 2 — network evidence

The second sample required moving beyond static file information, interpreting network activity, and translating the relevant observable into a firewall control.

I learned that an IP block can provide fast containment but limited resilience. In a real environment, I would confirm the activity across endpoint and network telemetry, scope affected hosts, check for shared infrastructure, and hunt for related behavioral evidence.

### Sample 5 — behavioral detection

The fifth sample did not present one obvious indicator. I had to identify a repeatable pattern in network evidence and express the behavior as structured detection logic.

This required separating stable behavior from incidental connection details, avoiding dependence on infrastructure the adversary could rotate, and considering legitimate applications that might produce similar traffic.

## Key takeaways

- Moving higher in the pyramid increases the adversary's cost of evasion, but also increases the defender's need for quality telemetry, testing, tuning, and triage guidance.
- Hashes, IP addresses, domains, and behavioral detections serve different purposes and are strongest when layered.
- The observable, log source, response objective, and false-positive risk should determine the control.
- Strong behavioral coverage may force an adversary to rebuild tooling or change procedures, but it does not guarantee disengagement.
- Sigma makes detection intent portable; converted rules still require validation in the target SIEM.

## Skills demonstrated

- Interpreting simulated malware-sandbox results
- Extracting and classifying indicators and behavioral observables
- Selecting hash, firewall, DNS, or log-based controls
- Recognizing adversary adaptation across malware variants
- Developing detection logic with a guided Sigma rule builder
- Distinguishing short-lived indicators from durable behavioral coverage
- Documenting technical limitations and follow-up actions

## Scope and limitations

- This was a guided simulation, not a production investigation.
- The samples and infrastructure were controlled training artifacts.
- Sigma rules were created through a guided builder, not independently tested in a live SIEM.
- No organizational baseline was available for measuring false-positive rates.
- Completion demonstrates familiarity with the workflow, not production-level malware-analysis or detection-engineering proficiency.

## Next step

I will extend this exercise by independently creating a behavior-based Sigma rule from synthetic telemetry, validating its syntax, converting it to Splunk SPL and Microsoft Sentinel KQL, and documenting tests, false positives, triage guidance, and evidence-supported ATT&CK mapping.

## References

- [TryHackMe — Summit](https://tryhackme.com/room/summit)
- [David J. Bianco — The Pyramid of Pain](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html)
- [Sigma — Getting Started](https://sigmahq.io/docs/)
- [MITRE ATT&CK — Frequently Asked Questions](https://attack.mitre.org/resources/faq/)
