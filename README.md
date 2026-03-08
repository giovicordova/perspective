# Perspective

A Claude Code skill that reality-checks your project before you go deep.

## What It Does

You're mid-project. You invoke `/perspective`. It reads your project docs, confirms it understands what you're building, researches the landscape for existing solutions and better approaches, audits your codebase against current (not training-data-stale) documentation, and writes a report with an honest recommendation: stay the course, adjust, pivot, or adopt something that already exists.

## Install

### 1. Install Context7 MCP (required)

Perspective uses Context7 to pull live, version-specific library docs at query time — so it can tell you if your code uses deprecated patterns even if Claude's training data says otherwise.

```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```

Requires Node.js 18+.

### 2. Install the skill

Copy the `skills/perspective/` folder into your Claude Code skills directory:

```bash
cp -r skills/perspective ~/.claude/skills/perspective
```

## Usage

```
/perspective
```

Or with a specific focus area:

```
/perspective authentication
/perspective state management
```

## How It Works

1. **Understand** — reads your project's root documents (README, CLAUDE.md, VISION, package.json, etc.), summarizes what you're building, and confirms with you before proceeding.
2. **Research** — searches for existing solutions, similar projects, alternative approaches, and current best practices.
3. **Audit** — uses Context7 MCP to check your actual code against live documentation. Catches deprecated APIs, outdated patterns, and stale dependency versions that Claude wouldn't know about from training data alone.
4. **Report** — writes a structured analysis to `perspective/{date}-perspective-report.md` with findings and a clear recommendation.

## Output

Reports go into a `perspective/` folder in your project root. Each report includes:

- What you're building (confirmed scope)
- What already exists (similar/identical projects)
- Alternative approaches with trade-offs
- Codebase audit findings (current vs outdated practices)
- A recommendation: stay the course, adjust, pivot, or adopt

## Requirements

- [Claude Code](https://claude.com/product/claude-code)
- [Context7 MCP](https://github.com/upstash/context7) (Node.js 18+)
