# Project

## What This Is

Perspective is a Claude Code plugin (distributed as a standalone repo) that reality-checks projects. It's a single skill (`skills/perspective/SKILL.md`) bundled with a Context7 MCP dependency. Users invoke `/perspective` to get a structured report on whether their project approach is sound — covering landscape research, live-docs audit, and a strategic recommendation.

## Core Value

One command gives you an honest, structured assessment of whether you're building the right thing the right way — before you've sunk weeks into the wrong direction.

## Current State

v1.0.0 — functional skill with 5 stages (Understand, Research, Audit, Report, Implement). 164 lines. Plugin packaging works. Has been self-dogfooded. Repo hygiene has gaps (no LICENSE, loose artifacts, gitignore ambiguity). No coherency analysis or lightweight quick mode yet.

## Architecture / Key Patterns

- **Product = one markdown file:** `skills/perspective/SKILL.md` is the entire deliverable
- **Plugin wrapper:** `.claude-plugin/plugin.json` + `.mcp.json` for distribution and Context7 bundling
- **Context: fork** — skill runs in isolated subagent to avoid polluting user's main context
- **Reports output to:** `perspective/{date}-perspective-report.md` in the target project
- **Line cap:** SKILL.md must stay under 500 lines

## Capability Contract

See `.gsd/REQUIREMENTS.md` for the explicit capability contract, requirement status, and coverage mapping.

## Milestone Sequence

- [ ] M001: Perspective v2 — Add coherency analysis, quick mode, and open-source polish
