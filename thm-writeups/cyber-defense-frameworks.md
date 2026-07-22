# THM Write-up — Cyber Defense Frameworks

**Path:** SOC Level 1
**Date completed:** 2026-07-22
**Time spent:** ~90 minutes (room + my own cheatsheet + this write-up)

### What I learned

The room taught me that three "frameworks" are not three competing standards — they answer different
questions. Kill Chain narrates an attack over time. ATT&CK is the catalogue of specific moves a
detector watches for. Diamond Model is the pivot lens for linking incidents to actors.

The reframe I needed: in a SOC, **ATT&CK is for detection, Kill Chain is for write-ups, Diamond is
for intel pivots.** I was treating them as parallel options; they're sequential tools in the same
workflow. That's what my [cheatsheet](../cheatsheets/01-cyber-defense-frameworks.md) tries to capture.

The other sharp bit: ATT&CK has three matrices (Enterprise / Mobile / ICS). For Dubai SOC L1 work
I'll live in Enterprise — and the bulk of my future detections will target T1059 (execution),
T1003 (credential access), and T1110 (brute force), because those are the techniques Splunk and
Sentinel actually surface most often in BOTS and M365 datasets.

### What I'd add if I wrote the room

Two things:

1. **A worked example.** Take one real APT campaign (e.g. APT29 / Cozy Bear) and tag every phase
   through all three frameworks simultaneously. Beginners don't see how the three lenses layer onto
   one incident.

2. **A "which one wins?" section.** Real SOC work has to pick one quickly when writing a ticket.
   The room dodges this. My answer: ATT&CK for the rule, Kill Chain for the report, Diamond only
   when pivoting. State this explicitly and juniors won't waste an hour debating it.

### Links

- Cheatsheet: [01-cyber-defense-frameworks.md](../cheatsheets/01-cyber-defense-frameworks.md)
- Commit: see `git log` — Day 2 commit titled `docs: add cyber defense frameworks cheatsheet and writeup`
