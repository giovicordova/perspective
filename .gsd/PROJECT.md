# Project

## What This Is

Perspective is a Claude Code plugin (distributed as a standalone repo) that reality-checks projects. It's a single skill (`skills/perspective/SKILL.md`) bundled with a Context7 MCP dependency. Users invoke `/perspective` to get a structured report on whether their project approach is sound — covering landscape research, coherency analysis, live-docs audit, and a strategic recommendation.

## Core Value

One command gives you an honest, structured assessment of whether you're building the right thing the right way — before you've sunk weeks into the wrong direction.

## Current State

v2 complete. Functional skill with 6 stages (Understand, Research, Coherency, Audit, Report, Implement) plus lightweight quick mode (`/perspective quick <topic>`). 226 lines, well under the 500-line cap. Plugin packaging works. Repo is OSS-ready with MIT LICENSE, clean README, correct plugin.json metadata.

All contract checks passing. R004 (repo hygiene) and R006 (line budget) validated. R002 (coherency), R003 (quick mode), R005 (report coherency section) are contract-verified — runtime validation requires user UAT.

## Architecture / Key Patterns

- **Product = one markdown file:** `skills/perspective/SKILL.md` is the entire deliverable
- **Plugin wrapper:** `.claude-plugin/plugin.json` + `.mcp.json` for distribution and Context7 bundling
- **Context: fork** — skill runs in isolated subagent to avoid polluting user's main context
- **6-stage pipeline:** Understand → Research → Coherency → Audit → Report → Implement (stages 1–5 mandatory, stage 6 optional)
- **Quick mode:** `/perspective quick <topic>` early-exits before the full pipeline with a 3-step lightweight flow
- **Reports output to:** `perspective/{date}-perspective-report.md` in the target project
- **Line cap:** SKILL.md must stay under 500 lines (currently 226, 274 remaining)

## Capability Contract

See `.gsd/REQUIREMENTS.md` for the explicit capability contract, requirement status, and coverage mapping.

## Milestone Sequence

- [x] M001: Perspective v2 — Coherency analysis, quick mode, OSS polish (complete, pending user UAT for runtime validation)
