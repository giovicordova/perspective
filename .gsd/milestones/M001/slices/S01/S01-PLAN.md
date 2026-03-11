# S01: Repo Hygiene

**Goal:** The repo root is clean for open-source distribution — LICENSE exists, .gitignore covers all runtime artifacts, no loose files.
**Demo:** `git clone` the repo and see a properly structured OSS project with MIT LICENSE, clean .gitignore, and no stray artifacts.

## Must-Haves

- MIT LICENSE file at repo root with correct year (2026) and author
- `.bg-shell/` added to `.gitignore`
- No loose artifacts (AUDIT-REPORT.md, stray output files) in repo root
- plugin.json remains valid and unchanged (string `repository` is fine per npm spec)

## Proof Level

- This slice proves: contract (file existence and content correctness)
- Real runtime required: no
- Human/UAT required: no

## Verification

- `test -f LICENSE && head -1 LICENSE | grep -q "MIT"` — LICENSE exists and is MIT
- `grep -q ".bg-shell" .gitignore` — .bg-shell is gitignored
- `! test -f AUDIT-REPORT.md` — no loose audit artifact
- `python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"` — plugin.json is valid JSON

## Observability / Diagnostics

- Runtime signals: none (static files only)
- Inspection surfaces: `ls -la` at repo root, `cat .gitignore`
- Failure visibility: git status shows untracked files if .gitignore is wrong
- Redaction constraints: none

## Integration Closure

- Upstream surfaces consumed: none (standalone slice)
- New wiring introduced in this slice: none
- What remains before the milestone is truly usable end-to-end: S02 (coherency stage), S03 (quick mode), S04 (final polish + line count verification)

## Tasks

- [x] **T01: Add MIT LICENSE and clean up .gitignore** `est:15m` ✅
  - Why: R004 requires LICENSE file and clean repo root. `.bg-shell/` is a runtime artifact that should be gitignored.
  - Files: `LICENSE`, `.gitignore`
  - Do: Create MIT LICENSE with year 2026 and author "Gio" (matching plugin.json). Add `.bg-shell/` to .gitignore near the other runtime ignores. Confirm no loose artifacts exist at root.
  - Verify: `test -f LICENSE && head -1 LICENSE | grep -q "MIT"` passes; `grep -q ".bg-shell" .gitignore` passes; `python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"` passes; `! test -f AUDIT-REPORT.md` passes
  - Done when: All four verification commands pass and `git status` shows only the intended new/modified files

## Files Likely Touched

- `LICENSE` (new)
- `.gitignore` (modified)
