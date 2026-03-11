# M001: Perspective v2 — Context

**Gathered:** 2026-03-11
**Status:** Ready for planning

## Project Description

Perspective is a Claude Code plugin with one skill that reality-checks projects. This milestone adds coherency analysis, a lightweight quick mode, and polishes the repo for open-source consumption.

## Why This Milestone

The skill works but is limited to strategic-level analysis. Users also want structural health checks (duplication, overlap, complexity) and a fast mode for focused questions. The repo needs basic OSS hygiene before promoting it publicly.

## User-Visible Outcome

### When this milestone is complete, the user can:

- Run `/perspective` and get a report that includes coherency analysis (duplication, overlapping code, unnecessary complexity) alongside the existing strategic analysis
- Run `/perspective quick auth setup` and get a fast, focused answer without the full 5-stage pipeline
- Clone the repo from GitHub and see a properly structured open-source project (LICENSE, clean README, no loose artifacts)

### Entry point / environment

- Entry point: `/perspective` command in Claude Code CLI
- Environment: any project directory with Claude Code installed
- Live dependencies involved: Context7 MCP (bundled), web search (for research stage)

## Completion Class

- Contract complete means: SKILL.md contains all new stages/modes, is under 500 lines, frontmatter is valid
- Integration complete means: `/perspective` invoked on a test project produces a report with coherency section; `/perspective quick <topic>` produces a focused answer
- Operational complete means: none (stateless skill, no lifecycle)

## Final Integrated Acceptance

To call this milestone complete, we must prove:

- `/perspective` on a real project produces a report with the coherency section populated with real findings
- `/perspective quick <focused question>` skips landscape research and gives a fast answer
- `git clone` of the repo shows LICENSE, clean README, no loose artifacts, correct plugin.json

## Risks and Unknowns

- **Line budget** — Currently 164 lines, cap is 500. Adding coherency analysis + quick mode + report format changes must fit in ~336 lines. This is the main constraint.
- **Quick mode detection** — Need a reliable heuristic to distinguish "focus area for full run" (`/perspective authentication`) from "quick question" (`/perspective quick why is my auth slow`). The `quick` keyword prefix is simplest.
- **Coherency analysis quality** — The skill instructs Claude to find duplication/overlap, but the quality depends on Claude's ability to analyze the target codebase in a forked context. Large codebases may hit context limits.

## Existing Codebase / Prior Art

- `skills/perspective/SKILL.md` — The entire product. 164 lines, 5 stages. All changes go here.
- `.claude-plugin/plugin.json` — Plugin metadata. Needs repo URL verified.
- `.mcp.json` — Context7 MCP config. No changes needed.
- `README.md` — Public-facing docs. Must be updated to reflect new features.
- `CLAUDE.md` — Dev instructions. May need minor updates.
- `AUDIT-REPORT.md` — Loose artifact at root. Should be removed or archived.
- `perspective/2026-03-08-perspective-report.md` — Self-dogfooding output. Gitignored but tracked — ambiguous state.

> See `.gsd/DECISIONS.md` for all architectural and pattern decisions — it is an append-only register; read it during planning, append to it during execution.

## Relevant Requirements

- R001 — Pre-existing core capability, extended by coherency stage
- R002 — Coherency analysis (new)
- R003 — Lightweight quick mode (new)
- R004 — Open-source repo hygiene
- R005 — Report includes coherency section
- R006 — Skill stays under 500 lines

## Scope

### In Scope

- Add coherency analysis as a new stage in the full pipeline
- Add coherency section to report template
- Add lightweight quick mode triggered by `/perspective quick <topic>`
- Fix all repo hygiene gaps from audit (LICENSE, loose files, gitignore, repo URL)
- Update README for new features
- Verify SKILL.md stays under 500 lines

### Out of Scope / Non-Goals

- Separate `/perspective coherency` command (deferred — R007)
- CONTRIBUTING.md, issue templates, CoC (out of scope — R008)
- CI/CD (out of scope — R009)
- Multi-agent or parallel execution architecture
- Changes to Context7 MCP configuration

## Technical Constraints

- SKILL.md must stay under 500 lines (currently 164, budget ~336)
- Plugin structure must remain compatible with `claude plugin add`
- `context: fork` must be preserved — skill runs in isolated context
- No new MCP dependencies — Context7 is sufficient

## Integration Points

- Context7 MCP — used for live-docs audit stage (no changes needed)
- Claude Code plugin system — must maintain valid plugin.json and skill frontmatter
- Web search tools — used for landscape research (no changes needed)

## Open Questions

- None — all questions resolved during discussion
