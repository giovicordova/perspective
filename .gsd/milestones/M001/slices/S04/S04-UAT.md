# S04: Final Polish and Line Budget — UAT

**Milestone:** M001
**Written:** 2026-03-11

## UAT Type

- UAT mode: mixed (artifact-driven for contract checks + human-experience for runtime)
- Why this mode is sufficient: Contract checks verify all static properties (line count, file existence, README content). Runtime behavior requires human invocation of `/perspective` and `/perspective quick` on a real project — cannot be automated in this context.

## Preconditions

- Claude Code installed with Perspective plugin
- A target project to run `/perspective` against
- Context7 MCP available (bundled via `.mcp.json`)

## Smoke Test

Run `wc -l < skills/perspective/SKILL.md` — should return < 500. Open README.md and confirm it lists 6 steps with Coherency at step 3.

## Test Cases

### 1. Full pipeline produces coherency findings

1. Open a target project in Claude Code
2. Run `/perspective`
3. Wait for the full 6-stage pipeline to complete
4. Open the generated report in `perspective/`
5. **Expected:** Report contains a `## Coherency` section with findings about duplication, overlapping responsibilities, unnecessary complexity, or dead code (or a clean bill of health if none found)

### 2. Quick mode gives fast focused answer

1. Open a target project in Claude Code
2. Run `/perspective quick auth setup`
3. **Expected:** Fast response (no landscape research or full audit) that reads relevant code, checks live docs, and answers the question directly

### 3. README accurately reflects features

1. Open README.md
2. Check "How It Works" section
3. **Expected:** 6 numbered steps: Understand Scope → Research Landscape → Coherency → Audit → Report → Implement
4. Check "Usage" section
5. **Expected:** Quick mode example present (`/perspective quick <topic>`)
6. Check "Output" section
7. **Expected:** Coherency findings listed as output

## Edge Cases

### Quick mode with ambiguous topic

1. Run `/perspective quick` with no topic
2. **Expected:** Graceful handling — either prompts for a topic or runs a general quick check

### Focus area vs quick mode distinction

1. Run `/perspective authentication` (focus area, full run)
2. Run `/perspective quick authentication` (quick mode)
3. **Expected:** First runs full 6-stage pipeline focused on auth; second gives a fast focused answer

## Failure Signals

- Report missing `## Coherency` section after full run
- Quick mode runs all 6 stages instead of the 3-step fast flow
- README step count doesn't match SKILL.md stages
- SKILL.md exceeds 500 lines

## Requirements Proved By This UAT

- R004 — Repo hygiene verified by automated contract checks (LICENSE, plugin.json, no loose artifacts)
- R006 — Line count verified at 226 (< 500)
- R001 — Full pipeline produces a report (proved by test case 1, requires human execution)
- R002 — Coherency analysis findings appear in report (proved by test case 1, requires human execution)
- R003 — Quick mode gives fast focused answer (proved by test case 2, requires human execution)
- R005 — Report includes coherency section (proved by test case 1, requires human execution)

## Not Proven By This UAT

- Runtime quality of coherency findings (are they actually useful and accurate?)
- Quick mode response quality (is it genuinely helpful?)
- Performance — no benchmarks on pipeline duration
- R007 (deferred) — Separate coherency-only invocation not implemented

## Notes for Tester

- Contract checks (line count, file existence, README content) are already verified by automated checks in S04. Focus human testing on the runtime experience.
- The coherency stage may find nothing in a clean, small project — that's fine. Verify the section appears even if findings are minimal.
- Quick mode detection uses the "quick" keyword prefix — test with and without it to confirm the branching works.
