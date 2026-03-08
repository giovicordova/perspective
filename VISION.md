# Perspective

A Claude Code skill that reality-checks your project before you go deep.

## How It Works

**1. Understand the scope.** Perspective reads your project's root documents — README, CLAUDE.md, VISION, structure — and builds a summary of what you're doing, why, and how. It confirms this with you before moving on. No assumptions go unchecked.

**2. Research the landscape.** It searches for existing solutions, similar projects, and alternative approaches. If something identical already exists, it says so directly. If better patterns or architectures are out there, it surfaces them with honest trade-offs.

**3. Audit the codebase.** Using Context7 MCP for live, version-specific documentation, it checks your actual code against current practices. Claude's training has a cutoff — APIs evolve, patterns get deprecated, new tools emerge. This step catches anywhere your codebase relies on stale knowledge instead of current ground truth.

**4. Report.** A short, structured analysis: what you're building, what already exists, where your code is current and where it's outdated, alternatives worth considering, and a clear recommendation — stay the course, pivot, or adopt something already built.

## Why

Time is the most expensive resource. Perspective catches wrong directions early, when changing course is cheap — not after weeks of invested work. It's the difference between "is this API call correct?" and "should we be using this API at all?"

## The Principle

Build the right thing, the right way, before building it fast.
