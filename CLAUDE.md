# CLAUDE.md — soc-portfolio (repo-scoped)

> This file is automatically loaded by Claude Code when working inside this repo.
> The personal/user-wide context lives at `~/.claude/CLAUDE.md` — that is **not** committed to git.
> Recruiters see this file. Keep it professional.

## What this repo is

A hands-on portfolio for SOC Analyst (Tier 1) applications in Dubai. Built over a 90-day structured
programme starting **2026-07-21**. Every project here was built and run by the owner; nothing is vendored
walkthrough code.

## Audience

Primary: recruiters and hiring managers in Dubai (CPX, HelpAG, MDS, Emirates NBD, ADCB, Big 4 cyber practices, gov roles).

## Stack and conventions

- **Markdown** for everything written (project READMEs, write-ups, runbooks, cheatsheets).
- **No secrets, no internal company data, no copyrighted material**. `.gitignore` covers this.
- **Commit prefixes:** `feat:`, `docs:`, `chore:`, `fix:`.
- **Atomic commits** — one concept per commit. Avoid `WIP` on main.
- **Branch strategy:** keep `main` clean. Feature branches per project (`feat/splunk-detections`, etc.).

## Folder map

```
projects/             # numbered hands-on projects (01, 02, ...)
thm-writeups/         # TryHackMe room write-ups
detection-rules/
  splunk/             # .spl files + README
  kql/                # .kql files + README
  sigma/              # .yml files + README
certifications/       # THM badges (PNG) — only if not personally identifying
cheatsheets/          # my own notes
```

## Project skeleton

Every project under `projects/NN-name/` looks like:

```
NN-name/
├── README.md         # what, why, how, results (recruiter-readable)
├── queries/          # actual queries / code
├── data/             # only small synthetic or anonymised data; never production
├── screenshots/      # png/jpg of dashboards, alert output
└── runbook.md        # if the project is incident-response shaped
```

`README.md` must include:
1. **Goal** — one sentence.
2. **Stack** — bullet list of tools and dataset.
3. **MITRE ATT&CK techniques covered** — technique IDs + names.
4. **Walk-through** — alert → investigation → containment → escalation.
5. **False positives / what I'd tune next** — shows awareness.

## How Claude should help

If a user pastes project work in chat and asks for review or improvement:

- Push for tradecraft quality, not coverage.
- Always flag missing time bounds, missing ATT&CK tagging, missing FP comments.
- Never suggest committing internal-company data, secrets, or copyrighted material.
- If you're going to push a commit on the user's behalf, use `gh` from `/home/jsoc/soc-portfolio/`
  with identity `couple-rounds`.
- Match the commit message style already used in `git log`.

## Anti-patterns to flag

- Queries without time bounds (`earliest=-24h` or equivalent).
- `index=*` queries — teach the discipline of named indexes.
- Detections without MITRE ATT&CK mapping.
- Detections without a documented false-positive hypothesis.
- Copy-pasted walkthroughs without adaptation.
- Large binary data dumps committed to the repo.

## Status

**Last updated:** 2026-07-21 — Day 1 of 90. Repo scaffolded; next milestone is Project 01 (Splunk BOTS detection).
