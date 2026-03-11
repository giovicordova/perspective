---
estimated_steps: 4
estimated_files: 1
---

# T01: Add quick mode branch to SKILL.md

**Slice:** S03 — Lightweight Quick Mode
**Milestone:** M001

## Description

Add a quick-mode early-exit branch to SKILL.md that intercepts `/perspective quick <topic>` invocations before the full 6-stage pipeline. Update frontmatter to advertise both modes. The quick pipeline is 3 steps: read relevant code for the topic, pull live docs via Context7, respond inline. No file output. Full pipeline stays untouched.

## Steps

1. Update `argument-hint` in SKILL.md frontmatter from `"[focus area]"` to `"[quick <topic> | focus area]"`
2. Add a `## Quick Mode` section between the intro paragraph ("You work in six stages...") and `## Stage 1: Understand the Scope`. The section must:
   - Detect: if `$ARGUMENTS` starts with `quick`, extract the topic (everything after `quick `)
   - Step 1: Read relevant code — use Read, Grep, Glob to find files related to the topic
   - Step 2: Check live docs — use Context7 MCP tools to pull current documentation for relevant dependencies
   - Step 3: Respond inline — synthesize findings into a direct answer with concrete recommendations
   - Explicitly instruct: do NOT write any files, do NOT create a `perspective/` directory, do NOT continue to Stage 1
   - Explicitly instruct: if `$ARGUMENTS` does NOT start with `quick`, skip this section entirely and proceed to Stage 1
3. Verify: run `wc -l`, check argument-hint, confirm section ordering, diff against previous to ensure no existing stage modifications
4. Verify negative case: confirm the Focus Area section at the bottom still works for non-quick arguments

## Must-Haves

- [ ] `argument-hint` frontmatter updated to include `quick`
- [ ] Quick mode section placed after intro, before Stage 1
- [ ] Detection logic checks for `quick` prefix in `$ARGUMENTS`
- [ ] 3-step quick pipeline: read code → check docs → answer inline
- [ ] Explicit no-file-output instruction
- [ ] Explicit early exit — do not continue to Stage 1
- [ ] Explicit fall-through — non-quick arguments skip this section
- [ ] No modifications to any existing Stage 1–6 content
- [ ] SKILL.md under 500 lines total

## Verification

- `wc -l < skills/perspective/SKILL.md` returns under 500
- `grep 'argument-hint' skills/perspective/SKILL.md` shows `quick` in the hint
- `grep -n '## Quick Mode' skills/perspective/SKILL.md` returns a line number
- `grep -n '## Stage 1' skills/perspective/SKILL.md` returns a higher line number than Quick Mode
- `git diff skills/perspective/SKILL.md` shows additions only in frontmatter and the new section — no changes within existing Stage 1–6 blocks
- Quick mode section contains references to Read/Grep/Glob, Context7/mcp tools, and "do not" write files

## Observability Impact

- Signals added/changed: None — prompt file, not runtime
- How a future agent inspects this: Read SKILL.md, check section ordering, count lines
- Failure state exposed: None — failure manifests as wrong pipeline running (observable by user)

## Inputs

- `skills/perspective/SKILL.md` — current 190-line file with 6-stage pipeline, frontmatter with `argument-hint: "[focus area]"`
- S03-RESEARCH.md — design: early-exit branch, 3-step flow, ~40-60 lines budget
- D002 — `/perspective quick <topic>` prefix keyword trigger

## Expected Output

- `skills/perspective/SKILL.md` — updated with quick mode section (~230-250 lines total), argument-hint showing both modes
