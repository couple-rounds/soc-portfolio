# TryHackMe Write-Up — Cyber Defense Frameworks

| Field | Detail |
| --- | --- |
| Learning path | SOC Level 1 |
| Module | Cyber Defense Frameworks |
| Completion date | 22 July 2026 |
| Supporting artifact | [Cyber Defense Frameworks — Operational Reference](../cheatsheets/01-cyber-defense-frameworks.md) |

## Objective

The objective of this module was to understand how the Cyber Kill Chain, MITRE ATT&CK, and the Diamond Model support different analytical tasks within a security operations environment.

## Framework summary

| Framework | Operational value | Typical output |
| --- | --- | --- |
| Cyber Kill Chain | Organizes intrusion activity into phases and highlights opportunities for disruption | Incident timeline or attack-progression narrative |
| MITRE ATT&CK | Provides a common language for observed adversary behavior | Technique mapping for a detection, hunt, or incident record |
| Diamond Model | Structures relationships among adversary, capability, infrastructure, and victim | Intelligence pivots, relationship mapping, or campaign hypothesis |

The frameworks are complementary, but they are not interchangeable. Framework selection should be driven by the analytical question and the available evidence.

## Key learning outcomes

### 1. Framework mappings must be evidence-based

An ATT&CK technique should be assigned only when the underlying telemetry supports the behavior. For example, process-creation telemetry showing `powershell.exe` supports PowerShell (`T1059.001`). An alert name containing the word "PowerShell" is not sufficient evidence by itself.

The same principle applies to the Cyber Kill Chain. A missing phase in the available telemetry is a visibility gap, not proof that the phase did not occur.

### 2. ATT&CK mapping does not validate a detection

A technique tag improves consistency and helps organize coverage, but it does not establish that a rule is accurate or operationally useful. Detection quality also depends on:

- Required telemetry and field availability
- Logic validation against representative data
- Documented false-positive scenarios
- Environment-specific tuning
- Triage guidance and response ownership

This distinction is important when interpreting an ATT&CK coverage matrix. A mapped technique is not necessarily a tested detection capability.

### 3. Correlation is not attribution

The Diamond Model supports pivots across infrastructure, capabilities, and victims. A repeated domain, IP address, certificate, or tool can connect investigations and generate a useful hypothesis.

However, infrastructure may be shared, compromised, rented, or intentionally reused. A matching indicator should therefore be treated as a correlation point rather than conclusive evidence of a common adversary. Attribution requires multiple independent sources and an explicit confidence assessment.

## Application to a SOC workflow

### Alert triage

1. Validate the event and the reliability of its data source.
2. Separate direct observations from assumptions or vendor-generated labels.
3. Map confirmed behavior to the most specific supported ATT&CK technique.
4. Establish the affected user, host, identity, and business context.
5. Determine whether the evidence supports escalation, containment, or continued investigation.

### Incident analysis

1. Build a timestamped sequence of confirmed events.
2. Use the Cyber Kill Chain where it clarifies attack progression and control opportunities.
3. Record phases that are supported, unsupported, or not observable with current telemetry.
4. Avoid forcing nonlinear or repeated activity into a single chronological path.

### Threat-intelligence enrichment

1. Structure known relationships using the Diamond Model.
2. Pivot from infrastructure and capabilities across endpoint, identity, email, DNS, and proxy data.
3. Record competing explanations for shared indicators.
4. State analytical confidence and identify the evidence needed to raise or lower it.

## Applied example

Assume endpoint telemetry records Microsoft Word launching PowerShell with an encoded command.

| Analytical lens | Assessment |
| --- | --- |
| Direct observation | `WINWORD.EXE` created a `powershell.exe` process with an encoded command-line argument |
| ATT&CK | PowerShell (`T1059.001`) is supported; User Execution: Malicious File (`T1204.002`) requires evidence that the user opened a malicious document |
| Cyber Kill Chain | The event may support exploitation, but its phase should be confirmed using delivery, execution, and payload evidence |
| Diamond Model | The process behavior contributes to capability analysis; network or email evidence is needed to identify infrastructure |
| Next actions | Recover and decode the command safely, review the process tree, inspect related network activity, establish prevalence, and determine whether isolation is required |

This example demonstrates why a framework label should follow the evidence rather than replace the investigation.

## Practical extension

To reinforce the module, I developed a separate [operational reference](../cheatsheets/01-cyber-defense-frameworks.md) that adds:

- Framework selection criteria and limitations
- Observable evidence and defensive objectives for each Kill Chain phase
- A correctly structured Sigma ATT&CK tagging example
- Attribution cautions for Diamond Model pivots
- A worked investigation sequence and reporting checklist

## Conclusion

The main outcome from this module was a repeatable method for selecting and applying each framework:

- Use the Cyber Kill Chain to explain progression and disruption opportunities.
- Use MITRE ATT&CK to describe evidence-supported adversary behavior.
- Use the Diamond Model to organize relationships and develop intelligence pivots.
- Document uncertainty, visibility gaps, and alternative explanations in every analysis.

The value of a framework is not the number of labels applied; it is the clarity and consistency it adds to evidence-based decisions.

## References

- [Cyber Defense Frameworks — Operational Reference](../cheatsheets/01-cyber-defense-frameworks.md)
- [MITRE ATT&CK — Get Started](https://attack.mitre.org/resources/)
- [Lockheed Martin — Intelligence-Driven Computer Network Defense](https://www.lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)
- [Diamond Model of Intrusion Analysis](https://threatconnect.com/wp-content/uploads/The_Diamond_Model_of_Intrusion_Analysis.pdf)
