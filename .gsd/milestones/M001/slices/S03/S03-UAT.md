# S03: Lightweight Quick Mode — UAT

**Milestone:** M001
**Written:** 2026-03-11

## UAT Type

- UAT mode: artifact-driven
- Why this mode is sufficient: The product is a prompt file (SKILL.md). Contract verification confirms the content is correct and structured properly. Live runtime testing (Claude Code interpreting the skill) is deferred to S04 final verification where all features are tested together.

## Preconditions

- `skills/perspective/SKILL.md` exists with quick mode section
- No server or runtime environment needed — this is a static prompt file

## Smoke Test

`grep -n '## Quick Mode' skills/perspective/SKILL.md` returns a line number before the first `## Stage` line, confirming quick mode is positioned as an early-exit branch.

## Test Cases

### 1. Quick mode section exists and is correctly positioned

1. `grep -n '## Quick Mode\|## Stage 1' skills/perspective/SKILL.md`
2. **Expected:** Quick Mode line number < Stage 1 line number

### 2. argument-hint signals both modes

1. `head -15 skills/perspective/SKILL.md`
2. **Expected:** `argument-hint` contains `[quick <topic> | focus area]`

### 3. Quick mode contains all 3 steps

1. `grep '### Step' skills/perspective/SKILL.md`
2. **Expected:** Step 1 (Read Relevant Code), Step 2 (Check Live Docs), Step 3 (Respond Inline)

### 4. No file output instruction present

1. `grep -i 'no file output\|no.*perspective.*directory' skills/perspective/SKILL.md`
2. **Expected:** At least one match confirming quick mode produces no file output

### 5. Line count within budget

1. `wc -l < skills/perspective/SKILL.md`
2. **Expected:** Under 500

### 6. Existing stages unmodified

1. `git log --oneline -1 -- skills/perspective/SKILL.md` (check most recent commit)
2. Review diff of that commit
3. **Expected:** Only additions (new section + frontmatter update), no modifications to Stage 1–6 content

## Edge Cases

### Quick mode with no topic

1. User invokes `/perspective quick` with no topic after `quick`
2. **Expected:** The skill extracts an empty topic. Behavior is undefined but non-destructive — worst case, the agent asks for clarification. Not explicitly handled; acceptable for v1.

### Ambiguous "quick" in focus area

1. User invokes `/perspective quick-start guide` (quick as part of a compound word)
2. **Expected:** `$ARGUMENTS` starts with `quick` so this would incorrectly trigger quick mode. Edge case accepted — the `quick` keyword is an explicit prefix convention.

## Failure Signals

- Quick mode section missing or positioned after Stage 1
- `argument-hint` doesn't mention `quick`
- Line count exceeds 500
- Existing stage content modified (regression)

## Requirements Proved By This UAT

- R003 (Lightweight quick mode) — contract-verified: SKILL.md contains correct quick mode logic with 3-step flow, early exit, no file output. Not runtime-verified.
- R006 (Line budget) — partially: 226 lines, well under 500. Full verification in S04.

## Not Proven By This UAT

- R003 runtime behavior — actual invocation of `/perspective quick <topic>` producing correct output (requires Claude Code interpreting the skill live; tested in S04)
- Whether Context7 integration in quick mode works correctly at runtime
- Edge case handling for empty topics or ambiguous `quick` prefix

## Notes for Tester

When S04 runs end-to-end UAT, test `/perspective quick auth setup` (or similar) on a real project. The quick mode should: read a few files, optionally check docs, and respond inline — no report file created, no multi-stage pipeline visible. If the full pipeline runs instead, the quick mode detection failed.
