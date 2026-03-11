# S03: Lightweight Quick Mode — Research

**Date:** 2026-03-11

## Summary

Quick mode adds an argument-triggered branch to SKILL.md that skips the full 6-stage pipeline when the user invokes `/perspective quick <topic>`. The implementation is straightforward: detect the `quick` prefix in `$ARGUMENTS`, run a focused 3-step flow (read code → check live docs → answer directly), and output an inline response instead of a file-based report.

The main design tension is how much of the existing pipeline to reuse vs. how much to write as a separate flow. The cleanest approach is a dedicated section at the top of SKILL.md (after frontmatter, before the full pipeline) that intercepts the `quick` prefix and runs a self-contained lightweight flow. This avoids polluting the existing 6-stage instructions with conditional branching.

Line budget is comfortable: SKILL.md is at 190 lines with 310 remaining. Quick mode should add ~40-60 lines, leaving ~250 lines of headroom for S04.

## Recommendation

Add a new top-level section between the intro paragraph and Stage 1 that acts as an early-exit branch. Structure:

1. **Detection** — Check if `$ARGUMENTS` starts with `quick`. If not, fall through to the full pipeline.
2. **Quick pipeline** — Three steps: (a) read relevant code for the topic, (b) pull live docs via Context7, (c) respond inline with findings and recommendations.
3. **No report file** — Quick mode responds directly in the conversation. No `perspective/` directory output.

This matches D002 (explicit `quick` keyword prefix) and keeps the full pipeline untouched.

## Don't Hand-Roll

| Problem | Existing Solution | Why Use It |
|---------|------------------|------------|
| Live docs lookup | Context7 MCP (`mcp__context7__*`) | Already bundled in `.mcp.json` and used by Stage 4 |
| Code exploration | `Read`, `Grep`, `Glob`, `Bash` | Already in `allowed-tools` |

No new tools or dependencies needed. Quick mode uses a subset of what's already available.

## Existing Code and Patterns

- `skills/perspective/SKILL.md:190` (Focus Area section) — Currently handles `$ARGUMENTS` as a focus area for the full pipeline. Quick mode must intercept _before_ this section to avoid the full pipeline treating "quick auth setup" as a focus area.
- `skills/perspective/SKILL.md:1-14` (frontmatter) — `argument-hint` is currently `"[focus area]"`. Needs updating to `"[quick <topic> | focus area]"` or similar to signal both modes.
- `skills/perspective/SKILL.md:16-18` (intro paragraph) — References "six stages" and "Stages 1–5 always run". Quick mode section should come after this intro so the intro remains accurate for the full pipeline path.
- S02 forward intel — Stage numbers appear in multiple places; quick mode should reference stages by name not number to avoid coupling.

## Constraints

- **Line budget** — 310 lines remaining. Quick mode should target 40-60 lines max. This is achievable — the flow is simple.
- **`argument-hint` frontmatter** — Must be valid and descriptive. Claude Code uses this to show usage hints.
- **`$ARGUMENTS` is a raw string** — No parsing library. Detection is just "does it start with `quick`". The remainder after `quick ` is the topic.
- **`context: fork` preserved** — Quick mode runs in the same forked context as full mode. No change needed.
- **Allowed tools unchanged** — Quick mode uses `Read`, `Grep`, `Glob`, `Bash`, `mcp__context7__*`, `WebSearch`, `WebFetch` — all already permitted.

## Common Pitfalls

- **Ambiguity with focus area** — If quick mode detection isn't the _first_ thing checked, `$ARGUMENTS = "quick auth"` could be interpreted as focus area "quick auth" by the full pipeline. Fix: quick mode section must appear before any full-pipeline logic and explicitly exit early.
- **Over-engineering the quick response** — Quick mode's value is speed. If it tries to produce a structured report or write files, it loses its purpose. Fix: respond inline only, no file output.
- **Scope creep in quick instructions** — Easy to add "also check for X, Y, Z" and turn quick mode into a mini full-run. Fix: keep it to exactly three steps — read code, check docs, answer.
- **Topic extraction** — `$ARGUMENTS` will be `"quick auth setup"`. The topic is everything after `"quick "`. Instructions should say "the topic is whatever follows the `quick` keyword" rather than trying to parse sub-arguments.

## Open Risks

- **Quick mode quality** — With no landscape research or coherency check, quick answers might miss important context. Acceptable trade-off per R003 — users explicitly want speed over comprehensiveness.
- **Proof strategy for D002** — The roadmap says "retire in S03 by testing with varied invocation patterns." The slice UAT should include at least: `/perspective quick auth setup`, `/perspective quick why is X slow`, and `/perspective authentication` (should NOT trigger quick mode).

## Skills Discovered

| Technology | Skill | Status |
|------------|-------|--------|
| Claude Code plugins | `anthropics/claude-code@plugin-structure` | available (1.3K installs) — not needed, we already know the format |

No skills needed for this slice. The work is editing a markdown skill file — no framework or library knowledge required beyond what's already in the codebase.

## Sources

- S02 summary forward intelligence — line count at 190, stage numbering fragility, budget guidance
- D002 decision — `/perspective quick <topic>` prefix keyword chosen as trigger
- R003 requirement — reads code, checks live docs, answers directly, skips full pipeline
- Current SKILL.md — argument handling at line 190, frontmatter at lines 1-14
