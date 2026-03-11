---
estimated_steps: 4
estimated_files: 2
---

# T01: Add MIT LICENSE and clean up .gitignore

**Slice:** S01 — Repo Hygiene
**Milestone:** M001

## Description

Create the MIT LICENSE file and add `.bg-shell/` to `.gitignore`. This is the only task in S01 — it directly satisfies R004 (open-source repo hygiene). The repo already has no loose artifacts (AUDIT-REPORT.md doesn't exist, perspective/ is gitignored), and plugin.json is valid as-is. The only gaps are the missing LICENSE file and the un-gitignored `.bg-shell/` directory.

## Steps

1. Create `LICENSE` at repo root — standard MIT text, year 2026, copyright holder "Gio" (matching plugin.json author)
2. Add `.bg-shell/` to `.gitignore` — place it in the GSD baseline section alongside other GSD runtime entries
3. Verify: LICENSE exists and starts with MIT, .gitignore includes .bg-shell, plugin.json is valid JSON, no AUDIT-REPORT.md
4. Confirm `git status` shows only the intended changes (new LICENSE, modified .gitignore)

## Must-Haves

- [ ] LICENSE file exists at repo root with MIT license text
- [ ] LICENSE has correct year (2026) and author ("Gio")
- [ ] `.bg-shell/` is in `.gitignore`
- [ ] No loose artifacts at repo root
- [ ] plugin.json remains valid JSON (not modified, just confirmed)

## Verification

- `test -f LICENSE && head -1 LICENSE | grep -q "MIT"` exits 0
- `grep -q ".bg-shell" .gitignore` exits 0
- `python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"` exits 0
- `! test -f AUDIT-REPORT.md` exits 0
- `git status --porcelain` shows only `A LICENSE` and `M .gitignore` (plus any GSD state files)

## Observability Impact

- Signals added/changed: None
- How a future agent inspects this: `ls LICENSE` and `cat .gitignore`
- Failure state exposed: None

## Inputs

- `.claude-plugin/plugin.json` — author name ("Gio") and license field ("MIT") for consistency
- `.gitignore` — existing content to append to
- S01-RESEARCH.md findings — no AUDIT-REPORT.md exists, perspective/ already gitignored, plugin.json string format is fine

## Expected Output

- `LICENSE` — new MIT license file at repo root
- `.gitignore` — modified with `.bg-shell/` entry added
