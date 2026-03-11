# S02: Coherency Analysis Stage

**Goal:** `/perspective` includes a coherency analysis stage that checks the codebase for structural health issues, and the report template includes a Coherency section for those findings.
**Demo:** Reading SKILL.md shows a new Stage 3 (Coherency Analysis) with instructions covering duplication, overlapping responsibilities, unnecessary complexity, and dead code. The report template has a `## Coherency` section. All stage numbers and references are consistent.

## Must-Haves

- New Stage 3: Coherency Analysis inserted between Research (Stage 2) and Audit (now Stage 4)
- Stage instructions cover five categories: duplication, overlapping responsibilities, unnecessary complexity, dead code, inconsistent patterns
- Report template has `## Coherency` section between `## Codebase Audit` and `## Recommendation`
- All stage count references updated ("six stages", "Stages 1–5 always run")
- Focus Area section updated to reflect new stage count
- Net addition stays under 55 lines (preserving budget for S03/S04)

## Proof Level

- This slice proves: contract (the SKILL.md file contains the correct structure and content)
- Real runtime required: no (this is prompt authoring — runtime proof deferred to S04's integration verification)
- Human/UAT required: no (S04 will run `/perspective` end-to-end)

## Verification

- `grep -c "Stage 3: Coherency" skills/perspective/SKILL.md` returns 1
- `grep -c "## Coherency" skills/perspective/SKILL.md` returns 1 (in report template)
- `grep "six stages" skills/perspective/SKILL.md` matches
- `grep "Stages 1–5 always run" skills/perspective/SKILL.md` matches
- `grep "Stage 4: Audit" skills/perspective/SKILL.md` matches (renumbered from 3)
- `grep "Stage 5: Report" skills/perspective/SKILL.md` matches (renumbered from 4)
- `grep "Stage 6: Implement" skills/perspective/SKILL.md` matches (renumbered from 5)
- `wc -l < skills/perspective/SKILL.md` is under 220 (164 + ~50 net addition)
- `grep "five stages" skills/perspective/SKILL.md` returns nothing (old reference removed)

## Observability / Diagnostics

- Runtime signals: None — this is a static markdown file, not a running system
- Inspection surfaces: `wc -l`, `grep`, direct file reading
- Failure visibility: Inconsistent stage numbering or missing sections are detectable by grep
- Redaction constraints: None

## Integration Closure

- Upstream surfaces consumed: None (S02 is independent per roadmap)
- New wiring introduced in this slice: New stage in SKILL.md pipeline, new section in report template
- What remains before the milestone is truly usable end-to-end: S03 (quick mode), S04 (integration verification with live `/perspective` run)

## Tasks

- [x] **T01: Add coherency stage and report section to SKILL.md** `est:20m` ✅
  - Why: This is the entire slice — insert the coherency analysis stage, update the report template, and fix all stage numbering references
  - Files: `skills/perspective/SKILL.md`
  - Do: (1) Update intro paragraph: "five stages" → "six stages", "Stages 1–4" → "Stages 1–5". (2) Insert new Stage 3: Coherency Analysis after Stage 2, with instructions covering duplication, overlapping responsibilities, unnecessary complexity, dead code, and inconsistent patterns — mirror the Audit stage's concise directive style (~20-30 lines). (3) Renumber Stage 3→4 (Audit), Stage 4→5 (Report), Stage 5→6 (Implement). (4) Insert `## Coherency` section in report template between Codebase Audit and Recommendation (~8-12 lines). (5) Update Focus Area section: "four stages" → "five stages". (6) Verify total line count stays under 220.
  - Verify: Run all verification grep/wc commands from slice verification section
  - Done when: All 9 verification checks pass, stage numbering is internally consistent, and coherency instructions cover all five categories from R002

## Files Likely Touched

- `skills/perspective/SKILL.md`
