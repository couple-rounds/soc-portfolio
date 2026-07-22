# Cyber Defense Frameworks — Operational Reference

This reference explains how the Cyber Kill Chain, MITRE ATT&CK, and the Diamond Model support security operations. The frameworks are complementary analytical lenses; none is a substitute for evidence, organizational context, or analyst judgment.

## Framework selection

| Framework | Primary question | Strongest SOC use cases | Important limitation |
| --- | --- | --- | --- |
| Cyber Kill Chain | Where is the activity in the intrusion lifecycle, and where can it be disrupted? | Incident timelines, campaign narratives, defensive control reviews, and stakeholder communication | The model is linear, while real intrusions may skip, repeat, or run phases in parallel |
| MITRE ATT&CK | Which adversary behavior was observed, and how was it performed? | Detection engineering, threat hunting, control-gap analysis, and consistent reporting | ATT&CK is a behavior knowledge base, not a chronological incident model, risk score, or detection verdict |
| Diamond Model | What relationships connect the adversary, capability, infrastructure, and victim? | Threat intelligence enrichment, pivoting, campaign correlation, and hypothesis development | Shared infrastructure or tooling does not independently establish attribution |

### Quick decision guide

- Use the **Cyber Kill Chain** to communicate attack progression and identify opportunities to disrupt an intrusion.
- Use **MITRE ATT&CK** to describe observed behaviors and organize detection or hunting coverage.
- Use the **Diamond Model** to structure relationships and generate intelligence pivots.
- Use more than one framework when it improves the analysis; do not force every event into all three.

## 1. Cyber Kill Chain

Lockheed Martin's intrusion kill chain describes seven phases. For a SOC, its value is not simply naming a phase—it is connecting evidence to a defensive decision.

| Phase | Evidence an analyst may encounter | Defensive objective |
| --- | --- | --- |
| Reconnaissance | External scanning, enumeration, phishing research, or collection of exposed organizational information | Reduce exposed information, monitor attack-surface changes, and identify targeted assets |
| Weaponization | Threat intelligence describing an exploit and payload combination; this phase often occurs outside defender visibility | Use intelligence to anticipate payloads, delivery methods, and required controls |
| Delivery | Email gateway events, malicious links or attachments, removable-media activity, or web downloads | Block delivery, remove artifacts, and identify all recipients or affected hosts |
| Exploitation | Vulnerability exploitation, malicious document execution, browser exploitation, or abuse of user action | Prevent code execution, isolate affected assets, and preserve evidence |
| Installation | Malware files, new services, scheduled tasks, startup changes, or other installed components | Remove the foothold, identify persistence, and determine the initial execution chain |
| Command and Control | Repeated outbound connections, suspicious DNS activity, proxy events, or encrypted beaconing patterns | Block communications, isolate compromised systems, and identify related infrastructure |
| Actions on Objectives | Credential theft, discovery, lateral movement, collection, exfiltration, encryption, or destructive activity | Contain the incident, establish scope and impact, recover safely, and meet escalation requirements |

### Analyst considerations

- Treat phase assignments as an analytical aid, not a verdict. One event may support more than one phase.
- Do not assume that every intrusion begins at reconnaissance or reaches command and control.
- Do not prioritize solely by the latest observed phase. Confidence, asset criticality, business impact, exposure, and control effectiveness also matter.
- Record both observed phases and visibility gaps. An unobserved phase is not evidence that it did not occur.

## 2. MITRE ATT&CK

MITRE ATT&CK is a knowledge base of adversary behavior derived from real-world observations. It provides a common language for describing what an adversary is attempting and how the behavior is performed.

| ATT&CK object | Meaning | Example |
| --- | --- | --- |
| Tactic | The adversary's tactical goal—the **why** | Execution (`TA0002`) |
| Technique | The behavior used to achieve a goal—the **how** | Command and Scripting Interpreter (`T1059`) |
| Sub-technique | A more specific form of a technique | PowerShell (`T1059.001`) |
| Procedure | A specific implementation observed in an intrusion or associated with a threat | A particular command line used to run an encoded PowerShell payload |

ATT&CK currently covers three technology domains: Enterprise, Mobile, and Industrial Control Systems. Within each domain, platforms and data requirements should be checked before applying a technique to a detection.

### Evidence-based mapping

Consider the following process telemetry:

```text
ParentImage: C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE
Image:       C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
CommandLine: powershell.exe -NoProfile -EncodedCommand <redacted>
```

A defensible mapping is **PowerShell (`T1059.001`)** because the telemetry directly shows PowerShell execution. The encoded content may increase suspicion, but encoding alone does not establish malicious intent. The analyst still needs context such as the parent process, user activity, script contents, network connections, prevalence, and host role.

### Correct ATT&CK tags in Sigma

```yaml
title: PowerShell Encoded Command
status: experimental
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains:
      - ' -enc '
      - ' -encodedcommand '
  condition: selection
falsepositives:
  - Authorized administrative or automation activity
level: medium
tags:
  - attack.execution
  - attack.t1059.001
```

This excerpt demonstrates metadata and mapping; production use would require environment-specific field validation, testing, and tuning.

### Mapping quality checklist

- Map the observed behavior, not the alert title or assumed intent.
- Use the most specific technique or sub-technique supported by the evidence.
- Document the telemetry and logic that support the mapping.
- Separate behavioral coverage from alert effectiveness: a technique tag does not prove that a rule is reliable.
- Validate identifiers and detection guidance against the current ATT&CK release because the knowledge base evolves.
- Avoid claiming complete ATT&CK coverage without confirming data availability, analytic quality, testing, triage procedures, and response capability.

## 3. Diamond Model of Intrusion Analysis

The Diamond Model represents an intrusion event through four connected core features:

```text
                  Adversary
                 /         \
                /           \
    Infrastructure         Capability
                \           /
                 \         /
                    Victim
```

| Feature | Questions to ask | Example evidence |
| --- | --- | --- |
| Adversary | Who is responsible or assessed to be responsible? What is the confidence level? | Threat reporting, campaign overlap, assessed intent, or operator behavior |
| Capability | What behavior, tool, malware, exploit, or technique was used? | Malware analysis, commands, scripts, exploited vulnerabilities, or ATT&CK techniques |
| Infrastructure | What technical resources supported the activity? | Domains, IP addresses, certificates, email accounts, hosting providers, or redirectors |
| Victim | Who or what was targeted, and why might it matter? | User, host, identity, business unit, sector, geography, or technology profile |

### Pivoting without over-attribution

A domain observed in one alert can be used to search DNS, proxy, email, and endpoint telemetry for additional affected systems. Those results may reveal related infrastructure or capabilities and support a campaign hypothesis.

However, infrastructure can be shared, compromised, rented, or deliberately reused to mislead investigators. Treat a matching indicator as a pivot and correlation point—not proof of a common adversary. Record alternative explanations and assign confidence to analytical judgments.

## Worked investigation example

The following hypothetical sequence shows how the frameworks add different context to the same evidence.

| Observation | Cyber Kill Chain view | ATT&CK view | Diamond Model contribution | Immediate analyst action |
| --- | --- | --- | --- | --- |
| A user receives an email with a malicious Word attachment | Delivery | Spearphishing Attachment (`T1566.001`) | Links the victim identity to delivery infrastructure | Remove the message, identify recipients, and preserve headers and the attachment |
| Word launches PowerShell with an encoded command | Exploitation | User Execution: Malicious File (`T1204.002`) and PowerShell (`T1059.001`), if supported by evidence | Adds process behavior to the capability | Isolate the host, recover the command, and review the process tree |
| The host makes periodic HTTPS requests to a newly registered domain | Command and Control | Application Layer Protocol: Web Protocols (`T1071.001`), if the traffic supports C2 behavior | Adds infrastructure and enables environment-wide pivots | Block the domain, search historical telemetry, and identify other communicating assets |
| A scheduled task is created | Installation | Scheduled Task/Job: Scheduled Task (`T1053.005`) | Expands the capability and may link related events | Capture the task definition, initiating account, payload, and affected hosts |
| An archive is transferred externally | Actions on Objectives | Archive Collected Data (`T1560.001`) and an applicable exfiltration technique, if validated | Connects capability and infrastructure to victim impact | Stop transfer, scope accessed data, escalate, and begin impact assessment |

The table does not imply that every mapping is automatic. Each assignment must be supported by the underlying telemetry and investigation results.

## Reporting standard

A professional incident record should state:

1. **Observation:** What the telemetry directly shows.
2. **Assessment:** What the analyst concludes and with what confidence.
3. **Framework mapping:** Which phase, behavior, or relationship applies and why.
4. **Scope and impact:** Which identities, assets, data, and business services are affected.
5. **Action:** What was contained, what remains open, and who owns the next step.
6. **Gaps:** Which telemetry, facts, or validation steps are missing.

Frameworks improve consistency, but the quality of the analysis depends on transparent reasoning and evidence.

## Primary references

- [Lockheed Martin — Intelligence-Driven Computer Network Defense](https://www.lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)
- [MITRE ATT&CK — Get Started](https://attack.mitre.org/resources/)
- [MITRE ATT&CK — Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/)
- [Diamond Model of Intrusion Analysis](https://threatconnect.com/wp-content/uploads/The_Diamond_Model_of_Intrusion_Analysis.pdf)
- [Sigma rule specification](https://sigmahq.io/sigma-specification/specification/sigma-rules-specification.html)
