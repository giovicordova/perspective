# S04: Final Polish and Line Budget — Research

**Date:** 2026-03-11

## Summary

S04 is a verification and documentation slice with no feature work. SKILL.md is at 226 lines (274 under budget), all prior slices landed cleanly, and the repo is structurally sound. The only real work is updating README.md to document coherency analysis and quick mode, then running end-to-end verification.

The README has three concrete gaps: the "How It Works" section describes 5 steps instead of 6 (missing coherency), there's no mention of quick mode usage, and the "Output" section doesn't list the Coherency report section. Everything else — LICENSE, plugin.json, .gitignore, no loose artifacts — is already confirmed passing from S01.

## Recommendation

Straightforward execution: update README, verify line count, verify repo state, commit. No architectural decisions, no new patterns, no risk. Single task.

## Don't Hand-Roll

No external dependencies or tools needed — this slice is pure markdown editing and verification.

## Existing Code and Patterns

- `skills/perspective/SKILL.md` (226 lines) — Complete with all 6 stages and quick mode. No changes needed — just verify line count.
- `README.md` (71 lines) — Needs 3 updates: add coherency to "How It Works" numbered list, add quick mode usage example, add coherency to "Output" bullet list.
- `.claude-plugin/plugin.json` — Valid JSON, correct fields. No changes needed.
- `LICENSE` — MIT, present. No changes needed.
- `.gitignore` — Clean, includes `.bg-shell/`. No changes needed.

## Constraints

- SKILL.md must stay under 500 lines (currently 226 — no changes planned, just verification)
- README updates must accurately reflect the actual SKILL.md behavior — stage numbering is 1–6 with 1–5 mandatory, Stage 6 optional
- Quick mode documentation must match the actual trigger: `/perspective quick <topic>` (not `/perspective quick` alone)

## Common Pitfalls

- **README step numbering drift** — README currently shows 5 numbered steps. SKILL.md has 6 stages. Must add Coherency as step 3 and renumber Audit→4, Report→5, Implement→6 to match SKILL.md exactly.
- **Forgetting the Output section** — The "Output" bullets list report contents but don't mention the Coherency section. Easy to miss since it's separate from "How It Works".

## Open Risks

- None. This is static markdown with deterministic verification. All inputs are known and stable.

## Skills Discovered

| Technology | Skill | Status |
|------------|-------|--------|
| N/A | N/A | No external technologies — pure markdown editing |

## Sources

- All context from S01/S02/S03 summaries and direct file inspection — no external research needed.
