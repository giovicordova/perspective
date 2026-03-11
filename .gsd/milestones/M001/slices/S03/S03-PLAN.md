# S03: Lightweight Quick Mode

**Goal:** `/perspective quick <topic>` runs a fast, focused 3-step flow (read code → check live docs → answer inline) without triggering the full 6-stage pipeline.
**Demo:** SKILL.md contains a quick-mode section that intercepts `quick` prefix in `$ARGUMENTS`, runs a lightweight pipeline, and responds inline with no file output. Full pipeline remains untouched.

## Must-Haves

- Quick mode section in SKILL.md placed after intro, before Stage 1 — acts as early-exit branch
- Detection logic: check if `$ARGUMENTS` starts with `quick`; topic is everything after the `quick` keyword
- Quick pipeline has exactly 3 steps: (a) read relevant code, (b) pull live docs via Context7, (c) respond inline
- No file output — quick mode responds directly in conversation, no `perspective/` directory writes
- `argument-hint` frontmatter updated to signal both modes (e.g. `[quick <topic> | focus area]`)
- Full pipeline (Stages 1–6) remains completely untouched — no conditional branching injected into existing stages
- SKILL.md stays under 500 lines after changes

## Proof Level

- This slice proves: contract (SKILL.md structure and content correctness)
- Real runtime required: no (skill is a prompt file — runtime = Claude Code interpreting it, which is UAT)
- Human/UAT required: yes (user must invoke `/perspective quick <topic>` to prove it works end-to-end; planned for S04 final verification)

## Verification

- `wc -l skills/perspective/SKILL.md` — under 500 lines
- `head -15 skills/perspective/SKILL.md` — `argument-hint` includes `quick`
- `grep -c 'quick' skills/perspective/SKILL.md` — quick mode content present (≥5 occurrences)
- `grep -n 'Stage 1' skills/perspective/SKILL.md` — quick mode section appears before Stage 1
- Content review: quick mode section contains all 3 steps (read code, check docs, answer inline) and explicitly says no file output
- Negative check: no changes to existing Stage 1–6 sections (diff shows additions only, no modifications to existing stage content)

## Observability / Diagnostics

- Runtime signals: None — this is a prompt file, not a runtime system
- Inspection surfaces: `skills/perspective/SKILL.md` content itself; line count via `wc -l`
- Failure visibility: If quick mode doesn't trigger, the full pipeline runs instead — user will notice the slow multi-stage flow instead of a fast inline answer
- Redaction constraints: None

## Integration Closure

- Upstream surfaces consumed: `skills/perspective/SKILL.md` (current 190-line file with 6-stage pipeline)
- New wiring introduced in this slice: `$ARGUMENTS` branching at top of skill — `quick` prefix → lightweight path, anything else → existing full pipeline
- What remains before the milestone is truly usable end-to-end: S04 final polish — README update documenting quick mode, line budget verification, end-to-end UAT with real invocations

## Tasks

- [x] **T01: Add quick mode branch to SKILL.md** `est:30m`
  - Why: This is the entire feature — R003 requires a lightweight mode that reads code, checks live docs, and answers directly. The quick mode section must intercept before the full pipeline and exit early.
  - Files: `skills/perspective/SKILL.md`
  - Do: (1) Update `argument-hint` in frontmatter to `[quick <topic> | focus area]`. (2) Add a new section after the intro paragraph and before Stage 1 titled "## Quick Mode" that checks if `$ARGUMENTS` starts with `quick`, extracts the topic, runs 3 steps (read code for topic, pull live docs via Context7, respond inline), and explicitly says to stop — do not continue to Stage 1. (3) Keep the section to ~40-50 lines max. (4) Do NOT modify any existing Stage 1–6 content. (5) Reference tools by name not stage number to avoid coupling per S02 forward intel.
  - Verify: `wc -l < skills/perspective/SKILL.md` under 500; `argument-hint` contains `quick`; quick mode section exists before Stage 1; grep confirms 3-step flow; diff shows no changes to existing stages
  - Done when: SKILL.md has a complete quick mode section that would cause Claude Code to take the fast path on `/perspective quick auth setup` and the full path on `/perspective authentication`

## Files Likely Touched

- `skills/perspective/SKILL.md`
