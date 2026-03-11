# S02: Coherency Analysis Stage — UAT

**Milestone:** M001
**Written:** 2026-03-11

## UAT Type

- UAT mode: artifact-driven
- Why this mode is sufficient: This slice is pure prompt authoring — a markdown file edit. There is no running system to test. The contract (correct structure, stage numbering, section presence) is fully verifiable via grep/wc. Runtime proof is deferred to S04's integration verification.

## Preconditions

- `skills/perspective/SKILL.md` exists and is readable

## Smoke Test

`grep -c "Stage 3: Coherency" skills/perspective/SKILL.md` returns 1

## Test Cases

### 1. Coherency stage exists with correct numbering

1. `grep "Stage 3: Coherency" skills/perspective/SKILL.md`
2. **Expected:** Matches exactly one line: `## Stage 3: Coherency Analysis`

### 2. Report template includes Coherency section

1. `grep "## Coherency" skills/perspective/SKILL.md`
2. **Expected:** Matches exactly one line in the report template area

### 3. All stages renumbered consistently

1. `grep "Stage 4: Audit" skills/perspective/SKILL.md`
2. `grep "Stage 5: Report" skills/perspective/SKILL.md`
3. `grep "Stage 6: Implement" skills/perspective/SKILL.md`
4. **Expected:** Each returns exactly one match

### 4. Intro paragraph updated

1. `grep "six stages" skills/perspective/SKILL.md`
2. `grep "Stages 1–5 always run" skills/perspective/SKILL.md`
3. **Expected:** Both match

### 5. Old references removed

1. `grep "five stages" skills/perspective/SKILL.md`
2. **Expected:** No matches (exit code 1)

### 6. Line budget maintained

1. `wc -l < skills/perspective/SKILL.md`
2. **Expected:** Under 220

## Edge Cases

### Coherency section covers all five categories

1. `grep -E "duplication|overlapping responsibilities|unnecessary complexity|dead code|inconsistent patterns" skills/perspective/SKILL.md`
2. **Expected:** At least 5 matches (one per category)

## Failure Signals

- Any verification grep returning unexpected count
- Line count exceeding 220
- "five stages" still present anywhere in the file
- Missing coherency category in Stage 3 instructions

## Requirements Proved By This UAT

- R002 (Coherency analysis) — contract-level proof that the stage exists with all five categories; runtime proof deferred to S04
- R005 (Report includes coherency section) — contract-level proof that report template has the section; runtime proof deferred to S04
- R006 (Under 500 lines) — file at 190 lines, well within budget

## Not Proven By This UAT

- R002/R005 runtime behavior — whether the coherency stage actually produces useful findings when `/perspective` runs on a real project (deferred to S04)
- R001 (Strategic reality check) — not affected by this slice, but the renumbering could theoretically break the flow (verified at runtime in S04)

## Notes for Tester

This is a static artifact slice. All verification is mechanical (grep/wc). The real test is S04, which will invoke `/perspective` on a test project and check that the coherency stage runs and produces findings in the report.
