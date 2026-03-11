# Perspective

A Claude Code plugin that reality-checks your project before you go deep.

## What It Does

You're mid-project. You invoke `/perspective`. It reads your project docs, confirms it understands what you're building, researches the landscape for existing solutions and better approaches, audits your codebase against current (not training-data-stale) documentation, and writes a report with an honest recommendation: stay the course, adjust, pivot, or adopt something that already exists.

## Install

### As a plugin (recommended)

```bash
claude plugin add /path/to/perspective
```

This installs the skill and its Context7 MCP dependency in one step.

### Standalone skill (manual)

If you prefer to install the skill without the plugin wrapper:

1. **Install Context7 MCP** (required for live documentation audit):

```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```

Requires Node.js 18+.

2. **Copy the skill:**

```bash
cp -r skills/perspective ~/.claude/skills/perspective
```

## Usage

```text
/perspective
```

Or with a specific focus area:

```text
/perspective authentication
/perspective state management
```

Or use quick mode for a fast, lightweight review of a specific topic:

```text
/perspective quick auth setup
```

## How It Works

1. **Understand** — reads your project's root documents (README, CLAUDE.md, VISION, package.json, etc.) and any previous perspective reports, then summarizes what you're building.
2. **Research** — searches for existing solutions, similar projects, alternative approaches, and current best practices. Skips areas covered by previous reports.
3. **Coherency** — analyzes your codebase for duplication, overlapping responsibilities, unnecessary complexity, and dead code that may have accumulated during development.
4. **Audit** — uses Context7 MCP to check your actual code against live documentation. Catches deprecated APIs, outdated patterns, and stale dependency versions that Claude wouldn't know about from training data alone.
5. **Report** — writes a structured analysis to `perspective/{date}-perspective-report.md` with findings, a recommendation, and concrete action items as checkboxes.
6. **Implement** (optional) — asks if you want the fixes applied. If yes, works through each action item, makes the changes, and checks off completed items in the report.

## Output

Reports go into a `perspective/` folder in your project root. Each report includes:

- What you're building (confirmed scope)
- What already exists (similar/identical projects)
- Alternative approaches with trade-offs
- Coherency findings (duplication, overlap, unnecessary complexity)
- Codebase audit findings (current vs outdated practices)
- A recommendation: stay the course, adjust, pivot, or adopt
- Action items with checkboxes (marked off as they're implemented)

## Requirements

- [Claude Code](https://claude.com/product/claude-code)
- Node.js 18+ (for Context7 MCP)

## License

MIT
