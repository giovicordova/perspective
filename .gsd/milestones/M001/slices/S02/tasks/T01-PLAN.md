---
estimated_steps: 6
estimated_files: 1
---

# T01: Add coherency stage and report section to SKILL.md

**Slice:** S02 — Coherency Analysis Stage
**Milestone:** M001

## Description

Insert a new Stage 3 (Coherency Analysis) into SKILL.md between the existing Research stage and Audit stage. Add a corresponding Coherency section to the report template. Update all stage numbering and count references throughout the file. This single task delivers the entire S02 slice — the work is a focused set of edits to one markdown file.

## Steps

1. **Update intro paragraph** — Change "five stages" to "six stages" and "Stages 1–4 always run" to "Stages 1–5 always run"
2. **Insert Stage 3: Coherency Analysis** — Add new section after Stage 2 (Research the Landscape) with instructions covering: duplication, overlapping responsibilities, unnecessary complexity/abstraction, dead code, and inconsistent patterns. Follow the Audit stage's style: name what to look for, keep it directive, ~20-30 lines
3. **Renumber subsequent stages** — Stage 3 (Audit) → Stage 4, Stage 4 (Report) → Stage 5, Stage 5 (Implement) → Stage 6
4. **Insert Coherency section in report template** — Add `## Coherency` between `## Codebase Audit` and `## Recommendation` with compact format guidance (~8-12 lines)
5. **Update Focus Area section** — Change "four stages" to "five stages" (or equivalent reference)
6. **Verify** — Run all grep/wc checks to confirm internal consistency and line budget

## Must-Haves

- [ ] Stage 3: Coherency Analysis exists between Research and Audit
- [ ] Coherency stage covers all five R002 categories: duplication, overlapping responsibilities, unnecessary complexity, dead code, inconsistent patterns
- [ ] Report template has `## Coherency` section in correct position
- [ ] All stage numbers are sequential 1-6 with no gaps or duplicates
- [ ] Intro paragraph says "six stages" and "Stages 1–5 always run"
- [ ] Focus Area section reflects updated stage count
- [ ] Net line count under 220

## Verification

- `grep -c "Stage 3: Coherency" skills/perspective/SKILL.md` → 1
- `grep -c "## Coherency" skills/perspective/SKILL.md` → 1
- `grep "six stages" skills/perspective/SKILL.md` → matches
- `grep "Stages 1–5 always run" skills/perspective/SKILL.md` → matches
- `grep "Stage 4: Audit" skills/perspective/SKILL.md` → matches
- `grep "Stage 5: Report" skills/perspective/SKILL.md` → matches
- `grep "Stage 6: Implement" skills/perspective/SKILL.md` → matches
- `wc -l < skills/perspective/SKILL.md` → under 220
- `grep "five stages" skills/perspective/SKILL.md` → no matches (old reference removed)

## Observability Impact

- Signals added/changed: None — static markdown file
- How a future agent inspects this: Read the file, grep for stage headers and section names
- Failure state exposed: Inconsistent numbering or missing sections are grep-detectable

## Inputs

- `skills/perspective/SKILL.md` — current 164-line file with 5 stages
- S02-RESEARCH.md findings on stage structure, line budget, and insertion points
- D005: coherency stage goes after Research, before Audit
- D003: coherency findings go in the same report
- R002: categories to cover (duplication, overlapping responsibilities, unnecessary complexity, dead code)

## Expected Output

- `skills/perspective/SKILL.md` — updated with 6 stages, coherency analysis as Stage 3, Coherency section in report template, all references consistent, total under 220 lines
