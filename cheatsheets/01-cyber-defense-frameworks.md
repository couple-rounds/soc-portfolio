# 01 — Cyber Defense Frameworks

> My notes from THM SOC Level 1 → "Cyber Defense Frameworks". Written after doing the room, in
> my own words. The point of this file is to remind me later *which framework to reach for and when*,
> not to recite definitions.

---

## Why three frameworks, not one?

Because each answers a different question. Conflating them is the #1 thing I see in junior write-ups.

| Framework | Question it answers | Time dimension |
|---|---|---|
| **Lockheed Cyber Kill Chain** | What does an attack *look like over time*? | Linear, 7 stages |
| **MITRE ATT&CK** | What *specific moves* could the attacker make at each stage? | Catalog of techniques, no time axis |
| **Diamond Model** | *Who* did this, *what* did they use, *against whom*, *via what infrastructure*? | Static — one incident = one diamond |

Reach for **Kill Chain** when writing an incident narrative ("the attacker did X, then Y"). Reach for
**ATT&CK** when writing a detection ("this alert maps to T1059.001 — PowerShell"). Reach for
**Diamond Model** when pivoting across multiple incidents looking for the same actor.

---

## Lockheed Cyber Kill Chain — 7 stages

1. **Reconnaissance** — research target (OSINT, employee LinkedIn, Shodan)
2. **Weaponization** — pair exploit with payload (e.g. malicious macro + reverse shell)
3. **Delivery** — transmit to victim (phishing email, USB, drive-by)
4. **Exploitation** — trigger the weaponised content (user opens the doc, browser renders the page)
5. **Installation** — persist on the host (registry run key, scheduled task, service)
6. **Command & Control (C2)** — open a channel back to attacker infrastructure (HTTPS beacon, DNS tunnel)
7. **Actions on Objectives** — the actual goal: data theft, ransomware, sabotage

**When I use it:** incident timeline write-ups, exec briefings, post-mortems. The visual narrative is
what non-technical stakeholders remember.

**When I don't:** real-time alerting. By the time I'm seeing alerts in Splunk, the attack is already
at stages 5–7. Detection work belongs in ATT&CK.

---

## MITRE ATT&CK

### What problem it solved

Before ATT&CK (~2013–2015), every vendor had its own naming for attacker behaviour. Splunk said
"PowerShell Encoded Command", CrowdStrike said "Suspicious PowerShell Activity", Microsoft said
"Scripting Executive". Analysts couldn't share detections cleanly. ATT&CK gave everyone the same
taxonomy — `T1059.001` means the same thing in Dubai, London, and Washington.

### Tactics vs Techniques (the bit beginners confuse)

- **Tactic** = the *why* — the attacker's goal at this moment. Example: TA0002 "Execution".
- **Technique** = the *how* — the specific way they achieve it. Example: T1059.001 "PowerShell".
- **Sub-technique** = the variant. Example: T1059.001 is a sub-technique of T1059 "Command and Scripting Interpreter".

Each detection I write will be tagged with a technique ID (or sub-technique), not a tactic ID. Tactics
are for the *narrative*; techniques are for the *detection logic*.

### Three matrices

- **Enterprise** — Windows, macOS, Linux, cloud (AWS/Azure/GCP), containers, network. *This is where I live as a SOC L1.*
- **Mobile** — iOS/Android. Out of scope for my day job.
- **ICS** — industrial control systems (SCADA, PLCs). Out of scope unless I move into OT security.

### An ATT&CK tag in a real detection looks like this

```yaml
# Sigma rule (excerpt)
title: Suspicious Encoded PowerShell Command
attack.tactic: Execution
attack.technique: T1059.001
detection: ...
```

When a recruiter sees `T1059.001` in my detection, they know I didn't just copy a rule — I know
*which execution sub-technique* I'm hunting.

---

## Diamond Model — 4 vertices

```
        Adversary
           /\
          /  \
         /    \
   Infra -----> Victim
         \    /
          \  /
           \/
        Capability
```

- **Adversary** — who is doing this (APT group, insider, script kiddie)
- **Capability** — what tool/malware/exploit they used
- **Infrastructure** — what C2 server, domain, IP they used
- **Victim** — who/what was attacked (industry, geo, specific asset)

**The lines between vertices = relationships.** Adversary uses Capability uses Infrastructure against
Victim. Two incidents share Infrastructure → probably same Adversary. Pivot by linking vertices.

**When I use it:** threat intel correlation. If three different alerts across the week all touch the
same IP, I model them as a diamond and look for the actor.

**When I don't:** single-alert triage. Diamond is overkill for one brute-force attempt.

---

## Mapping between frameworks (how they fit)

- Kill Chain stage 4 "Exploitation" ≈ ATT&CK tactic "Initial Access" / "Execution"
- Kill Chain stage 6 "C2" ≈ ATT&CK tactic "Command and Control"
- Every Kill Chain stage has *dozens* of ATT&CK techniques inside it

Diamond Model sits *orthogonal* — it doesn't map 1:1 to either, it's a separate lens.

---

## The senior-analyst framing (what I'd add to the THM room)

The room presents the three frameworks as parallel. In real SOC work they're **sequential**:

1. An alert fires in Splunk → I tag it with **ATT&CK** (T1059.001).
2. I write up the incident → I narrate it with **Kill Chain** ("stage 5: persistence established").
3. I link this incident to others → I pivot with **Diamond Model** ("same C2 IP as last week's phishing").
4. The detection rule that fired? Goes into the library tagged with ATT&CK. The incident report?
   Kill Chain narrative. The intel note? Diamond Model pivot.

If the room taught only one thing, that's it.
