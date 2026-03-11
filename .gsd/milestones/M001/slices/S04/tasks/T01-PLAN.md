---
estimated_steps: 4
estimated_files: 1
---

# T01: Update README and verify all slice contracts

**Slice:** S04 — Final Polish and Line Budget
**Milestone:** M001

## Description

Update README.md to document the coherency analysis stage and quick mode, then run all verification commands to confirm SKILL.md line count, repo hygiene, and README accuracy. This is the only task in the slice — it closes R004 (README documentation) and R006 (line budget verification).

## Steps

1. Edit README.md "How It Works" section: insert step 3 "**Coherency**" describing duplication/overlap/complexity/dead-code analysis, renumber existing steps 3→4 (Audit), 4→5 (Report), 5→6 (Implement)
2. Edit README.md "Usage" section: add quick mode example after the focus area examples — `/perspective quick auth setup` with a brief explanation
3. Edit README.md "Output" section: add "Coherency findings (duplication, overlap, unnecessary complexity)" to the bullet list
4. Run all 8 verification commands from S04-PLAN.md and confirm all pass

## Must-Haves

- [ ] "How It Works" has 6 numbered steps with Coherency at position 3
- [ ] Quick mode usage documented with `/perspective quick <topic>` example
- [ ] "Output" section includes Coherency findings bullet
- [ ] `wc -l < skills/perspective/SKILL.md` < 500
- [ ] LICENSE exists, plugin.json valid JSON, no AUDIT-REPORT.md, .gitignore has .bg-shell

## Verification

- `grep -cP '^\d+\.' README.md` returns 6
- `grep -q "Coherency" README.md` passes
- `grep -q "perspective quick" README.md` passes
- `test $(wc -l < skills/perspective/SKILL.md) -lt 500` passes
- `test -f LICENSE && python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))" && ! test -f AUDIT-REPORT.md && grep -q ".bg-shell" .gitignore` passes

## Observability Impact

- Signals added/changed: None
- How a future agent inspects this: `cat README.md`, `wc -l skills/perspective/SKILL.md`
- Failure state exposed: None

## Inputs

- `README.md` — current 76-line file missing coherency and quick mode documentation
- `skills/perspective/SKILL.md` — 226-line file with all 6 stages and quick mode (read-only, just verify line count)
- S01 summary — confirms LICENSE, .gitignore, plugin.json already correct
- S02 summary — confirms Stage 3 Coherency Analysis exists in SKILL.md
- S03 summary — confirms Quick Mode section exists in SKILL.md

## Expected Output

- `README.md` — updated with 6-step pipeline description, quick mode usage example, and coherency in output list
