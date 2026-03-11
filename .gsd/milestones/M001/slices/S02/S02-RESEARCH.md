# S02: Coherency Analysis Stage — Research

**Date:** 2026-03-11

## Summary

S02 adds a coherency analysis stage to the Perspective skill and a corresponding Coherency section to the report template. The current SKILL.md is 164 lines with 5 stages. Per D005, the new stage slots in as Stage 4 (after Research/Stage 2, before the existing Audit/Stage 3), renumbering existing Stage 3→4 (Audit becomes Stage 4), Stage 4→5 (Report), and Stage 5→6 (Implement).

The work is purely additive markdown authoring — no code, no dependencies, no build steps. The main constraints are line budget (336 lines available, this slice should use ~40-60 to leave room for S03's quick mode) and prompt quality (the stage instructions must reliably guide Claude to find real coherency issues across diverse codebases).

Recommendation: Write the coherency stage as a concise, directive prompt section (~25-35 lines) and add a compact Coherency section to the report template (~8-12 lines). Keep instructions specific and actionable — tell Claude exactly what to look for and how to present findings. Total addition target: ~40-50 lines.

## Recommendation

**Insert a new Stage 3: Coherency Analysis** between the current Stage 2 (Research) and Stage 3 (Audit). Renumber all subsequent stages. The stage should instruct Claude to scan the codebase for five specific categories of structural issues, using the project understanding from Stage 1 as context. The report template gets a new `## Coherency` section between `## Codebase Audit` and `## Recommendation`.

Keep the stage lean — this is prompt engineering, not code. Overly detailed instructions will eat line budget and may actually constrain Claude's analysis. The sweet spot is: name the categories, give 1-2 examples per category, set the output expectation, and let Claude's code analysis capabilities do the rest.

## Don't Hand-Roll

| Problem | Existing Solution | Why Use It |
|---------|------------------|------------|
| N/A — this slice is prompt authoring | N/A | No libraries or tools needed beyond what SKILL.md already uses |

## Existing Code and Patterns

- `skills/perspective/SKILL.md` lines 71-86 — **Stage 3 (Audit)** is the closest pattern to follow. It's 15 lines, uses bullet lists for "what to check", focuses on 3-5 most important items, and delegates judgment to Claude. The coherency stage should mirror this structure.
- `skills/perspective/SKILL.md` lines 87-145 — **Report template** uses H2 sections inside a markdown code fence. The Coherency section must be inserted here, matching the existing style (H2 heading, brief format guidance, bullet structure).
- `skills/perspective/SKILL.md` lines 23-50 — **Stage 1 (Understand)** reads the codebase structure. Coherency analysis depends on this mental model being built first. No need to re-read files — Stage 1 output is in Claude's context.
- `skills/perspective/SKILL.md` line 21 — `"Stages 1–4 always run"` must be updated to `"Stages 1–5 always run"` (since Report becomes Stage 5, Implement becomes Stage 6).

## Constraints

- **Line budget:** 336 lines available total across S02, S03, S04. This stage should target ~40-50 net new lines to leave ~280+ for quick mode and polish. The Audit stage (15 lines) is a good reference for stage length.
- **Stage renumbering cascade:** Inserting a new Stage 3 means updating: the intro paragraph ("five stages" → "six stages", "Stages 1–4" → "Stages 1–5"), all stage headers after the insertion point, and the Focus Area section's reference to "four stages".
- **No new tools needed:** The coherency stage uses the same `Read`, `Bash`, `Grep`, `Glob` tools already in the skill's `allowed-tools`. No MCP calls needed — this is pure codebase analysis, not docs lookup.
- **Fork context:** The skill runs in `context: fork`. Claude has the full codebase available but cannot modify it during analysis stages. This is fine — coherency analysis is read-only.
- **Report section ordering:** Per D003, coherency goes in the same report. Logical placement is after Codebase Audit (which checks deps/patterns against docs) and before Recommendation (which synthesizes everything). This means the report template section order becomes: What You're Building → What Already Exists → Alternative Approaches → Codebase Audit → Coherency → Recommendation → Action Items.

## Common Pitfalls

- **Vague instructions produce vague findings** — Don't say "check for code quality issues." Name specific categories: duplication, overlapping responsibilities, dead code, unnecessary abstraction layers, inconsistent patterns. Concrete categories produce concrete findings.
- **Over-specifying the analysis method** — Don't prescribe exact grep commands or file-by-file analysis steps. Claude knows how to explore code. Specify *what to find*, not *how to find it*. The Audit stage follows this pattern well.
- **Coherency section too verbose in template** — The report template sections are compact (2-6 lines of format guidance each). The Coherency section should match: category name, what was found, severity/impact indication. Don't template a 20-line structure.
- **Forgetting to update stage count references** — The intro paragraph says "five stages" and "Stages 1–4 always run." The Focus Area section says "four stages." All must be updated or the skill will be internally inconsistent.
- **Confusing coherency with the existing audit** — Audit checks code against *external docs and current practices*. Coherency checks the codebase against *itself* — internal consistency, duplication, structural health. The stage description must make this distinction clear so Claude doesn't duplicate Audit work.

## Open Risks

- **Analysis quality on large codebases** — Claude's coherency findings depend on how much of the codebase fits in the fork context window. Very large projects may get shallow analysis. Mitigation: the stage can instruct Claude to focus on the most architecturally significant areas rather than trying to cover everything.
- **Line count creep** — If the stage instructions and report section together exceed ~50 lines, S03 (quick mode) will be squeezed. Must be disciplined about conciseness.
- **Stage ordering debate** — D005 places coherency after Research and before Audit. This means coherency runs without the benefit of the docs audit findings. The rationale (coherency is about internal structure, not external practices) is sound, but it's worth noting that some coherency issues (like using deprecated patterns inconsistently) straddle both stages.

## Skills Discovered

| Technology | Skill | Status |
|------------|-------|--------|
| Code analysis / modularization | `dotneet/claude-code-marketplace@code-modularization-evaluator` (50 installs) | available — not relevant (different purpose: evaluates modularization of a specific codebase, not prompt authoring for a skill) |
| Code analysis | `oimiragieo/agent-studio@code-analyzer` (65 installs) | available — not relevant (general code analyzer, not related to writing analysis prompts) |

No skills are directly useful for this slice. The work is prompt engineering in a markdown file — no frameworks or libraries involved.

## Sources

- Current SKILL.md structure and patterns (source: `skills/perspective/SKILL.md`, 164 lines)
- Decision D005: Coherency stage position — after Research, before Audit (source: `.gsd/DECISIONS.md`)
- Decision D003: Same report, new section (source: `.gsd/DECISIONS.md`)
- Requirement R002: Coherency analysis scope — duplication, overlapping responsibilities, unnecessary complexity, dead code (source: `.gsd/REQUIREMENTS.md`)
- Requirement R005: Report includes Coherency section (source: `.gsd/REQUIREMENTS.md`)
