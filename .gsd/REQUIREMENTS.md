# Requirements

## Active

### R001 — Strategic reality check (full mode)
- Class: core-capability
- Status: active
- Description: `/perspective` runs the full pipeline: understand scope, research landscape, audit against live docs, produce a report with recommendation (stay/adjust/pivot/adopt), and optionally implement fixes
- Why it matters: This is the existing core product — everything else builds on it
- Source: user
- Primary owning slice: pre-existing (already implemented)
- Supporting slices: M001/S02 (coherency stage added)
- Validation: validated
- Notes: Already working. M001 extends it with coherency analysis.

### R002 — Coherency analysis
- Class: core-capability
- Status: active
- Description: Every `/perspective` run includes a coherency check that finds unnecessary complexity, code duplication, overlapping responsibilities across files/modules, dead code, and structural issues that make the project harder to maintain
- Why it matters: Users want a "health check" that catches the kind of rot that accumulates over time — not just strategic direction but structural quality
- Source: user
- Primary owning slice: M001/S02
- Supporting slices: none
- Validation: unmapped
- Notes: Integrated as a stage in the full pipeline, not a separate command. Findings go in the same report.

### R003 — Lightweight quick mode
- Class: core-capability
- Status: active
- Description: When invoked with a focused argument (e.g. `/perspective authentication`), the skill runs a fast, lightweight check — reads code, checks live docs, answers the question — skipping landscape research and full strategic analysis
- Why it matters: Full runs are heavy. Quick questions deserve quick answers without the 5-stage ceremony.
- Source: user
- Primary owning slice: M001/S03
- Supporting slices: none
- Validation: unmapped
- Notes: Must detect whether the argument is a focus area for a full run vs. a quick question. Heuristic needed.

### R004 — Open-source repo hygiene
- Class: launchability
- Status: active
- Description: Repo has LICENSE file, clean README, correct repository URL in plugin.json, no loose audit artifacts, gitignore is intentional and documented
- Why it matters: The project is public on GitHub — incomplete OSS hygiene hurts credibility and discoverability
- Source: user + audit
- Primary owning slice: M001/S01
- Supporting slices: none
- Validation: unmapped
- Notes: Gaps identified in audit: no LICENSE, loose AUDIT-REPORT.md, /perspective/ gitignored but contains tracked file, repo URL unverified.

### R005 — Report includes coherency section
- Class: primary-user-loop
- Status: active
- Description: The perspective report output format includes a "Coherency" section with findings on duplication, overlap, unnecessary complexity, and concrete action items
- Why it matters: Coherency findings must be visible and actionable in the same report the user already reads
- Source: inferred
- Primary owning slice: M001/S02
- Supporting slices: none
- Validation: unmapped
- Notes: Same report file, new section.

### R006 — Skill stays under 500 lines
- Class: constraint
- Status: active
- Description: SKILL.md must remain under 500 lines after all additions
- Why it matters: CLAUDE.md explicitly sets this cap. Skill files that are too long degrade context quality.
- Source: user (via CLAUDE.md)
- Primary owning slice: M001/S04
- Supporting slices: all slices
- Validation: unmapped
- Notes: Currently 164 lines. Budget is ~336 lines for new features.

## Deferred

### R007 — Separate coherency-only invocation
- Class: differentiator
- Status: deferred
- Description: A `/perspective coherency` mode that runs only the coherency check without research or strategic analysis
- Why it matters: Could be useful for quick structural checks, but user chose "always included" over separate mode
- Source: inferred
- Primary owning slice: none
- Supporting slices: none
- Validation: unmapped
- Notes: Deferred — user explicitly chose coherency as always-included. Revisit if full runs feel too slow.

## Out of Scope

### R008 — Contributor ceremony
- Class: admin/support
- Status: out-of-scope
- Description: CONTRIBUTING.md, issue templates, code of conduct, PR templates
- Why it matters: Prevents over-engineering the repo for contributions that haven't materialized
- Source: user
- Primary owning slice: none
- Supporting slices: none
- Validation: n/a
- Notes: User chose minimal OSS setup. Add if people actually show up.

### R009 — CI/CD pipeline
- Class: operability
- Status: out-of-scope
- Description: Automated testing, linting, or release automation
- Why it matters: The product is a single markdown file — CI adds complexity without proportional value
- Source: inferred
- Primary owning slice: none
- Supporting slices: none
- Validation: n/a
- Notes: A smoke test checking frontmatter and line count would be nice but isn't worth a CI pipeline for one file.

## Traceability

| ID | Class | Status | Primary owner | Supporting | Proof |
|---|---|---|---|---|---|
| R001 | core-capability | active | pre-existing | M001/S02 | validated |
| R002 | core-capability | active | M001/S02 | none | unmapped |
| R003 | core-capability | active | M001/S03 | none | unmapped |
| R004 | launchability | active | M001/S01 | none | unmapped |
| R005 | primary-user-loop | active | M001/S02 | none | unmapped |
| R006 | constraint | active | M001/S04 | all | unmapped |
| R007 | differentiator | deferred | none | none | unmapped |
| R008 | admin/support | out-of-scope | none | none | n/a |
| R009 | operability | out-of-scope | none | none | n/a |

## Coverage Summary

- Active requirements: 6
- Mapped to slices: 6
- Validated: 1 (R001 — pre-existing)
- Unmapped active requirements: 0
