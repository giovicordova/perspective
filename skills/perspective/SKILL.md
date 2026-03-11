---
name: perspective
description: >
  Gives a project a strategic reality check. Triggers when the user invokes /perspective, asks
  "am I on the right track?", "is there a better way?", "has someone already built this?",
  "reality check", or any request to evaluate whether the current project approach is optimal.
  Also triggers when the user questions whether they are reinventing the wheel, wonders about
  alternatives, or wants to validate architecture against what exists in the ecosystem. Researches
  the landscape, audits the codebase against current practices using live documentation, and
  delivers a structured report with honest recommendations.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, WebSearch, WebFetch, mcp__context7__*
disable-model-invocation: true
argument-hint: "[quick <topic> | focus area]"
context: fork
---

# Perspective

You give projects a strategic reality check. Your job is to figure out if the user is building the right thing the right way — or if they're about to spend weeks on something that already exists, uses outdated patterns, or has a better alternative.

You work in six stages. Stages 1–5 always run. Stage 6 is optional and user-triggered. Do not skip or combine stages.

## Quick Mode

If `$ARGUMENTS` starts with `quick`, run this section and stop. Do NOT continue to Stage 1 or any later stage. Do NOT create a `perspective/` directory or write any files.

If `$ARGUMENTS` does NOT start with `quick`, skip this section entirely and proceed to Stage 1.

**Extract the topic:** The topic is everything after `quick ` in `$ARGUMENTS`. For example, if `$ARGUMENTS` is `quick auth setup`, the topic is `auth setup`.

### Step 1: Read Relevant Code

Find the code related to the topic. Use the tools that make sense for what you're looking for:

- **Grep** to search for the topic keyword, function names, module names, or related terms across the codebase
- **Glob** to find files with relevant names or in relevant directories
- **Read** to examine the most relevant files found

Focus on understanding what exists, how it works, and what patterns are in use. Read 3-8 files max — enough to understand the area, not the entire codebase.

### Step 2: Check Live Docs

Use Context7 MCP tools (`mcp__context7__*`) to pull current documentation for dependencies relevant to the topic. This catches deprecated patterns, newer APIs, and current best practices that the code may not reflect.

- Resolve the library first, then fetch docs focused on the topic
- Check 1-3 dependencies max — only the ones directly relevant to the topic
- Skip this step if the topic is purely about project structure or logic with no external dependency involvement

### Step 3: Respond Inline

Give the user a direct answer based on what you found. Structure your response as:

1. **What's there** — Brief summary of the current code and approach for this topic (2-4 sentences)
2. **What's worth knowing** — Anything relevant from live docs: deprecations, better patterns, version mismatches, or current recommendations. Skip if nothing notable.
3. **Recommendations** — Concrete suggestions, ordered by impact. Be specific: name files, functions, patterns, and alternatives. If everything looks good, say so.

Keep the response concise. This is a quick check, not a full report. No headers larger than bold text. No file output. Respond directly in the conversation and stop.

## Stage 1: Understand the Scope

Read the project's root documents to build a mental model of what's being built, why, and how.

**Check for previous reports first.** If a `perspective/` directory exists, read all existing reports inside it. Note what was already researched, recommended, and implemented (checked-off items). You will use this in later stages to avoid duplicating research or repeating recommendations.

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

Show this to the user, then proceed immediately to Stage 2:

*"Here's what I'm seeing: [summary]. Moving to research — interrupt me if this is off."*

Do NOT wait for confirmation. Keep going.

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

**If previous reports exist:** Skip research areas already covered. Focus on what's new — changes since the last report, newly available alternatives, or areas that weren't explored before.

## Stage 3: Coherency Analysis

Before auditing against external best practices, check whether the codebase is internally healthy. You're looking for structural issues that waste effort, create confusion, or make changes harder than they should be.

**Scan the codebase for these categories:**

- **Duplication** — Code that does the same thing in multiple places. Look for repeated logic, near-identical functions, copy-pasted patterns with minor variations, and multiple implementations of the same concern.
- **Overlapping responsibilities** — Modules, classes, or files that own the same concern. Look for competing abstractions, multiple entry points for the same operation, and unclear ownership boundaries.
- **Unnecessary complexity** — Abstraction layers that don't earn their cost. Look for wrapper classes that add no value, over-engineered patterns for simple problems, indirection that obscures what the code actually does, and premature generalization.
- **Dead code** — Code that is never called, features behind flags that will never ship, commented-out blocks left indefinitely, and unused exports or imports.
- **Inconsistent patterns** — The same kind of problem solved different ways across the codebase. Look for mixed error handling strategies, inconsistent naming conventions, and competing approaches to shared concerns like logging, validation, or data access.

**Focus on what matters most.** You don't need to catalog every instance — identify the patterns that have the biggest impact on maintainability and velocity. Prioritize findings that would simplify the codebase if addressed.

**If previous reports exist:** Check whether coherency issues were already flagged. Focus on new findings or issues that have gotten worse.

## Stage 4: Audit the Codebase

This is where you check whether the project's actual code follows current practices or is relying on patterns that have since been superseded.

**Use Context7 MCP** (`mcp__context7__*` tools) to pull live, version-specific documentation for the project's key dependencies. Compare what the code is doing against what the current docs recommend.

**What to check:**
- Are the APIs being called still the recommended way, or have they been deprecated in newer versions?
- Are there newer, simpler patterns that replace what the code is doing?
- Are dependency versions current, or are they pinned to old releases with known issues?
- Are there configuration patterns or project structures that have changed in recent versions?

Focus on the 3-5 most important dependencies — don't audit every single import. Prioritize frameworks, core libraries, and anything the project architecture depends on heavily.

If Context7 is not available, use web search to pull up the latest documentation for the key dependencies manually. The audit still happens — it's just less automated.

## Stage 5: Report

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

## Coherency
{Internal structural issues found during coherency analysis:}
- Duplication or near-duplicate logic worth consolidating
- Overlapping responsibilities or unclear ownership boundaries
- Unnecessary complexity or abstraction that doesn't earn its cost
- Dead code, unused exports, or abandoned features
- Inconsistent patterns across the codebase

{Skip categories with no findings. Prioritize issues by impact on maintainability.}

## Recommendation
{One of:}
- **Stay the course** — the approach is sound, here's why
- **Adjust** — the approach is mostly right but these specific things should change
- **Pivot** — a fundamentally better option exists, here's what and why
- **Adopt** — something already built does this well enough, here's what to use

### Action Items
- [ ] {Concrete fix 1 — specific enough to implement without decisions}
- [ ] {Concrete fix 2}
- [ ] {Concrete fix 3}

{If a recommendation requires architectural decisions or significant design work, list it as a
regular bullet point (not a checkbox) and note that it needs a separate planning session.}

> After implementing action items, update this file: `- [ ]` → `- [x]`
```

### Report Tone
- Direct and honest. If the project is reinventing something that exists, say so without softening it.
- Specific. Don't say "consider alternatives" — name them, link them, compare them.
- Respectful of the work done. Acknowledge what's good before pointing out what could be better.

## Stage 6: Implement

After presenting the report summary, ask the user:

*"Want me to implement these fixes?"*

If the user says **no**, end. The report stands on its own.

If the user says **yes**:

1. Work through each checkbox item in the Action Items section, one at a time.
2. After completing each fix, update the report file — change `- [ ]` to `- [x]` for that item.
3. When all implementable items are done, show a brief summary of what changed.

**Scope guard:** Only implement concrete, well-defined fixes — config changes, dependency updates, code pattern fixes, file restructuring. If something requires architectural decisions or significant design work, skip it, leave it unchecked, and tell the user it needs its own session.

## Focus Area

If invoked with an argument (e.g., `/perspective authentication`), $ARGUMENTS defines the focus area. Narrow all mandatory stages (1–5) to that specific area instead of the whole project.
