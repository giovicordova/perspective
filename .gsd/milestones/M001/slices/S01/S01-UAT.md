# S01: Repo Hygiene — UAT

**Milestone:** M001
**Written:** 2026-03-11

## UAT Type

- UAT mode: artifact-driven
- Why this mode is sufficient: This slice produces only static files (LICENSE, .gitignore edits). No runtime behavior to test — correctness is fully verifiable by checking file existence and content.

## Preconditions

- Repo cloned locally
- No special environment or server required

## Smoke Test

`test -f LICENSE && head -1 LICENSE | grep -q "MIT"` — confirms the primary deliverable exists and is correct type.

## Test Cases

### 1. LICENSE file exists and is MIT

1. Run `cat LICENSE`
2. **Expected:** File exists, first line contains "MIT", body contains year 2026 and copyright holder "Gio"

### 2. .gitignore covers runtime artifacts

1. Run `grep ".bg-shell" .gitignore`
2. **Expected:** `.bg-shell/` appears in the file

### 3. No loose artifacts at repo root

1. Run `ls AUDIT-REPORT.md 2>&1`
2. **Expected:** File not found — no stray audit output in repo root

### 4. plugin.json remains valid

1. Run `python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"`
2. **Expected:** No error — JSON parses cleanly

## Edge Cases

### Empty .gitignore scenario

1. If .gitignore were missing, the slice would need to create it
2. **Expected:** Not applicable — .gitignore already existed with content

## Failure Signals

- `LICENSE` file missing or wrong license type
- `.bg-shell/` not in `.gitignore`, causing runtime artifacts to appear in `git status`
- `AUDIT-REPORT.md` still present at repo root
- `plugin.json` invalid JSON after any edits

## Requirements Proved By This UAT

- R004 (partial) — LICENSE exists, .gitignore is clean, no loose artifacts. The README and final repo polish portions of R004 are deferred to S04.

## Not Proven By This UAT

- R004 README documentation — deferred to S04
- R004 final end-to-end repo structure check — deferred to S04
- All other requirements (R001–R003, R005–R006) are unrelated to this slice

## Notes for Tester

All checks are automated one-liners. No manual judgment needed — this is pure contract verification.
