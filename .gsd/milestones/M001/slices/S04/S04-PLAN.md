# S04: Final Polish and Line Budget

**Goal:** README documents all features (coherency analysis + quick mode), SKILL.md verified under 500 lines, repo state confirmed clean.
**Demo:** `README.md` accurately describes the 6-stage pipeline and quick mode usage; all contract checks pass; repo is ready for distribution.

## Must-Haves

- README "How It Works" lists 6 steps matching SKILL.md stages (Coherency at step 3, renumber 4–6)
- README includes quick mode usage example (`/perspective quick <topic>`)
- README "Output" section lists Coherency findings
- SKILL.md line count verified under 500
- Repo state clean: LICENSE exists, plugin.json valid, no loose artifacts, .gitignore correct

## Proof Level

- This slice proves: final-assembly
- Real runtime required: no (contract verification — runtime UAT is a separate user step)
- Human/UAT required: yes (user runs `/perspective` and `/perspective quick` on their own project as milestone DoD)

## Verification

```bash
# R006: SKILL.md under 500 lines
test $(wc -l < skills/perspective/SKILL.md) -lt 500

# README documents coherency stage
grep -q "Coherency" README.md

# README documents quick mode
grep -q "perspective quick" README.md

# README has 6 numbered steps in How It Works
grep -cP '^\d+\.' README.md | grep -q '6'

# R004: Repo hygiene (carried from S01)
test -f LICENSE
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"
! test -f AUDIT-REPORT.md
grep -q ".bg-shell" .gitignore
```

## Observability / Diagnostics

- Runtime signals: none — static markdown files
- Inspection surfaces: `wc -l`, `grep`, `cat` on repo files
- Failure visibility: verification commands above produce immediate pass/fail
- Redaction constraints: none

## Integration Closure

- Upstream surfaces consumed: `skills/perspective/SKILL.md` (S02 coherency stage, S03 quick mode), `LICENSE` and `.gitignore` (S01)
- New wiring introduced in this slice: none — documentation only
- What remains before the milestone is truly usable end-to-end: user UAT — invoke `/perspective` and `/perspective quick <topic>` on a real project to validate runtime behavior

## Tasks

- [x] **T01: Update README and verify all slice contracts** `est:15m`
  - Why: README is the only file that needs changes — it's missing coherency (step 3) and quick mode documentation. Everything else is verification of prior slice work.
  - Files: `README.md`
  - Do: (1) Add Coherency as step 3 in "How It Works", renumber Audit→4, Report→5, Implement→6. (2) Add quick mode usage example after the focus area examples. (3) Add Coherency findings to "Output" bullet list. (4) Run all verification commands.
  - Verify: All 8 verification commands above pass.
  - Done when: README accurately reflects SKILL.md behavior, all contract checks green, `git diff` shows only README.md changed.

## Files Likely Touched

- `README.md`
