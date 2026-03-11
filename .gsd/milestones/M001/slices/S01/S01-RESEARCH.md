# S01: Repo Hygiene — Research

**Date:** 2026-03-11

## Summary

S01 owns R004 (Open-source repo hygiene). The repo is close to clean — most of the gaps identified in the original audit have already been partially addressed. `AUDIT-REPORT.md` was never tracked in git. The `perspective/` directory is gitignored and has no tracked files. The main gaps are: no `LICENSE` file, and the `plugin.json` repository field uses a string instead of the npm-standard object format.

The `.gitignore` currently ignores the entire `/perspective/` directory (the report output folder). This is correct — perspective reports are project-specific output, not part of the plugin source. No change needed.

## Recommendation

Straightforward execution — create the LICENSE file (MIT, matching plugin.json and README), verify/fix plugin.json repository field format, and confirm no loose artifacts remain. No external research or library docs needed. This is pure file operations.

## Don't Hand-Roll

| Problem | Existing Solution | Why Use It |
|---------|------------------|------------|
| n/a — no libraries or frameworks involved | — | — |

## Existing Code and Patterns

- `.claude-plugin/plugin.json` — Plugin metadata. `repository` field is a plain string (`"https://github.com/giovicordova/perspective"`). npm spec allows string shorthand, but the object form `{ "type": "git", "url": "..." }` is more explicit. The string form is valid and sufficient — changing it is optional polish.
- `.gitignore` — Has `/perspective/` at the top (ignores report output dir). GSD-generated entries below. Clean and intentional.
- `README.md` — Already says "MIT" in the License section but no actual LICENSE file exists at root.
- `AUDIT-REPORT.md` — Does NOT exist on disk or in git history. The context doc mentioned it as a loose artifact, but it was either already cleaned up or never committed. No action needed.
- `perspective/` directory — Does not exist on disk currently. Gitignored. No tracked files. Clean.

## Constraints

- LICENSE must be MIT (matches plugin.json `"license": "MIT"` and README)
- plugin.json must remain valid JSON consumable by `claude plugin add`
- No changes to SKILL.md in this slice (repo hygiene only)

## Common Pitfalls

- **Wrong year or name in LICENSE** — Use the author name from plugin.json ("Gio") and current year (2026). Check if the GitHub profile has a full name to use instead.
- **Breaking plugin.json format** — The `claude plugin add` command parses this file. Only change fields we're confident about. The string form of `repository` is valid per npm spec — don't "fix" what isn't broken.
- **Forgetting .bg-shell in .gitignore** — `.bg-shell/` directory exists at repo root (GSD runtime artifact). It's not gitignored and shows as untracked. Should be added to `.gitignore`.

## Open Risks

- **Repository URL correctness** — The URL `https://github.com/giovicordova/perspective` matches `git remote -v`. Appears correct. Low risk.
- **plugin.json `repository` format** — String shorthand is valid npm convention. Changing to object form is optional. Risk of breaking `claude plugin add` if it expects a specific format — safest to leave as string unless we know the plugin system prefers objects.

## Skills Discovered

| Technology | Skill | Status |
|------------|-------|--------|
| n/a — no frameworks or services involved | — | — |

## Sources

- Repo state verified via `git status`, `git log`, `git ls-files`, filesystem inspection (source: local exploration)
- npm `repository` field spec allows string shorthand (source: npm docs, prior knowledge)
