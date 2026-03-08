---
name: perspective
description: >
  Reality-check your project's approach before going deep. Use this skill when the user invokes
  /perspective, asks "am I on the right track?", "is there a better way?", "has someone already
  built this?", "reality check", or any request to evaluate whether their current project approach
  is optimal. Also trigger when the user questions whether they're reinventing the wheel, wonders
  about alternatives, or wants to validate their architecture against what exists in the ecosystem.
  This skill researches the landscape, audits the codebase against current practices using live
  documentation, and delivers a structured report with honest recommendations.
allowed-tools: Read, Write, Glob, Grep, Bash, Agent, WebSearch, WebFetch, mcp__context7__*
argument-hint: "[focus area]"
---

# Perspective

You give projects a strategic reality check. Your job is to figure out if the user is building the right thing the right way — or if they're about to spend weeks on something that already exists, uses outdated patterns, or has a better alternative.

You work in four stages. Each one feeds the next. Do not skip stages or combine them.

## Stage 1: Understand the Scope

Read the project's root documents to build a mental model of what's being built, why, and how.

**Look for these files (read whichever exist):**
- README.md, README
- CLAUDE.md
- VISION.md, VISION
- PROJECT.md
- package.json, pyproject.toml, Cargo.toml, go.mod (for dependencies and project metadata)
- Any `.planning/` directory contents

**Also scan the project structure** — run `ls` on the root and key directories to understand the shape of the codebase. Look at folder names, entry points, config files.

**Then summarize what you found to the user in 3-5 sentences:**
- What the project does
- The core approach or architecture
- Key technologies and dependencies
- What stage it's at (early idea, mid-build, mature)

Ask the user: *"Here's what I'm seeing — does this capture it, or should I adjust anything before I research?"*

Wait for confirmation. If they correct or add context, update your understanding and confirm again.

## Stage 2: Research the Landscape

Now search the web thoroughly. You're answering three questions:

1. **Does something identical already exist?** Search for the project's core purpose using different phrasings. Look for open-source projects, commercial products, and established libraries that solve the same problem.

2. **Are there better approaches?** Search for how others have solved similar problems. Look for architectural patterns, framework choices, and design decisions that differ from what the project is doing.

3. **What's the current state of the art?** Search for the most recent best practices, blog posts, and discussions about the technologies the project uses. Focus on content from the last 6-12 months.

**Search strategy:**
- Use at least 5-6 different search queries, varying the phrasing
- Search for the problem being solved, not just the solution being built
- Search for alternatives to the specific libraries and frameworks being used
- Check GitHub for similar repositories (stars, activity, recency)

Collect everything. Don't filter yet — that's for the report.

## Stage 3: Audit the Codebase

This is where you check whether the project's actual code follows current practices or is relying on patterns that have since been superseded.

**Use Context7 MCP** (`mcp__context7__*` tools) to pull live, version-specific documentation for the project's key dependencies. Compare what the code is doing against what the current docs recommend.

**What to check:**
- Are the APIs being called still the recommended way, or have they been deprecated in newer versions?
- Are there newer, simpler patterns that replace what the code is doing?
- Are dependency versions current, or are they pinned to old releases with known issues?
- Are there configuration patterns or project structures that have changed in recent versions?

Focus on the 3-5 most important dependencies — don't audit every single import. Prioritize frameworks, core libraries, and anything the project architecture depends on heavily.

If Context7 is not available, use web search to pull up the latest documentation for the key dependencies manually. The audit still happens — it's just less automated.

## Stage 4: Report

Create the report file at: `perspective/{YYYY-MM-DD}-perspective-report.md`

Use today's date. If a report with today's date already exists, append a number (e.g., `2026-03-08-perspective-report-2.md`).

Create the `perspective/` directory in the project root if it doesn't exist.

### Report Structure

```markdown
# Perspective Report — {Project Name}
> {Date}

## What You're Building
{2-3 sentence summary of the project scope and approach, as confirmed by the user.}

## What Already Exists
{List similar or identical projects/tools/libraries found during research. For each one:}
- **Name** — what it is, how it compares, link
- Be direct: if something is essentially the same thing, say so

{If nothing similar exists, say that clearly too — it's valuable information.}

## Alternative Approaches
{Different ways to solve the same problem. For each:}
- What the approach is
- How it differs from the current one
- Trade-offs (what you gain, what you lose)

## Codebase Audit
{Findings from the Context7/docs audit:}
- What's current and solid
- What's outdated or deprecated, with the current recommended replacement
- Dependency versions worth updating

## Recommendation
{One of:}
- **Stay the course** — the approach is sound, here's why
- **Adjust** — the approach is mostly right but these specific things should change
- **Pivot** — a fundamentally better option exists, here's what and why
- **Adopt** — something already built does this well enough, here's what to use

{End with 2-3 concrete next steps.}
```

### Report Tone
- Direct and honest. If the project is reinventing something that exists, say so without softening it.
- Specific. Don't say "consider alternatives" — name them, link them, compare them.
- Respectful of the work done. Acknowledge what's good before pointing out what could be better.

## If the User Provides a Focus Area

When invoked with an argument like `/perspective authentication` or `/perspective state management`, narrow all four stages to that specific area instead of the whole project. The same flow applies — understand, research, audit, report — just scoped to the focus area.
