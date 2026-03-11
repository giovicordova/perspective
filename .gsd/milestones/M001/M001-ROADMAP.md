# M001: Perspective v2

**Vision:** Evolve Perspective from a strategic-only reality check into a versatile code quality tool — adding coherency analysis to every run, a fast lightweight mode for focused questions, and polishing the repo for open-source distribution.

## Success Criteria

- `/perspective` produces a report that includes a Coherency section with real findings about duplication, overlap, and unnecessary complexity
- `/perspective quick <topic>` gives a fast focused answer without running the full pipeline
- The GitHub repo has LICENSE, clean README, correct metadata, no loose artifacts
- SKILL.md remains under 500 lines

## Key Risks / Unknowns

- **Line budget pressure** — 336 lines available for two new features plus report format changes
- **Quick mode detection** — need a clean heuristic to distinguish quick questions from focus areas

## Proof Strategy

- Line budget → retire in S04 by verifying final line count < 500
- Quick mode detection → retire in S03 by testing with varied invocation patterns

## Verification Classes

- Contract verification: SKILL.md line count, frontmatter validity, LICENSE exists, plugin.json fields correct
- Integration verification: invoke `/perspective` and `/perspective quick <topic>` on a real project, inspect report output
- Operational verification: none (stateless skill)
- UAT / human verification: user runs `/perspective` and `/perspective quick` on their own project

## Milestone Definition of Done

This milestone is complete only when all are true:

- All four slices are complete with passing verification
- SKILL.md is under 500 lines with coherency stage + quick mode + updated report format
- Repo has LICENSE, clean README, no loose artifacts, correct plugin.json
- `/perspective` has been invoked on a test project and produces a report with coherency findings
- `/perspective quick <topic>` has been invoked and produces a focused fast response

## Requirement Coverage

- Covers: R001, R002, R003, R004, R005, R006
- Partially covers: none
- Leaves for later: R007 (separate coherency command)
- Orphan risks: none

## Slices

- [ ] **S01: Repo hygiene** `risk:low` `depends:[]`
  > After this: `git clone` shows a properly structured OSS project — LICENSE, clean README, no loose artifacts, correct plugin.json metadata.

- [ ] **S02: Coherency analysis stage** `risk:medium` `depends:[]`
  > After this: `/perspective` on any project produces a report with a Coherency section identifying duplication, overlapping responsibilities, unnecessary complexity, and dead code.

- [ ] **S03: Lightweight quick mode** `risk:medium` `depends:[]`
  > After this: `/perspective quick auth setup` gives a fast, focused answer — reads code, checks live docs, responds directly — without running the full 5-stage pipeline.

- [ ] **S04: Final polish and line budget** `risk:low` `depends:[S01,S02,S03]`
  > After this: SKILL.md is verified under 500 lines, README documents all new features, and a full `/perspective` run on a test project produces the expected output.

## Boundary Map

### S01 (standalone)

Produces:
- `LICENSE` — MIT license file
- `.claude-plugin/plugin.json` — corrected repository field
- Clean repo root — no loose AUDIT-REPORT.md
- `.gitignore` — intentional perspective/ handling

Consumes:
- nothing (repo hygiene, independent of skill changes)

### S02 → S04

Produces:
- `skills/perspective/SKILL.md` — new coherency analysis stage (Stage 4, renumbering existing stages)
- Report template update — new `## Coherency` section in report format

Consumes:
- nothing (additive change to SKILL.md)

### S03 → S04

Produces:
- `skills/perspective/SKILL.md` — quick mode logic (detects `/perspective quick <topic>`, runs lightweight pipeline)
- Modified `$ARGUMENTS` handling to branch between full and quick modes

Consumes:
- nothing (additive change to SKILL.md)

### S04 (integration)

Produces:
- `README.md` — updated with coherency and quick mode documentation
- Final verified SKILL.md under 500 lines
- End-to-end verification evidence

Consumes from S01:
- Clean repo state (LICENSE, plugin.json, no loose files)

Consumes from S02:
- Coherency stage in SKILL.md, report template with Coherency section

Consumes from S03:
- Quick mode logic in SKILL.md, argument branching
