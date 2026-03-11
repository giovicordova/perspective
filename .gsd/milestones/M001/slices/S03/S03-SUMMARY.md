---
id: S03
parent: M001
milestone: M001
provides:
  - Quick mode early-exit branch in SKILL.md — `/perspective quick <topic>` runs lightweight 3-step flow
requires: []
affects:
  - S04
key_files:
  - skills/perspective/SKILL.md
key_decisions:
  - Quick mode section uses ### sub-headings for its 3 steps, keeping ## reserved for top-level pipeline sections
patterns_established:
  - Early-exit branch pattern: detect prefix in $ARGUMENTS, run lightweight flow, explicitly stop before full pipeline
observability_surfaces:
  - none
drill_down_paths:
  - tasks/T01-SUMMARY.md
duration: 10m
verification_result: passed
completed_at: 2026-03-11
---

# S03: Lightweight Quick Mode

**Added 36-line quick mode section to SKILL.md that intercepts `/perspective quick <topic>` before the full 6-stage pipeline, running a fast 3-step flow (read code → check live docs → respond inline) with no file output.**

## What Happened

Single-task slice. Updated `argument-hint` frontmatter to `"[quick <topic> | focus area]"` to signal both invocation modes. Added a `## Quick Mode` section (lines 23–57) between the intro and Stage 1 that:

- Detects `quick` prefix in `$ARGUMENTS` and extracts the topic
- Runs 3 steps: (1) Read relevant code via Grep/Glob/Read, (2) Check live docs via Context7 MCP, (3) Respond inline with findings and recommendations
- Explicitly stops — no file output, no `perspective/` directory, no continuation to Stage 1
- Skips entirely for non-quick arguments, falling through to the full pipeline

No existing Stage 1–6 content was modified.

## Verification

- `wc -l < skills/perspective/SKILL.md` → 226 (under 500) ✓
- `argument-hint` frontmatter includes `quick` ✓
- `grep -c 'quick'` → 5 occurrences (≥5) ✓
- Quick Mode section at line 23, Stage 1 at line 59 — correct ordering ✓
- Content review: all 3 steps present with clear instructions, no file output explicitly stated ✓
- Negative check: git diff shows no modifications to existing Stage 1–6 content ✓

## Requirements Advanced

- R003 — Quick mode logic fully implemented in SKILL.md; contract-verified but not yet runtime-tested

## Requirements Validated

- none (R003 requires live invocation for validation, planned in S04 UAT)

## New Requirements Surfaced

- none

## Requirements Invalidated or Re-scoped

- none

## Deviations

None.

## Known Limitations

- Quick mode is contract-verified only — actual runtime behavior depends on Claude Code interpreting the prompt correctly, which is tested in S04 UAT
- Context7 step says "skip if no external dependency involvement" — edge cases where the topic straddles internal/external may behave inconsistently

## Follow-ups

- S04: README update documenting quick mode usage
- S04: End-to-end UAT invoking `/perspective quick <topic>` on a real project

## Files Created/Modified

- `skills/perspective/SKILL.md` — Added quick mode section (lines 23–57) and updated argument-hint frontmatter

## Forward Intelligence

### What the next slice should know
- SKILL.md is at 226 lines — 274 lines of budget remaining for S04 polish (no new feature content expected)
- Quick mode section is self-contained; S04 just needs to document it in README and verify via live invocation

### What's fragile
- The `quick` prefix detection relies on Claude Code correctly parsing `$ARGUMENTS` — there's no programmatic enforcement, just prompt instructions

### Authoritative diagnostics
- `wc -l < skills/perspective/SKILL.md` and `grep -n '## Quick Mode' skills/perspective/SKILL.md` — fastest way to confirm quick mode is present and sized correctly

### What assumptions changed
- None — implementation matched the plan exactly
