---
id: T01
parent: S03
milestone: M001
provides:
  - Quick mode early-exit branch in SKILL.md
key_files:
  - skills/perspective/SKILL.md
key_decisions:
  - Quick mode section uses ### sub-headings for its 3 steps, keeping ## reserved for top-level pipeline sections
patterns_established:
  - Early-exit branch pattern: detect prefix in $ARGUMENTS, run lightweight flow, explicitly stop before full pipeline
observability_surfaces:
  - none
duration: 10m
verification_result: passed
completed_at: 2026-03-11
blocker_discovered: false
---

# T01: Add quick mode branch to SKILL.md

**Added 36-line quick mode section to SKILL.md that intercepts `/perspective quick <topic>` before the full 6-stage pipeline.**

## What Happened

Updated `argument-hint` frontmatter from `"[focus area]"` to `"[quick <topic> | focus area]"` so Claude Code shows both invocation modes.

Added a `## Quick Mode` section (lines 23–57) between the intro paragraph and Stage 1. The section:
- Detects `quick` prefix in `$ARGUMENTS` and extracts the topic
- Runs a 3-step pipeline: (1) Read relevant code via Grep/Glob/Read, (2) Check live docs via Context7 MCP, (3) Respond inline with findings and recommendations
- Explicitly instructs: no file output, no `perspective/` directory, no continuation to Stage 1
- Explicitly instructs: skip entirely for non-quick arguments

No existing Stage 1–6 content was modified. Focus Area section at the bottom remains intact.

## Verification

- `wc -l < skills/perspective/SKILL.md` → 226 (under 500) ✓
- `grep 'argument-hint' skills/perspective/SKILL.md` → includes `quick` ✓
- `grep -n '## Quick Mode'` → line 23; `grep -n '## Stage 1'` → line 59 (correct ordering) ✓
- `grep -c 'quick'` → 5 occurrences (≥5) ✓
- `git diff` shows additions only in frontmatter line and new section — zero changes to Stage 1–6 blocks ✓
- Focus Area section still present at line 224, unchanged ✓

## Diagnostics

None — this is a prompt file. Inspect via `cat skills/perspective/SKILL.md` and `wc -l`.

## Deviations

None.

## Known Issues

None.

## Files Created/Modified

- `skills/perspective/SKILL.md` — Added quick mode section (lines 23–57) and updated argument-hint frontmatter
